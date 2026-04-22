# 10DLC Campaign Submission — ByteStreams LLC Internal Operations

**Purpose:** This is ByteStreams LLC's first 10DLC campaign, filed before any restaurant-facing campaigns. It registers a simple, low-risk internal notification flow that (1) establishes ByteStreams as an approved Brand, (2) builds a clean Trust Score track record with TCR and the carriers, and (3) demonstrates the CSP is operational before onboarding the first pilot restaurant.

**What this campaign is NOT:** It does not cover any messages to restaurant operators, restaurant diners, or any external recipient. It is strictly internal operations alerts to ByteStreams team members who have explicitly authorized their own mobile numbers to receive system notifications.

**Filing order:** This campaign first. Wait for approval + 30 clean days. Then file restaurant-operator campaigns and restaurant-customer campaigns as separate submissions.

---

## Brand Registration (Do This First)

Before the Campaign can be submitted, ByteStreams LLC must be registered as a Brand with The Campaign Registry. Use real values only:

| Field | Value |
|---|---|
| Legal business name | **ByteStreams LLC** *(verify exact capitalization against your SS-4 / Articles of Organization before submitting — mismatches are a top rejection trigger)* |
| EIN | *(from your IRS SS-4 letter)* |
| Business address | 501 Union St Ste 545, Nashville, TN 37219 |
| Country of registration | United States |
| State of registration | Tennessee |
| Business type | Private Company / LLC |
| Vertical | Technology / Professional Services |
| Website | https://bytestreams.ai |
| Support email | hello@bytestreams.ai |
| Support phone | 629-282-9555 |
| Brand contact (authorized rep) | *(your name, title, business email — not Gmail)* |

**Before submitting the Brand:**
- bytestreams.ai must resolve over HTTPS with real content (not a placeholder)
- `/privacy` must be live and readable
- `/sms-terms` must be live and readable
- The website must clearly identify ByteStreams LLC, its address, and its business purpose
- Contact information on the site must match what's on the Brand registration

Brand registrations typically take 2–5 business days. Campaigns cannot be submitted until the Brand is approved.

---

## Campaign Registration Fields

### Use Case Type

**Account Notification** *(single use case — do NOT select Mixed for this campaign)*

This is the cleanest possible use case for an internal operations campaign. Reviewers see it routinely and approve it quickly when the supporting details are tight.

---

### Campaign Description

> ByteStreams LLC operates DialTone, an AI voice ordering platform for restaurants. This campaign covers internal operational notifications sent only to authorized ByteStreams team members (currently a small set of technical staff) who have explicitly registered their mobile numbers in the internal admin system to receive alerts about production systems.
>
> Messages include: (a) production system alerts such as failed API calls to third-party services, elevated error rates, or health check failures; (b) Stripe webhook delivery failures; (c) deployment notifications when a new version of the platform is released; (d) on-call paging when a critical system component is unavailable. No messages are sent to restaurant operators, restaurant customers, or any external recipient under this campaign.
>
> Team members opt in by logging into the internal admin dashboard at admin.bytestreams.ai, navigating to their personal notification preferences page, entering their mobile number, and affirmatively enabling SMS alerts through a checkbox interface. Each opt-in is logged with the team member's user ID, timestamp, IP address, and the exact consent language shown at the time of opt-in. Team members may disable SMS alerts at any time via the same dashboard or by replying STOP to any message.

**Why this description works:**

- Narrow scope: internal team members only. No external recipients.
- Names the platform (DialTone) and the company (ByteStreams LLC) so reviewers can verify consistency with the Brand.
- Lists message types concretely — not "alerts and notifications" but specifically "API failures, webhook failures, deployments, on-call paging."
- Describes opt-in with enough detail that a reviewer can picture the flow without visiting the dashboard (they can't — it's behind auth).
- Explicitly rules out the failure modes reviewers worry about ("no messages to external recipients").
- Includes STOP keyword.

---

### Call-to-Action / Message Flow

> Opt-in is restricted to authorized ByteStreams team members. The flow is:
>
> 1. Team member is granted access to admin.bytestreams.ai as part of their employment or contracting agreement with ByteStreams LLC.
> 2. Team member logs into the admin dashboard using SSO or password authentication.
> 3. Team member navigates to Settings → Notification Preferences.
> 4. Team member enters their personal mobile number in the "SMS alerts number" field.
> 5. Team member checks an unchecked consent checkbox directly above the Save button reading: "☐ Send operational SMS alerts to this number. Message frequency varies based on system activity. Message and data rates may apply. Reply STOP to unsubscribe. See our SMS Terms at bytestreams.ai/sms-terms."
> 6. Team member clicks Save. A verification SMS is immediately sent to the provided number with language: "ByteStreams: Your number is now registered for system alerts. Reply STOP to cancel. Reply HELP for help."
> 7. Opt-in is logged with: team member user ID, mobile number, timestamp, IP address, user agent, and the version of the consent language shown.
>
> Numbers are never added without the account holder's own action. There is no bulk upload, no import from a list, no automated enrollment.

**Why this flow works:**

- Describes exactly where the opt-in happens (admin.bytestreams.ai, a specific page, a specific field)
- Describes the checkbox as unchecked by default
- Quotes the exact consent language verbatim
- Describes the double-opt-in confirmation (verification SMS)
- Rules out the failure modes reviewers worry about (bulk upload, list imports, auto-enrollment)

---

### Sample Messages

Four sample messages. Each begins with the brand name and ends with STOP language.

