# Delaware Hybrid Funding Formula Calculator

A single-file HTML calculator that models Delaware's K-12 school funding formula — Division I units, school and Central Office positions, and the PEFC Hybrid Funding Model (Opportunity + Operational funding) — for every district, charter, and school in the state.

**Not an official DDOE tool.** This is an independent working model built to make the funding formula's business rules visible, traceable to source, and testable — not a substitute for DDOE's own calculations.

## Live file

`index.html` — open it directly in any browser. No build step, no dependencies, no server required. All data is embedded in the file itself.

## What's in it

The calculator has four tabs:

**Business Rule Register.** Every funding component (Division I units, Principal, Assistant Principal, Central Office positions, Opportunity/Operational weights, etc.) listed with its formula, FY25-26 rate, and a status badge — Verified (matches the reference workbook exactly), Business rule confirmed (matches the workbook, legal citation still pending), Pending, or Open discrepancy. The goal is that every number in the calculator traces back to either the reference workbook or a specific statute/appropriation citation, and anything that doesn't is flagged rather than hidden.

**Locked Model.** Real FY25-26 data for all 259 Delaware schools and 43 independent LEAs (19 districts + 24 charters), plus a Statewide total. Pick an LEA and optionally a school, and it shows exactly what the current formula produces — enrollment, positions, and dollars — using the real numbers, read-only. This tab answers "what does the current formula actually produce right now."

**What-If Model.** Type in hypothetical enrollment (K-3, 4-12, Basic, Intense, Complex, Low-Income, MLL, Vocational) for a school or LEA that doesn't exist yet, and see the positions and funding the formula would produce. Every rate is editable, including the statewide Opportunity ($163M) and Operational ($279,026,800) funding pools — useful for modeling a future year's appropriation, a new school opening, or a different funding level. Every row is tagged **Verified** (the confirmed reference-workbook formula) or **Pattern-derived** (a step-function I inferred by pattern-matching against the real position counts in all 259 schools, because the underlying formula for Principal, Assistant Principal, and Admin Support Professionals isn't published anywhere I've found — including inside the reference workbook itself, where those values are precomputed rather than formula-driven). Pattern-derived numbers are estimates, not entitlement calculations — treat them accordingly.

**Plan Forward.** Open items: things that are correct-per-the-reference-workbook but still conflict with cited statute (Reading Cadre's charter exclusion), formulas that couldn't be verified or derived (Principal/AP/Admin Support Professionals/Food Services Supervisor), and policy questions the reference workbook itself flags as unresolved (Equalization, Local Funding Reform, tiering of LI/MLL/Vocational weights).

## Data source

All enrollment, position, and rate data comes from `Hybrid Funding Formula Calculator for 25-26 w Charter version 05152026.xlsm` — the authoritative FY25-26 workbook — extracted with `openpyxl` in cached-value mode and embedded directly into `index.html` as a JSON constant. Nothing in the Locked Model tab is typed in by hand.

`Delaware School Funding Formula Technical Reference.xlsx` is a supporting workbook with a Rule Register and Legend sheet, used earlier in this project to track legal citations for each business rule before the reference workbook was available. It's not read by the calculator at runtime.

## Known limitations

- **Principal, Assistant Principal, Admin Support Professionals**: no verified closed-form formula exists (even the reference workbook stores these as precomputed values, not formulas). The What-If Model's numbers for these are pattern-derived from the real dataset and explicitly labeled as such.
- **Food Services Supervisor**: doesn't appear to be enrollment-driven at all — only 8 of 259 real schools have a nonzero value, always exactly 1. Modeled as a manual yes/no toggle in the What-If tab rather than a fabricated formula.
- **Reading Cadre**: confirmed business rule (1 per district/charter) conflicts with the cited FY2026 appropriation statute, which excludes charters. This conflict exists in the reference workbook too — it's a genuine open policy question, not a bug. See the Register tab.
- **Vocational Division I / Vocational Deduct**: pulled as precomputed values from the reference workbook rather than re-derived from the underlying CTE participation-percentage chain, to avoid introducing a new error while re-deriving a working value.
- **Dover Air Force Base**: appears in the reference workbook's data as a distinct entity but is not one of the 43 independent LEAs (its own Assistant Superintendent/Director fields are 0). Its enrollment is folded into the Statewide totals but it is not separately selectable in either calculator tab.

## History

This calculator went through several iterations before landing on a dataset-driven design: an early version used manually-entered enrollment and formulas sourced from Title 14 statute text and GPT-assisted notes, several of which turned out to deviate from the workbook DDOE and district staff actually use. It was rebuilt from the ground up against the real reference workbook on 2026-07-22 — see the discrepancy box on the Register tab for the specific corrections that resulted (removal of a standalone Pre-K bucket, Instructional Supports modeled as a flat 20% catch-all instead of individually-modeled positions, Assistant Superintendent applying equally to charters and districts, and 11-Month Supervisor/Transportation Supervisor using continuous division with no floor).
