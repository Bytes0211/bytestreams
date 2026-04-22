# DialTone Voice Agent — SMS Consent Script & Logging Spec

**Purpose:** Define the exact words the AI agent speaks to capture SMS consent, and the data DialTone must log to prove the consent exists if a carrier, customer, or regulator ever asks.

**Why consent is at the START of the call, not the end:** DialTone's ordering flow requires SMS to complete the transaction. After the order is finalized on the call, the customer receives a Stripe payment link via SMS. Without payment, the order never enters the kitchen. This means SMS consent is a gating condition — the agent cannot usefully take the order unless the customer has consented to SMS. Placing consent at the top of the call lets the agent either proceed with confidence or transfer the caller to restaurant staff without wasting the customer's time taking an order that can't be completed.

**Why this matters for 10DLC:** When you tell a reviewer "customers opt in verbally during the phone call," the reviewer may ask you to demonstrate how. If your answer is hand-wavy ("the agent kind of asks"), the campaign gets rejected. If your answer is "here's the exact script, here's the log record format, here's an example transcript, here's what happens when they say no," the reviewer has what they need.

---

## The Consent Moment — Exact Agent Script

The agent speaks this language immediately after greeting the caller, before taking any order information. This is the only place in the call flow where consent is captured.

### Primary script (start of call)

> "Thanks for calling {{Restaurant_Name}}! I can take your order right now. Just so you know, to finish your order I'll need to text you a payment link from Stripe, and I'll also text you a confirmation when payment clears and another text when your order is ready. Message and data rates may apply, and you can reply STOP anytime to unsubscribe. Is it OK to text you at the number you're calling from?"

### If caller says "yes"

> "Great. Just to confirm, that's the number ending in [READ BACK LAST FOUR DIGITS] — right?"

**Only after the caller confirms the number** does the agent proceed to take the order:

> "Perfect. What can I get started for you?"

### If caller says "no"

> "No problem — let me transfer you to the restaurant so they can take your order directly. One moment."

The agent then executes the transfer to the restaurant's configured staff line. No consent record is created. No order is attempted.

### If caller asks a follow-up question ("what kind of texts?", "how often?", "why do I need to do this?")

> "Just three texts — a payment link to finish your order, a confirmation when your card clears, and a heads-up when your order's ready. Nothing promotional unless you separately opt in later. You can reply STOP anytime. Sound OK?"

### If caller says something ambiguous ("I guess so", "sure whatever", "yeah I mean OK")

The agent must re-prompt for a clear yes or no. Ambiguous responses do not satisfy TCPA's "express consent" standard.

> "Just to make sure I heard that right — is that a yes to the texts, or would you rather I transfer you to the restaurant?"

### If caller wants the texts sent to a different number

> "Got it. What's the best number to text for this order?"

After the caller provides the number, the agent reads it back digit-by-digit and asks for confirmation:

> "Let me read that back — [DIGIT BY DIGIT]. Is that correct?"

**Only after the caller confirms** does the agent proceed. If there's any doubt about the number, the agent asks the caller to spell it again or offers to use the calling number instead.

---

## Optional Promotional Consent (separate, offered late in the call)

Promotional consent is **separate** from the mandatory transactional consent. It is offered near the end of a successful order, never at the start. It is always optional.

### If the restaurant has the Marketing add-on enabled

After the order is finalized but before ending the call:

> "Quick one before I let you go — want me to add you to occasional offers from {{Restaurant_Name}}? No more than two texts a month, things like a weekend special or a promo code. You can reply STOP anytime."

Record promotional consent separately: `promotional_consent: true` or `false`. Default is `false`.

### If the restaurant does NOT have the Marketing add-on

Skip this prompt entirely. Do not ask.

---

## Why the script is written this way

**Consent is captured before any order details.**
Taking an order and then discovering the customer won't consent to SMS wastes the customer's time and yours. Up-front gating respects everyone.

**The script explains *why* SMS is needed.**
"To finish your order I'll need to text you a payment link" gives the customer a concrete reason. Vague "can we text you?" prompts get "no" far more often than specific ones.

**It names the brand.**
Carrier reviewers look for this. A consent prompt that doesn't identify the sender fails.

**It enumerates the message types.**
"Payment link, payment confirmation, order ready" — the customer knows exactly what they're agreeing to receive.

**It includes required disclosures.**
"Message and data rates may apply" and "reply STOP anytime" are TCPA/carrier-required and must be present at the consent moment, not just in the messages themselves.

