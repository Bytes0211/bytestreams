# fix(css, sass, cloudflare): prevent hero accent text from rendering as a blue block in production

## Summary

After Cloudflare deployment, the hero headline accent text could render as a solid blue rectangle ("blueout") instead of clipped gradient text, while local preview looked correct.

This issue tracks a compatibility hardening fix to ensure gradient text only uses transparent text when browser support is complete.

---

## Problem

In production rendering paths, some engines report partial support for text clipping. The existing `@supports` condition allowed transparent text + gradient background in scenarios where clipping was not fully honored, causing a rectangular gradient/blue block artifact behind the hero accent.

Affected area:
- Hero headline accent text in the home page (`Build Your` / accent line)

---

## Root Cause

The gradient text mixin guarded on background clipping support alone:
- `(-webkit-background-clip: text) or (background-clip: text)`

That check was not strict enough for all production browser/rendering combinations behind Cloudflare-delivered assets.

---

## Fix Implemented

Updated the Sass gradient text mixin to require both:
- text background clipping support, and
- `-webkit-text-fill-color: transparent` support

New condition:
- `@supports (((-webkit-background-clip: text) or (background-clip: text)) and (-webkit-text-fill-color: transparent))`

Fallback behavior remains solid brand blue text when strict support is not available.

---

## Files Changed

- `sass/abstracts/_mixins.scss`
- `dist/css/main.css` (regenerated)

---

## Acceptance Criteria

- Hero accent text displays as readable text (never a rectangle) across production browsers.
- Browsers with full support render gradient-clipped text.
- Browsers without full support render fallback solid blue text.
- Cloudflare deployment matches local appearance for hero headline rendering.

---

## Labels

- `bug`
- `css`
- `sass`
- `cloudflare`
