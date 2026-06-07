---
name: project-lionize-construction
description: Context about Lionize Construction (AIO Enterprise LLC) — a one-person JOC contracting business in Tampa, FL doing City housing rehab work. Key details about the business, contract structure, compliance requirements, and systems review done in June 2026.
metadata:
  type: project
---

Lionize Construction (AIO Enterprise, LLC, DBA) is a one-person contracting business run by Luz B. Polanco (CFO/sole operator) doing Job Order Contracting (JOC) for the City of Tampa's Housing Rehabilitation & Repair program. The contractor-systems-review repo is a systems audit of their workflows.

**Why:** Luz manages everything alone — bidding, permits, subcontractor coordination, City compliance forms, payment collection — with no centralized system. The review was done to understand current state and recommend improvements.

**How to apply:** When working in this repo, understand that all suggestions should be practical for a one-person operation with limited tech overhead. Prioritize reducing manual work, preventing missed compliance steps, and speeding up payment collection.

**Key business details:**
- Company: Lionize Construction / AIO Enterprise LLC
- License: CBC1267427 | Federal ID: 26-4346308
- Contact: Luz@aioenterprise.com | 407-466-3689
- Address: 38439 5th Avenue, PMB 128, Zephyrhills, FL 33542

**Full job lifecycle (from Operating Procedures Manual, Bid # 25-P-00039):** 6 homes assigned at the start of EACH month → joint home inspection with City CRA/Healthy Homes/Gordian reps (lead test if pre-1978, radon for all Healthy Homes, termite) → Scope Proposal (Excel "Home Repair Proposal" with subs' estimates/comparison) → homeowner approval meeting → post approved date to Gordian proposal tab → **Critical Path Method: each home must be COMPLETED in 60 days** → build → closeout/payment. Subs must provide insurance (per "Insurance Requirements" doc) + Tampa W/MBE and SLBE certs. There is a CSI MasterFormat "Cost Code Master" (01050 lead … 26100 wiring … 23100 HVAC) driving estimating in Excel.

**Canonical Dropbox structure (defined in the manual):** `Lionize Construction / 01. Tampa JOC - Home Rehab / 01. 2025. Home Rehab & Repairs Services /` then `01. CRA Homes - BE` OR `01. Healthy Homes - Marquaz`; each home folder has: Walk Through, Scopes, Quotes, Blueprint, Test & Permit Fees, Documents. Adopt this in any tooling — don't invent a new structure.

**How the business works:**
- Gets work through Gordian (City of Tampa JOC platform) — work orders issued per project
- Hires 8–12 subcontractors per job (roofing, HVAC, electrical, plumbing, painting, etc.)
- Must comply with City of Tampa DMI (Division of Minority & Small Business) forms: DMI-20 (sub schedule with bid) and DMI-30 (sub payment report with every invoice)
- SLBE (Small Local Business Enterprise) utilization must be tracked and reported
- Gets paid by City of Tampa after submitting a 12+ document package (invoice, cost breakdown, payment request, affidavit, lien releases from all subs, warranty acknowledgement, DMI-30)

**Current active/recent project:**
- 3302 Chipco Street, Tampa FL 33605 (Owner: Carlton Mallard)
- Contract HCDHH1-25-883-0020, PO 125210320
- Total: $147,335.42 | Final payment $49,832.42 submitted 3/23/2026
- Status as of June 2026: Final payment package submitted, awaiting City payment

**Two key external systems:**
- Gordian — JOC work order/pricing platform (City-side)
- Accela Citizen Access (aca.tampagov.net) — City of Tampa building permit portal

**Biggest workflow pain points identified:**
1. Payment package assembly is fragile (12+ docs, multiple parties)
2. No project status dashboard — everything in memory
3. No subcontractor database — rebuilt per job
4. Compliance deadlines (DMI-30, permit expiry, warranty) not tracked
5. Files named inconsistently, duplicates exist

**Top recommended next steps:**
1. Standardized per-project closeout checklist
2. Master subcontractor spreadsheet (name, trade, Federal ID, SLBE status, insurance expiry)
3. Simple project tracker (now decided: Notion — see [[user-contractor-systems-context]])
4. Consistent file naming convention: [Address]-[DocType]-[Date].pdf

**Work done (June 2026):** Built the `Notion-import/` pack — three CSVs (Projects tracker, Subcontractor master list, Closeout checklist) + a `README.md` wiring guide for relations/rollups/views. Projects has a CPM Start/Due (60-day) deadline tracker and Dropbox Folder URL; subs split SLBE vs W/MBE certs; closeout has a Phase column so the DB can later span the whole lifecycle. Settled on a 4-DB skeleton (reversed the earlier "defer the junction" call): Subcontractors (reusable master) + Projects (hub) + Job Subs (junction: one row per sub-per-job with Amount/SLBE flag/lien-release/COI/invoice checkboxes) + Closeout Items. Job Subs is the core fix for the top pains — it auto-calculates SLBE % (rollup: sum SLBE Amount / sum Amount) and drives the lien-release chase + payment blockers. Rationale: reimport is cheap so get the table/relation skeleton right now (expensive to retrofit); views/data/reminders stay cheap to change. Also locked in file-naming convention `[Address]-[DocType]-[YYYYMMDD].pdf`. Deferred: full lifecycle checklist rows, Cost Code Master as a DB, DMI-30 recurring reminder.

**Real 3302 Chipco sub data extracted (June 2026) from DMI-20 + DMI-30 P1/P2 into the Notion pack:** 11 real firms now in `02-subcontractor-master-list.csv` (Ambit Engineering [SLBE, engineer], Sunny R&R [roofing], KapCook [SLBE, portable toilet], Gruntle [windows/kitchen/bath, ~47% of sub value], W. Endless Solutions/Wanda Vivieca [cleaning], Orange Painting [SLBE, paint/drywall/soffit], Colossus [plumbing], Authentic Air [SLBE, HVAC], All in One Electrical [SLBE], Burgo Construction [int. painting], Guajiro [material]). `04-job-subs.csv` seeded from the DMI-20 utilization and reconciles EXACTLY to the documented SLBE: $112,121.14 total / $38,623 SLBE = 34.45%. **Discrepancies to confirm with Luz:** (1) All in One Electrical has two FEINs in the file — 93-1390385 (DMI-20 & P2) vs 33-1792777 (P1); (2) Wanda Vivieca bid as an individual (FEIN 050-72-1499) but paid under W. Endless Solutions LLC (FEIN 42-2273266); (3) contract total reads $147,335.72 on DMI-30 P2 vs $147,335.42 elsewhere (30¢); (4) Burgo ($4,000 int. painting) and Guajiro ($650 material) were added during construction via DMI-30 P2 — not on the bid — so as-built SLBE % differs slightly from the bid 34.45%. Earlier deliverable — a final-payment closeout checklist in `Payment-closeout/`: a reusable markdown template, a Notion-import CSV (14 line items across responsible parties with Status + Blocks-Payment flags), and a worked example filled from the real 3302 Chipco package. The checklist surfaced a likely gap: Roof Mitigation statement was still a blank template on a roofing job.

**Schedule add-on (June 2026) — the 5th database, the "time" layer:** Decided to keep Excel as the CPM *planner* (Notion can't do float/critical-path math) and make Notion the *tracker*. Chose **two linked tables over merging** (user's call): kept **Job Subs** (money + compliance) and added a new **Schedule** DB (one row = one task on one job: Task, Phase [9 phases], Type [Task/Rough Insp/Final Insp/Milestone], Critical Path? checkbox, Start + End dates [two separate Date props so Timeline draws Start→End], Status, Notes). A Schedule task relates to **Job Subs** (pick the sub from a dropdown — pulls sub/$/trade, so money is entered ONCE; no double-entry — this was the user's key concern) AND directly to **Project List** (needed for City-inspection rows that have no Job Sub, and so the hub can roll up schedule data). New Project rollups: Planned Finish (latest task End), Tasks Outstanding, and **Days vs Deadline** formula `dateBetween(CPM Due Date, Planned Finish, "days")` (positive = late) — auto-watches the 60-day clock. Four Schedule views: Timeline (Gantt by phase), This Week/Overdue, By Sub (call list), Board by Status. Seeded with the **real live 3610 E. MLK Blvd job**: `Notion-import/05-schedule.csv` (20 tasks parsed from the Excel CPM/Gantt — 11 critical-path, 06/10→08/10/2026, Mon–Sat) and `06-job-subs-3610.csv` (13 DMI-20 sub lines, seed ≈ $125,711 / 32.3% SLBE — NOTE the 3610 budget Excel has competing versions reading 32–39% SLBE, confirm finals). Setup guide: `Notion-import/SCHEDULE-README.md`. Note: 3610 subs new to the master to add = JM Drafting, Tronco's Land of FL, KapCook, Hughes Exterminators. Planned Finish (08/10) is ~1 day past CPM Due (08/09) — flag trips, which is accurate.

**Full final-payment package = 13 items:** (A, Luz prepares) Company Invoice, Contractor Payment Request (Luz+homeowner+City sign), Cost Breakdown, DMI-30; (B, notarized) Final Payment Affidavit & Release; (C, from subs) Consolidated Lien Release signed by EVERY sub + sub invoices; (D) Warranty Acknowledgement; (E, roofing jobs only) Roof Mitigation statement; (F, supporting) cancelled checks, receipts, Actual-vs-Budget Excel reconciliation. Submit by EMAIL to either Healthy Homes (marquaz.mcghee / jasmine.alvarado / knikira.marvray @tampagov.net) or CRA (Latasha.Hicks / vanassa.ross / be.parks @tampagov.net) depending on program.
