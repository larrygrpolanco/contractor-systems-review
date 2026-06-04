# Notion Import Pack — Lionize Construction

Turns the scattered project / sub / payment tracking into **four linked Notion databases**.
You (not Luz) set it up once; afterward Luz only uploads files and you walk her through each
job. The structure below is deliberately the part worth getting right *now* — tables and
relations are painful to retrofit, while views, data, and reminders are cheap to change anytime.

## The four databases — three layers

| File | → Database | Layer | One row = |
|---|---|---|---|
| `02-subcontractor-master-list.csv` | **Subcontractors** | reusable master (built once) | one sub company |
| `01-projects-tracker.csv` | **Projects** | the hub | one job (e.g. 3302 Chipco St) |
| `04-job-subs.csv` | **Job Subs** | the link | one sub on one job (with $, lien release, COI) |
| `03-closeout-checklist.csv` | **Closeout Items** | the link | one required document on one job |

```
  REUSABLE MASTER          THE HUB              THE LINKS (per-project)
┌────────────────┐    ┌──────────────┐    ┌──────────────────────────┐
│ Subcontractors │◀───│   Projects   │───▶│ Closeout Items (documents)│
└────────────────┘    │  (60-day     │    │ Job Subs ($, lien, COI)   │
        ▲             │   clock)     │───▶└──────────────────────────┘
        └──── Job Subs links sub ◀──▶ job ──────────────┘
```

