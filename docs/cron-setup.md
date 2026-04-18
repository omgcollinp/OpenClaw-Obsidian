# OpenClaw + Obsidian Cron Setup (Reference)

This doc covers practical cron setup for the workflow.

Related docs:
- [Main Tutorial](../README.md)
- [Routing Rules](./routing-rules.md)
- [Examples](./examples.md)

## Recommended jobs

1. **Obsidian Daily Digest**
   - Schedule: `0 8 * * *` (America/New_York) - 8am every day
   - Purpose: generate a daily digest of news, sports, whatever you want + link it in today’s daily note

2. **oauth-reauth-watchdog**
   - Schedule: `0 9 * * *` - 9am every day
   - Purpose: OpenAI codex auth expires every couple weeks. When it expires you dont get a message, openclaw will randomlly stop working and you will need to go back to your mac and reauth, this is often inconvientnt. this job alerts you via telegram 3 days before you need to log back in, 2 days before and the day before.

3. **hci-daily-brief-1000**
   - Schedule: `0 10 * * *` - 10am everyday 
   - Purpose: Give a run down of what is going on this week in my HCI class. I have my class schedule and course materials in the vault. Openclaw can read it and give me a run down.

4. **daily-heartbeat-maintenance-1230**
   - Schedule: `30 12 * * *` - every 30 mins every day.
   - Purpose: run HEARTBEAT tag-processing rules (`#quote` + `#remindme` included). this is the main cron job that handles all the tag routing rules.

5. **YouTube Analyzer 4x Daily**
   - Schedule: `0 0,6,12,18 * * *` - runs 4 times a day every day
   - Purpose: Analyze youtube vids that are on my publicly available youtube playlist. put the analysis files in my vault in the correct folder and link the analysis in todays daily note. 

6. **healthcheck:security-audit**
   - Schedule: `0 15 * * 0` - every sunday
   - Purpose: a security audit to make sure openclaw is safe and secure. 

## OpenClaw CLI commands

List jobs:

```bash
openclaw cron list --json
```

Add job example:

```bash
openclaw cron add \
  --name "daily-heartbeat-maintenance-1230" \
  --tz "America/New_York" \
  --cron "30 12 * * *" \
  --agent main \
  --message "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. For #quote lines, append 3-5 relevant tags to the quote when writing to 06_Journal/Quote.dashboard.md. For #remindme lines, route reminders to 06_Journal/Reminder.dashboard.md (Inbox/Scheduled) and send a concise alert for due-today or overdue items. If nothing needs attention, reply HEARTBEAT_OK." \
  --announce \
  --channel telegram \
  --to "<YOUR_CHAT_ID>"
```

Enable/disable/remove:

```bash
openclaw cron enable <job_id>
openclaw cron disable <job_id>
openclaw cron rm <job_id>
```

Run now for testing:

```bash
openclaw cron run <job_id>
```

## Sync cron metadata to Obsidian

```bash
python3 <OBSIDIAN_VAULT>/09_Scripts/OpenClaw/sync_cron_to_vault.py
```

Expected output path:
- `<OBSIDIAN_VAULT>/13_Cron/jobs`
