# cass-run9 — Credit-note adjustment basis (Julian J1) + F-214/F-220 landing evidence

Staging witnesses for the 2026-08-01 train (`ed497579` batch + `72d92f0f` caption fix).
All stills are sha-stamped in the filename; captured on `stg-comply.coralledger.com`.

## F-214 — "verification is by the dialog, not the migration" (Cass rider)

- `F214-1-nassau-generate-dialog-ed497579.png` — Generate VAT Return dialog for
  Nassau Harbour Consulting Ltd. (business `90790032-19f9-4798-86df-0fca87e29813`)
  after the one-row frequency correction: the period selector is **Month**, no
  Quarter selector renders.
- `F214-2-nassau-month-options-open-ed497579.png` — the Month dropdown open:
  July (1), August … December; period line reads "Aug 1, 2026 - Aug 31, 2026".
  **Result: months offered — no generator defect to report.**

DB state (readonly-sql, staging): `filing_frequency = 'monthly'` for the business;
correction migration affected exactly the one intended row.

## Julian J1 — the credit-note reason, captured never inferred

- `J1-1-reason-pending-chip-and-caption-72d92f0f.png` — import review surface for a
  referenced credit note: 0 Ready / 1 Needs review / 0 Blocked; the "Reason needed"
  chip and the caption "Held out of returns until the credit-note reason is
  answered" **[copy: chip label + caption pending Cass ratification]**; the
  "Credit note" chip and the unconfirmed-suggestion marker also render.
- `J1-2-receipt-cn-under-held-72d92f0f.png` — the receipt: Imported 0 / Held 1 /
  Excluded 0 / Failed 0, held-rows drill-down ("Resolve in the app"), and the
  filing-block notice. Note for Cass: the drill-down names the Unclassified hold
  only — the row also holds for the credit-note reason; both remedies live in the
  review queue. Wording-completeness question, behavior is correct.
- `J1-3-reason-dialog-frozen-labels-72d92f0f.png` — the review-queue reason dialog
  with the two FROZEN labels + helpers verbatim: "Price or consideration changed" /
  "A discount or price alteration reduced what the customer owes." and "Over-charge
  correction" / "The original invoice overstated VAT; the supply and price are
  unchanged." Capture-only: the dialog was cancelled — answering CN-NHC-0001 is
  Cass's own in-app act.

## Staging DB witnesses (readonly-sql)

- `CN-NHC-0001` → `Status = PendingReview`, `CreditNoteAdjustmentBasis = NULL`
  (the legacy demotion; zero Processed basis-less credit notes remain).
- Nassau Harbour `filing_frequency = 'monthly'`.

## Addendum 2026-08-01 — decision 1 executed: CN-NHC-0001 answered (mini-witness)

- `W1-cn-nhc-0001-dialog-answered-preconfirm-72d92f0f.png` — the resolution dialog on
  staging, "Price or consideration changed" selected, note field citing the run-9
  ruling, immediately before Confirm. Driven as the Nassau Harbour Owner.
- **Audit entry (immutable_audit_entries, business 90790032-19f9-4798-86df-0fca87e29813):**
  `TRANSACTION_CREDIT_NOTE_BASIS_RESOLVED` — "Resolved credit-note adjustment basis to
  ConsiderationChanged for 1 transaction(s) awaiting review." — actor
  ef273a3c-e673-4a12-b579-ff20be00b15d — 2026-08-01T19:28:15.507823Z.
- **Row state:** CN-NHC-0001 `Status = PendingReview`, `CreditNoteAdjustmentBasis =
  ConsiderationChanged`. The answer never promotes: the standard promote disposition
  (one deliberate click in the queue) remains before January generates — per the
  two-step discipline.

## Addendum 2026-08-01 — the light confirm (staging leg)

Query: post-draft returns (`Lodged`/`Filed`/`AwaitingDirConfirmation` — the actual
status strings on staging) whose period overlaps a `CreditNoteIssued` transaction with
no line items (the fallback path that carried the Math.Abs L4 sign defect).
**Result: 0 rows.** No lodged artifact on staging ever rode the defective path with a
credit note. The prod twin of this query is on Robbie's channel; prediction zero.
