# TaskBoard for Hermes

## Install (once, on the Hermes host)

```bash
curl -fsSL https://raw.githubusercontent.com/krburchel/taskboard/main/tb -o /usr/local/bin/tb
chmod +x /usr/local/bin/tb
```

Requires python3 (stdlib only). Then make the token available to Hermes's
shell sessions, e.g. in the environment Hermes runs under:

```bash
export TASKBOARD_TOKEN="github_pat_..."   # fine-grained PAT: taskboard-data only, Contents read/write
```

Sanity check: `tb info` should print the boards.

## Paste-ready instruction block

Copy everything below into Hermes's instructions/skill:

---

You can manage Kevin's TaskBoard (his Monday-style task tracker for Work and
Personal) using the `tb` shell command. Always use `tb` for task operations —
do not edit tasks.json directly.

Reading:
- `tb week` — what's on Kevin's plate: overdue / today / this week, across
  both boards. Use this for "what do I have today"-style questions.
- `tb list [work|personal]` — active tasks by group, with ids.
- `tb show <id>` — one task's full detail, including subitem ids and notes.
- `tb info` — boards, groups, and each board's status ids. Statuses are
  per-board and Kevin can customize them, so check `tb info` rather than
  assuming names.

Writing:
- `tb add "Task name" --board work|personal [--group "Name"] [--due YYYY-MM-DD] [--notes "text"]`
  (defaults: personal board, its first group)
- `tb complete <id>` — mark done (uses the board's done-type status)
- `tb status <id> <status-id>` — any other status; ids come from `tb info`
- `tb due <id> YYYY-MM-DD` / `tb due <id> none`
- `tb rename <id> "New name"`, `tb note <id> "text"`
- `tb sub <id> "Subitem"` to add a checklist item, `tb check <id> <sub-id>` to toggle one
- `tb move <id> "Group name" [--board X]`
- `tb archive <id>` to tidy a finished task away; `tb archive-done` to sweep
  all done tasks; `tb restore <id>` to bring one back
- `tb delete <id>` is permanent — prefer `tb archive` unless Kevin explicitly
  says delete

Conventions:
- `<id>` accepts any unique prefix of the id shown by `tb list`/`tb week`.
- Archived tasks are not active work; never count them in "what's due".
- When Kevin gives a fuzzy instruction ("mark the dentist thing done"), run
  `tb list` first, match the task by name, then act on its id.
- If a command fails with an ambiguity or unknown-status error, read the
  message — it tells you what to run to disambiguate.
- Sync conflicts are handled automatically (the tool retries); you never need
  to deal with shas or JSON.

---

## Notes

- The tool talks to `krburchel/taskboard-data` `tasks.json` via the GitHub
  Contents API; every write is a git commit prefixed `agent:`, so all agent
  activity is auditable (and revertable) in that repo's history.
- Due-date bucketing in `tb week` uses the host's local date — set the VPS
  `TZ` to Kevin's timezone for correct "today" behavior.
- `TASKBOARD_REPO` / `TASKBOARD_PATH` env vars override the defaults if the
  data ever moves.
