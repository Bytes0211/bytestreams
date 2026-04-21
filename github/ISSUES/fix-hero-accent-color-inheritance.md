# fix(css, sass): hero accent text color inheritance to match headline

## Summary

Updated hero accent text rendering to use color inheritance instead of solid blue, eliminating the blue block artifact while ensuring "Digital Future" matches the parent headline's white text color.

---

## Problem

After removing the gradient clipping approach (which was rendering as a blue rectangle), the hero accent was displaying in solid brand blue (`#3B82F6`), creating a visual mismatch with the parent headline "Build Your" which displays in white (`#F0F6FC`).

---

## Solution

Changed the gradient-text mixin to use `color: inherit;` instead of a hardcoded color. This ensures:

1. Hero accent inherits the parent `.hero__headline` color (white)
2. No blue block artifact
3. Consistent visual appearance across "Build Your" and "Digital Future"
4. Reliable cross-browser rendering without complex clipping rules

---

## Files Changed

- `sass/abstracts/_mixins.scss` — Updated `@mixin gradient-text` to use `color: inherit;`
- `dist/css/main.css` — Updated `.gradient-text` and `.hero__headline .hero__accent` selectors

---

## Acceptance Criteria

- "Build Your" and "Digital Future" display in matching white text
- No blue rectangle or solid blue text visible
- Consistent rendering in Chrome, Firefox, and Safari
- Local preview and Cloudflare deployment render identically

---

## Related

- Closes issue #3 (blue block rendering in production)

---

## Labels

- `bug`
- `fix`
