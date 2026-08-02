# F-250 correction — the hypothesis is superseded, and the finding is worse

**For:** Cass Turnquest · **Date:** 2026-08-02 · staging, readonly-sql verified · issue **#4160**

## What you hypothesised

The aborted commit wrote duplicate **fingerprints** under the mis-resolved tenant id;
transactions never followed; the residue haunts Anna; "neither holding the row."

## What the database says

**The transactions did follow.** Anna and Associates
(`36f0d46e-208f-4a02-933d-55500f9ba6c3`) holds **16 full Transaction rows,
INV-NHC-1001 … INV-NHC-1016**, `Status = Processed`, `SourceSystem = MANUAL`,
created in a one-second burst at **2026-08-02T09:33:51.686 – 09:33:52.467** (a batch
signature). Nassau holds its own 16 with identical numbers and amounts.

**Class sweep:** exactly those 16 invoice numbers exist under two tenants each — 32
rows, 16 pairs. **No other invoice in the database is cross-tenant.** The
contamination is real, and it is bounded to this event.

Anna has **0 VAT returns**, so nothing has consumed the foreign rows yet — but they
are Processed, so a return generated for Anna today would compute Nassau's sales into
Anna's figures.

## What this changes

1. **The duplicate flag in Anna was never a ghost.** It was correct detection against
   rows that genuinely exist in that tenant. Nassau did not flag because Nassau's own
   1011 did not exist yet at that moment (it imported cleanly at 10:47). Both verdicts
   were locally truthful; the defect is the **tenanting of the write**, not the detector.
2. **This is a multi-tenant isolation breach** — one tenant's transactions in another
   tenant's ledger, in a state that flows into VAT computation. It sits above F-247 as
   its consequence.
3. **The purge you ordered must cover transaction rows, not fingerprints**: 16
   identified rows under Anna, soft-delete only, each audited, enumeration recorded
   before and after. Scope is exactly the 16 pairs listed in #4160.
4. **Order 4 grows a leg**: the tenant-scope assertion belongs on the **write path**,
   not only the detection layer — the import commit asserts the resolved BusinessId
   equals the session's business context at the row-write boundary and fails closed.
   Order 3 (fingerprints inside the row transaction) stays correct but would not have
   prevented this: both the rows and their artifacts were written under the wrong id,
   so atomicity would have made the contamination complete rather than partial.

## Not remediated

No rows touched. The purge is a bounded, recorded act awaiting your go — and it wants
the F-247 root cause landed first, or the next import re-creates it.
