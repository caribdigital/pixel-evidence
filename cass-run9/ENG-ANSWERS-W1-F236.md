# Eng answers — W1 (void-and-regenerate) and F-236 (worksheet contradiction)

**For:** Cass Turnquest · **Date:** 2026-08-02 · code read at master `4c222c38`

## W1: does an Awaiting-Lodgement return support void-and-regenerate? — NO

`VatReturnRowActionResolver` at `AwaitingDirConfirmation` offers exactly: View Filed
Return, PDF, Confirm DIR Lodgement, History. No void, no regenerate. Retraction
exists only one step LATER (Lodged → AwaitingDirConfirmation, FW-002). The gap you
named is real: the state where errors get found has no correction path. Filed as
**#4154** with the build shape (audited transition back to Draft — a regulatory
state transition, so a durable audit row per your and Julian's audit-emit ruling;
artifacts soft-superseded with provenance, never deleted). It lands before the
train ships so January has its path. January's DIR acknowledgement stays
UNRECORDED per the hardened guard.

## F-236: the diagnosis, and it narrows Julian's question

**The sealed summation semantics are correctly implemented** — as computed
properties: `L26 = Box24_TotalInputVAT + L25_InputAdjustment`,
`L27 = L10 − L26`, L29 per JR-3987-F6 v2. The defect is upstream of the math:
the grid's **Recalculate path copies a hand-picked list of fields** from the fresh
computation onto the stored return (TotalOutputVAT, Box19, Box14, TotalInputVAT,
Box3, Box5 family) and **omits `Box24_TotalInputVAT`**. On a recalculated return
the stored component goes stale at its original value while the summary fields
update — January (generated 600/0/600, then recalculated to 600/200/400)
reproduces your table exactly: L26 = 0 + stale, L27/L29 = 600 − 0, footer = the
freshly-copied 400.

**Ruled-fix shape** (#4152): one engine, literally — the recalculation applies the
same complete generator→entity mapping used at creation (preserving only the L7
manual adjustment, payment fields, and identity/status), never a hand-picked
list; plus the F6-family invariant (rendered totals equal the sum of rendered
components; worksheet net equals summary net) on every generated return.

**Julian's countersign question, narrowed:** the fix does not touch summation
semantics — it fixes component freshness. His confirm is that the sealed
relationships (Box24+L25 feed L26; L27/L29 = L10 − L26 per the v2 D-section) are
the correct target, which is a re-read of his own JR-3987-F6 seal.

## Board state

- Gating train: F-226 + F-230 (built, staging `29edf756`, suite 12,207 green,
  mutation-checked) + F-236 (diagnosed, fix shape ready, merge waits on Julian's
  countersign) + F-235 quarantine (small gate-off, next build) + W1 path (#4154).
- **Prod HOLDS** until the train is whole. F-237 (#4153) rides the sweep — eng
  note: the Q2-for-monthly obligation is almost certainly stale pre-generated
  FilingPeriods rows from before the frequency correction; frequency changes must
  regenerate periods.
- Filed: #4151 (F-235), #4152 (F-236), #4153 (F-237), #4154 (W1 gap).
