# OpenClaw + Obsidian Routing Rules (Reference)

This is the detailed behavior spec for tag-driven routing.

Related docs:
- [Main Tutorial](../README.md)
- [Cron Setup](./cron-setup.md)
- [Examples](./examples.md)

## Rule: Idea

Trigger: line contains `#idea`

Actions:
- Create one idea note in `02_Ideas` using existing template
- Add link to that idea note on the same source line if missing

## Rule: Grocery

Trigger: line contains `#grocery`

Actions:
- Update `07_Reference/groceries` when item text is clear
- Add groceries link on that same source line if missing

## Rule: New Project

Trigger: line contains both `#project/new` and `#projects/<Name>`

Actions:
- Create project note in `01_Projects`
- Use `<Name>` as canonical name
- Folder routing:
  - with `#project/non-tech` -> `01_Projects/non tech/`
  - otherwise -> `01_Projects/tech/`
- Add project note link on source line

## Rule: Project Update

Trigger: line contains both `#project/update` and `#projects/<Name>`

Actions:
- Resolve existing project from `#projects/<Name>`
- Create update note:
  - `updates/YYYY-MM-DD - <slug>.md`
- Populate file with source line content
- Add update-file link on source line

## Rule: Project Task

Trigger: line contains both `#project/task` and `#projects/<Name>`

Actions:
- Resolve target project
- Create task note:
  - `tasks/YYYY-MM-DD-<slug>.md`
- Ensure links exist in all three places:
  1. Source line in daily note
  2. Project note task list
  3. `00_Dashboard/Projects Tasks.kanban.md` (Inbox or Next)

## Rule: Shopping

Trigger: line contains `#shopping`

Actions:
- Parse item text from line (remove tags)
- Route destination in `07_Reference/shopping/Shopping.dashboard.md`:
  - with `#shopping/need` -> `## Need`
  - with `#shopping/want` or `#shopping/wishlist` -> `## Want`
  - otherwise -> `## Want`
- Add item line in one of allowed formats:
  - `- Item name`
  - `- Item name — $Price`
  - `- Item name — Link`
  - `- Item name — $Price — Link`
- Skip duplicates using case-insensitive/fuzzy matching
- Add `[[07_Reference/shopping/Shopping.dashboard]]` link on source line if missing

## Rule: Quote

Trigger: line contains `#quote`

Actions:
- Parse quote text from line (remove tags)
- Infer and append 3-5 relevant tags (lowercase hashtags)
- Append one bullet to `06_Journal/Quote.dashboard.md`
- Skip duplicates using case-insensitive matching (ignoring tag differences)
- Add `[[06_Journal/Quote.dashboard]]` link on source line if missing

## Rule: Remind Me

Trigger: line contains `#remindme`

Actions:
- Parse reminder text from line (remove tags)
- Parse optional schedule metadata:
  - `due:YYYY-MM-DD`
  - `at:YYYY-MM-DD HH:MM`
  - optional priority `p1|p2|p3`
- Add reminder to `06_Journal/Reminder.dashboard.md`:
  - scheduled reminders -> `## Scheduled`
  - unscheduled reminders -> `## Inbox`
- Use checkbox format and include source daily-note link
- Skip duplicates using case-insensitive matching
- Add `[[06_Journal/Reminder.dashboard]]` link on source line if missing
- At end of run, send concise alert for due-today or overdue reminders

## Idempotency guidance

- Prefer stable slug generation
- Check for existing note/link before write
- On filename conflict, append deterministic suffix
