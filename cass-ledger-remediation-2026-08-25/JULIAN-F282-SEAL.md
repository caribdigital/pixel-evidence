# F-282 — L7 footnote: request for Julian's sealed per-component wording

**Date:** 2026-08-25 · **Master:** `c37359f70` · **Owner of this decision:** Julian (regulatory citation canon)

## Where it stands
The **fabricated** statutory claim is gone from every customer/runtime artifact and is pinned:

- The old footnote read *"† Section 53 Bad Debt Adjustment (L7): … per VAT Act 2014 Sections 52–53. Negative value = relief on output invoices **≥12 months old** …"*. Two faults you flagged: there is **no 12-month statutory threshold** (that window is product policy, never law), and the L7 figure on the reference return was a **§52(1)(d) credit-note adjustment**, not §53 bad-debt relief — the footnote cited the wrong basis for the number beside it.
- **Now (production, `VATReturnPdfService.cs`):** neutral, provenance-true copy, gated on a non-zero L7:
  > *"Calculated from this period's credit notes and bad-debt adjustments (DIR Form 32a Section B, Line 7). This value is derived from your transactions."*
  No "12 months", no §52/§53 citation, no bad-debt statutory claim.
- Pinned by `DirAssertionFamilyPinsTests.L7Footnote_MakesNoFabricatedStatutoryClaim` (asserts "12 months" absent, "Section 53 Bad Debt Adjustment" absent, neutral copy present, L7 gate present).
- The non-production `tools/AuditSampleGenerator` was carrying the same forbidden strings; it has been updated to mirror the neutral production copy (this pass).

## What needs your seal
The neutral copy is deliberately **generic** because the correct fix — a footnote that **derives from the actual L7 breakdown and cites only the component present** — needs your sealed wording per component. Specifically:

1. **§52 credit-note adjustment component** — the verbatim sealed sentence to print when L7 contains a credit-note adjustment (this was the actual −50.00 on the reference return, §52(1)(d)).
2. **§53 bad-debt component** — the verbatim sealed sentence to print when L7 contains a genuine bad-debt relief component, **and** your ruling on whether any time condition (the "≥12 months") is stated at all, and if so whether it is marked explicitly as **policy**, not law.
3. Confirmation that when L7 has **no** components the footnote does not render (already the case).

**Hard rule already enforced in code:** no §52 or §53 wording returns to any artifact before your seal. Until then the neutral copy stands.

## Where the code is
- Production composer: `src/CoralComply.Web/Services/VATReturnPdfService.cs` (L7 footnote block, ~line 597).
- Canon: `docs/decisions/REGULATORY_CITATION_CANON.md` (§53 sealed row states the no-12-month-threshold fact).
- Once you provide the two sealed sentences, the L7 breakdown is already available to the composer, so threading them is a small, testable change (a component→sentence map) — I'll pin each component's rendering.

**Please reply with the two verbatim sealed sentences (and the policy-vs-law call on any time condition).**
