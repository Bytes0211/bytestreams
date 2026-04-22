# 10DLC Campaign Submission — DialTone (ByteStreams LLC)

**Registration model:** ISV / Reseller. Each restaurant client will register their own Brand and Campaign via ByteStreams as the CSP. This document is the **template** that each restaurant's submission will follow, with per-restaurant fields noted in `{{ double_braces }}`.

**About this revision:** This submission reflects DialTone's actual end-to-end flow. The critical detail a reviewer needs to understand — and your previous submission likely missed — is that **the Stripe payment link SMS is the mechanism by which an order is completed**. Consent is captured verbally at the start of the call, before order-taking begins, because without consent the payment link cannot be sent and the order cannot be placed. Every field below is written to make this flow transparent and verifiable to a carrier reviewer.

---

## Campaign Registration Fields

### Use Case Type

**Mixed Use Case** — covers three distinct message purposes within a single customer relationship:

- **Customer Care / Account Notification** — payment link, order confirmation, order-ready status
- **Delivery Notifications** — when delivery is supported (out-for-delivery updates)
- **Marketing** — occasional promotional messages, only to customers who separately opt in

*If the messaging provider's form forces single-purpose campaigns: split into two campaigns. The primary campaign is "Mixed (Customer Care + Delivery)" covering all transactional messages — this must be approved before the restaurant can take orders at all. A secondary "Marketing" campaign is registered only for restaurants on the Marketing add-on. Do not combine promotional sends into the transactional campaign.*

---

### Campaign Description

> {{Restaurant_Name}} is a {{cuisine_type}} restaurant located at {{full_street_address}}, operating in Tennessee / {{state}}. {{Restaurant_Name}} uses DialTone, an AI voice ordering platform operated by ByteStreams LLC (a Tennessee LLC), to answer inbound customer calls and process food orders.
>
> When a customer calls {{Restaurant_Phone}}, DialTone's AI voice agent answers, captures verbal SMS consent at the start of the call, and then takes the customer's order. Once the order is finalized, DialTone sends the customer a Stripe payment link via SMS. After the customer completes payment through Stripe Checkout, DialTone sends a payment confirmation SMS containing the order details, followed by a "your order is ready" SMS when the restaurant marks the order complete. Customers who also opt in to promotional messages may receive occasional offers from {{Restaurant_Name}}, no more than two (2) per month.
>
> Consent is captured verbally during the phone call and the call is recorded. An alternate consent channel is available on {{Restaurant_Website_URL}} via an unchecked consent checkbox on the online ordering form. All messages are sent on behalf of {{Restaurant_Name}}, identify {{Restaurant_Name}} as the sender, and include opt-out language (Reply STOP). Customers who decline SMS consent during the call are politely offered a transfer to restaurant staff so their order can be placed manually.

**Why this works:**

- Explicitly names the business, cuisine, and physical address (reviewer can verify)
- Explicitly describes DialTone's role — prevents the reviewer from thinking this is direct-to-consumer ByteStreams messaging
- **Leads with the payment link SMS as the core transactional message** — this is the critical detail that justifies why SMS is required and why consent is captured upfront
- Specifies message types in order of occurrence: consent capture → payment link → payment confirmation → order ready → optional promotional
- Specifies promotional frequency limit (2/month)
- States what happens when consent is refused (transfer to staff) — this preempts the reviewer's "what if someone doesn't want texts?" objection
- Identifies the STOP keyword

---

### Call-to-Action / Message Flow *(how users opt in)*

