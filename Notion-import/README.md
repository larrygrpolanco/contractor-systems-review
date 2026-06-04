# Notion Import Pack — Lionize Construction

This folder turns the scattered project/sub/payment tracking into **three linked Notion
databases**. You (not Luz) set it up once; afterward Luz only uploads files and you walk
her through each closeout.

| File | Becomes the Notion database… | One row = |
|---|---|---|
| `01-projects-tracker.csv` | **Projects** (the hub) | one job (e.g. 3302 Chipco St) |
| `02-subcontractor-master-list.csv` | **Subcontractors** (master list) | one sub company |
| `03-closeout-checklist.csv` | **Closeout Items** | one required document, for one project |

The three are pre-seeded with the **3302 Chipco St** job so you can see the relations
working immediately. The Subcontractors file has two `SAMPLE -` rows — delete them.

---

## The one thing to know before you start

> **CSV import in Notion creates everything as plain Text, and it CANNOT create
> relations.** You import the data first, then (1) fix the property types and
> (2) add the relation links by hand. That's normal — it takes ~15 minutes. Steps below.

---

## Step 1 — Import the three CSVs

For each file:

1. In Notion, go to the page where this should live (make a page called
   **"Lionize Construction"** first).
2. Click **+ New page** → type a name (e.g. *Projects*) → in the page, type `/table`
   and pick **Table - Full page**, OR use the top-right **`···` → Import → CSV**.
3. The cleanest path: **`···` menu (top right) → Import → CSV → choose the file.**
   Notion creates a new database with all the columns.

Do this for all three. You'll now have three separate databases full of text columns.

## Step 2 — Fix the property (column) types

Click each column header → **Edit property** → change **Type**. Recommended types:

**Projects**
| Column | Type |
|---|---|
| Project | Title (already) |
| Status | **Status** → groups: `Bidding`, `Active`, `Payment Pending`, `Closed` |
| Program | **Select** → `Healthy Homes`, `CRA` |
| Roofing Job? | **Checkbox** (or Select Yes/No) |
| Contract Total / Amount Received / Amount Outstanding | **Number** → format **Dollar** |
| SLBE % | **Number** → format Percent (enter 34.45 as 34.45, or 0.3445 if you pick Percent) |
| All `Date …` columns | **Date** |
| Notes | Text |

**Subcontractors**
| Column | Type |
|---|---|
| Company | Title |
| Trade | **Select** (Roofing, HVAC, Electrical, Plumbing, Painting, …) |
| SLBE Status | **Select** → `S` (SLBE), `O` (Other) |
| Insurance Expiry | **Date** |
| Active? | **Checkbox** |
| rest | Text |

**Closeout Items**
| Column | Type |
|---|---|
| Document | Title |
| Category | **Select** |
| Status | **Status** → `Not started`, `In progress`, `Awaiting signature`, `Awaiting notary`, `Done` |
| Needs Signature / Needs Notary / Blocks Payment | **Checkbox** (Notion reads `Yes`/`No` text — convert to checkbox and it ticks the Yes rows) |
| Notes | Text |

## Step 3 — Add the relations (the wiring)

This is what makes it a *system* instead of three lists.

**A. Closeout Items → Projects** (each document belongs to one job)
1. Open **Closeout Items**. Add a new property → Type **Relation** → choose **Projects**.
   Name it **Project**.
2. Turn ON **"Show on Projects"** so the link is two-way.
3. For each row, set its **Project** to *3302 Chipco St*. (The text `Project` column from
   the CSV is just a helper — once the relation is set, you can delete the text column.)

**B. Subcontractors ↔ Projects** (which subs worked which jobs — many-to-many)
1. Open **Subcontractors**. Add property → **Relation** → **Projects**.
   Name it **Jobs Worked**. Turn on "Show on Projects" (it appears as *Subcontractors* there).
2. A sub can link to many projects and a project to many subs — that's expected.

After this you'll have:

```
        ┌──────────────┐
        │   Projects   │  ◀── the hub
        └──────┬───────┘
       ┌───────┴────────┐
       ▼                ▼
┌──────────────┐  ┌──────────────────┐
│ Closeout     │  │ Subcontractors   │
│ Items        │  │ (master list)    │
│ (1 job →     │  │ (many ↔ many)    │
│  14 docs)    │  │                  │
└──────────────┘  └──────────────────┘
```

## Step 4 — Add rollups (auto-summaries on Projects)

On the **Projects** database, add **Rollup** properties so each job shows its own health:

- **Open items** → Rollup → relation *Closeout Items* → property *Status* →
  Calculate **Count** (or count-not-`Done`). Tells you how far from done each job is.
- **Blockers left** → Rollup → *Closeout Items* → *Blocks Payment* → Count checked.
  When this hits 0, the package is submittable.

## Step 5 — Build the views

- **Projects → Board view**, grouped by **Status** — your dashboard (Bidding → Active →
  Payment Pending → Closed).
- **Closeout Items → filtered view**: filter `Blocks Payment = checked` AND
  `Status ≠ Done`, grouped by Project — "what's still blocking payment."
- **Subcontractors → Table** sorted by **Insurance Expiry** ascending, with a filter
  `Insurance Expiry is within the next 1 month` — catches expiring COIs before a job needs them.

## Step 6 — Make new projects one-click

So you don't rebuild the 14 closeout rows by hand each job:

- **Easiest:** Keep the 3302 Chipco closeout rows as a "master set." For a new job,
  select all 14 → **Duplicate** → bulk-edit the **Project** relation to the new job.
- **Cleaner (optional):** In Closeout Items, make a **Database Template** containing the
  14 standard documents, so "New → [template]" drops them in pre-filled. (Notion templates
  can't auto-set the Project relation, so you still pick the project after — but the 14
  rows + statuses come for free.)

---

## How this connects to the rest of the system

- **Files stay in Dropbox.** Don't upload PDFs into Notion. Instead add a **URL** property
  (e.g. *Dropbox Folder*) on Projects and paste the share link. Notion is the index; Dropbox
  is the file store. (Matches the decided stack.)
- **Claude "cowork" layer.** Before submitting, export/copy the filtered Closeout view (or
  paste the per-project checklist) and ask Claude *"what's outstanding and what's blocking
  payment?"* — same flow as `Payment-closeout/closeout-checklist-template.md`.
- The standalone markdown checklist in `Payment-closeout/` still works for paper/Claude use;
  `03-closeout-checklist.csv` is the same content shaped for the Notion database.

---

## Recommended next step (the 4th table — only when you need it)

The three databases above can't store **how much each sub was paid on each job** — a plain
many-to-many relation has no place for the dollar amount. The moment you want Notion to
**auto-calculate SLBE %** (instead of you computing it), add a 4th junction database:

**Job Subs** — one row per (project × sub):
`Project (relation)`, `Subcontractor (relation)`, `Amount Paid ($)`, `SLBE? (rollup from sub)`,
`Lien Release Signed? (checkbox)`, `Invoice Received? (checkbox)`.

Then a rollup on Projects sums SLBE-sub dollars ÷ total → SLBE %, and the lien-release
checkboxes feed straight into the closeout. This also replaces the "Subs who must sign"
table in the closeout template. **Hold off until the 3-table version is in daily use** —
don't front-load the complexity on Luz's first exposure.
