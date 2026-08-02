# For Julian — what needs your read

**From:** Robbie (engineering) · **Date:** 2026-08-02
**Evidence pack:** https://github.com/caribdigital/pixel-evidence/tree/trunk/cass-run9

Six items. The first is the one that matters most, because I nearly shipped a change against your
own sealed record and stopped only when the canon contradicted the instruction I was executing.

---

## 1. §47A canon status — did I nearly delete verified law? (HEADLINE)

Cass ruled today that the penalty banner computed figures "from unsealed formulas with **unsealed
§47A pinpoints**", and extended that to the Help guide: "unsealed figures on a reference page are
worse than on a banner." I acted on both.

Then I read `docs/decisions/REGULATORY_CITATION_CANON.md`, which records the opposite:

| Anchor | Behavior | Tier | Readers | Source |
|---|---|---|---|---|
| §47A(3)(a) | greater of $100 or 2% of tax payable; single fine, not per-day, no cap | **PRIMARY** | Cass+Julian | 2026-06-08 co-read, `2014-0032.pdf` p.60 |
| §47A(3)(b) | 10% of tax owed | **PRIMARY** | Cass+Julian | 2026-06-08 co-read |
| §47A(4) | interest = prime + 1% | **PRIMARY** | Cass+Julian | 2026-06-08 co-read |
| §47A(4A) | RP supply 21-day interest commencement | **HELD** | Cass+Julian | citation stripped 2026-06-08 (your walkback) |

`PenaltyRiskService.cs` carries the same three as sealed constants with your provenance comments.
Only **(4A)** is HELD.

On that record, `Help/FilingGuide.razor` was stating **verbatim-read, correctly-cited law** — and I
removed it on a premise the canon contradicts. **I have reverted that edit and shipped nothing to
the Help guide.** The canon's own rule decided it for me: *"Changes to citations go through a
Cass+Julian co-read — file an issue, never silently update."* Stripping a verified citation is as
much a change as adding one.

**Your read, please:**
- **(a)** Do §47A(3)(a), (3)(b) and (4) still stand as PRIMARY from the 2026-06-08 co-read? If yes,
  the Help guide's text was legitimate and B1 needs revisiting with Cass.
- **(b)** Cass also ruled the admin surface "may keep its citation **marked unverified**". If §47A(4)
  is PRIMARY, that marking would itself be inaccurate — a verified citation labelled unverified.
  Which way?

**One thing I want to be square about:** the banner removal was still correct, but not for the
reason given. The deleted `CalculatePenaltyExposure` applied those **sealed rates** to an
**invented base** — its own doc comment says it "uses the current in-progress filing's net VAT as a
proxy; falls back to the most recent acknowledged filing in history". The rates were yours; the tax
base was fabricated. That removal stands on the proxy-base ground.

---

## 2. J-PENALTY — narrower than it looked

Cass activated this today. If item 1 resolves as the canon records, the question is **not whether
exposure may be quantified** — the rates are sealed — but **what inputs are lawful**.

- Which tax base may a fine be computed against: the period's *assessed* tax only, or may a draft
  or proxy figure ever be used?
- May any figure render **before** a return is assessed?
- Does §47A(2)'s "automatic and immediately due" character change what the product may assert about
  a fine that DIR has not yet raised?

---

## 3. J-FORM301 — ready to close

Your read stands: no verified evidence of a "Form 301", and no verified DIR electronic-lodgement
specification. **Nobody has sourced an artifact since.** The export is quarantined (gated off,
absence pinned across all eight return states), and the XML artifact's existence depends entirely
on this answer.

**Does the absence now stand as the finding** — i.e. is the XML export **removed** rather than left
quarantined?

---

## 4. F-245 — the signatory-capacity enumeration

Cass ruled this comes through her **with your read** before it ships as worded. The list is
`RegisteredTaxpayer / BicaLicensedPractitioner / AuthorisedEmployee / AuthorisedAgent`, presented
to the signatory at the moment of approval. Her flag: the **AuthorisedAgent** limb brushes the held
Type-3 question. Nothing has shipped.

---

## 5. F-251 — does the audit-emit ruling extend this far? (NEW)

Sixteen transactions crossed a tenant boundary — one client's rows written into another client's
ledger, Processed, where a generated return would have computed them. The graver half: **there are
zero audit events anywhere in the database for that write window.** Sixteen rows persisted leaving
no trace, which is why only a two-tenant accident surfaced it.

Your and Cass's audit-emit ruling of 2026-06-22 requires a durable row for the enumerated
regulatory state-transition classes. **Scope question:** does it extend to *any* write that
persists a transaction — making a write with no audit row a violation of the ruling itself — or is
it confined to those enumerated transitions, leaving ordinary persistence out of scope?

The answer decides whether F-251 is a ruling breach or a new control to be designed.

---

## 6. F-236 — your condition 2 is satisfied (evidence, no action needed)

You granted the countersign on two conditions.

- **Condition 1** — an invariant test on the *recalculate* path, not just create — shipped with the
  fix, alongside a reflection "mirror" pin that fails naming any figure field the two paths stop
  sharing.
- **Condition 2** — January re-verified post-fix — **done on staging.** The return now reads
  **600 / 200 / 400** and, decisively, the stored `Box24_TotalInputVAT` refreshed from its stale
  **0** to **200**. So L26 sums to 200 and L27/L29 to 400: the worksheet and the footer finally
  tell one story. Corrected through the ruled path (void → Draft → recalculate), with the void's
  audit row written before the transition and the three artifacts superseded, not deleted.

Your reading of the defect was exact: freshness, not semantics. Nothing you sealed was touched.