> Customers opt in through one of two channels:
>
> **(1) Voice opt-in at the start of a phone order.** When a customer calls {{Restaurant_Name}} at {{Restaurant_Phone}}, DialTone's AI voice agent answers and captures SMS consent before taking the order. The agent says:
>
> "Thanks for calling {{Restaurant_Name}}! I can take your order right now. Just so you know, to finish your order I'll need to text you a payment link from Stripe, and I'll also text you a confirmation when payment clears and another text when your order is ready. Message and data rates may apply, and you can reply STOP anytime to unsubscribe. Is it OK to text you at the number you're calling from?"
>
> If the customer says yes, the agent confirms the last four digits of the calling number and proceeds to take the order. If the customer says no, the agent responds: "No problem — let me transfer you to the restaurant so they can take your order directly. One moment." The call is transferred to the restaurant's staff line.
>
> Verbal consent is recorded, time-stamped, and retained with a transcript excerpt showing the exact consent moment. No order is taken until consent has been captured.
>
> **(2) Website opt-in at {{Restaurant_Website_URL}}/order.** When a customer places an order through the restaurant's online ordering page, the form includes an unchecked consent checkbox directly above the submit button:
>
> "☐ Yes, text me a Stripe payment link, order confirmation, and pickup/delivery updates from {{Restaurant_Name}}. Message frequency varies. Message and data rates may apply. Reply STOP to unsubscribe. See our Privacy Policy at {{Restaurant_Website_URL}}/privacy and SMS Terms at {{Restaurant_Website_URL}}/sms-terms."
>
> Below it, a separate, also-unchecked checkbox for promotional opt-in:
>
> "☐ Also text me occasional offers from {{Restaurant_Name}}, up to 2 per month. Reply STOP to unsubscribe."
>
> Both checkboxes are unchecked by default. Transactional consent is required to submit the form; promotional consent is optional. The submission timestamp, IP address, user agent, and the exact consent language shown at the time of submission are logged.

**Why this works:**

- Two concrete channels, both fully described
- **Exact consent language is quoted verbatim**, not paraphrased — reviewers can compare it to what's on the website and what's on the call recording
- Addresses how consent refusal is handled (transfer to staff) — this is a key differentiator from generic "we ask, if they say no, we don't text them" flows
- Describes the consent *logging* in detail (timestamp, transcript excerpt, IP address) — reviewers want to know the evidence trail is real
- Transactional and promotional consent are clearly separated
- Checkboxes are explicitly unchecked by default (common rejection trigger)

---

### Sample Messages

Include **six** sample messages, representing every message type the customer may receive in a typical order. Each identifies the sender and includes opt-out language.

**Sample 1 — Payment Link (sent after order is finalized on the call)**
> {{Restaurant_Name}}: Your order is ready to pay. Total: $24.80. Tap to pay securely with Stripe: https://pay.dialtone.menu/a4821k9. Link expires in 20 min. Reply STOP to opt out.

**Sample 2 — Payment Confirmation (sent after Stripe webhook fires)**
> {{Restaurant_Name}}: Payment confirmed, thank you! Order #A4821. 1x Margherita Pizza, 1x Caesar Salad. Ready for pickup around 6:45 PM. Reply STOP to opt out.

**Sample 3 — Order Ready (sent when restaurant marks order complete)**
> {{Restaurant_Name}}: Your order #A4821 is ready for pickup. See you soon! Questions? Call (555) 123-4567. Reply STOP to opt out.

**Sample 4 — Reservation Confirmation (v1.1, reservation flow)**
> {{Restaurant_Name}}: Reservation confirmed for Fri Apr 24 at 7:00 PM for 4 guests. Reply CANCEL to cancel or call (555) 123-4567. Reply STOP to opt out.

**Sample 5 — Promotional (only on Marketing add-on, only to promotional-opted-in customers)**
> {{Restaurant_Name}}: Happy Friday! 15% off any pasta entrée tonight with code PASTA15. Order at {{short_link}}. Reply STOP to opt out.

**Sample 6 — HELP Response (required, often omitted)**
> {{Restaurant_Name}}: For help with your order, call (555) 123-4567 or email hello@{{restaurant_domain}}. Msg & data rates may apply. Reply STOP to cancel.

**Why these work:**

- Every sample begins with the brand name
- Every sample ends with STOP language
- **Sample 1 explicitly shows the payment link use case and shows the short-link domain (pay.dialtone.menu)** — reviewers scrutinize links heavily, and a branded domain passes scrutiny better than a raw stripe.com URL
- Sample 1 includes a link expiry time — shows you've thought about the security posture
- Sample 2 is the confirmation-after-payment (not an opt-in confirmation) — this distinction matters: the opt-in already happened verbally
- Sample 3 uses a concrete order number and contact fallback
- Sample 4 covers reservations separately
- Sample 5 is clearly promotional and clearly gated
- Sample 6 handles HELP — frequently omitted and frequently rejected for that reason

