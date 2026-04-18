# OpenClaw + Obsidian README

This guide documents my practical setup for running OpenClaw with an Obsidian vault using tag-driven automation.

## 1) How to install OpenClaw

### 1.1 Get Codex Plus plan

Sign up for a [Codex Plus plan](https://chatgpt.com/codex) — this is what powers OpenClaw.

> **Important:** Anthropic explicitly bans using Claude Code with OpenClaw and will cancel your membership without notice. Stick with Codex (OpenAI). OpenAI partnered with Peter, the creator of OpenClaw, so you're clear to use it there.

### 1.2 Install Codex CLI

```bash
npm i -g @openai/codex
```

While you're at it, install the [Codex desktop app](https://chatgpt.com/codex) too. It's useful for interactive sessions — you can paste images directly into the chat, which comes in handy when debugging or giving visual context.
### 1.3 Tell Codex to install OpenClaw

Paste this prompt into Codex to kick off the install:

> Install OpenClaw on my machine by following the official installation guide at https://docs.openclaw.ai/install. Walk through each step, confirm dependencies are met, and let me know when it's done or if you hit anything that needs my input.

Then:

1. Wait for Codex to confirm the install is complete.
2. Verify install:

```bash

openclaw --help
```

3. Start and validate services:

```bash
openclaw gateway start
openclaw gateway status
openclaw status
```

4. Set/confirm the two key paths:
- OpenClaw workspace: `/Users/collin/.openclaw/workspace`
- Obsidian vault: `/Users/collin/Documents/ParteeLabs`

---

## 2) Setup Obsidian

### 2.1 Vault setup

Use a stable folder structure so automation has deterministic destinations:

```text
00_Dashboard/
01_Projects/
  tech/
  non tech/
02_Ideas/
03_Learning/
04_Research/
05_Content/
06_Journal/
  daily notes/
  digest/
07_Reference/
  groceries/
  shopping/
09_Scripts/
10_Templates/
11_Context/
12_Aux/
13_Cron/
  jobs/
  security-audit-runs/
```

What each folder is for:

- `00_Dashboard` — operational views and control pages you review frequently.
- `01_Projects` — active project records; split between technical and non-technical work.
- `02_Ideas` — individual idea notes captured from daily notes.
- `03_Learning` — courses, learning plans, and study artifacts.
- `04_Research` — deeper research notes, source synthesis, and references-in-progress.
- `05_Content` — publishable writing drafts and long-form guides.
- `06_Journal` — daily notes and digest/history of day-to-day activity.
- `07_Reference` — stable reference material (groceries, shopping, and other evergreen lists).
- `09_Scripts` — automation scripts and helper tooling used by the workflow.
- `10_Templates` — note templates used by Obsidian Templates and automation outputs.
- `11_Context` — supporting context docs (briefs, background material, supporting assets).
- `12_Aux` — miscellaneous supporting files that do not fit core workflow folders.
- `13_Cron` — cron job metadata/status mirrored into the vault.

### 2.2 Daily Note

Daily notes are the main entry point for this whole system. Enable them in **Settings → Core Plugins → Daily Notes**, then set your template to `10_Templates/Template.Daily Note.md`.

Configure it to open automatically on launch so you always have somewhere to drop thoughts immediately — no friction, no hunting for the right note. Everything gets tagged and routed from here.
### 2.2 Plugins

Required/important:

- **Dataview** (dashboard/query layer)
- **Templates** core plugin (for repeatable note structure)

Recommended tag conventions (for reliable Dataview + routing):

- `#project/new`, `#project/update`, `#project/task`
- `#projects/<Name>`
- `#idea`, `#grocery`, `#shopping`, `#quote`, `#remindme`

### 2.3 Templates

Set Obsidian Templates folder to:

- `10_Templates`

Templates used heavily:

- `10_Templates/Template.Daily Note.md`
- `10_Templates/Template.Daily Digest.md`

If a daily note is missing, automation can create it from the daily template first.

### 2.4 Sync

You want obsidian to sync somehow. that way you can install it on your phone, computer, tablet, etc. you have a few options. Some paid, some free. 

- Obsidian Sync
- Google Drive
- Dropbox

---

## 3) How I use OpenClaw + Obsidian

### 3.1 Telegram setup

I use Telegram as the chat interface into OpenClaw. To set it up, follow the Telegram configuration steps in the OpenClaw docs (or just ask Codex on the CLI or app — it can walk you through it). Tip: you can paste images directly into the CLI chat, which is useful for giving visual context.

I run 2 separate agent chats:
- **Main chat** — general ops, daily routing, reminders, projects
- **School chat** — coursework-specific context, kept separate to avoid mixing concerns

Running two agents lets me kick off a task in one chat and keep working in the other while it runs.

If you have any issues setting up a second chat agent just ask codex to create a second agent for you.

### 3.2 Routing rules

This is the heart of the workflow. Obsidian has a concept of daily notes. I Capture all my thoughts or whatever throughout the day in my daily notes, then openclaw reads my note every half hour and takes action depending on different tags.

Core behavior:

- `#idea` → create individual idea note in `02_Ideas`
- `#grocery` → update `07_Reference/groceries`
- `#project/new + #projects/<Name>` → create new project note
- `#project/update + #projects/<Name>` → create dated update note under project
- `#project/task + #projects/<Name>` → create project task note + link into boards
- `#shopping` (+ `#shopping/need|want|wishlist`) → `Shopping.dashboard`
- `#quote` → append to `Quote.dashboard` with inferred tags
- `#remindme` (+ optional `due:` / `at:` / `p1|p2|p3`) → `Reminder.dashboard`

Reference docs:

- [routing-rules.md](docs/routing-rules.md) — full tag reference and routing logic
- [examples.md](docs/examples.md) — real daily note examples with tag → output walkthroughs
- [examples/](docs/examples/) — sample vault files (daily note, templates, dashboards) for bootstrapping a fresh setup

### 3.3 Cron jobs

Cron is the heartbeat of this workflow.

Active pattern:

- Daily heartbeat maintenance around 12:30 PM ET
- Daily digest generation
- Weekly security audit
- Other task-specific automation as needed

Important vault mirrors:

- Cron definitions/state: `13_Cron/jobs`
- Security audit run outputs: `13_Cron/security-audit-runs`

Reference:

- [cron-setup.md](docs/cron-setup.md) — full cron job definitions, schedule config, and vault mirror setup

### 3.4 Dashboards

Dashboards are where I review and act.

Key dashboards:

- `06_Journal/Reminder.dashboard.md`
- `06_Journal/Quote.dashboard.md`
- `07_Reference/shopping/Shopping.dashboard.md`
- `00_Dashboard/Ideas.dashboard.md`
- `00_Dashboard/Projects Tasks.kanban.md`

Operating loop:

1. Capture quickly in daily note
2. Tag intent
3. Let OpenClaw route
4. Work from dashboards

### 3.5 Multi agent

Current model:

- Multiple agents can be used for specialized domains (example: general ops vs coursework specialist).
- Agents can run in separate chats/sessions to keep context clean.
- Routing between agents is currently intentional/manual unless explicitly automated.
- You can also run different models on different agents. for example you could have the main agent on gpt4o and a coding agent on codex5.3

---

## Notes

- Keep destination folders stable once automation is active.
- Use deterministic naming and links to keep reruns idempotent.
- Prefer concise, parseable bullet lines in daily notes.