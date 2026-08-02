# For Cass — what needs your ruling, and what is waiting on you

**From:** Robbie (engineering) · **Date:** 2026-08-02
**Evidence pack:** https://github.com/caribdigital/pixel-evidence/tree/trunk/cass-run9

Everything below is either (a) copy I drafted to satisfy a ruling of yours and cannot ratify
myself, (b) a finding that needs a ruling before anything ships, or (c) an act that is yours.
Nothing here is blocked on engineering.

---

## A. Copy I drafted under your rulings — ratify or amend

You rule copy. These strings shipped because your rulings required *something* to replace what
came off, and holding the surfaces empty would have been worse. Each is flagged in source as
pending your ratification.

**A1 · F-239, the overdue banner body** (replaced the B$100/2%/10% figures and the §47A pinpoints):

> This return is past its filing deadline. Late filing and unpaid VAT attract fines and interest
> under the VAT Act. File the overdue return now to limit further charges.

**A2 · F-239, the turnover-mismatch line** (replaced "potential $150,000 penalty"):

> A turnover mismatch was detected between your VAT and CESRA records

**A3 · F-241, the lodgement vocabulary** that replaced the submission family:

| Was | Now |
|---|---|
| Submit before midnight to avoid penalties. | Lodge before midnight to avoid penalties. |
| Submit immediately. | Lodge immediately. |
| Approve return for submission | Approve this return for lodgement |
| Submission Blocked | Filing paused |
| Submission Blocked: {reasons}. Resolve these issues to submit. | Filing paused: {reasons}. Resolve these items to continue. |
| prepare your next submission | prepare your next return |
| prepared and submitted on time | prepared and lodged on time |

**A4 · W1, the void-for-correction dialog** (whole surface is new):

> **Void for correction**
> This returns {period} to draft so it can be corrected and regenerated. The documents already
> generated are kept and marked superseded, so they can never be mistaken for the current return.
> Use this only if you have not yet recorded a lodgement for this return.
> *Reason for voiding \** — helper: "This is recorded on the audit trail before the return is voided."
> Buttons: **Cancel** · **Void and return to draft**

Note on A4: my first draft said "…has not been lodged with the Department of Inland Revenue". The
REQ-MSG-001 guard rejected it — the product may not assert the DIR-side act even in a conditional.
It now names the observable event instead ("recorded a lodgement"). Worth knowing the guard bites
on our own drafting.

---

## B. Findings that need a ruling before anything ships

**B1 · Two more surfaces carry the same unsealed §47A figures.** F-239 took them off the filing
banner. These were left untouched deliberately, because customer-facing regulatory copy is yours:

- `Help/FilingGuide.razor` — states verbatim: "Late Filing Penalty (§47A(3)(a)): the greater of
  B$100 or 2% of the tax payable", "Late Payment Penalty (§47A(3)(b)): 10% of the tax owed", and
  "prime rate plus 1% (§47A(4))".
- `Admin/PrimeRateManagement.razor` — cites §47A(4) for the prime + 1% interest rate (internal ops
  surface, not customer-facing).

`Payments.razor` is already compliant — neutral pointer, no computed figure (your 2026-06-22
ruling). **Question:** does F-239's disposition extend to B1, and does the Help guide count as
customer-facing for this purpose?

**B2 · F-242's disposition — confirm.** The Filing Centre's compliance gauge now renders **nothing**.
The alternative was rendering the portal's honest "Not yet assessed" state. I chose silence because
the two surfaces disagreeing was the defect; a second surface publishing *any* verdict re-opens the
question of which is authoritative. The health data still gates the filing act — I held the verdict,
not the gate. **Confirm silence, or ask for the not-yet-assessed state here too.**

**B3 · F-245's capacity enumeration — NOT shipped.** The signatory-capacity list
(RegisteredTaxpayer / BicaLicensedPractitioner / AuthorisedEmployee / AuthorisedAgent) is
regulatory-substantive and its "agent" clause brushes the held Type-3 question. It ships only after
your read with Julian. The dialog's amendment sentence *was* fixed per your ruling — full stop after
"Once approved, the return cannot be modified."

**B4 · Duplicate preparation records.** Nassau's January has **two** `filing_preparations` rows for
one period. Pre-existing, not touched. It is not currently visible to a user, but two records for
one period is the shape that breeds the state disagreements you have been ruling against.
**Do you want it investigated as a finding?**

---

## C. F-250 — the correction, and your riders

The hypothesis was fingerprint residue with no rows behind it. **The database says otherwise: the
transactions followed.** Anna and Associates holds **16 full Processed transaction rows**
(INV-NHC-1001…1016) written in a one-second burst, duplicating Nassau's. A class sweep found those
16 invoice numbers are the *only* cross-tenant invoices in the database — real, and bounded.

This makes it a **multi-tenant isolation breach**, not detection residue: Anna's duplicate flag was
*correct detection against rows that genuinely exist there*. Anna has no VAT returns yet, so nothing
has consumed them — but they are Processed, so a return generated for Anna today would compute
Nassau's sales.

Your three riders stand and are unchanged. Two notes:

1. **The purge scope is transaction rows, not fingerprints** — 16 rows, soft-delete, audited,
   enumerated before and after.
2. **Order 4 grew a leg.** I built the write-boundary tenant assertion you ordered, then **reverted
   it unshipped**: it only fires when the explicit and ambient ids *disagree*, and the evidence says
   they agreed on the wrong tenant. It would have read as protection without being protection. It
   also collides with an older security fix that rules the opposite way (explicit id *wins* over
   ambient drift) — two security contracts that contradict, which is a decision, not an eng
   preference. Full reasoning on #4160.

**Still blocked on F-247's root cause**, as you ruled. New sub-finding worth your attention: **there
are zero audit events anywhere in the database for that write window.** 16 rows persisted with no
audit trail at all — which is why the breach was silent until your two-tenant experiment surfaced it.

---

## D. Yours to act on

1. **January** sits **Draft and correct** (600 / 200 / 400, with Box24 refreshed from its stale zero
   — your worksheet and the footer now agree). It lodges when you say so.
2. **The X1–X4 pixel pass** you deferred — the void → recalculate sequence.
3. **Nassau's Filing Centre** — the quarter-shaped "Q1 2026 / Q2 2026" obligations Robbie hit are
   fixed this train. Root cause was mine: the 2026-08-01 correction changed the frequency in raw
   SQL, which bypasses the service and so never regenerated the periods. Your permanent rule is now
   applied retroactively — un-lodged periods regenerated as monthly, lodged periods untouched.

---

## E. Shipped this train (no action needed, listed for the record)

F-226 (period identity + the killed default parameter) · F-230 (treatment guard) · F-236 (the
partial mirror, Julian countersigned) · F-235 (Form 301 quarantined) · F-244 (one return state
machine) · W1 service + UI + the mirror reset · F-237 sweep + Nassau regeneration · F-239 · F-241 ·
F-242 · F-245's copy half.
