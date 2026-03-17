# YouTube Content Multiplier — Setup Guide

Send a YouTube URL to Telegram, get a LinkedIn post and a 5-tweet X thread saved to Google Docs automatically. One video → multiple pieces of content in under a minute.

**Tools used:** n8n · Telegram · SearchAPI · OpenAI · Google Docs

---

## What it does

1. You send a YouTube URL to your Telegram bot
2. The workflow extracts the video ID and fetches the full transcript via SearchAPI
3. OpenAI reads the transcript and generates a LinkedIn post + a 5-part X thread
4. Both posts are appended to a Google Doc with the date and video URL
5. Telegram confirms it's done

---

## What you need

| Tool | Purpose | Cost |
|---|---|---|
| n8n (self-hosted or cloud) | Workflow automation | Free (self-hosted) |
| Telegram Bot | Trigger | Free |
| SearchAPI.io | Fetch YouTube transcripts | Free tier available |
| OpenAI API | Generate the posts | Pay per use |
| Google Docs | Store the output | Free |

---

## Step 1 — Get your credentials

### Telegram Bot
1. Open Telegram → search `@BotFather` → `/newbot`
2. Follow the prompts → copy the bot token
3. In n8n → Credentials → New → Telegram → paste token

### SearchAPI
1. Go to [searchapi.io](https://www.searchapi.io) → sign up → copy your API key
2. In the **Fetch Transcript via API** node → replace `YOUR_SEARCHAPI_KEY` with your key

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com) → API keys → Create new key
2. In n8n → Credentials → New → OpenAI → paste your key

### Google Docs
1. In n8n → Credentials → New → Google Docs OAuth2
2. Follow the OAuth flow → sign in with your Google account

---

## Step 2 — Import the workflow

1. Download the `workflow.json` file from this folder
2. In n8n → top right menu → **Import from file**
3. Select the JSON file

---

## Step 3 — Swap in your credentials and Doc URL

| Node | What to update |
|---|---|
| Telegram Trigger | Select your Telegram credential |
| Fetch Transcript via API | Replace `YOUR_SEARCHAPI_KEY` with your actual key |
| OpenAI Chat Model | Select your OpenAI credential |
| Update a document | Select your Google Docs credential + replace `YOUR_GOOGLE_DOC_URL` with your Doc URL |
| Send Confirmation | Select your Telegram credential |

---

## Step 4 — Set up your Google Doc

Create a blank Google Doc. Copy its URL and paste it into the **Update a document** node.

Each run appends a new section with the date, video URL, LinkedIn post, and X thread. Over time it becomes your full content library.

---

## Step 5 — Activate and test

1. Toggle the workflow **Active** in n8n
2. Open your Telegram bot → send a YouTube URL
3. Wait ~20 seconds → check your Google Doc for the new content
4. Check Telegram for the confirmation message

---

## How to customise it

- **Change the tone** — edit the system prompt in the **Generate Social Posts** node
- **Add more platforms** — extend the output schema to include Instagram captions, newsletter intros, etc.
- **Change the AI model** — defaults to GPT-4o mini; switch to GPT-4o for higher quality
- **Save to Sheets instead** — swap the Google Docs node for a Google Sheets append if you prefer a structured log
