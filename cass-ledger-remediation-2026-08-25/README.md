# Cass Ledger Remediation (F-266→F-300) — Evidence Package for Cass & Kai

**Date:** 2026-08-25 · **Master commit:** `c37359f70` (coralledgercomply) · **Build:** solution clean · **Tests:** full `CoralComply.Web.Tests` Release suite green (12,726 passed, 0 failed).

This package tells you **(1)** where every remediated finding is pinned by a test in the code repo, and **(2)** which findings still need a **visual capture**, where those captures will be posted here, and what to look for.

> Repo references are `path :: TestClass.TestMethod` in `caribdigital/coralledgercomply` at `c37359f70`.

---

## 1. Findings remediated — with their pinning test

### P1 (correctness)
| Finding | Pinning test |
|---|---|
| **F-272** file-scoped invoice-gap alarm suppressed (regression) | `src/CoralComply.Web.Tests/Services/Import/ImportIssueTypeCoverageTests.cs` :: `SequentialGap_IsNotSurfaced_UntilScopedToTheBusinessInvoiceRecord` |
| **F-283** imported credit notes surface in the register | `src/CoralComply.Web.Tests/Services/CreditNoteServiceTests.cs` :: `GetImportedCreditNotesForBusinessAsync_ReturnsImportedCreditNoteTransactions_ExcludingManualAndDeleted` |
| **F-270** Net VAT card figure + label share one period axis | `src/CoralComply.Web.Tests/Components/VATSummaryMetricCardTests.cs` :: `NetVatLabel_OpenPeriod_CarriesPeriodToDateQualifier`, `NetVatLabel_SettledOrLodgedPeriod_DropsPeriodToDateQualifier`; and `src/CoralComply.Web.Tests/Security/VatFigurePeriodLabelTests.cs` :: `TheAccrualCard_LabelsTheSamePeriodItsFiguresWereComputedOver` |
| **F-297** Approved page states truth, no fabricated "reviewed and approved" | `src/CoralComply.Web.Tests/Components/ApprovedTransactionsSurfaceTests.cs` :: `F297_DoesNotClaimEachRowWasReviewedAndApproved` |

### P2
| Finding | Pinning test |
|---|---|
| **F-298** output VAT / input VAT reported separately | `ApprovedTransactionsSurfaceTests` :: `F298_ReportsOutputAndInputVatSeparately_NeverAMixedTotal` |
| **F-280** step counter never exceeds total; frozen completion copy | `src/CoralComply.Web.Tests/Components/CassP2P3RemediationPinsTests.cs` :: `F280_StepCounterNeverExceedsTotal_AndCompletionCopyIsCorrect` |
| **F-287** legal name trimmed/validated on save | `src/CoralComply.Web.Tests/Services/LegalNameNormalizerTests.cs` :: (6 theory cases + idempotence + null/blank passthrough) |

### P3 vocabulary + residuals
| Finding | Pinning test |
|---|---|
| **F-P3-1** money renders B$ (arch guard, Dev harness excluded) | `CassP2P3RemediationPinsTests` :: `FP3_1_NoCustomerMoneyRendersWithTheCultureDependentCurrencyFormat` |
| **F-P3-2** day pluralisation + no "-" connector | `CassP2P3RemediationPinsTests` :: `FP3_2_PreflightDeadlineMessages_ArePluralisedAndUseNoHyphenConnector` |
| **F-P3-5** "Record a payment" | `CassP2P3RemediationPinsTests` :: `FP3_5_PaymentQuickAction_UsesRecordVocabulary` |
| **F-P3-6** Net VAT Due card names its period | `CassP2P3RemediationPinsTests` :: `FP3_6_NetVatCard_NamesTheComputedPeriod` |
| **F-P3-7** "Unknown" fallbacks get vocabulary | `CassP2P3RemediationPinsTests` :: `FP3_7_PaymentStatusFallback_HasVocabulary` |
| **F-275** grid/dialog can't contradict (parity pin) | `src/CoralComply.Web.Tests/Services/Filing/FilingStateStatusParityTests.cs` :: `FilingWorkflow_CoSetsStatusWithFilingState_AtEachTerminalTransition` |
| **F-276** casing "Awaiting lodgement" · **F-296** friendly source · **F-281/F-282** sample-tool strings | covered by the surface pins above + `DirAssertionFamilyPinsTests` (already on master) |

**Commits:** `aa2c88e1d` (P1 import) · `40e5b7e7f` (P1 dashboard/approved) · `7da72608f` (P2) · `c37359f70` (P3 + residuals).

---

## 2. Visual captures still needed (Cass's Q2) — where they will land

Every item above is test-pinned, but the following are **visual/appearance** findings whose closure Cass requires a rendered screenshot for. **These captures are NOT yet produced** — they require `c37359f70` on a running environment (staging is currently one commit behind at `13813a48b`; Comply is never auto-deployed). Once `c37359f70` is on staging, the deployed-env visual harness (or a tester) produces the captures and they are pushed into **this folder** as `NN-<finding>-<surface>.png`.

| Capture id | Finding | Surface to shoot | What "correct" looks like |
|---|---|---|---|
| 01 | F-283 | `/credit-notes` with an imported credit note present | The "Imported credit notes" section renders it; empty state does **not** say "No credit notes yet" |
| 02 | F-298 | `/transactions/approved` summary strip | "Output VAT (sales)" and "Input VAT (purchases)" cards — no "Total VAT" mixing directions |
| 03 | F-297 | `/transactions/approved` header | "included in your VAT return" — no "you've reviewed and approved" over unconfirmed rows |
| 04 | F-270 | Client dashboard Net VAT card, recent-activity ≠ earliest-unfiled | Label names the **figure's** period; lodged period shows no "(period to date)" |
| 05 | F-280 | Filing wizard at the final step | "Step 5/5" (never 6/5) + "Filing prepared. Ready to lodge" |
| 06 | F-292 | Import review treatment column | "Exempt" with no "0%"/"zero-rated" adjacency |
| 07 | F-288/F-294 | Import food-store banner (both states) | Names the qualification (not a licence); links to `/settings/business`; no "2025 VAT Reform" |
| 08 | F-276 | `/vatreturns/{id}/view` banner | "Read-only · Awaiting lodgement" (lowercase l), matching the PDF |

**Convention:** name files `NN-<finding>-<short>.png`, add a one-line caption in `captions.md`. If a surface renders wrong, file it AMBER (product issue) — do **not** paint it green.

---

## 3. The one thing that needs your decision — F-221 (Cass + Kai)

The import pre-commit preview's **substance** is already correct (the three buckets reconcile; held rows are no longer dropped). The open item is **copy only**:

- **Shipped (Kai's taxonomy):** "Ready / Needs review / Blocked".
- **Cass's frozen literal (F-221):** "Will import n · Will hold n · Will exclude n".

I did **not** change this — swapping the copy would revert Kai's shipped decision. **Please reconcile:** keep the shipped weather-report taxonomy, or restore the frozen literal. Whatever you rule, I'll pin it.

Surface: `src/CoralComply.Web/Components/Pages/ImportTransactions.razor` (`ComputeReviewSummary`, the `Ready / Needs review / Blocked` strip); pinned today by `ImportReviewWeatherReportTests`.
