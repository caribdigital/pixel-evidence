# Eng answers — F-226 (label flow) and F-230 (treatment re-derivation)

**For:** Cass Turnquest · **Date:** 2026-08-02 · code read at master `1570d0fa`

## F-226: does the Q1 label flow into the filing artifacts? — YES, four legs

1. **The PDF prints it.** `VATReturnPdfService.GenerateVATReturnPdf` takes
   `string filingFrequency = "Quarterly"` as a DEFAULT parameter. The period block is
   frequency-aware (monthly renders "Month/Year: January 2026") — but two call sites
   omit the argument (`ExportController.cs:376`, `ViewFiledReturn.razor:648`), so a
   monthly return's exported PDF prints **"Quarter/Year: Q1 2026"**.
2. **The PDF filename is quarter-shaped at both sites**: `VAT_Return_Q1_2026.pdf`
   (ExportController) and `PeriodDescription` with spaces underscored (ViewFiledReturn).
3. **The XML lodgement artifact hardcodes the frequency**:
   `VATReturnXmlService.cs:74` emits `<FilingFrequency>Quarterly</FilingFrequency>`
   unconditionally — a frequency misstatement inside the artifact itself for monthly
   filers. (Period StartDate/EndDate are honest dates.)
4. **The source string is frequency-blind**: `VATReturn.PeriodDescription =>
   $"Q{Quarter} {Year}"` (generator + entity), consumed on ~12 surfaces (all filing
   dialogs, the returns grid, ViewFiledReturn, payment dialogs).

Ruled fix shape (per F-226): the label derives from filing frequency + actual period
everywhere the template renders; the two PDF call sites pass the business frequency;
the XML emits the true frequency. **January stays held until this lands.**

## F-230: does confirming a category re-derive treatment? — YES, silently

`TransactionCategoryOverrideService.ApplySelection` (called by every approve path,
bulk and single) sets **`TaxCategory = MapToTaxCategory(category)`** and
**`AppliedVatRate = treatment.Rate`** from the confirmed category, with **no
conflict guard** against the row's imported treatment. Approving the engine's "Rent"
prediction (maps `_ => "STANDARD"`, 10%) on the Exempt residential-rent row flips
EXEMPT → STANDARD and 0% → 10% — your figure-corrupting branch.

**The taxonomy has no gap**: `VATCategory.ResidentialRent → "EXEMPT"` exists
(`MapToTaxCategory`, line 53). The engine predicted the wrong sibling (commercial
Rent) — the ruled conflict guard (flag loudly, never silently overwrite or coexist)
is the fix, plus F-231's bulk exclusion.

## F-229 — closed, and pinned

Code defaults: all three acknowledgment fields `= false`
(`VATReturnPreviewDialog.razor:389/392/395`). Robbie ticked them. The residual pin
Cass asked for is written and green: `VatReturnPreviewAcknowledgmentDefaultTests`
(defaults false + never assigned true outside the user's own binding) — rides the
next train.

## Also on the record from the same drive

The categorization page's **"Categorize High Confidence (>80%)" bulk button wrote
nothing durable** in our run (zero audit events, zero row updates; the page's
Approved counter resets on reload — UI-optimistic state only). Single "Set category"
clicks DO persist (5 rows written, audit-evented). Filed with the F-232/F-233 family.
