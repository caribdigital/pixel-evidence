# Run 9 — Credit-note adjustment basis (Julian J1) + F-220/F-214 landings

**For:** Cass Turnquest, VP Regulatory Product
**From:** Robbie (engineering)
**Date:** 2026-08-01
**Evidence:** https://github.com/caribdigital/pixel-evidence/tree/trunk/cass-run9
**Prod sha:** 357dfc4a (health/version flip verified; live 200; root 200)

## What shipped

### 1. The credit-note reason — captured, never inferred (Julian J1, sealed)

- **Schema:** nullable `CreditNoteAdjustmentBasis` on Transactions (enum-as-string;
  `ConsiderationChanged` | `OverchargeCorrection`). Null = unanswered = held.
- **Engine:** L7 always moves; **L4 moves only on "Price or consideration changed"**;
  over-charge correction is L7-only, on both generator paths. This also fixed a
  pre-existing sign defect: the transaction-fallback path ADDED credit-note gross to
  L4 (Math.Abs) — it now subtracts under case A and holds still under case B. First-ever
  Box4 credit-note assertions pin all of it.
- **Fail-closed backstop:** a Processed credit note with no captured basis throws at
  generation — the engine never guesses. The persist/promotion gates + the legacy
  demotion make that state unreachable; the guard is the last line.
- **Capture at creation (VATEntry):** required radio group with your frozen labels +
  helpers; model-level validation (the submit gate cannot bypass it).
- **After import:** a referenced credit note saves-and-holds (PendingReview) — a file
  cannot carry the answer. Grid: "Reason needed" chip + caption **[both flagged for
  your ratification]**. Receipt: reports under Held with the drill-down and the
  filing-block notice.
- **Resolution:** review-queue dialog (bulk-capable), audit-FIRST
  (`TRANSACTION_CREDIT_NOTE_BASIS_RESOLVED` with before/after) — the signed entry is
  the record of the practitioner's statutory-case decision. Answering never promotes;
  the promotion gate blocks a basis-less credit note; Reject stays ungated.
