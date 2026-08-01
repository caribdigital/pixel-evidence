# Eng answers — F-214 (database), F216-Q1 (enumeration + screenshot), and the capture evidence under F-215

**From:** the dev seat via Robbie · **Answers to:** the Filing Preferences spec (F-215/F-216, F-214 reroute)

## F-214: the database answer

```
SELECT business_name, filing_frequency FROM businesses
WHERE business_id = '90790032-19f9-4798-86df-0fca87e29813';
→ Nassau Harbour Consulting Ltd. | quarterly
```

**The persisted value is `quarterly`** — so the Generate dialog offering quarters was CONSISTENT
with the database, and per your ruled branches this is NOT the beta-gating generator defect. It is
the **capture finding, and your rider 3 called it before the evidence arrived**: the value was
never captured at registration. The full trail:

- `Business.FilingFrequency` entity default: `"quarterly"` (Business.cs:26).
- `ClientOnboarding.razor:698` hardcodes `_model.FilingFrequency = "quarterly"` at page init as a
  "default value".
- The onboarding form renders a **"Filing Frequency" heading** (line 201) — but underneath it sits
  turnover-derived **deadline** text from `FilingFrequencyHelper` (14-day/21-day large-taxpayer
  messaging), and **no frequency input exists anywhere in the flow**. A-04's "captured as a
  MONTHLY filer" was the tester's intent; the form never asked, and the hardcoded default
  persisted.
- Secondary honesty note: that "Filing Frequency" heading over deadline-class text is itself a
  mislabel — the helper answers "which DEADLINE applies", not "which FREQUENCY is assigned".

So the F-214 branch resolves to: **generator ruling stands and the generator behaved correctly;
the defect is that frequency was never capturable at registration** (F-215's scope now includes
the capture, not just the display). January generation on Nassau Harbour will run at the
granularity of whatever the DB holds — currently `quarterly`. If the business SHOULD be monthly,
that value needs correcting before Generate (your call whether that's a data fix now or waits for
the F-215 build's admin-gated edit).

## F216-Q1: the enumeration, with the screenshot you predicted you'd need

The "Default Filing Method" dropdown renders **three options, all fully selectable, none
disabled**:

1. `Manual` — "Manual Filing"
2. `Portal` — **"Government Portal"**
3. `API` — **"API Integration"**

Two standing doors to capabilities the product does not have — worse than the
present-but-disabled case you flagged. The open dropdown, photographed on staging at `4d196bad`:

- Page: https://raw.githubusercontent.com/caribdigital/pixel-evidence/trunk/cass-run8/F216-1-filing-preferences-page-4d196bad.png
- Dropdown open: https://raw.githubusercontent.com/caribdigital/pixel-evidence/trunk/cass-run8/F216-2-method-dropdown-open-4d196bad.png

The second still is the whole finding in one frame: the three-option dropdown floating over the
page whose intro carries the unsealed §47(1A)/§47(1)/B$5M/§2 citations, with the impossible-event
toggles (Submission Confirmation / Acknowledgement Received / Filing Failure Alerts) visible below
— **and all defaulted ON**, which sharpens F-216.1: users aren't just able to opt into the
impossible events, they are opted in.

With the enumeration in hand, your stated lean (the selector becomes descriptive text — one
truthful answer) looks confirmed from here; we hold the selector's final form for your
post-enumeration ruling per the spec, while the "submitting" helper text dies in the build now
starting.

## Build now starting (per the ruled items; report follows when shipped)

- **F-215**: the ruled display line on Filing Preferences + the business profile; admin-gated edit
  with a consequence-stating confirm (confirm copy will come to you with the build).
- **F-216.1**: Acknowledgement Received and Filing Failure Alerts removed (no real internal event
  exists for either); Submission Confirmation re-scoped to your named candidate **"Lodgement
  recorded"** (its one-line description will be flagged for your ratification). Deadline
  Reminders / Overdue / Payment Due survive as date math. The words "submitted", "Tax Authority
  confirms", "rejected" do not survive on the page.
- **F-216.2 interim**: the "Preferred method for submitting VAT returns" helper is deleted.
- **F-216.3**: your interim intro line ships verbatim; J-FILINGPREFS sits with Julian.

---

## Addendum — the Q-AMEND evidence ask, complete

Amendment-record counts (`SELECT COUNT(*) FROM "VATReturns" WHERE "IsAmendment" = true`):
**staging = 0** (readonly-sql, verified) · **prod = 0** (run by Robbie on the prod access path).
Prediction met in both environments — no amendment records exist anywhere; the gated-off code
never produced data. The Amend action is gated off on `4f9f9f51` (live on prod) with the ruled
absence pin across every return state.
