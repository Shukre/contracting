# Billable — contract log

A shared task board for contract work: every task carries a status, a running
timer for billing, and links to its GitHub issue and pull request.

**Live page:** https://claude.ai/artifact/QHeXg3oFH81F3HQVbs58cS

## What it does

- **Statuses as a pipeline** — In development → Pushed PR → Merged → Completed,
  with Blocked as a side state. Merged and Completed both count as finished:
  Merged for code that landed, Completed for work with no PR behind it. Filter
  chips across the top double as counts per stage.
- **Two sections** — open work sits above Done (Merged and Completed), each
  with its own task count, hours and amount. Done collapses, and stays
  collapsed for that browser.
- **Days on each row** — beside the amount, the same time as a decimal run of
  8-hour days (12 hours reads `1.50 d`), from the working day the rate basis
  defines. Totals elsewhere stay in hours.
- **Billable timer per task** — start/stop from the row; elapsed time and the
  running amount update every second. Starting a timer stops the one you already
  had running, so two tasks never bill the same minute.
- **GitHub links** — paste a full URL (`https://github.com/owner/repo/issues/42`)
  or the short form (`owner/repo#42`) and the row renders a labelled Issue / PR
  chip. Setting a task to Merged stops its timer.
- **Payment tracking** — a default hourly rate with a per-task override, running
  totals for the last 7 days and everything unbilled, a per-task "Mark invoiced"
  flag, and a CSV timesheet export of whatever is currently filtered.
- **Where the rate comes from** — the rate is a monthly retainer spread over a
  calendar-accurate working month. `settings/app` holds `hoursPerWeek` (40) and
  `hoursPerMonth` (173.93): 365.2425 days ÷ 7 ÷ 12 = 4.35 weeks a month, not a
  flat four. The statement spells the arithmetic out for whoever is paying:
  $5,000 a month ÷ 173.93 hours = $28.75 an hour, an 8-hour day $229.98, a
  40-hour week $1,149.89. Tasks carrying their own rate keep it, so historical
  work is not restated when the default changes.
- **Manual corrections** — a total you can type over (`45:30:17`, `12.5`, `90m`),
  ±5m / ±15m / +1h adjustments, and a backdated entry
  ("Sep 11, 1:30") for work you did before you opened the board. Durations parse
  as `1:30:00`, `1:30`, `1.5`, `90m` or `1h30`. Entries are still recorded — the
  7-day total reads them — they just are not listed in the editor.
- **Clients** — tag a task with a client, filter the board to one, and every
  total (tracked, unbilled, open) follows that filter. The CSV export is scoped
  to it too and names the file after it.
- **Entering work after the fact** — the new-task form takes a date, hours
  already worked (`48`, `1:30`, `90m`), a status, and an "already paid" tick, so
  a piece of finished work goes on the board in one step. The date carries the
  hours with it, so backdated work counts toward the week it belongs to rather
  than today. Only the title is required; the issue and PR are both optional.
- **Runaway timer warning** — a timer running longer than four hours flags
  itself on the row, so an overnight timer doesn't quietly become nine
  billable hours.
- **Anyone with access can add tasks** — the board is backed by the artifact's
  shared store, so tasks, timers and status changes are live for every viewer.
  The page shows who added each task, and flags when a teammate is the one
  timing it.
- **Keyboard** — `n` opens the new-task form, `Esc` closes it. The board
  remembers your status and client filter between visits.

The board opens with three tasks tagged **Example** — delete them once your own
work is on it.

## Sharing

The published page is private until it is shared. Open it, use **Share**, and
give people "Can interact" so they can add tasks and run timers; "Can view"
leaves them read-only (the page disables the controls it knows they cannot use).

## Source

`tracker.html` is the published page. It is written in artifact form — no
`<!doctype>`, `<html>`, `<head>` or `<body>` wrapper, since the platform supplies
those at publish time. Shared state lives in the artifact's document store
(`tasks/*` and `settings/app`). When that store is unavailable the page says so
and keeps tasks in `localStorage` for that browser instead.
