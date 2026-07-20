# CLC-DES-040 M1 — source-audit evidence for Kai (D-040-MOTION-SYSTEM)

Source for the M1 register audit (§1 of the routing-package response). Main repo is
private; these are the exact M1-branch files so Kai's sandbox can fetch them raw.

- `coral-animations.css` — the responsive token scale (`--motion-micro/small/medium` 120/180/240ms + easings) added to the existing `:root`; global `prefers-reduced-motion` block at the foot.
- `03-mudblazor-overrides.css` — the responsive-register block (buttons lift/press 120ms across variants; menus origin-aware pop 180ms). M1 state only.
- `RULING_ID_MANIFEST.md` — `D-040-MOTION-SYSTEM` allowlist entry.
- `M1-PR4077.diff` — the focused 74-line diff (PR #4077).

Cleared after close, per channel convention.
