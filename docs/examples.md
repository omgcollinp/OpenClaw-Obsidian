# OpenClaw + Obsidian Examples (Reference)

Related docs:
- [Main Tutorial](../README.md)
- [Routing Rules](./routing-rules.md)
- [Cron Setup](./cron-setup.md)

## Input examples (daily-note lines)

```md
- New app concept for async standups #idea
- Buy avocados, eggs, and spinach #grocery
- Start GT capstone workspace #projects/GT-Capstone #project/new
- Ship onboarding checklist #projects/ClientPortal #project/task
- Added SSO scope and owner notes #projects/ClientPortal #project/update
- Vaseline recommended by dermatologist #shopping #shopping/need
- Clean Seiko diver I saw online #shopping #shopping/wishlist
```

## Expected outcomes

- Idea note created in `02_Ideas/`
- Grocery reference updated in `07_Reference/groceries`
- Project note created in `01_Projects/tech` or `01_Projects/non tech`
- Task note created in project `tasks/` + linked in daily note/project/kanban
- Project update note created in `updates/` + linked in daily note
- Shopping items added to `07_Reference/shopping/Shopping.dashboard.md` under Need/Want
- Quote appended to `06_Journal/Quote.dashboard.md` with 3-5 relevant hashtags
- Reminder routed to `06_Journal/Reminder.dashboard.md` under Scheduled/Inbox, with due/overdue alerting on heartbeat runs

## Validation checklist

- [ ] Tags are on same line as the content
- [ ] Files created in expected folders
- [ ] Source line backlinks inserted
- [ ] Rerun does not duplicate artifacts
- [ ] Dashboard links resolve
