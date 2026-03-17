# AI Lead Qualification Agent — Setup Guide

A prospect fills in a form, and the AI scrapes their website, scores the lead, classifies their company type, and routes them automatically — qualified leads go to your team, unqualified leads get a polite rejection email.

**Tools used:** n8n · OpenAI · Airtop · Gmail

---

## What it does

1. A prospect submits an n8n form (name, company, website, email, request)
2. The AI Agent scrapes their company website using Airtop
3. OpenAI analyses the data and returns a lead score (1–10), company type (SaaS or Agency), and a sales summary
4. A qualifier node checks if the lead meets the threshold (score ≥ 6 + qualified)
5. Qualified leads → routed by company type → email sent to the right team
6. Unqualified leads → automatic rejection email sent to the prospect

---

## What you need

| Tool | Purpose | Cost |
|---|---|---|
| n8n (self-hosted or cloud) | Workflow automation | Free (self-hosted) |
| OpenAI API | Lead scoring and analysis | Pay per use |
| Airtop | Website scraping | Free tier available |
| Gmail | Send emails | Free |

---

## Step 1 — Get your credentials

### OpenAI
1. Go to [platform.openai.com](https://platform.openai.com) → API keys → Create new key
2. In n8n → Credentials → New → OpenAI → paste your key

### Airtop
1. Go to [airtop.ai](https://www.airtop.ai) → sign up → copy your API key
2. In n8n → Credentials → New → Airtop → paste your key

### Gmail
1. In n8n → Credentials → New → Gmail OAuth2
2. Follow the OAuth flow → sign in with the Gmail account you want to send from

---

## Step 2 — Import the workflow

1. Download the `workflow.json` file from this folder
2. In n8n → top right menu → **Import from file**
3. Select the JSON file

---

## Step 3 — Swap in your credentials and email addresses

| Node | What to update |
|---|---|
| OpenAI Chat Model | Select your OpenAI credential |
| Website Scraper Tool | Select your Airtop credential |
| Email SaaS Team | Select your Gmail credential + replace `YOUR_EMAIL@gmail.com` |
| Email Agency Team | Select your Gmail credential + replace `YOUR_EMAIL@gmail.com` |
| Rejection Email | Select your Gmail credential |

---

## Step 4 — Customise for your business

### Update the AI system prompt
Open the **AI Agent** node → system message. Update:
- The company description (replace "business automation company" with your business)
- The qualification criteria — what types of companies are a good fit for you?
- The lead scoring criteria — what makes a lead Hot vs Cold for your business?

### Update the email content
- **Email SaaS Team / Email Agency Team** — update the subject line and body to match your team's style
- **Rejection Email** — personalise the rejection message with your name and contact details

### Adjust the qualification threshold
In the **Qualifier** node, the default is: `isQualified = true AND leadScore >= 6`. Change the score threshold to whatever fits your standards.

---

## Step 5 — Get your form URL

1. Activate the workflow in n8n
2. Open the **On form submission** node → copy the **Production URL**
3. Share this URL on your website, LinkedIn, or anywhere you want leads to come from

---

## How it works under the hood

- The AI scrapes the prospect's website automatically — no manual research needed
- Company type classification: **SaaS** (sells a software product) vs **Agency** (sells services) vs **Unknown**
- Qualified leads are routed to the right team email based on company type
- Unqualified leads never reach your inbox — they get a polite automated response
