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

**Work done (June 2026):** Built the first deliverable — a final-payment closeout checklist in `Payment-closeout/`: a reusable markdown template, a Notion-import CSV (14 line items across responsible parties with Status + Blocks-Payment flags), and a worked example filled from the real 3302 Chipco package. The checklist surfaced a likely gap: Roof Mitigation statement was still a blank template on a roofing job.

**Full final-payment package = 13 items:** (A, Luz prepares) Company Invoice, Contractor Payment Request (Luz+homeowner+City sign), Cost Breakdown, DMI-30; (B, notarized) Final Payment Affidavit & Release; (C, from subs) Consolidated Lien Release signed by EVERY sub + sub invoices; (D) Warranty Acknowledgement; (E, roofing jobs only) Roof Mitigation statement; (F, supporting) cancelled checks, receipts, Actual-vs-Budget Excel reconciliation. Submit by EMAIL to either Healthy Homes (marquaz.mcghee / jasmine.alvarado / knikira.marvray @tampagov.net) or CRA (Latasha.Hicks / vanassa.ross / be.parks @tampagov.net) depending on program.