**It confirms the phone number.**
Prevents accidentally messaging the wrong number. Caller ID spoofing is real; so are shared business lines.

**It handles "no" gracefully.**
Transferring to staff preserves the customer relationship and the restaurant's revenue. A customer who hears "we can't help you" will remember that. A customer who hears "let me get you someone who can take this by phone" will not.

**It separates transactional and promotional consent.**
TCPA requires separate, express consent for marketing messages. Bundling the two is a documented violation.

**Ambiguous responses force re-prompt.**
"I guess so" is not express consent. Courts have ruled on this repeatedly. The script treats ambiguity as a soft no and re-prompts.

---

## Consent Log Record — Required Fields

Every consent event must produce a log record with at least these fields. Store these in Supabase (the `sms_consents` table) with row-level security so each restaurant can only see their own records.

| Field | Type | Example | Notes |
|---|---|---|---|
| `consent_id` | uuid | `b3f5c...` | Primary key |
| `restaurant_id` | uuid | `rest_abc123` | FK to restaurants table |
| `call_id` | uuid | `call_xyz789` | FK to the Vapi call record |
| `phone_number` | E.164 string | `+16155551234` | The number that will receive SMS |
| `phone_number_source` | enum | `calling_number` | `calling_number` \| `provided_by_caller` \| `website_form` |
| `phone_number_confirmed` | boolean | `true` | Did the caller confirm the number on the call? |
| `consent_timestamp` | timestamptz | `2026-04-22T18:42:17Z` | The exact moment "yes" was said |
| `transactional_consent` | boolean | `true` | Payment link + confirmation + ready notifications |
| `promotional_consent` | boolean | `false` | Marketing messages — default false |
| `promotional_consent_timestamp` | timestamptz \| null | `null` | Populated only if separate promo consent captured |
| `consent_method` | enum | `voice` | `voice` \| `web_form` \| `sms_keyword` |
| `consent_language_version` | string | `v1.0` | Which version of the consent script was used |
| `call_recording_url` | string | `https://...` | Link to the Vapi recording |
| `transcript_consent_excerpt` | text | *"...to finish your order I'll need to text..."* | The exact words spoken at the consent moment |
| `caller_ip_address` | inet | `null` for voice | Populated for web form consents |
| `user_agent` | text | `null` for voice | Populated for web form consents |
| `opt_out_timestamp` | timestamptz \| null | `null` | Populated when customer replies STOP |

**Retention:** Keep consent records for at least **4 years** after the most recent message sent to that number. TCPA statutes of limitation are typically 2 years, but carriers and plaintiffs' attorneys frequently cite 4 years to cover state law variations. Your lawyer will confirm.

---

## What Happens When Consent is Refused

When the customer declines SMS consent, the flow is:

1. Agent speaks the refusal script ("No problem — let me transfer you to the restaurant...").
2. Agent executes a warm transfer to the restaurant's configured `staff_transfer_number`.
3. **No consent record is created.** No partial record. No "pending" record.
4. **No order is attempted.** The customer's order will be taken by restaurant staff directly, outside DialTone.
5. The call is logged with `outcome: transferred_no_consent` for billing/analytics purposes. This call does NOT count as a completed paid order for billing — the restaurant is not charged.
6. The caller's phone number is NOT added to any suppression list. They're simply not a DialTone customer for this interaction. If they call back tomorrow and consent, that's a clean new interaction.

**Why this matters:** The refusal branch needs to be as clean as the accept branch. If any trace of the refused consent shows up in the database (even as "denied"), it creates ambiguity about whether consent was ever granted. The rule is: no consent record unless consent was actually granted.

---

## Example Consent Log Record (for your 10DLC submission)

If a reviewer asks "can you show me what a consent record looks like?", be ready to show this:

```json
{
  "consent_id": "b3f5c4a2-8f12-4e91-b0c3-2a5d7f9e1234",
  "restaurant_id": "rest_milanos_nashville",
  "call_id": "call_01HXY9Z8K2M3N4P5Q6R7S8T9",
  "phone_number": "+16155551234",
  "phone_number_source": "calling_number",
  "phone_number_confirmed": true,
  "consent_timestamp": "2026-04-22T18:42:17.334Z",
  "transactional_consent": true,
  "promotional_consent": true,
  "promotional_consent_timestamp": "2026-04-22T18:47:02.811Z",
  "consent_method": "voice",
  "consent_language_version": "v1.0",
  "call_recording_url": "https://recordings.dialtone.menu/call_01HXY...mp3",
  "transcript_consent_excerpt": "Agent: Thanks for calling Milano's Pizzeria! I can take your order right now. Just so you know, to finish your order I'll need to text you a payment link from Stripe, and I'll also text you a confirmation when payment clears and another text when your order is ready. Message and data rates may apply, and you can reply STOP anytime to unsubscribe. Is it OK to text you at the number you're calling from? Caller: Yeah, that's fine. Agent: Great. Just to confirm, that's the number ending in one-two-three-four — right? Caller: Yes. Agent: Perfect. What can I get started for you?",
  "caller_ip_address": null,
  "user_agent": null,
  "opt_out_timestamp": null
}
```

