# 📊 n8n Annotation Task Tracker

> **Automate task discovery across 20+ AI annotation platforms. Get urgent opportunities first.**

A production-ready n8n workflow that aggregates task notifications from multiple annotation/eval platforms, logs them to Google Sheets, and sends you smart summaries with AI-powered urgency detection.

## The Problem

You're working with **AfterQuery, Handshake, Scale AI, Outlier, Turing, Braintrust, Micro1** — the list goes on. Every platform sends task notifications to different inboxes, using different formats. By the time you see that "limited spots left" email, 10 other people already grabbed the good tasks.

You need a single source of truth. **Fast.**

## The Solution

This workflow runs every 30 minutes and:
- 🔍 **Searches Gmail** for new task emails from all your annotation platforms
- 📝 **Parses & structures** subject, snippet, platform, and timestamp
- 📊 **Logs to a Google Sheet** for historical tracking
- 🚨 **Flags urgent tasks** (job offers, interviews, assessments, payment alerts, tight deadlines)
- 📬 **Sends you a summary email** with new tasks and urgent items highlighted

No manual work. No missed opportunities.

---

## Features

✅ **20+ Platform Support** — AfterQuery, Handshake, Scale AI, Outlier, Turing, Pasiflora, Braintrust, Hume AI, Labelbox, Surge AI, DataAnnotation, Rationale, Anthropic, OpenAI, Micro1, Astoria AI, Fleet AI, Remotasks, Appen, and more  
✅ **Smart Urgency Detection** — Regex-based keyword matching for job offers, interviews, assessments, payments, and tight deadlines  
✅ **Google Sheets Integration** — Persistent log of all tracked tasks (sortable, filterable, searchable)  
✅ **Email Summaries** — Daily/hourly digest with platform breakdown and urgent flagging  
✅ **Customizable Keywords** — Easily adjust what counts as "urgent" for your workflow  
✅ **No Code Required** — Just import, authenticate, and run  
✅ **Localhost-Friendly** — Works perfectly on self-hosted n8n (Windows, Mac, Linux, Docker)

---

## Quick Start

### Prerequisites
- n8n running locally (`http://localhost:5678`)
- Google account (personal or Workspace)
- 5 minutes to set up OAuth

### Installation

**Step 1: Import the Workflow**
1. Download `workflow.json` from this repo
2. In n8n: **Workflows → Import from File**
3. Select the JSON file → confirm

**Step 2: Set Up Google OAuth**

Follow [docs/setup-guide.md](docs/setup-guide.md) for detailed instructions, or:

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a new project → enable **Gmail API** + **Google Sheets API**
3. **APIs & Services → Credentials → Create OAuth2 Client ID (Web application)**
4. Add redirect URI: `http://localhost:5678/rest/oauth2-credential/callback`
5. Copy Client ID + Client Secret

**Step 3: Connect Credentials in n8n**
1. Open the **Gmail search node** → add new credential
2. Paste Client ID + Secret → click **Sign in with Google**
3. Repeat for **Google Sheets node** (can reuse same credentials)

**Step 4: Create Your Tracker Sheet**
1. Go to [sheets.google.com](https://sheets.google.com) → **Create**
2. Rename the first sheet to `Tracker`
3. Add headers in row 1: `Platform | Subject | Snippet | Received At | Urgent | Status`
4. Copy the Sheet ID from the URL: `docs.google.com/spreadsheets/d/{SHEET_ID}/edit`

**Step 5: Update the Workflow**
1. Click **"Log Task To Tracker Sheet"** node
2. Paste your Sheet ID in the **Document ID** field
3. Save

**Step 6: Activate & Go**
1. Click **Activate** (top right)
2. Workflow runs every 30 minutes automatically
3. Check your email for summaries

Done. You're now tracking all your annotation tasks in one place.

---

## How It Works

```
Every 30 Minutes
     ↓
Search Gmail (past 1 hour, unread emails from 20+ platforms)
     ↓
Parse Email Content (extract platform, subject, snippet, timestamp)
     ↓
Log to Google Sheets (persistent historical record)
     ↓
Check Urgency (regex scan for keywords: job offer, interview, assessment, deadline, etc.)
     ↓
Aggregate Results (combine all new tasks from this run)
     ↓
Send Email Summary (to your inbox with urgent items flagged)
```

### Example Output

**Email Notification:**
```
3 new annotation tasks found this run

• AfterQuery: CAD Blueprint QA — 50 spots (URGENT)
• Braintrust: Product Image Evaluation
• Scale AI: LLM Behavior Assessment — $18/hr (URGENT)

Full log in your tracker sheet.
```

**Tracker Sheet:**
| Platform | Subject | Snippet | Received At | Urgent | Status |
|----------|---------|---------|-------------|--------|--------|
| AfterQuery | CAD Blueprint QA | Empty spots closing... | 2026-07-02 14:20 | true | New |
| Braintrust | Product Image Eval | Please evaluate product... | 2026-07-02 14:15 | false | New |
| Scale AI | LLM Behavior Assessment | $18/hr, assess model... | 2026-07-02 14:10 | true | New |

---

## Customization

### Add More Platforms
Edit the **"Parse Task Emails"** code node, add to the `platformMap` array:

```javascript
{ match: /yourplatform\.com|yourplatform/i, name: 'Your Platform' }
```

### Adjust Urgency Keywords
Edit the `urgentKeywords` regex in the same node:

```javascript
const urgentKeywords = /urgent|deadline|YOUR_KEYWORD_HERE/i;
```

### Change Frequency
Click **"Every 30 Minutes"** trigger → adjust interval (5 min, 1 hour, etc.)

### Email Recipient
Click **"Send Summary Notification"** → change `sendTo` field

### Filter by Platform
Edit the Gmail search query in **"Search Gmail For New Tasks"** to only monitor specific platforms

See [docs/customization.md](docs/customization.md) for more examples.

---

## Tech Stack

- **n8n** — Workflow automation platform
- **Gmail API** — Email search & parsing
- **Google Sheets API** — Task logging & persistence
- **JavaScript (ES6)** — Email parsing logic
- **Regex** — Urgency detection

---

## Troubleshooting

### "Tracker sheet not found"
→ Make sure your Google Sheet has a tab named **Tracker** (exact spelling)

### "Redirect URI mismatch"
→ Copy `http://localhost:5678/rest/oauth2-credential/callback` exactly (including `http://` and port)

### "Test user limit exceeded"
→ Add your email to **OAuth consent screen → Audience → Test users**

### Tokens expiring every 7 days
→ Normal for testing-mode apps. Reconnect in n8n credentials, or publish your OAuth app (optional)

See [docs/troubleshooting.md](docs/troubleshooting.md) for more.

---

## Why This Matters

**For Annotation Workers:**
- Stop context-switching between 10 inboxes
- Never miss a high-paying task again
- Track your annotation workload in one place
- Prioritize urgent opportunities automatically

**For Portfolio:**
- Shows practical API integration (Gmail + Sheets)
- Demonstrates real-world problem-solving
- Clean workflow design & documentation
- Self-hosted automation chops

---

## License

MIT — Use freely, fork it, build on it.

---

## Questions?

- Check [docs/](docs/) for detailed guides
- Open an issue on GitHub
- See what's broken, fix it, ship it

---

**Built with n8n, Google APIs, and JavaScript. Tested on localhost. Works on Windows, Mac, Linux, Docker.**

Made for people juggling multiple annotation platforms. If you are, this workflow is for you.