**Why Job Subs exists:** your three biggest pains — chasing a lien release from every sub
(#1), no running sub system (#3), and SLBE % (#4) — are all the same missing fact: *which sub
is on which job, for how much, and is their paperwork in.* That can't live in a plain
relation (a relation has nowhere to put the dollar amount), so it gets its own table. Once it
exists, SLBE %, "lien releases outstanding," and "what's blocking payment" all calculate
themselves.

The pack is pre-seeded with **3302 Chipco St** so relations work immediately. Rows prefixed
`SAMPLE -` (in Subcontractors and Job Subs) are illustrations — **delete them after import.**

---

## The one thing to know before you start

> **CSV import creates every column as plain Text and CANNOT create relations.** Import the
> data first, then (1) fix property types and (2) add the relation links by hand. ~20 minutes.

---

## Step 1 — Import the four CSVs

For each file: in your **Lionize Construction** page, top-right **`···` → Import → CSV** →
choose the file. Notion makes a new database. Do all four.

## Step 2 — Fix the property (column) types

Click a column header → **Edit property** → change **Type**.

**Projects**
| Column | Type |
|---|---|
| Project | Title |
| Status | **Status** → `Assigned`, `Inspection`, `Scope Proposal`, `Approved / In Build`, `Payment Pending`, `Closed` |
| Program | **Select** → `Healthy Homes`, `CRA` |
| Batch (Month) | **Select** (e.g. `2026-06`) — 6 homes assigned at the start of each month |
| Roofing Job? | **Checkbox** |
| Contract Total / Amount Received / Amount Outstanding | **Number → Dollar** |
| SLBE % | **Number → Percent** (manual for historical jobs; auto-calculated for new ones — see Step 4) |
| CPM Start Date | **Date** — clock starts at scope approval |
| CPM Due Date | **Formula** → `dateAdd(prop("CPM Start Date"), 60, "days")` — the 60-day deadline |
| Permit Expiry | **Date** |
| other `Date …` | **Date** |
| Dropbox Folder | **URL** |
| Notes | Text |

**Subcontractors**
| Column | Type |
|---|---|
| Company | Title |
| Trade | **Select** |
| SLBE Certified? / W/MBE Certified? / Insurance on File? / Active? | **Checkbox** |
| Insurance Expiry | **Date** |
| Address / Phone / Email / Notes | Text |

**Job Subs**
| Column | Type |
|---|---|
| Job Sub | Title |
| Amount | **Number → Dollar** |
| Counts toward SLBE? / COI Valid for Job? / Lien Release Signed? / Sub Invoice Received? | **Checkbox** |
| Scope of Work / Trade / Notes | Text |

**Closeout Items**
| Column | Type |
|---|---|
| Document | Title |
| Phase | **Select** → `Inspection`, `Scope`, `Sub Qualification`, `Build`, `Closeout` (seed rows are all `Closeout`) |
| Category | **Select** |
| Status | **Status** → `Not started`, `In progress`, `Awaiting signature`, `Awaiting notary`, `Done` |
| Needs Signature / Needs Notary / Blocks Payment | **Checkbox** |
| Notes | Text |

## Step 3 — Add the relations (the wiring)

For each, add a new property → Type **Relation** → pick the target database → turn ON
**"Show on …"** so it's two-way. Then set each seed row's relation to *3302 Chipco St* /
the matching sub. (The text `Project` / `Subcontractor` helper columns from the CSVs are just
for matching — delete them once the relations are set.)

| In this database | Add relation → | Name it |
|---|---|---|
| Closeout Items | Projects | **Project** |
| Job Subs | Projects | **Project** |
| Job Subs | Subcontractors | **Subcontractor** |

(That's it — `Subcontractors ↔ Projects` is now reachable *through* Job Subs, so you don't
need a separate direct relation.)

## Step 4 — Add the rollups (this is the payoff)

On **Job Subs**, add one formula:
- **SLBE Amount** → Formula → `if(prop("Counts toward SLBE?"), prop("Amount"), 0)`

On **Projects**, add rollups over the **Job Subs** relation:
- **Subbed Total** → Rollup → Job Subs → *Amount* → **Sum**
- **SLBE Subbed** → Rollup → Job Subs → *SLBE Amount* → **Sum**
- **SLBE % (calc)** → Formula → `prop("SLBE Subbed") / prop("Subbed Total")` → format Percent.
  *This replaces hand-calculating SLBE — watch it against the 34.45%-type threshold live.*
- **Lien Releases Outstanding** → Rollup → Job Subs → *Lien Release Signed?* → **Count unchecked**
- **COIs Outstanding** → Rollup → Job Subs → *COI Valid for Job?* → **Count unchecked**

And over the **Closeout Items** relation:
- **Payment Blockers Left** → Rollup → Closeout Items → *Blocks Payment* → count rows where
  Blocks Payment is checked AND Status ≠ Done. When this hits **0**, the package is submittable.

> Tie it together: the "Consolidated Final Lien Release" row in Closeout Items isn't marked
> Done until **Lien Releases Outstanding = 0**. The junction feeds the checklist.

## Step 5 — Build the views

- **Projects → Board**, grouped by **Status** — the dashboard (Assigned → … → Closed).
- **Projects → 60-Day Clock**: Table/Timeline sorted by **CPM Due Date** ↑, filter `Status ≠ Closed`.
  The deadline radar — every home must finish 60 days after scope approval, 6 new homes a month.
- **Closeout Items → Blockers**: filter `Blocks Payment = checked` AND `Status ≠ Done`, grouped
  by Project — "what's still stopping payment."
- **Job Subs → Lien-Release Chase**: filter `Lien Release Signed? = unchecked`, grouped by
  Project — your call list when assembling a payment package.
- **Subcontractors → COI Watch**: sort by **Insurance Expiry** ↑, filter `within the next 1 month`.

## Step 6 — Make new projects (almost) one-click

- Keep the 3302 Chipco **Closeout Items** rows as a master set; for a new job, select all 14 →
  **Duplicate** → bulk-set the **Project** relation. (Or build a database **Template** holding
  the 14 standard documents.)
- Add Job Subs rows as you award each sub — picking the Subcontractor from the master list
  pulls in their Fed ID, certs, and COI status, so you never rebuild the DMI-20 roster from scratch.

---

## File naming + Dropbox (pain #5 — decide this now, it's free)

Files stay in **Dropbox**; Notion just links to them. Two conventions to lock in *now*, while
Luz is uploading going forward (retrofitting names later is the painful part):

**1. Folder structure — use the one the Operating Procedures manual already defines.** Don't invent one.
```
Dropbox // Lionize Construction
  └── 01. Tampa JOC - Home Rehab
      └── 01. 2025. Home Rehab & Repairs Services
          ├── 01. CRA Homes - BE        ← Program = CRA
          └── 01. Healthy Homes - Marquaz ← Program = Healthy Homes
              └── [Each home]
                  ├── Walk Through
                  ├── Scopes        (Cost Code Master)
                  ├── Quotes
                  ├── Blueprint
                  ├── Test & Permit Fees
                  └── Documents
```
Paste each home's folder link into the Project's **Dropbox Folder** URL property.

**2. File names:** `[Address]-[DocType]-[YYYYMMDD].pdf` → e.g. `3302Chipco-Invoice-20260323.pdf`.
Sortable, scannable, kills the duplicate-version problem (two affidavits, two lien releases).

---

## How this connects to the rest of the stack

- **Claude "cowork" layer:** before submitting, copy the per-project **Blockers** view (or paste
  the filled checklist) and ask Claude *"what's outstanding and what's blocking payment?"* — same
  flow as `Payment-closeout/closeout-checklist-template.md`.
- The standalone markdown checklist in `Payment-closeout/` still works for paper/Claude use.

---

## Deliberately deferred (not worth the effort yet)

- **Full lifecycle checklist rows.** The `Phase` column lets Closeout Items eventually hold
  Inspection (lead test pre-1978, radon for Healthy Homes, termite), Scope (Home Repair Proposal
  Excel, homeowner approval, **post date to Gordian**), and Sub Qualification (COI, W/MBE + SLBE)
  tasks. Add these template rows once the closeout flow is in daily use.
- **Cost Code Master as a database.** The manual's CSI line items (01050 Lead … 26100 Wiring …)
  drive estimating, which lives fine in the *Home Repair Proposal* Excel. Only promote to a Notion
  database if you later want Notion-driven scope tracking.
- **DMI-30 recurring reminder.** Once jobs are flowing, a Notion automation can nudge the DMI-30
  each pay period off a date field. Structure first.