**Sample 1 — Opt-in Verification (sent immediately after consent)**
> ByteStreams: Your number is now registered for system alerts. Msg freq varies. Msg & data rates may apply. Reply HELP for help, STOP to cancel.

**Sample 2 — Production Alert**
> ByteStreams Alert: Vapi API error rate elevated above threshold (4.2% over last 5 min). Dashboard: https://admin.bytestreams.ai/ops. Reply STOP to opt out.

**Sample 3 — Deployment Notification**
> ByteStreams: Deploy v1.4.2 to production completed successfully at 02:14 UTC. 0 errors in post-deploy check. Reply STOP to opt out.

**Sample 4 — HELP Response**
> ByteStreams: For help with system alerts, contact hello@bytestreams.ai or 629-282-9555. Msg & data rates may apply. Reply STOP to cancel.

**Why these work:**

- Every sample begins with "ByteStreams" — brand name is explicit
- Every sample ends with STOP (or HELP/STOP in sample 4)
- Samples are clearly internal-ops in nature — no hint of marketing, transactions, or external recipient communication
- Sample 2 contains a link to admin.bytestreams.ai (auth-gated, which reviewers can verify)
- Sample 3 is a concrete, realistic deploy notification — not a placeholder

---

### Embedded Links

**Yes.** Some alert messages contain links to admin.bytestreams.ai (for example, "Dashboard: admin.bytestreams.ai/ops"). All links resolve to the ByteStreams admin domain, which is authenticated and accessible only to team members.

---

### Embedded Phone Numbers

**Yes.** The HELP response contains the ByteStreams support phone number (629-282-9555).

---

### Age-Gated Content

**No.** Internal operational alerts. No alcohol, tobacco, cannabis, firearms, gambling, or age-restricted content.

---

### Number Pooling

**No.** A single dedicated 10DLC number is used for this campaign.

---

### Direct Lending Arrangement

**No.**

---

### Subscriber Opt-In

**Yes** — as described in the Call-to-Action section above.

---

### Subscriber Opt-Out

**Yes.** Team members may reply STOP, END, CANCEL, UNSUBSCRIBE, or QUIT at any time. Opt-outs are processed automatically by the platform. A final confirmation is sent:

> ByteStreams: You've opted out of system alerts. To re-enable, log into admin.bytestreams.ai. Reply START to resubscribe.

---

### Subscriber Help

**Yes.** Replies of HELP or INFO receive the HELP response (Sample 4 above).

---

## Required Website Elements

Before submitting this campaign, the following must be live at bytestreams.ai:

1. **Homepage** at https://bytestreams.ai describing ByteStreams LLC and DialTone in plain language. Must clearly identify the company and its business purpose.

2. **Privacy Policy** at https://bytestreams.ai/privacy. For this campaign specifically, it must include:
   - A section on SMS / text messaging
   - Explicit language: "SMS opt-in data is not shared with third parties for marketing or promotional purposes"
   - Contact information matching the Brand registration

3. **SMS Terms** at https://bytestreams.ai/sms-terms. The page I built for you earlier covers this — confirm it's live before submitting.

4. **About or Contact page** identifying ByteStreams LLC as a Tennessee limited liability company with the Nashville address matching the Brand registration.

5. The admin dashboard at admin.bytestreams.ai does not need to be publicly accessible, but the login page should at minimum render (not a 404). Reviewers may click the domain to verify it resolves.

**If any of the above is missing or returns a 404, the campaign will be rejected.**

---

## What This Campaign Establishes for Your Trust Score

A clean approval on this campaign gives ByteStreams:

1. An approved Brand (reusable for all future restaurant campaigns — you don't re-register the Brand every time)
2. A track record of successful campaign operation as a CSP
3. Evidence to carriers that ByteStreams operates compliant infrastructure
4. A reference campaign to point to when restaurant-operator and restaurant-customer campaigns are submitted later

**Keep this campaign tidy during the first 30 days:**
- Actual sent volume should be low and consistent with the stated use case (production alerts happen, but not at marketing frequencies)
- No spam complaints (trivially easy when the only recipients are your team)
- No opt-out spikes
- Every sent message should match one of the approved sample types — don't drift the content

**Then, around day 30–45:**
- Submit the restaurant-operator notification campaign (for DialTone account holders — signup confirmations, billing alerts, security notifications)
- Onboard your first pilot restaurant and file their restaurant-customer campaign (the one in `10dlc-campaign-submission.md`)

Your CSP track record at that point will be: one clean internal campaign running for 30+ days, zero rejections, zero complaints. Restaurant campaigns submitted from that position have meaningfully better approval odds than first-time CSP submissions.

---

## Fields to Confirm Before Submitting

Unlike the restaurant-facing campaign template, this submission uses real ByteStreams data everywhere. Before clicking submit, verify:

- [ ] Exact legal entity name on the Brand form matches the SS-4 / Articles of Organization
- [ ] EIN on the Brand form matches the IRS letter
- [ ] Business address is current (not a prior address if you've moved)
- [ ] bytestreams.ai resolves over HTTPS
- [ ] /privacy is live with SMS-specific language and the "not shared with third parties" statement
- [ ] /sms-terms is live and matches the flow described in this submission
- [ ] admin.bytestreams.ai at minimum shows a login page (not a 404)
- [ ] The support email is a business email on the bytestreams.ai domain (not Gmail)
- [ ] All sample messages read naturally and include "ByteStreams" plus STOP language
- [ ] The Brand is approved before this Campaign is submitted (submitting the Campaign before the Brand is approved causes confusing errors)
