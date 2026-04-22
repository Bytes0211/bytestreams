# chore(copy, footer): update footer row copy and product links

## Summary

Update footer navigation copy and ordering to match the requested row-first reading pattern beside the logo.

Requested reading order:
- Row 1: Company -> About -> Security
- Row 2: Product -> DialTone.menu -> DialTone.med (comming soon) -> Features
- Row 3: Legal -> Privacy -> Terms -> Cookie Policy

Also link `DialTone.menu` to its live domain.

---

## Changes

1. Reworked footer content ordering and wording
- Switched from column-first navigation to row-first copy structure.
- Added row labels for `Company`, `Product`, and `Legal` with left-to-right link flow.

2. Product link updates
- Linked `DialTone.menu` to `https://dialtone.menu`.
- Kept `DialTone.med` as placeholder and updated label text to:
  - `DialTone.med (comming soon)`

3. Legal links
- Ensured `Cookie Policy` points to `cookies.html`.

4. New legal page included in nav
- `cookies.html` added and included in footer legal links.

---

## Files Touched

- `index.html`
- `privacy.html`
- `terms.html`
- `cookies.html`
- `sass/layout/_footer.scss`
- `sass/base/_typography.scss`
- `dist/css/main.css`

---

## Acceptance Criteria

- Footer reads top-down by rows next to logo: Company, Product, Legal.
- Within each row, links read left-to-right with separators.
- `DialTone.menu` opens `https://dialtone.menu`.
- `DialTone.med` displays `DialTone.med (comming soon)`.
- `Cookie Policy` links to `cookies.html` on all pages.

---

## Labels

- `enhancement`
- `documentation`
