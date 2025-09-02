**AI Chatbot (n8n + HubSpot + Google Sheets + Microsoft 365)**

An end-to-end chatbot workflow built in n8n that:

Chats with website visitors,

Collects Name/Email/Phone,

Checks for existing leads in HubSpot, then creates/updates contacts,

Logs every lead into Google Sheets,

Exports the sheet to CSV and backs it up to OneDrive,

Sends lead notifications via Outlook,

Fetches invoice details from HubSpot when a visitor enters an invoice number (e.g. INV-1234).

**📸 Demo & Screenshots**

Workflow overview
assets/Aichatbot_n8n_workflow.png

Chat interaction (Contact Sales flow)
assets/chat_interaction.png

HubSpot contact results
assets/hubspot_results.png

**🧱 Architecture (High level)**
Web Chat (n8n Chat Trigger)
      ↓
AI Agent ──► Branch: Invoice Number? (INV-xxxx)
      │
      ├─ Yes → Invoice lookup in HubSpot → Return status (amount, PDF link, status)
      │
      └─ No  → Collect Name → Email → Phone
                   ↓
            HubSpot Contact Upsert
                   ↓
             Google Sheet Append
                   ↓
          Export CSV → OneDrive
                   ↓
         Outlook Email Notification
**🗂 Repo Structure (suggested)**

├─ README.md
├─ workflow/
│  └─ aichatbot.json                 # n8n export
├─ assets/
│  ├─ Aichatbot_n8n_workflow.png
│  ├─ chat_interaction.png
│  └─ hubspot_results.png
└─ data/
   └─ Leadinfo(Results).xlsx         # sample output (optional)
**🚀 Quickstart**
Install n8n (Docker, npm, or desktop app).

Clone this repo and open n8n.

Import workflow/aichatbot.json.

Configure credentials in n8n: OpenAI, HubSpot, Google Sheets, Outlook, OneDrive.

Update IDs: Sheet ID/gid, OneDrive folder, HubSpot properties, Outlook email.

Activate and test via the Chat Trigger.

**🔧 Node Guide**
Chat Trigger – Starts the flow.

AI Agent – Collects user details OR detects invoice number pattern.

If / If1 – Routes invoice queries separately.

Invoice branch

Edit Fields → HubSpot Invoices Search (HTTP) → Format invoice result

Returns: Invoice ID, amount, status, and PDF link

Lead branch

Extract data → Check HubSpot contacts → Create/Update contact → Append to Google Sheet → Export CSV → Upload to OneDrive → Send Outlook email

**🧪 Testing**

Lead flow

Start chat → choose “Contact Sales”

Enter name, email, phone

Confirm new contact in HubSpot + updated sheet + CSV in OneDrive + email received

Invoice flow

Start chat → type INV-1234

Bot queries HubSpot and returns invoice details:

Invoice ID

Amount

Status

PDF link

**📝 Customization**

Add email/phone validation regex.

Match leads by email instead of name.

Extend invoice lookup with payment due dates.

Localize prompts for multilingual support.

Send Slack/Teams notifications.

**⚠️ Troubleshooting**

Invoice not found → Ensure HubSpot API scopes allow invoice search.

No row in Sheets → Check OAuth + Sheet ID.

Duplicate contacts → Normalize emails before comparison.

CSV missing → Verify OneDrive folder ID.

No email → Check Outlook OAuth + recipient config.

📄 License

MIT License

**✍️ Author**

Pavan