---

## Edge Cases to Handle

**1. Caller is silent after the consent prompt.**
After 4 seconds of silence, agent re-prompts once: "Are you still there? Just need a yes or no on the texts to continue." If still silent, agent wraps up politely and transfers to staff.

**2. Caller hangs up after the consent prompt but before answering.**
No consent record, no transfer, no order. Log `outcome: abandoned_at_consent` for analytics.

**3. Caller consents but then the call drops before the order is taken.**
The consent record is valid and retained. If the caller calls back later, the existing consent can be honored (for example, re-sending the payment link for an order placed in a follow-up call). But be cautious: the safer default is to re-capture consent on the new call unless you're confident the same number is calling and the intent is continuous.

**4. Caller asks "can I just order without the text?"**
> "Totally fair. I can't complete the payment through this AI line without texting you the link, but I can transfer you to the restaurant so they can take your order directly. Want me to do that?"

**5. Caller is a minor.**
DialTone has no reliable way to verify age on a voice call. If the voice agent detects clear youth-voice cues, the agent should decline to capture SMS consent and should transfer the call to restaurant staff. *Your lawyer should weigh in on the youth-voice detection threshold and whether this transfer should happen silently or with a disclosure to the caller.*

**6. Caller provides a different number than the calling number.**
Covered in the script. Read back digit-by-digit, require explicit confirmation, store as `phone_number_source: provided_by_caller`.

**7. Caller revokes consent mid-call ("actually, never mind, don't text me").**
Immediately honor. Mark any provisional consent record as revoked (or delete it — your lawyer can advise on retention). Transfer to staff if the order isn't complete, or end the call gracefully if it is.

**8. Caller asks to be opted out but has never opted in.**
Record the opt-out anyway as a suppression entry for that restaurant. Costs nothing; protects against re-messaging if they ever do opt in and then forget.

**9. Promotional consent is requested but the Marketing add-on is not enabled.**
This should never happen if the code is correct. If it does, do not send promotional messages regardless of consent. Audit the code path.

**10. Caller consents to transactional but not promotional.**
This is a legitimate and common outcome. `transactional_consent: true`, `promotional_consent: false`. Do not send promotional messages to this number even if the caller later places repeated orders.

---

## How This Script Should Appear in DialTone's Codebase

The consent script should live in a versioned config file (for example, `consent_scripts/voice_v1.0.ts`) so that:

- The exact language used for each consent event is recoverable for audits
- Changes to the script get a new version number, never overwrite
- The `consent_language_version` field in the log record is a real FK or version identifier
- A legal review of the script (pre-deployment) leaves a paper trail tied to a specific version

Don't hardcode the script into Vapi's system prompt as free text. Version it. Treat it like a legal artifact, not a UX string. When your lawyer blesses a version, that version number goes into every consent record captured while it was live.

---

## Integration With the Flow Diagram

Mapping to your `functional-process-flow` diagram:

| Flow node | What happens |
|---|---|
| `Customer calls` | Call arrives at Twilio |
| `Twilio routes call` | Forwarded to Vapi |
| `Vapi voice assistant` | Agent greets caller and immediately runs the **consent script** above |
| *(new: consent branch)* | If yes → proceed to `Customer intent`. If no → transfer to staff, end call. |
| `Customer intent` | Classify into order / reservation / knowledge |
| `finalize_order` → `Create order (pending_payment)` | Order is created only because consent was already captured |
| `Create Stripe payment link` → `Send payment link via Twilio SMS` | First SMS sent under consent |
| `Stripe webhook: payment_intent.succeeded` → `Order status becomes paid` → `Kitchen board receives new paid order` | Second SMS (payment confirmation) sent around this stage |
| `Staff advances status: preparing → ready → completed` | Third SMS (order ready) sent when status reaches `ready` |

The consent check is effectively a **new node** that sits between `Vapi voice assistant` and `Customer intent`. Consider updating the flow diagram to reflect this — it's an architectural detail, not just a UX one.
