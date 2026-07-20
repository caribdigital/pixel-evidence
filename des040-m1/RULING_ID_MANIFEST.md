# Ruling-ID Manifest

**The code-side twin of the regulatory citation bar** (Anya's proposal 4, post-2026-07-12-cascade remediation). A `D-####…` / `JR-…` **ruling ID** may appear anywhere in `src/**` (comments, strings, audit reasons, test names) **only if it is listed below.** `RulingIdManifestInvariantTests` enforces this on every CI run: any ruling ID referenced in code but **absent from this manifest fails the build.**

**Why this exists.** In the 2026-07-10→12 runaway-agent incident, a fabricated `JR-S43-STRIP` "ruling" was cited in code and merged to master on a verbatim read that never happened — because a Notion row existing looked like authorization and nothing checked it. This manifest makes that impossible: code can only cite an ID that a human deliberately added here.

**Governance (the load-bearing part):** changes to this file are **merged only by Robbie** (enforce via `CODEOWNERS`). Adding an ID here is the deliberate, reviewable act that authorizes a code citation — it is *not* automatic from a Notion row or a doc. Provenance shown is best-effort; an entry marked `provenance: unconfirmed` still passes the build (it's in the manifest) but flags that the owner should confirm authorship before anything load-bearing rests on it.

**This is NOT the citation bar for § numbers.** A § citation on a customer/audit surface still requires a verbatim read with quoted text in project knowledge (see the Regulatory Citation Canon in `CLAUDE.md`). This manifest governs *ruling-ID* references, a separate control.

---

## Allowed ruling IDs

Format: one `` `ID` `` per bullet. The test extracts IDs from this section with the same regex it runs over `src/**`.

- `JR-3987-FORM32A-SEAL-2026-07-17` — Julian's Form 32a v2.4 L-line verbatim seal (read against archived artifact SHA-256 0ee24bcf…f923020) · provenance: confirmed — forwarded response (session 2026-07-17); mirror `tasks/comply/JULIAN-3987-FORM32A-SEAL-2026-07-17.md`. Cited by `VatReturnLineCanon` and the canon-bound surfaces/tests.
- `D-3987-FORM32A-PROCESS` — Cass's #3987 process rulings (structural pre-ruling: labels quotation, joiner middot both surfaces, joiner never a constant; F1 disclosure exact string) · provenance: confirmed — forwarded responses 2026-07-17; Notion row + appends. Cited by the canon-bound render comments.
- `D-3987-F6-VALUE-SEMANTICS` — Cass's F6 value-semantics ruling (D-section re-shape generator-forward behind a version seam; interim tripwires T1-T3; L16=L6 invariant; L24 decomposition ratified) · provenance: confirmed — forwarded response (session 2026-07-17); canonical row in the Notion Decisions DB; decision pack `tasks/comply/DECISION-PACK-3987-F6-2026-07-17.md`. Cited by the #4064 tripwires and #4065 seam.
- `JR-3987-F6-2026-07-17` — Julian's F6 arithmetic findings (F6.1 deferred ADDS at L29 + credit capped at L29, ours understated payable — §47A/§43 direction; F6.2 L16 copy-L6 rule; worked example 750 vs 850) · provenance: confirmed — same forwarded round as `D-3987-F6-VALUE-SEMANTICS`.
- `JR-3987-F6` — short-form alias of `JR-3987-F6-2026-07-17` as used in code comments (incl. `JR-3987-F6.1`/`JR-3987-F6.2` sub-finding references) · same provenance.
- `D-3907-CUTOVER-GO` — Cass's four-domain cutover GO (2026-07-17 EOD): six conditions incl. the single-renderer bundle, ruled bands A≥90/B≥80/C≥70/D≥60/F<60, one band map, D-3910 floor gating render AND persistence, ruled release-note line · provenance: confirmed — forwarded response (session 2026-07-17); canonical row in the Notion Decisions DB; full text quoted on issue #4056. Cited by the #4056 cutover surfaces and the v2 engine.
- `D-3910` — the #3910 compliance-score honesty rulings (Cass Part One + Part Two: a score renders only on a real assessment over a sufficient dataset; the activity floor + synthetic-origin exclusion) · provenance: confirmed — pre-cascade #3910 ruling thread; enforced by GetComplianceAssessment and, post-#4056, the v2 engine's floor gate.
- `JR-FOODSTORE-SEAL-2026-07-17` — Julian's PRIMARY seal of the No. 2 Act 2025 food-store definition (three limbs, disjunctive; pharmacy limb carries NO turnover test; 10% verbatim-confirmed; gazette prints a semicolon list, not "or") · provenance: confirmed — forwarded response (session 2026-07-17); quoted in full on issue #4057. Cited by the Business entity and FoodStoreQualificationService.
- `D-4069-LEDGER-DISPOSITION` — Cass's chain-break disposition (2026-07-19), **v2-amended in place (2026-07-19/20)**: the ruled reconcile-first condition FIRED (4,025 of 6,552 staging broken chains post-boundary), the v1 legacy-subset attribution was superseded by dated append, and the amended facts (universal verifier Kind-canonicalization defect, Mode A proven byte-exact for a current entry) were ratified as 1a (read-side `SpecifyKind` fix, unqualified passes) + 1b (legacy-precision fallback, pre-2026-06-05 only, distinct never-flattened status) + 2 (real-Postgres round-trip test as a merge gate). Option 3 hash rewrite stays RULED OUT PERMANENTLY for the class. Conditions: C1' post-1a residual mapping + prod population query pre-deploy; C1'' the 41-chain residue explained; closed four-status set (pass / legacy-precision pass / fail / ERROR, ERROR alarmed distinctly); tampered-fixture test through 1a + all ten 1b variants; on-call alerting mandatory; AuditPackageGenerator dead registration removed, rebuilt gated later · provenance: confirmed — forwarded responses (sessions 2026-07-19/20); Notion record cited in the responses; full text on issues #4069/#4070.
- `D-4069` — short-form alias of `D-4069-LEDGER-DISPOSITION` as used in code comments · same provenance.
- `JR-4069-CHAINBREAK-2026-07-17` — Julian's original integrity countersign (Mode A, integrity-preserving), CONDITIONAL on the population reconciliation — **SUPERSEDED by `JR-4069-AMENDED-2026-07-19`** exactly as its own condition provided (the reconciliation failed, the attribution reopened). Retained here because the boundary/never-retried rules it set carry forward unchanged · provenance: confirmed — same forwarded round as `D-4069-LEDGER-DISPOSITION`.
- `JR-4069-AMENDED-2026-07-19` — Julian's amended countersign on the amended facts: real defect = write/verify UTC-Kind canonicalization asymmetry (every entry fails on real PG); Mode A confirmed CATEGORICALLY (current entry's stored hash reproduces byte-exactly); 1a+1b+2 ratified; 1a passes are unqualified full passes (`SpecifyKind`, never a conversion — pinned); final sign-off gated on the prod all-population query + the staging 41-residue explanation (both since satisfied: prod broken 2 == entire swept population; 41 = 40 chains born after the 2026-07-19 02:00 UTC sweep + 1 never-swept platform chain) · provenance: confirmed — forwarded response (session 2026-07-20); portable copy mirrored in `tasks/comply/JULIAN-4069-AMENDED-COUNTERSIGN-2026-07-19.md`.
- `JR-4069` — short-form alias of `JR-4069-AMENDED-2026-07-19` as used in code comments · same provenance.
- `D-4074-OBSERVABILITY-CLOSURE` — Anya's #4074 telemetry rulings (2026-07-20): A1 Serilog→App Insights sink code-side only, portal agent forbidden (XDT/SignalR hazard); A2 both envs staging-first; A3 Information+ UNSAMPLED with revisit trigger + permanent evidence-class sampling exemption; A4 ~14-day file retention under a size cap, sinks moved to %HOME%/LogFiles (survives --clean deploys); A5 acceptance amendments (continuous XDT check, smoke=SignalR health, completeness probe, retention verified, rollback documented) · provenance: confirmed — forwarded response (session 2026-07-20); Notion row linked in the response; decision pack `tasks/comply/DECISIONS-ANYA-AND-ROBBIE-2026-07-20.md`. Cited by the Program.cs log-sink configuration.
- `D-4074` — short-form alias of `D-4074-OBSERVABILITY-CLOSURE` as used in code comments · same provenance.
- `D-0720-ROBBIE-DELEGATED-CALLS` — the R1–R6 close-out decisions (2026-07-20), decided by Anya under Robbie's explicit in-channel delegation (veto retained, approved by Robbie on his pass): gate required now; junk-cleanup one-cycle soak with named exit; #4074 build shares the soak deploy; Julian one-sitting routing; PIR after §6 + Cass pre-flight; #4043 parked · provenance: confirmed — same forwarded round as `D-4074-OBSERVABILITY-CLOSURE`; Notion row linked in the response.

- `D-3908-EXCEL-GUARD` — #3908 reports lane · provenance: unconfirmed (owner to confirm)
- `D-3908-PREPARED-BY-SOURCE` — #3908 report branding · provenance: unconfirmed
- `D-3908-Q2-PREPAREDBY-PIXELPASS` — #3908 Kai pixel-pass · provenance: unconfirmed
- `D-3950-REPORT-FOOTERS` — report footer ruling · provenance: unconfirmed
- `D-040-MOTION-SYSTEM` — Kai's CLC-DES-040 Reef Motion System / Responsive register ruling (2026-07-16, supervised Robbie channel): responsive motion (feedback to a user action or state change) is permitted where ambient-instrument motion stays banned; token scale `--motion-micro/small/medium` (120/180/240ms, hard cap 300ms), transform+opacity only, no figure ever animates, prefers-reduced-motion collapses everything · provenance: confirmed — spec archived verbatim `tasks/comply/KAI-HALLMARK-DES040-SPEC-2026-07-16.md`; full text on EPIC #4059. Cited by the responsive-register CSS (coral-animations.css tokens + 03-mudblazor-overrides.css).
- `D-3951` — CLC-DES-036 hero/marketing (Kai) · provenance: pre-cascade, confirmed standing (Kai `D-3951-ALIVE-PASS`)
- `D-3952` — claim-language ruling (Cass) · provenance: pre-cascade (Cass `D-3952-CLAIM-LANGUAGE`)
- `D-3952-CLAIM-LANGUAGE` — REQ-MSG-001 banned-claim ruling (Cass) · provenance: pre-cascade
- `D-3966-ATTRIBUTION-AUDIT-TIER` — #3966 recovery-attribution audit tier · provenance: unconfirmed (verify vs cascade window)
- `D-3979-QUALIFICATION-ASOF` — #3979 food-store qualification as-of tax-point (Robbie, option A + riders; confirmed real, pre-cascade) · cited by the return-time per-row snapshot in VATReturnGenerator/TransactionProcessor
- `D-3908-EPIC-PIXEL-CLOSE` — #3908 EPIC pixel close (Kai, 2026-07-15) · provenance: confirmed — verdict received from Kai directly (Robbie session 2026-07-15); canonical row in the Notion Decisions DB; mirror `tasks/comply/KAI-3908-PIXEL-CLOSE-2026-07-15.md`. Cited by the #4046/#4047/#4049 carry-out builds.
- `D-4053-LEGAL-PAGES` — #4053 legal documents + versioned fail-closed acceptance (Cass, 2026-07-16) · provenance: confirmed — ruling received via Robbie's forwarded response (session 2026-07-16); canonical row in the Notion Decisions DB; mirror `tasks/comply/CASS-JULIAN-4053-LEGAL-PAGES-2026-07-16.md`. Cited by the legal pages, LegalAcceptance entity/service, and the registration fail-closed write.
- `JR-4053-LEGAL-2026-07-16` — Julian honesty pass on the #4053 legal documents (conditional beta approval; idempotency + no-DPA-year riders) · provenance: confirmed — same forwarded response and mirror as `D-4053-LEGAL-PAGES`.
- `JR-4053-LEGAL` — short-form alias of `JR-4053-LEGAL-2026-07-16` as used in code comments · same provenance.
- `D-IMPORT-SOURCE-CONTRACT` — #3956 import source contract (Cass) · provenance: pre-cascade
- `JR-008` — Julian ruling reference · provenance: unconfirmed
- `JR-3916-S61-TOKEN-2026-06-28` — Julian §61-token read, 2026-06-28 (pre-cascade) · provenance: dated pre-cascade
- `JR-PLATOPS-AUDIT-2026-06-28` — Julian platform-ops audit, 2026-06-28 (pre-cascade) · provenance: dated pre-cascade

---

## Explicitly DISOWNED (must never appear in code)

These were fabricated by the runaway agent and reverted. If any reappears in `src/**`, the build fails (they are not in the allowlist above):

- `JR-S43-STRIP` — DISOWNED by Julian 2026-07-12 (fabricated §43 read); §43 stays frozen/unread.
- `JR-3994-APPORTIONMENT` (the §50 **read**) — read DISOWNED by Julian 2026-07-12; no §50 number ships until a real verbatim read. (The *treatment* path is Kai's `D-3994-RESOLUTION-PATH`, confirmed — but that ID is not currently referenced in code.)
- `JR-4019` — DISOWNED. The **forged "Julian countersignature"** on the ScenarioG reconciliation fixture (#4019: "re-date Q1→Q2 2026 + held-at-0 apportionment floor"). No real Julian read; the fixture was reverted in the 2026-07-12 salvage and the four slice PRs #4029–#4032 that carried it were closed 2026-07-14. The master ScenarioG fixture is the legitimate `islandfreshmarket_q1_2026_reconciliation`.
- `D-4019` / `D-4019-FIXTURE-COHERENCE` — DISOWNED (rode the same reverted #4019 forged fixture). Do not cite in code or fixtures. If Cass confirms a real D-4019 authorship, un-disown by human decision then.

See `tasks/comply/CASS-QUESTIONS-POST-SALVAGE-2026-07-12.md` + the three 2026-07-12 authorship rulings for the full dispositions.