- **Legacy demotion:** every Processed basis-less credit note re-entered the review
  queue. On staging that is CN-NHC-0001 — **January's generation now waits on you
  answering its reason question in the app** (your annotation says "Price or
  consideration changed"). Answering it live-exercises the new flow before January.

### 2. F-220 — register monthly, read monthly

The onboarding form now carries a real, required, assignment-framed Filing frequency
select (Monthly | Quarterly). Found and fixed during test-writing: the request path
was still overriding the declared value with the turnover-computed default whenever
turnover was present — the exact swallow you ruled against. The declared value now
always wins; regression test onboards monthly WITH a quarterly-computing turnover and
reads monthly back. The mislabeled heading is gone — the turnover text now sits under
"Filing deadline".

### 3. Nassau Harbour correction + the by-the-dialog witness (F-214 riders)

- Recorded data migration corrected the one row (staging: exactly 1 row affected;
  prod: the business does not exist — 0 rows, confirmed in deploy logs).
- **The dialog witness passed:** Generate VAT Return for Nassau Harbour now offers
  MONTHS (July…December; period "Aug 1 – Aug 31, 2026"). No Quarter selector renders.
  **No generator defect to report.** Stills F214-1/F214-2.

### 4. The three frozen strings — shipped verbatim, byte-pinned

1. Frequency consequence-confirm body on the business profile + the matching
   acknowledgment checkbox gating Confirm.
2. "Lodgement Recorded" — "Get notified when a return's lodgement is recorded in
   CoralLedger Comply."
3. Default Filing Method is no longer a selector — descriptive text only; the
   Government Portal / API Integration doors are dead.

## The Reef now witnesses the J1 flow nightly

Scenario G (your L1–L31 hand-calc keystone) initially went AMBER — correctly: its
fixture's two credit notes (CR-IFM-001, CR-IFM-006) now import as held-for-reason, so
the draft refused to seed. The scenario now answers "Price or consideration changed"
in the queue (both are refunds against a prior supply — matches your hand-calc's L4
reduction) and promotes through the two-step confirm before seeding. Your fixture +
the ruled flow are now exercised end-to-end every night.

## Decisions waiting on you

1. **CN-NHC-0001** — answer its reason question in the review queue (unblocks January).
2. **Ratify or reword** the flagged import copy: chip "Reason needed" + caption "Held
   out of returns until the credit-note reason is answered" (J1-1 still).
3. **Receipt drill-down wording:** a row holding for BOTH category and reason is named
   only "held as Unclassified" (J1-2 still). Behavior correct; is one-reason naming
   acceptable, or should it enumerate?
4. **Pre-selection question:** the new onboarding frequency select pre-selects
   Quarterly (reseeded from turnover as a visible soft default). The
   D-4085-TURNOVER-CAPTURE class pin says declaration-class selects ship
   placeholder-first. Frequency is an assignment (not a self-declaration), so we kept
   the pre-selection — flagging the tension rather than re-scoping silently. Your call.
5. **Case-B live witness** stays pending until a real over-charge correction arrives —
   the unit fixtures pin it meanwhile.

## Train record

- Batch `ed497579` + caption fix `5f031071` + smoke conformance `72d92f0f`/`6881362d`/`cdf6d234`.
- Full Release suite 12,189 green; mutation checks on the case-B branch and the
  promotion gate (both caught, both reverted).
- Staging DB witnesses: CN-NHC-0001 → PendingReview/basis NULL; Nassau → monthly.
- Reef: **GREEN 32/32** (run 30692545403) after two conformance AMBERs (both
  test-side; Must-Not-Ship gates green on every run); a same-sha confirmation
  run was dispatched alongside the prod deploy.
- Prod: deploy 30696185993 — sha flip 4f9f9f51 -> 357dfc4a verified, live 200,
  root 200; VAT migration applied pre-swap ("VATContext now at head, 59
  migrations") including the legacy demotion; Nassau correction is a no-op on
  prod by construction (business absent).
- One prod query for Robbie (readonly-sql is staging-only): current count of
  credit notes re-held by the demotion —
  SELECT COUNT(*) FROM "Transactions" WHERE "Category" ILIKE 'CreditNoteIssued' AND "Status" = 'PendingReview';

---

## Addendum 2026-08-01 — the run-9 ruling batch is EXECUTED and on prod (b5fac446)

All five decisions actioned; 7th train shipped (Reef GREEN 32/32 run 30717846794;
prod sha flip 357dfc4a -> b5fac446 verified, live 200, root 200).

1. **CN-NHC-0001 answered** as "Price or consideration changed" via the ruled dialog
   (Nassau Harbour Owner). Mini-witness above: W1 still + the signed
   TRANSACTION_CREDIT_NOTE_BASIS_RESOLVED entry (2026-08-01T19:28:15Z). The row sits
   at the promote step — one deliberate click in the queue and January can generate.
2. **Ratified copy flags flipped** — chip + caption now marked ratified verbatim in
   source and pins.
3. **Enumeration shipped**: receipt lines name every blocking reason ("held for
   category not confirmed; credit-note reason not answered"), the import grid stacks
   an "Also held until its category is confirmed" caption, the review queue stacks a
   "Reason needed" chip beside the category state. Mutation-checked.
4. **Placeholder-first shipped**: the quarterly pre-selection and the turnover reseed
   are gone; required at model level + submit gate; the turnover-derived value is a
   labeled "Suggested from turnover... (pending Comptroller determination)" applied
   only by an explicit click.
5. **Case-B witness** remains pending on reality; pins hold.

Light confirm, staging leg: **0** post-draft returns (Lodged/Filed/
AwaitingDirConfirmation) ever overlapped a fallback-path credit note. Prod twin on
Robbie's channel, prediction zero.

**New find for the record — #4139** (the #4007 class, surfaced by this train's AMBER
and pinned by trace + server logs): CS A1's proceed-to-attestation button silently
swallows the click while the user's identity is still resolving — no navigation, no
snackbar, no log. On the attestation pathway, a slow circuit gives a practitioner a
dead button with no explanation. Filed with the trace evidence; fix is the #4007
identity-gate pattern. Ops nit executed: nothing was dispatched inside the deploy
bracket this train.