---

### Embedded Links

**Yes.** Transactional messages contain a Stripe Checkout link, typically via a branded short domain (`pay.dialtone.menu/...`) that redirects to `checkout.stripe.com`. Promotional messages may contain a link to {{Restaurant_Website_URL}} or a branded short link.

**Domain disclosure:** Short-link redirects are served from `pay.dialtone.menu` (operated by ByteStreams LLC). The final destination for payment links is always `checkout.stripe.com`. This can be verified by the reviewer.

---

### Embedded Phone Numbers

**Yes.** HELP responses and some transactional messages include the restaurant's customer service phone number.

---

### Age-Gated Content

**No** for food-only restaurants. **Yes** for restaurants selling alcohol — see separate note in the "If Alcohol is Sold" section at the bottom of this document.

---

### Number Pooling

**No.** Each restaurant Brand is assigned a dedicated 10DLC number. Numbers are not shared across restaurants.

---

### Direct Lending Arrangement

**No.**

---

### Subscriber Opt-In

**Yes** — as described in the Call-to-Action section above.

---

### Subscriber Opt-Out

**Yes.** Customers may reply STOP, END, CANCEL, UNSUBSCRIBE, or QUIT at any time. DialTone's platform automatically processes opt-out requests and sends a final confirmation:

> {{Restaurant_Name}}: You've opted out and will no longer receive messages. To re-subscribe, reply START.

**Important:** An opt-out after a payment link has been sent but before payment has been completed triggers a phone call from restaurant staff to resolve the in-flight order. This is logged and disclosed in the SMS Terms page.

---

### Subscriber Help

**Yes.** Customers may reply HELP or INFO at any time and receive the HELP response (Sample 6 above).

---

## Critical Flow Details to Disclose When Asked

Reviewers sometimes ask follow-up questions before approving. Pre-drafted answers to the most likely ones:

**Q: What happens if the customer doesn't pay the payment link?**
The order stays in `pending_payment` status for 20 minutes, then automatically cancels. No further SMS messages are sent. If the customer wanted to complete the order, they can call back.

**Q: What happens if the payment link SMS doesn't deliver?**
DialTone detects delivery failures via Twilio's status callbacks. If the payment link fails to deliver, the voice agent will (during the same call, if still connected) inform the customer and offer to transfer to restaurant staff. If the call has already ended, the order is marked as failed and no further action is taken.

**Q: Can someone other than the caller pay the payment link?**
Technically yes — anyone with access to the text message can tap the link. This is a known limitation of SMS-based payment links across the industry. Stripe Checkout itself handles payment authentication, so the risk is limited to misuse of a phone with the recipient's knowledge. This is disclosed in the SMS Terms page.

**Q: Does the restaurant owner receive any SMS?**
No. Restaurant owners and staff interact with the DialTone admin dashboard (web-based) to receive new order notifications, manage orders, and update the menu. SMS is strictly consumer-facing.

**Q: How is caller consent different from website consent in terms of proof?**
Voice consent is proven by (a) the call recording, (b) a transcript with the consent moment timestamped and highlighted, (c) a consent record in the database with the `call_id` and `consent_language_version` fields. Website consent is proven by (a) the form submission log including the IP address, user agent, and timestamp, (b) a record of the exact consent language shown at the time of submission.

---

## Required Website Elements (for each restaurant Brand)

For the reviewer to approve each restaurant's campaign, the restaurant's website must have:

1. **A working HTTPS site at the URL listed on the Brand registration.** Reviewer will visit and verify.

2. **A Privacy Policy** at `/privacy` or `/privacy-policy` that includes:
   - A section covering SMS/text messaging specifically
   - Explicit statement that SMS opt-in data is **not shared with third parties for marketing or promotional purposes**
   - Contact information

