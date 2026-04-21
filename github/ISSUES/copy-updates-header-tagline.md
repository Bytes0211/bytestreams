# chore(copy, html, sass): update site copy and add tagline under header logo

## Summary

Two related content and markup changes made on the `chore/copy-updates` branch:

1. **Hero section copy update** — replaced generic placeholder copy with brand-aligned messaging.
2. **Header tagline added** — a branded tagline is now displayed beneath the ByteStreams logo in the site header across all pages.

---

## Changes

### 1. Hero description copy (`index.html`)

**Before:**
> Transform your ideas into reality with our cutting-edge solutions.
> We help businesses grow and thrive in the digital age.

**After:**
> Scalable AI workflows that move as fast as your ambition. From data pipelines to intelligent automation — engineered to perform.

---

### 2. Header tagline (`index.html`, `privacy.html`, `terms.html`)

The logo `<a>` in the header is now wrapped in a `.header__brand` `<div>`, with a new `<p class="header__tagline">` rendered beneath the logo image:

```
Smarter Workflows, Stronger Results.
```

The tagline is hidden on mobile viewports (`< 600px`) via a `@include mobile { display: none; }` rule.

---

### 3. Sass updates (`sass/layout/_header.scss`)

New BEM modifiers added to support the header brand block:

- **`.header__brand`** — flex column container wrapping the logo link and tagline with a 2px gap.
- **`.header__tagline`** — styled with `$font-size-tiny`, `var(--text-muted)`, and `$ls-tiny` letter-spacing. Hidden on mobile.

---

## Files Changed

| File | Change |
|------|--------|
| `index.html` | Hero copy update; header logo wrapped in `.header__brand`; tagline `<p>` added |
| `privacy.html` | Header logo wrapped in `.header__brand`; tagline `<p>` added |
| `terms.html` | Header logo wrapped in `.header__brand`; tagline `<p>` added |
| `sass/layout/_header.scss` | Added `.header__brand` and `.header__tagline` modifier blocks |
| `dist/css/main.css` | Regenerated from Sass compilation |

---

## Branch

`chore/copy-updates`

## Labels

`chore`, `copy`, `html`, `sass`
