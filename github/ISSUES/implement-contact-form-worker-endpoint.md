# feat(contact, cloudflare): wire Get In Touch form to Worker API and email delivery

## Summary

The Get In Touch form currently validates on the client but does not deliver messages. This issue tracks implementing a server-side endpoint on the existing Cloudflare Worker so form submissions are processed and forwarded to the configured inbox.

---

## Problem

The form in `index.html` appears functional but is not connected to a backend transport.

Current behavior:
- Required-field and email validation run in the browser.
- Success text is shown after submit.
- No request is sent to any server endpoint.

Impact:
- Visitor messages are silently dropped.
- Contact intent is lost and cannot be recovered.

---

## Scope

Implement and deploy a server-backed contact flow using existing infrastructure:
- Add Worker endpoint `POST /api/contact`.
- Validate and sanitize payload (`name`, `email`, `message`).
- Add basic bot protection (honeypot field support).
- Forward valid submissions to destination mailbox via FormSubmit.
- Return structured JSON responses for success and failure states.
- Update frontend form logic to submit to `/api/contact` and surface API errors.

---

## Implementation

### Worker
- New entry file: `worker.js`
- Route handling:
  - `POST /api/contact` -> process contact payload
  - everything else -> `env.ASSETS.fetch(request)`
- Behavior:
  - Reject invalid JSON and invalid/missing fields (`400`).
  - Return `503` if destination email binding is missing.
  - Return `502` on upstream delivery failure.
  - Return `200` JSON on success.

### Frontend
- File: `js/main.js`
- Replace placeholder submit handler with async fetch to `/api/contact`.
- Preserve client-side validation and improve UX states:
  - "Sending your message..."
  - success confirmation
  - API error message display

### Config
- File: `wrangler.toml`
- Add:
  - `main = "worker.js"`
  - `[vars] CONTACT_EMAIL = "hello@bytestreams.com"`

---

## Validation Performed

- `npm run sass` completes successfully.
- `npx wrangler deploy --config wrangler.toml --dry-run` succeeds and shows `CONTACT_EMAIL` binding.
- Local endpoint tests via `wrangler dev`:
  - invalid payload -> `400 Bad Request`
  - valid payload -> `200 OK`

---

## Acceptance Criteria

- Submitting the Get In Touch form sends a request to `/api/contact`.
- Valid submissions return success and are forwarded to configured email destination.
- Invalid payloads return clear error responses and user-visible form feedback.
- Worker deploy remains compatible with static assets routing.

---

## Follow-up

FormSubmit may require first-time mailbox activation for `hello@bytestreams.com`.
Confirm activation email and complete provider verification to ensure live message delivery.

---

## Labels

- `enhancement`
- `cloudflare`
- `contact-form`
- `worker`
- `ci/cd`
