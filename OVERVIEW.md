# Lionize Construction — Systems Review & Forward Plan

## Who This Is

**Lionize Construction** (AIO Enterprise, LLC, DBA Lionize Construction)  
Luz B. Polanco — CFO / sole operator  
License: CBC1267427 | Federal ID: 26-4346308  
Contact: Luz@aioenterprise.com | 407-466-3689

This is a one-person contracting business doing **Job Order Contracting (JOC)** work for the City of Tampa under their Housing Rehabilitation & Repair program. Work involves managing a general contract with the City, pulling permits, hiring and coordinating 8–12 subcontractors per job, and navigating a significant paper-and-form compliance burden to get paid.

---

## What Each Folder Contains

| Folder | What it is |
|---|---|
| [Building-permits-inspections/](Building-permits-inspections/README.md) | Reference docs and screenshots for navigating Tampa's Accela permit portal |
| [Homeowner-selections/](Homeowner-selections/README.md) | Color/material selection forms homeowners sign before work begins |
| [Operating-procedure-manual/](Operating-procedure-manual/README.md) | DMI compliance forms (subcontractor schedules + payment reports) and Gordian workflow notes |
| [Request-payments/](Request-payments/README.md) | Complete final payment package for 3302 Chipco St — the 12+ documents required to collect payment from the City |
| `EBO-How-To-Do-Business-JOC.pptx` | City of Tampa orientation on doing business through JOC (reference material) |

---

## The Core Workflow (As It Exists Today)

```
Win JOC Work Order in Gordian
        ↓
Pull building permits (Accela portal, manually)
        ↓
Get homeowner color/material selections (Word doc, print & sign)
        ↓
Hire & coordinate subcontractors
        ↓
Track sub payments manually, complete DMI-30 each pay period
        ↓
Assemble 12+ document payment package
        ↓
Submit to City of Tampa → wait for payment
```

Every step is done manually. Documentation is scattered across PDFs, Word docs, Excel notes, and saved portal screenshots. There is no central project dashboard, no automated reminders, and no shared system — everything lives in Luz's head and these folders.

---

## The Biggest Problems

### 1. Payment Package Assembly is Fragile
Collecting the final payment requires 12+ documents from multiple sources (Luz, homeowner, 5+ subs, notary, City forms). There is a Word checklist (`To Do to submmit payment Request to Tampa.docx`) but no way to track which items are done, which are pending, and which are blocking payment. Miss one document and the City delays payment.

### 2. No Project Status Visibility
There is no single place to see: what projects are active, what phase each is in, what compliance forms are outstanding, or when warranty periods expire. This all lives in memory.

### 3. Subcontractor Coordination Has No System
Lien releases, insurance certificates, Federal IDs, SLBE status, payment amounts — all must be collected from 8–12 subs per job. There is no running subcontractor database, so this gets rebuilt from scratch for each job.

### 4. Compliance Deadlines Are Invisible
DMI-30 forms must be submitted with every payment request. SLBE percentages must hit a threshold. Permits have expiration dates. Warranty periods need tracking. Nothing generates reminders for any of these.

### 5. Files Are Named Inconsistently and Duplicated
Multiple versions of the same form exist (two affidavit files, two lien release files). Filenames have typos. No version control. No way to know which file is current without opening each one.

---

## Ideas for Moving Forward

These are ranked roughly by impact vs. effort — start with the highest-value, lowest-effort wins.

---

### Immediate (Low effort, high impact)

**A. Build a payment package checklist template**  
Turn the `To Do to submmit payment Request to Tampa.docx` into a standardized checklist — one per project, stored with that project's files. Each item has a checkbox, a responsible party (Luz, sub, homeowner, notary), and a due date. This alone prevents missed documents and delayed payments.

**B. Create a subcontractor master list**  
A simple spreadsheet: sub name, trade, Federal ID, SLBE status (S or O), phone, email, insurance expiry, typical cost range. Reuse it across every job instead of rebuilding the DMI-20 from scratch each time.

**C. Standardize file naming**  
Adopt a consistent format: `[ProjectAddress]-[DocumentType]-[Date].pdf`  
Example: `3302Chipco-Invoice-20260323.pdf`  
This makes the folder scannable without opening files.

---

### Short-Term (Moderate effort, major ongoing benefit)

**D. Set up a simple project tracker**  
A Google Sheet or Notion board with one row per active job:
- Address, owner name, contract #, WO #
- Contract amount, amount billed, amount received
- Current status (bidding / active / payment pending / closed)
- Key dates: contract signed, permits pulled, work complete, payment submitted, payment received
- Warranty expiry dates

This becomes the single source of truth replacing scattered files and memory.

**E. Create a per-project folder structure template**  
Every new job gets the same folder layout:
```
[Address]/
  ├── 01-Bid/           (DMI-20, bid docs)
  ├── 02-Permits/       (Accela screenshots, permit numbers)
  ├── 03-Selections/    (homeowner sign-offs)
  ├── 04-Progress/      (DMI-30 partials, inspection docs)
  └── 05-Closeout/      (invoice, affidavit, lien releases, warranty)
```

---

### Claude Code / AI-Assisted Opportunities

**F. DMI-20/30 Form Auto-Fill**  
Since the subcontractor list and header info (contractor name, Federal ID, address, contract number) is the same every time, Claude could help pre-populate these forms from a simple project data input. Reduces manual retyping and transcription errors.

**G. Payment Package Review**  
Before submitting a payment request, paste the checklist to Claude and ask: "Is everything here? What's missing?" Claude can cross-reference the document list against requirements and flag gaps.

**H. Gordian Work Order Guidance**  
The `To Do in Gordian.docx` checklist represents institutional knowledge that would be lost if that file were deleted. Converting it to a Claude-accessible prompt or runbook means Luz (or anyone helping) can get step-by-step Gordian guidance on demand.

**I. Subcontractor SLBE Calculator**  
Claude can quickly calculate SLBE percentage from a list of subs and their amounts, flag if the threshold is at risk, and suggest which subs to prioritize to stay compliant.

---

### Longer-Term (Higher effort, structural improvements)

**J. Digital Signature Workflow**  
Homeowner selections and payment request forms currently require wet signatures and physical handoffs. A free tool like DocuSign (or even Adobe Acrobat's free tier) would allow Luz to send documents for e-signature, get a timestamped audit trail, and eliminate printing/scanning.

**K. Permit Status Alerts**  
Building a simple script (or using a service like Zapier) that checks the Accela portal for permit status changes on active jobs and sends an email/text when an inspection is approved or a permit is about to expire.

**L. Recurring DMI-30 Reminder**  
A calendar trigger that fires on each pay period to remind Luz to complete and file the DMI-30 before submitting the invoice — preventing compliance gaps.

---

## Recommended Starting Point

The single highest-leverage thing right now is:

> **Create a standardized closeout checklist for each project** — listing every document needed for final payment, who is responsible for each, and what its current status is.

This directly addresses the most painful bottleneck (getting paid) and takes less than an hour to build. Everything else builds on top of having that structure.

---

## Key Numbers to Know

- City of Tampa JOC compliance contact: (813) 274-5522 (Office of Equal Business Opportunity)
- ACA portal permit help: 813-274-3100, option 1
- SLBE threshold: 34.45% was achieved on 3302 Chipco — track this carefully on future bids
- Gordian is the work order platform; ACA (aca.tampagov.net) is the permit portal — these are two different systems
