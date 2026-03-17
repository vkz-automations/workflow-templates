# Google Maps Lead Scraper — Setup Guide

Scrape leads from Google Maps, enrich them with AI summaries, find email addresses, send personalised cold emails, and log everything to Google Sheets — all triggered from Telegram.

**Tools used:** n8n · Apify · OpenAI · Google Sheets · Gmail · Telegram

---

## What it does

1. You send a message to your Telegram bot: `restaurants in London, 20`
2. Apify scrapes Google Maps for matching businesses (up to your limit)
3. Duplicates and businesses without websites are filtered out
4. Already-contacted leads (from your Sheets log) are skipped
5. For each new lead, OpenAI writes a short AI company summary
6. The workflow scrapes the business website to find an email address
7. OpenAI writes a personalised cold email for each lead
8. Gmail sends the email automatically
9. Every lead (sent or skipped) is logged in Google Sheets
10. Telegram notifies you when the run is done

---

## What you need

| Tool | Purpose | Cost |
|---|---|---|
| n8n (self-hosted or cloud) | Workflow automation | Free (self-hosted) |
| Apify | Google Maps scraping | Free tier available |
| OpenAI API | AI summaries + cold email writing | Pay per use |
| Google Sheets | Lead log | Free |
| Gmail | Send cold emails | Free |
| Telegram Bot | Trigger + notifications | Free |

---

## Step 1 — Get your credentials

### Apify
1. Go to [apify.com](https://apify.com) → Sign up → Settings → Integrations → API token
2. In n8n → Credentials → New → Apify → paste your API token

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com) → API keys → Create new key
2. In n8n → Credentials → New → OpenAI → paste your key

### Telegram Bot
1. Open Telegram → search `@BotFather` → `/newbot`
2. Follow the prompts → copy the bot token
3. In n8n → Credentials → New → Telegram → paste token

### Google Sheets
1. In n8n → Credentials → New → Google Sheets OAuth2
2. Follow the OAuth flow → sign in with your Google account

### Gmail
1. In n8n → Credentials → New → Gmail OAuth2
2. Follow the OAuth flow → sign in with the Gmail account you want to send from

---

## Step 2 — Import the workflow

1. Download the `workflow.json` file from this folder
2. In n8n → top right menu → **Import from file**
3. Select the JSON file

---

## Step 3 — Swap in your credentials

Open each of these nodes and select your credentials:

| Node | Credential to set |
|---|---|
| Telegram Trigger | Telegram account |
| Send Start Notification | Telegram account |
| Send Done Notification | Telegram account |
| Run Google Maps Scraper | Apify account |
| AI Company Summary | OpenAI account |
| Generate Personalised Cold Email | OpenAI account |
| Get Existing Contacts | Google Sheets account |
| Save to Google Sheets | Google Sheets account |
| Update Lead in Sheets | Google Sheets account |
| Send Cold Email via Gmail | Gmail account |

---

## Step 4 — Set up your Google Sheet

Create a new Google Sheet with these column headers (exact order matters):

`Business Name | Address | Phone | Website | Email | Status | AI Summary | Email Sent At | Notes`

Then in n8n:
- Open **Get Existing Contacts**, **Save to Google Sheets**, and **Update Lead in Sheets**
- Update the Google Sheets URL in each node to point to your spreadsheet

---

## Step 5 — Activate and test

1. Toggle the workflow **Active** in n8n
2. Open your Telegram bot → send a message in this format:
   `restaurants in London, 10`
   (sector/query, then a comma, then the number of results you want)
3. Wait for the "Starting scrape" notification, then the "Done" notification
4. Check your Google Sheet — leads should be logged
5. Check Gmail sent items — cold emails should be there

---

## How to customise it

- **Change the niche** — the query in Telegram drives everything. Try `dentists in Manchester, 15` or `gyms in Dubai, 20`
- **Change the cold email tone** — edit the system prompt in the **Generate Personalised Cold Email** node
- **Skip auto-sending** — remove the Gmail node and use the Sheet as a manual review queue instead
- **Change the AI model** — the OpenAI nodes default to GPT-4o mini. You can switch to GPT-4o for higher quality
