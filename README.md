🧠 AI → n8n Autonomous Workflow Generator
Fully automated DeepSeek-powered system that generates, validates, imports, versions, monitors, regenerates, and backs up n8n workflows every 48 hours — with zero human involvement.
________________________________________
🚀 What This System Does
This project automatically:
✔ Generates workflows using DeepSeek
Based on trending tech data (Hacker News RSS) every 48 hours.
✔ Validates & sanitizes generated JSON
Advanced schema validation + type normalization + fallback for invalid JSON.
✔ Imports workflows into n8n
Target instance: https://learnwithsathya.qzz.io
✔ Auto-versioning
Every generated workflow receives v1, v2, v3, … automatically.
✔ Backs up every workflow to GitHub
Stored safely in a dedicated branch: ai-backups
(Main branch stays clean.)
✔ Monitors executions & regenerates failures
If a generated workflow fails within the last 6 hours, a new version is generated and imported automatically.
✔ Logs activity to Google Sheets
Run summary, fallback usage, prune info, regeneration info, import response, backup path.
✔ Sends Telegram alerts on errors only
You get notified immediately when something breaks.
✔ Provides an n8n Dashboard workflow
Allows you to view:
•	Total workflows
•	Count of auto-generated workflows
•	Active/inactive breakdown
•	Recent versions
•	Creation timestamps
________________________________________
📁 Project Folder Structure
ai-n8n-autogen/
│
├── auto_ai_n8n_runner.py        # Main autonomous runner
├── fallback_workflow.json       # Safe workflow if LLM fails
├── n8n_dashboard_workflow.json  # For dashboard summary inside n8n
│
├── backups/                     # Empty in main; filled in ai-backups branch
│
├── .github/
│   └── workflows/
│       └── auto_ai_n8n.yml      # GitHub Actions pipeline (48h schedule)
│
├── requirements.txt             # Local dev dependencies
└── README.md                    # This file
Backups never pollute main — they are committed to ai-backups automatically.
________________________________________
🔧 Tech Stack
Component	Purpose
DeepSeek API	LLM generation of n8n workflow JSON
Python	Runner automation
n8n Public API	Import, deactivate, list workflows, list executions
Google Sheets API (service account)	Logging
Telegram Bot API	Error alerts
GitHub Actions	Automatic scheduling and backup commits
________________________________________
⚙️ How It Works (Architecture Overview)
1️⃣ GitHub Actions → Every 48 hours
The workflow runs:
•	Installs dependencies
•	Executes auto_ai_n8n_runner.py
•	Stores backup to backups/
•	Commits backup to ai-backups branch
2️⃣ Auto Runner Sequence
Step 1. Gather Trends
Fetch top Hacker News titles.
Step 2. DeepSeek Generation
Builds prompt using trends → DeepSeek generates workflow JSON.
Step 3. Advanced Validation
System fixes common LLM errors:
•	Missing typeVersion
•	Wrong node type names
•	Missing id on node
•	Unsupported top-level fields
•	Missing connections
•	Missing tags
Step 4. Fallback
If JSON still invalid → uses fallback_workflow.json.
Step 5. Versioning
System checks existing vN versions → increments automatically:
AI Trend Workflow v1
AI Trend Workflow v2
AI Trend Workflow v3
Step 6. Import to n8n
Using /workflows endpoint.
Step 7. Backup to GitHub
Saves imported workflow to:
ai-backups/
   20250101_034002__AI_Trend_v32.json
Commits automatically.
Step 8. Prune Old Workflows
Deletes tagged workflows older than PRUNE_DAYS=30.
Step 9. Detect Failed Executions
Scans /executions:
•	If a workflow tagged auto-generated-by-deepseek failed in last 6 hours → auto-regenerated and old version deactivated.
Step 10. Log Summary to Google Sheets
Includes:
•	Run time
•	Status
•	Trends used
•	Fallback used or not
•	Import result
•	Pruned workflows
•	Regeneration results
•	Backup path
Step 11. Telegram Alert (errors only)
Complete stack traces (trimmed automatically).
________________________________________
🧑‍💻 Local Development (VS Code)
1️⃣ Clone Repo
git clone https://github.com/<your-user>/ai-n8n-autogen.git
cd ai-n8n-autogen
2️⃣ Open in VS Code
code .
3️⃣ Install Dependencies
pip install -r requirements.txt
4️⃣ Create .env (ignored from Git)
DEEPSEEK_API_KEY=...
N8N_API_URL=https://learnwithsathya.qzz.io
N8N_API_KEY=...
GOOGLE_SERVICE_ACCOUNT_JSON=... (full json)
GOOGLE_SHEET_ID=...
TELEGRAM_BOT_TOKEN=...
TELEGRAM_CHAT=...
5️⃣ Run Locally
python auto_ai_n8n_runner.py
________________________________________
🔑 Required GitHub Secrets
Name	Description
DEEPSEEK_API_KEY	DeepSeek API key
N8N_API_URL	Your n8n instance URL
N8N_API_KEY	(Optional) n8n API access token
GOOGLE_SERVICE_ACCOUNT_JSON	Full JSON string
GOOGLE_SHEET_ID	ID from Google Sheets URL
TELEGRAM_BOT_TOKEN	Bot token from BotFather
TELEGRAM_CHAT	Chat ID or @username
________________________________________
🎛 n8n Dashboard Workflow
Import n8n_dashboard_workflow.json into n8n:
This displays:
•	Total workflows
•	Auto-generated workflow count
•	Active vs inactive
•	Recent versions with timestamps
A lightweight but effective “internal dashboard”.
________________________________________
🗂 Backup Strategy
✔ All imported workflows saved to backups/
✔ Auto-committed to ai-backups branch
✔ Clean separation from main code
✔ Perfect for diff, version comparison & rollback
To restore any workflow:
n8n → Import from File → Choose backup JSON
________________________________________
🧪 Regeneration-on-Failure
If a workflow generated by the system fails execution within last 6 hours:
1.	System generates a newer version automatically
2.	Imports it
3.	Deactivates the failing workflow
4.	Logs everything in Google Sheets
This creates a self-healing automation system.
________________________________________
🛡 Security Checklist
•	Never commit API keys — GitHub Secrets only
•	Rotate tokens every 6–12 months
•	Service account must have only Sheets access
•	Workflow backups contain no credentials (placeholders only)
•	Telegram warnings protect you from silent failures
________________________________________
🐞 Troubleshooting
Workflow import failing?
Check GitHub Action logs → import failed: 4xx
Google Sheet not updated?
Check:
•	GOOGLE_SERVICE_ACCOUNT_JSON valid
•	Service account has Editor permission on the sheet
No telegram alerts?
Ensure bot is added to your chat.
Backups not appearing?
Check ai-backups branch exists and has commits.
Dashboard empty?
Make sure the HTTP Request node has valid API credentials.
________________________________________
🤝 Want More Enhancements?
I can add:
•	🔍 Deep n8n-node-type schema validator
•	📊 GitHub Pages public dashboard
•	🧵 Auto link PR → regenerate workflow
•	🔄 Slack & Email integrations
•	🖥 CLI control panel
•	🧩 Pluggable LLMs (DeepSeek + GPT + Claude + Grok)
Just ask.
________________________________________
🎉 You’re all set
You now have a TRUE autonomous AI system generating self-correcting, versioned n8n workflows — with backups, monitoring, pruning, dashboards, and error alerting.

