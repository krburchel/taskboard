# TaskBoard

A Monday.com-inspired personal/work task manager. Single-file PWA hosted on
GitHub Pages; installable on PC, Mac, and iPhone.

**Live app:** https://krburchel.github.io/taskboard/

## How it works

- The app itself is static (this repo, public — it contains no data or secrets).
- Tasks live in **`tasks.json` in the private repo `krburchel/taskboard-data`**,
  read and written through the GitHub Contents API.
- On first launch, each device asks for a fine-grained GitHub personal access
  token (Contents read/write on the data repo only). The token is stored in
  that device's localStorage and never leaves the browser except to call
  `api.github.com`.

## Features

- Two boards (Work / Personal) with colored groups
- Status pills: Not Started / Working on it / Stuck / Done
- Due dates with overdue + due-today highlighting
- Subitem checklists and notes per task
- Three views: **Table**, **Kanban** (drag cards between statuses), and
  **My Week** (everything across both boards bucketed by Overdue / Today /
  This Week / Later)
- Offline-capable: last-synced data is cached locally; edits retry until they
  reach GitHub

## Agent access

Agents (Claude, Hermes, …) interact with the same `tasks.json` — see the
README in `taskboard-data` for the schema and API examples.

## Install

- **iPhone:** open the URL in Safari → Share → Add to Home Screen
- **Mac/PC:** open in Chrome/Edge → install icon in the address bar
