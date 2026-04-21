# WARP.md

Guidance for WARP (warp.dev) when working with the **ByteStreams** marketing
site repository.

## Project Overview

Static single-page marketing site for ByteStreams — a SaaS platform for
AI-powered workflow automation. Built with:

- Pure HTML5
- Sass (7-1 architecture) compiled with dart-sass
- Vanilla JavaScript (no bundler)
- Inter + JetBrains Mono (Google Fonts), Font Awesome (CDN)

Source of truth for design tokens: `ByteStreams-Brand-Kit.pdf` and
`branding.md`. Reference page layouts are in `site/*.png`.

## Development Commands

### Sass compilation
- **Watch mode**: `npm run sass:watch`
- **Production build**: `npm run sass`

### Local server
In a second terminal after starting sass watch:
- `npm run serve`  (wraps `python3 -m http.server 8000`)
- or `npx serve .`

Site is available at `http://localhost:8000`.

## Sass Architecture (7-1)

```
sass/
├── abstracts/
│   ├── _variables.scss   # Brand Kit tokens: colors, type, spacing, radii, motion
│   └── _mixins.scss      # flex, container, breakpoints, animations, gradient-text
├── base/
│   ├── _reset.scss       # Modern reset + CSS custom properties (dark + light theme)
│   └── _typography.scss  # Type scale from Brand Kit §3.2
├── layout/
│   ├── _header.scss      # Sticky blurred header with responsive nav
│   └── _footer.scss      # 4-column footer
├── components/
│   ├── _buttons.scss     # .btn (primary / ghost / outline / sm / lg / block)
│   ├── _cards.scss       # .card, .stat-card, .feature-card, .checklist
│   └── _contact-form.scss
├── pages/
│   └── _home.scss        # hero, about, features, contact layouts
└── main.scss             # entry; @use's every partial
```

### Rules
1. **Never edit `dist/css/main.css` directly** — it's generated.
2. Always pull colors / type / spacing from `_variables.scss` tokens.
3. New partials must be `@use`d from `sass/main.scss`.

## JavaScript

All interactivity lives in `js/main.js`, wrapped in a single IIFE:
- Mobile hamburger with outside-click + Escape-to-close
- Smooth scroll for in-page anchors (respects `prefers-reduced-motion`)
- Active nav link tracking via `IntersectionObserver`
- Sticky header shadow on scroll
- Reveal-on-scroll for `.stat-card` and `.feature-card`
- Contact form client-side validation + status message

## Theming

Dark-mode is the default (Brand Kit §7.1 "dark-mode-first"). Light mode is
wired up through CSS custom properties and activated by setting
`<body data-theme="light">`. A toggle UI has not been added yet.

## Responsive Design

Mobile-first, with these breakpoints (in `_variables.scss`):
- Mobile: `< 600px`
- Tablet: `600–899px`
- Desktop: `≥ 1024px`
- Wide:   `≥ 1280px`

Use mixins: `@include mobile`, `@include tablet-up`, `@include desktop`,
`@include wide`, `@include below-desktop`.

## Deployment

Static — no build step beyond Sass compilation. Deploy `index.html`,
`dist/`, `js/`, and `assets/` to GitHub Pages, Netlify, Vercel, S3 + CloudFront,
etc.

## Git Commit Convention

Use conventional commits: `type(scope): brief description`.

### Types
- **feat**, **fix**, **docs**, **refactor**, **test**, **chore**, **perf**, **style**

### Scopes
- **sass**, **js**, **html**, **assets**, **nav**, **theme**, **responsive**,
  **animation**, **deploy**

### Guidelines
- First line under 72 characters
- Imperative mood ("add" not "added")
- Always append `Co-Authored-By: Oz <oz-agent@warp.dev>` on a new line at the
  end of every commit message
- Reference issues when applicable: `fix(sass): resolve hero overflow (#123)`
