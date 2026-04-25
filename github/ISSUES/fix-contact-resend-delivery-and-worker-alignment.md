# fix(contact, cloudflare): restore Resend delivery and align Worker email flow with DialTone

## Summary

Production contact submissions returned `503` and did not deliver email because the deployed Cloudflare Worker was missing the `RESEND_API_KEY` secret.

This issue tracks the operational restore plus the code/config alignment work that brought ByteStreams' email flow in line with the DialTone Worker template.

---

## Problem

The ByteStreams contact form posted to `POST /api/contact`, but live submissions failed with:

- `{"error":"Contact form is temporarily unavailable. Please try again shortly."}`

Impact:

- Visitors could submit the form, but no email was delivered.
- Local testing and production behavior could drift when `public/` was stale or Worker secrets were missing.

---

## Root Cause

Two issues combined:

- The production Worker did not have `RESEND_API_KEY` configured, so `handleContact` returned `503` before calling Resend.
- ByteStreams had drifted from the DialTone Worker template for email metadata and local dev workflow, which made verification less reliable.

---

## Fix Implemented

### Operational restore

- Uploaded `RESEND_API_KEY` to the production Worker via Wrangler secret management.

### Worker/template alignment

Updated `worker.js` to match the DialTone email implementation pattern:

- Added normalized static asset fallback handling (`handleAssetRequest`, `notFoundResponse`).
- Added `SITE_NAME`-driven email metadata for:
  - `from`
  - `subject`
  - text body copy
  - HTML body copy
- Kept the existing ByteStreams routes and asset paths intact.

### Local dev workflow hardening

- Added `build:public` to rebuild the Worker asset directory from current source files.
- Added `dev:worker` to rebuild `public/` and run `wrangler dev` in one command.
- Documented local Worker-based email testing in `README.md`.
- Ignored `.dev.vars` so local Resend credentials stay out of git.

---

## Files Changed

- `.gitignore`
- `README.md`
- `package.json`
- `worker.js`
- `wrangler.toml`

---

## Validation

- Local endpoint test returned `200 OK` from `http://127.0.0.1:8787/api/contact`.
- Production endpoint initially returned `503` with the temporary-unavailable error.
- After uploading the missing secret, production returned `200 OK` with `{"ok":true}` from `https://bytestreams.ai/api/contact`.

---

## Acceptance Criteria

- Submitting the live ByteStreams contact form delivers through Resend again.
- Production `POST /api/contact` returns `200` for a valid payload.
- ByteStreams Worker email metadata follows the same `SITE_NAME` template pattern as DialTone.
- Local dev testing uses the Worker path instead of a stale static-only server.

---

## Labels

- `bug`
- `cloudflare`
- `contact-form`
- `email`
- `worker`