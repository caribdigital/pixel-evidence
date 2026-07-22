# CLC-DES-040 M3 — source audit checklist (for Kai, D-040-M3-RULINGS)

Branch `feat/des040-m3` (commit `4cde3c16`), delta vs master `aa4c4007`. Files in this folder:
`m3.diff` (full delta — css + the three wired flows + the SavedIndicator primitive),
`05-navigation.full.css`, `03-mudblazor-overrides.full.css`. Full Release suite green
(11943/0/18); staging Reef smoke GREEN 32/32 on this build (sha `a86d9b7c`).

Kai's stated checklist, each item addressed:

- [x] **Tokens-only timing** — every M3 animation uses `--motion-micro/small/medium` +
  `--motion-ease-standard/reversible`. No raw ms except the 150ms chip cross-fade recipe carried
  verbatim from the M2 `.resolve-crossfade` (already ruled).
- [x] **300ms per-animation cap** — nav entrance per-item = `--motion-medium` (240ms); dialog/
  overlay = 240ms; chip cross-fade = 150ms; saved-label = `--motion-small` (180ms). All ≤ 300ms.
- [x] **Nav 400ms sequence-tail ceiling** — stagger step 20ms, cap 8 (items 9+ share the 8th's
  140ms delay); last item ends at 140 + 240 = **380ms ≤ 400ms**. (05-navigation.css)
- [x] **Transform + opacity + the amended paint-color set** — entrances/dialog/chip use transform
  + opacity only; the drawer width retarget is a transition-timing change, not a new transform.
- [x] **Reduced-motion coverage of every new class** — `.coral-nav-entrance` (nav block),
  `.mud-dialog` / overlay / `.coral-chip-crossfade` / `.coral-saved-indicator__label` (overrides
  block) all collapse to end-state; the global sweep in coral-animations.css backstops.
- [x] **Once-per-circuit entrance guard** — pure CSS keyed to element mount on the persistent
  MudNavMenu (MainLayout is per-circuit; NavMenu re-renders only on business-context change, and
  Blazor diffs rather than recreates), so the entrance plays once and not on same-context
  re-render. Only genuinely new sections after a context switch fade in (a real appearance).
- [x] **CoralAnimatedNumber static, no count-up** — QuickStatusCard renders the figure statically
  (`@EffectivePrefix@Value.ToString(...)@EffectiveSuffix`); the count-up component is no longer
  invoked anywhere. Magnitude never animates.
- [x] **No infinite** — no new `infinite` animation; all M3 animations are one-shot `both`.
- [x] **No double dialog animation** — MudBlazor's own `.mud-dialog { animation:
  mud-open-dialog-center .1s ... both }` is REPLACED by ours on the same element (equal
  specificity, our override loads after MudBlazor), so exactly one animation runs; the overlay
  animates opacity only (no transform). Documented in the CSS. No element carries two transforms.
- [x] **D-040-M3-RULINGS manifest entry scoped** — added to `docs/decisions/RULING_ID_MANIFEST.md`
  and cited by the M3 CSS; `RulingIdManifestInvariantTests` green.

## Two stills (pending capture — Dev fixture)
- SavedIndicator beside its save button, both themes (weight + placement).
- One open dialog (the rise+fade settled state).

Robbie's staging hand covers feel per the close protocol.
