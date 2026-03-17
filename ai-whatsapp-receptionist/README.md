# AI WhatsApp Receptionist — Setup Guide

An AI receptionist called Alex that handles WhatsApp messages for your restaurant — taking bookings, managing modifications, processing cancellations, and answering FAQs. All logged automatically to Google Sheets. Remembers customers across conversations.

**Tools used:** n8n · Whapi · OpenAI · Supabase (Postgres) · Google Sheets

---

## What it does

1. A customer messages your WhatsApp number
2. Alex (the AI) greets them and collects the 4 booking details: name, date, time, party size
3. Booking is saved to Google Sheets automatically
4. Modifications, cancellations, and special requests are handled and the Sheet is updated
5. Alex remembers the last 10 messages per customer — no need to repeat themselves
6. If anything fails, a fallback message tells the customer to call directly

---

## What you need

| Tool | Purpose | Cost |
|---|---|---|
| n8n (self-hosted or cloud) | Workflow automation | Free (self-hosted) |
| Whapi.cloud | WhatsApp API connection | Paid (~$10–30/month) |
| OpenAI API | Powers the AI receptionist | Pay per use |
| Supabase | Stores conversation memory (Postgres) | Free tier available |
| Google Sheets | Bookings log | Free |

---

## Step 1 — Get your credentials

### Whapi
1. Go to [whapi.cloud](https://whapi.cloud) → create an account → connect your WhatsApp number
2. Copy your **API Token** from the dashboard
3. In n8n → Credentials → New → Whapi → paste your token

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com) → API keys → Create new key
2. In n8n → Credentials → New → OpenAI → paste your key

### Supabase (conversation memory)
1. Go to [supabase.com](https://supabase.com) → New project
2. Go to Settings → Database → copy the **Connection string** (URI format)
3. In n8n → Credentials → New → Postgres → paste the connection string
4. Name it `Postgres - Supabase`

### Google Sheets
1. In n8n → Credentials → New → Google Sheets OAuth2
2. Follow the OAuth flow → sign in with your Google account

---

## Step 2 — Set up your Google Sheet

Create a new Google Sheet with these exact column headers:

`Customer Name | Phone | Date and Time | No.of guests | Date and time added | Special Requests | Booking_ID | Status`

Name the first sheet tab `Reservations`.

---

## Step 3 — Import the workflow

1. Download the `workflow.json` file from this folder
2. In n8n → top right menu → **Import from file**
3. Select the JSON file

---

## Step 4 — Swap in your credentials and Sheet URL

Open each of these nodes and update:

| Node | What to update |
|---|---|
| Get Existing Booking | Select your Google Sheets credential + paste your Sheet URL |
| Save New Booking | Select your Google Sheets credential + paste your Sheet URL |
| Update Booking | Select your Google Sheets credential + paste your Sheet URL |
| Cancel Booking | Select your Google Sheets credential + paste your Sheet URL |
| Update Special Requests | Select your Google Sheets credential + paste your Sheet URL |
| OpenAI Chat Model | Select your OpenAI credential |
| Postgres Chat Memory | Select your Supabase/Postgres credential |
| Send WhatsApp Message | Select your Whapi credential |
| Send Error Fallback | Select your Whapi credential |

---

## Step 5 — Set up the Whapi webhook

1. Activate the workflow in n8n
2. Copy the **Production Webhook URL** from the Webhook Trigger node
3. Go to your Whapi dashboard → Settings → Webhooks
4. Paste the URL → save

Now any message sent to your WhatsApp number will trigger the workflow.

---

## Step 6 — Customise for your business

The AI prompt is in the **AI Receptionist Agent** node → system message. Update these to match your restaurant:

- **Restaurant name** — replace "The Bistro"
- **Opening hours** — update the hours section
- **Cuisine type** — update the cuisine line
- **Phone number** — replace `+1 (555) 123-4567` with your actual number (used for large party redirects and error fallback)
- **Large party threshold** — currently set to 15 guests, change to whatever works for your venue

---

## How it handles different scenarios

| Customer says | Alex does |
|---|---|
| "Hi, I want to book a table" | Collects name, date, time, party size — saves to Sheets |
| "Can I change my booking to Friday?" | Updates the existing row in Sheets |
| "Cancel my reservation" | Marks the row as Cancelled in Sheets |
| "I have a nut allergy" | Updates Special Requests in Sheets |
| "What are your opening hours?" | Answers from the system prompt — no Sheets update |
| Any node fails | Sends fallback message asking customer to call |
