# The gating train on staging — verified end to end

**For:** Cass Turnquest, Julian Rolle · **Date:** 2026-08-02
**Staging sha:** `766b092c` · **Reef: GREEN 32/32** (run 30753961257)
**Prod: UNCHANGED and unauthorized** — awaiting Robbie's call.

## The train (8 legs + 1 gate fix)

F-226 (period identity + the killed default parameter), F-230 (treatment guard),
F-236 (the partial mirror), F-235 (Form 301 quarantine), F-244 (one return state
machine), W1 service + UI (void and regenerate), F-237 sweep. Suite 12,253 green.

## The proof: January corrected through the ruled path

January carried the F-236 artifact into staging exactly as predicted: summary fields
600/200/400 but a **stale stored Box24 = 0**, so the statutory worksheet would still
render L26 0.00 and L27/L29 600 — your evidence table. Recalculate is Draft-only, so
the ONLY correct route was W1. Driven live on staging:

1. `X1` — January at Awaiting Lodgement before the void.
2. `X1b` — the void affordance, offered in the row overflow and nowhere else.
3. `X2` — the void dialog with the reason recorded before anything changes.
4. `X3` — back to Draft.
5. `X4` — after recalculation: **NET VAT B$400.00, status Draft, the honest
   162-day overdue banner intact.**

**Database after (the numbers that matter):**

| Field | Before | After |
|---|---|---|
| Status | AwaitingDirConfirmation | Draft |
| TotalOutputVAT | 600.00 | 600.00 |
| TotalInputVAT | 200.00 | 200.00 |
| NetVATDue | 400.00 | 400.00 |
| **Box24_TotalInputVAT** | **0.00 (stale)** | **200.00** |

Box24 refreshing to 200.00 is Julian's condition 2 satisfied: L26 now sums to 200,
L27/L29 to 400, and the worksheet agrees with the footer. The partial mirror is dead
in data, not just in code.

**The three artifacts are superseded, not deleted** — Pdf, Xml and Excel each carry
`superseded_at` and the provenance reason ("Return voided for regeneration on
2026-08-02 by caribdigital@proton.me…"), so the quarter-named documents cannot present
as current. **The audit row exists** (`RETURN_VOIDED_FOR_REGENERATION`, 15:42:58Z),
written before the transition, as the fail-closed contract requires.

## One finding the staging gate caught — mine

The first deploy of the train went AMBER on `SchemaState_ReportsNoDrift`. My
supersession migration created `superseded_at` / `superseded_reason` in snake_case
(correct — canon forbids new PascalCase columns) on a legacy all-PascalCase table,
but EF was never told, so it expected PascalCase columns that did not exist. The
migration applied cleanly, the columns were present, and the 12k unit suite was green:
**only the drift preflight would ever have caught this.** Fixed with explicit
`HasColumnName` mappings (`766b092c`); the re-run is the GREEN above. This is the
strongest argument for holding prod until staging is proven — the same check would
otherwise have fired against production's schema.

## Still open

- January is Draft and correct; it lodges when you say so, through the F-244-clean
  surfaces.
- The dialog copy is flagged pending your ratification.
- F-250 (#4160) is unremediated by design: the 16 mis-tenanted rows await the F-247
  root cause, and the ordered purge is yours to authorize.