3. **An SMS Terms page** at `/sms-terms` (linked from the consent checkbox and from the site footer) that covers:
   - Who is sending (brand name)
   - What messages will be sent (payment link, confirmations, order status, optional promotional)
   - Message frequency
   - "Message and data rates may apply"
   - How to opt out (Reply STOP)
   - How to get help (Reply HELP)
   - The platform provider (ByteStreams / DialTone)
   - Contact info

4. **The online ordering page** with the exact consent checkboxes quoted above. Both unchecked by default. Privacy Policy and SMS Terms links present near the checkboxes.

If any of these four elements is missing or the URLs don't resolve, the campaign will be rejected regardless of how good the submission text is.

---

## Platform-Level Requirements (for bytestreams.ai)

Because ByteStreams LLC is registering as the CSP (platform) supporting multiple restaurant Brands, the ByteStreams site must have:

1. A **Privacy Policy** covering the platform's role as a data processor
2. An **SMS Terms page** at `bytestreams.ai/sms-terms` explaining the platform's role in message delivery, the exact voice consent script, and the payment link flow
3. An **About page** that clearly identifies ByteStreams LLC, its EIN, registered address (Nashville, TN), and business purpose
4. A **sub-processor list** at a predictable URL (Twilio, Vapi, ElevenLabs, Supabase, Cloudflare, Stripe)
5. **Contact information** — `hello@bytestreams.ai` and `629-282-9555`

The reviewer will verify that the business is real and that the EIN on the Brand registration matches IRS records for "ByteStreams LLC" (or whatever the exact legal name is on the SS-4 — verify before submitting).

---

## If Alcohol is Sold

Restaurants that serve beer, wine, or cocktails require additional compliance:

- The online ordering flow must require **manual entry of date of birth (MM/DD/YYYY)** before the customer can see or order alcohol items. "Are you 21?" yes/no toggles do not pass review.
- Voice ordering of alcohol should either (a) route the customer to a human for ID verification at pickup, or (b) route to staff at the start of the call, bypassing DialTone for alcohol orders entirely.
- SMS samples should avoid explicit promotion of alcohol unless age-gating is demonstrably implemented on the website.
- **Recommendation for v1.0:** Exclude alcohol promotion from the Campaign entirely, even for restaurants that serve it. Order confirmations may include alcohol items already ordered, but promotional SMS stays food-only. Alcohol age-verification is better addressed as a v1.1 or v1.2 feature after the base platform has a clean compliance track record.

---

## Resubmission Strategy

Since the first submission was rejected, the resubmission should:

1. **Explicitly address the original rejection reason in the description.** If the rejection cited "insufficient opt-in detail," the new description should visibly resolve that. If it cited "unclear use case," the new description's structure should demonstrate exactly why the payment link SMS is needed.

2. **Not submit until every referenced URL resolves.** `{{Restaurant_Website_URL}}/privacy`, `/sms-terms`, and `/order` must all be live HTTPS pages with real content. Reviewers test them.

3. **Use a business email for the Brand admin.** `hello@bytestreams.ai` — not Gmail, Yahoo, or a personal address. Generic email domains are a documented rejection trigger.

4. **Verify legal entity name consistency.** "ByteStreams LLC" on the Brand form must match exactly what's on the IRS EIN letter, the SS-4, the Articles of Organization, and the website footer. Small capitalization differences ("Bytestreams" vs "ByteStreams") are flagged by automated review tools.

5. **Register ByteStreams itself as a Brand first, before any restaurant.** Get a platform-level Brand approved with a simple, low-stakes internal campaign (for example, DialTone's own signup confirmations to onboarding restaurants). Once that's approved and clean for 30 days, restaurant Brands benefit from your established CSP track record.

6. **Keep records of every consent event.** Reviewers may audit after approval. For voice consent, retain the call recording, the transcript with the consent moment highlighted, and the database record linking them.

---

## Fields to Fill Before Submitting

Anywhere you see `{{ double_braces }}` in this document, replace with the actual value. Before clicking submit, ctrl-F for `{{` to verify nothing slipped through. A submission containing literal `{{Restaurant_Name}}` is an instant rejection.
