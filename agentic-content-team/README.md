# Agentic Content Team — Setup Guide

Build a 3-agent AI workflow that researches, writes, critiques, and posts LinkedIn content — with a human approval gate before anything goes live.

**Tools used:** n8n · Anthropic API · Telegram · Google Docs · Google Sheets · LinkedIn

---

## What it does

1. You send a topic to a Telegram bot
2. Research Agent pulls key facts and angles for a LinkedIn post
3. Writer Agent drafts the post (150–250 words)
4. Critic Agent scores it 0–10. Below 7 → auto-rewrite (max 2 times)
5. Draft lands in Telegram with Approve / Reject buttons
6. Approve → saves to Google Docs + logs to Sheets + posts live to LinkedIn

---

## What you need

| Tool | Purpose | Cost |
|---|---|---|
| n8n (self-hosted or cloud) | Workflow automation | Free (self-hosted) |
| Anthropic API | Powers the 3 AI agents | Pay per use (~$0.001/run) |
| Telegram Bot | Trigger + approval interface | Free |
| Google Docs | Archive approved posts | Free |
| Google Sheets | Content log | Free |
| LinkedIn Developer App | Post to LinkedIn | Free |

---

## Step 1 — Get your credentials

### Anthropic API
1. Go to console.anthropic.com → API Keys → Create key
2. In n8n → Credentials → New → Anthropic → paste your key

### Telegram Bot
1. Open Telegram → search @BotFather → `/newbot`
2. Follow the prompts → copy the bot token
3. In n8n → Credentials → New → Telegram → paste token

### Google Docs + Sheets
1. In n8n → Credentials → New → Google Docs OAuth2
2. Follow the OAuth flow → sign in with your Google account
3. Repeat for Google Sheets OAuth2

### LinkedIn
1. Go to linkedin.com/developers → Create App
2. Add product: **Share on LinkedIn**
3. In n8n → Credentials → New → LinkedIn OAuth2 → connect your account

---

## Step 2 — Import the workflow

1. Download the `workflow.json` file from the description
2. In n8n → top right menu → Import from file
3. Select the JSON file

---

## Step 3 — Swap in your credentials

Open each of these nodes and select your credentials:

| Node | Credential to set |
|---|---|
| Telegram Trigger | Telegram account |
| Research Agent - Claude | Anthropic account |
| Writer Agent - Claude | Anthropic account |
| Critic Agent - Claude | Anthropic account |
| Send Draft Message | Telegram account |
| Send Approval Buttons | Telegram account |
| Save to Google Docs | Google Docs account |
| Append to Content Log | Google Sheets account |
| Create a post | LinkedIn account |

---

## Step 4 — Set your Sheets URL

In the **Append to Content Log** node → update the Google Sheets URL to your own spreadsheet.

Your sheet needs these column headers:
`Date | Topic | Draft | Score | Word Count | Rewrites | Status | Posted At`

---

## Step 5 — Activate and test

1. Toggle the workflow Active in n8n
2. Open your Telegram bot → send a topic (e.g. `AI automation for restaurants`)
3. Wait ~30 seconds → check for the draft in Telegram
4. Hit Approve → verify the post appears on LinkedIn

---

## Customising it

- **Change the audience** — edit the system prompts in Research Agent and Writer Agent
- **Change the platform** — swap the LinkedIn node for Twitter, Instagram, etc.
- **Change the scoring threshold** — edit the IF node (currently set to 7/10)
- **Add web search** — connect a web search tool to the Research Agent node
