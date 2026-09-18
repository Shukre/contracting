# Billable — contract log

A shared task board for contract work: every task carries a status, a running
timer for billing, and links to its GitHub issue and pull request.

**Live page:** https://claude.ai/artifact/QHeXg3oFH81F3HQVbs58cS

## What it does

- **Statuses as a pipeline** — In development → Pushed PR → Merged, with Blocked
  as a side state. Filter chips across the top double as counts per stage.
- **Billable timer per task** — start/stop from the row; elapsed time and the
  running amount update every second. Starting a timer stops the one you already
  had running, so two tasks never bill the same minute.
- **GitHub links** — paste a full URL (`https://github.com/owner/repo/issues/42`)
  or the short form (`owner/repo#42`) and the row renders a labelled Issue / PR
  chip. Setting a task to Merged stops its timer.
- **Payment tracking** — a default hourly rate with a per-task override, running
  totals for the last 7 days and everything unbilled, a per-task "Mark invoiced"
  flag, and a CSV timesheet export of whatever is currently filtered.
- **Manual corrections** — ±5m / ±15m / +1h adjustments, a backdated entry
  ("Sep 11, 1:30") for work you did before you opened the board, and a per-task
  log of every time entry. Durations parse as `1:30`, `1.5`, `90m` or `1h30`.
- **Clients** — tag a task with a client, filter the board to one, and every
  total (tracked, unbilled, open) follows that filter. The CSV export is scoped
  to it too and names the file after it.
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
