# fix(deploy, cloudflare): publish all legal HTML pages in Worker static assets

## Summary

Cloudflare deployment serves `404 Not Found` for legal pages that exist in the repository:
- `https://bytestreams.ai/sms-terms.html`
- `https://bytestreams.ai/cookies.html`

This issue tracks the deployment packaging fix so all top-level HTML pages are included in the Worker static assets.

---

## Problem

The site renders locally with all legal pages available, but production returned 404 for `sms-terms.html` and `cookies.html`.

---

## Root Cause

The GitHub Actions deploy workflow only staged three HTML files into `public/`:
- `index.html`
- `privacy.html`
- `terms.html`

Because Cloudflare Workers Static Assets serve only files staged in `public/`, `sms-terms.html` and `cookies.html` were never deployed.

---

## Fix Implemented

Updated deploy staging in `.github/workflows/deploy.yml`:

- **Before:**
  - `cp index.html privacy.html terms.html public/`
- **After:**
  - `cp *.html public/`

This ensures all top-level HTML pages are deployed, including legal pages.

---

## Files Changed

- `.github/workflows/deploy.yml`

---

## Validation

Local artifact simulation now includes:
- `cookies.html`
- `sms-terms.html`
- `index.html`
- `privacy.html`
- `terms.html`

---

## Acceptance Criteria

- `https://bytestreams.ai/sms-terms.html` returns `200` and renders page content.
- `https://bytestreams.ai/cookies.html` returns `200` and renders page content.
- Future deploys include all top-level `*.html` files without manual edits to the workflow.

---

## Labels

- `bug`
- `cloudflare`
- `ci/cd`
- `deploy`
