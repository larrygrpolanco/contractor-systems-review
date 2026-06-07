# Schedule Add-On — the time layer for Lionize Notion

Adds a **5th database, Schedule**, so each home shows *what work happens when, who's doing
it, and whether it'll beat the 60-day clock* — without re-typing anything you already track.

> **Design rule that keeps it simple:** Excel stays the *planner*, Notion is the *tracker*.
> You keep building the rigorous CPM/Gantt in Excel (it does the float / critical-path math
> Notion can't). Notion holds the same tasks for day-to-day tracking, reminders, and a visual
> timeline. A Schedule task **picks its Job Sub from a dropdown** — that one click pulls in the
> sub, trade, and dollars, so money/compliance live *only* in Job Subs. Nothing is entered twice.

This pack is seeded with the **real 3610 E. MLK Blvd job** — the 20 tasks from your Excel CPM
and the 13 DMI-20 sub lines — so the timeline and rollups work the moment you wire them.

```
 Subcontractors (rolodex)         Project List (hub, 60-day clock)
        ▲                                ▲             ▲
        │ pick sub                       │             │ direct relation (rollups + inspections)
   ┌────┴─────┐   pick Job Sub       ┌───┴────┐        │
   │ Job Subs │◀────────────────────│Schedule│────────┘
   │ $ + COI/ │   (no retyping)      │ tasks  │
   │ lien/inv │                      │ + time │
   └──────────┘                      └────────┘
```

---

## Two files in this add-on

| File | → Goes into | One row = |
|---|---|---|
| `06-job-subs-3610.csv` | **Job Subs** (existing DB — add rows) | one sub on the 3610 job ($, trade, scope) |
| `05-schedule.csv` | **Schedule** (NEW DB) | one task on the 3610 job (dates, who, status) |

> ⚠️ The dollar amounts in `06-job-subs-3610.csv` are **seed values pulled from the 3610
> DMI-20 working file** (totals ≈ **$125,711 / 32.3% SLBE**). Your budget Excel has a few
> competing versions (SLBE reads 32–39% across tabs) — confirm the final numbers and adjust
> in Notion. That live SLBE % is exactly the cross-check this is meant to give you.

---

## Step 1 — Add the 3610 subs to Job Subs

`06-job-subs-3610.csv` has the **same columns** as the original Job Subs import, so it merges in.

- Open the **Job Subs** database → top-right **`···` → Merge with CSV** → pick
  `06-job-subs-3610.csv`. (If "Merge with CSV" isn't offered, import it as a new database, then
  copy the 13 rows into Job Subs — same columns, so paste lines up.)
- Set each new row's **Project** relation to **3610 E. Martin Luther King Blvd**, and its
  **Subcontractor** relation to the matching master row. (The `Project` / `Subcontractor` text
  columns are just there for matching — they don't auto-link.)
- A few subs may not be in the **Subcontractors** master yet (JM Drafting, Tronco's, KapCook,
  Hughes Exterminators). Add them to the master first, then link — so next job they're reusable.

Now the 3610 Project row's existing SLBE %, lien-release, and COI rollups light up for the live job.

## Step 2 — Import the Schedule database

- In your **Lionize Construction** page: **`···` → Import → CSV** → `05-schedule.csv`.
  Notion makes a new database — rename it **Schedule**.

## Step 3 — Fix the Schedule property types

Click each column header → **Edit property** → set **Type**:

| Column | Type |
|---|---|
| Task | Title |
| Phase | **Select** → Envelope, Demo, Rough-In, Inspections (Rough), Insulation & Close, Trim-Out & Windows, Finishes, Termites & Clean, Inspections (Final) |
| Type | **Select** → Task, Rough Inspection, Final Inspection, Milestone |
| Critical Path? | **Checkbox** |
| Start | **Date** |
| End | **Date** |
| Status | **Status** → Not started, In progress, Blocked, Done |
| Notes | Text |
| Project / Job Sub | leave as **Text** for now → become **Relations** in Step 4 |

> We use **two separate date columns** (Start, End) on purpose — CSV import handles them
> cleanly, and Timeline view lets you point the bar at Start → End (Step 6).

## Step 4 — Add the two relations (the wiring)

For each: add a new property → Type **Relation** → pick the target → turn ON **"Show on …"**.

| In Schedule, add relation → | Name it | Then set each row to… |
|---|---|---|
| **Project List** | **Project** | 3610 E. MLK Blvd (select all 20 rows, bulk-set at once) |
| **Job Subs** | **Job Sub** | the matching `3610 - …` row (inspection rows stay **blank**) |

Then delete the two text helper columns (`Project`, `Job Sub`) once the relations are set.

> The Job Sub dropdown is the no-double-entry trick: picking "3610 - Gruntle" links the task to
> that sub's money/COI/lien record. The sub's contact comes through Job Sub → Subcontractor.

## Step 5 — Add the 60-day rollups on Project List (the payoff)

On **Project List**, add over the **Schedule** relation:

- **Planned Finish** → Rollup → Schedule → *End* → **Latest date**.
- **Tasks Outstanding** → Rollup → Schedule → *Status* → **Count** rows where ≠ Done.
- **Days vs Deadline** → Formula →
  `dateBetween(prop("CPM Due Date"), prop("Planned Finish"), "days")`
  — **negative = on track, positive = finishing late.** Optional flag formula:
  `if(prop("Days vs Deadline") > 0, "⚠️ LATE", "✅")`.

For 3610: Planned Finish should read **08/10/2026** vs CPM Due **08/09/2026** → ~1 day over,
so the flag trips. That's real — it shows the plan is a hair past the clock and worth tightening.

## Step 6 — Build the Schedule views (covers all three of Luz's needs)

1. **Timeline** — *the Gantt*. Add a Timeline view → set it to show by **Start → End**. Group or
   color by **Phase** (or by **Critical Path?**). Filter to one home when working it.
2. **This Week / Overdue** — Table view → filter `Status` is **not** Done **AND** `End` is
   **on or before** end of this week → sort by **Start** ↑. *What's due or behind.*
3. **By Sub** — Board/Table grouped by **Job Sub** → filter `Status ≠ Done`. *Your call list —
   who to schedule next.*
4. **Board by Status** — Board grouped by **Status**. *Quick field updates.*

---

## Making the NEXT home's schedule (the repeatable recipe)

1. Create/confirm the **Project** row; set **CPM Start Date**.
2. Build the scope + the **Excel CPM/Gantt** as usual (the rigorous plan).
3. Add that home's **Job Subs** rows (pick sub from master, enter $ + scope).
4. Bring the tasks into Schedule — either:
   - **Duplicate:** keep the 3610 tasks as a master set → select all 20 → **Duplicate** →
     bulk-set the new **Project** → adjust **Start/End** and pick each **Job Sub**. *(Same move
     the original README uses for Closeout Items.)*
   - **CSV-merge:** export the task list from the Excel CPM into the same columns as
     `05-schedule.csv` and **Merge with CSV** into Schedule, then set the relations.
5. Track daily from the four views; the **Days vs Deadline** flag watches the 60-day clock.

## Deliberately left out (keep it light)
- **Auto-reminders / recurring tasks** — need a paid Notion plan; the *This Week / Overdue*
  view covers "what's due" without them. Add later if the plan gets upgraded.
- **Dependency arrows & Float** — that math stays in the Excel CPM, on purpose.
