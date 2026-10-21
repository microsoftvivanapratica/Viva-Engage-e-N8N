# Viva Engage Weekly Summary with n8n and Microsoft Teams

> Automatically collect Viva Engage (Yammer) discussions, generate a weekly digest, and publish it directly to Microsoft Teams using n8n.

## 📌 Overview

This project demonstrates how to build a real-world Microsoft 365 automation using:

- Microsoft Entra ID
- Viva Engage (Yammer API)
- n8n
- Microsoft Teams
- OAuth 2.0

The solution retrieves Viva Engage messages, processes the content, generates a concise summary, and automatically posts the results into a Microsoft Teams channel.

This project can be used as:

- A portfolio project
- Internal communications automation
- Employee engagement monitoring
- Viva Engage analytics foundation
- Future AI-powered insights solution

---

# 🏗 Solution Architecture

```text
Viva Engage
     │
     ▼
Yammer API
     │
     ▼
n8n Workflow
     │
     ▼
Message Processing
     │
     ▼
Weekly Summary
     │
     ▼
Microsoft Teams
```

---

# 🚀 Features

✅ Connect to Viva Engage using OAuth2

✅ Retrieve messages from Viva Engage communities

✅ Filter messages from the last:

- 24 hours
- 7 days

✅ Remove empty posts

✅ Remove URLs and unnecessary content

✅ Generate a formatted summary

✅ Publish automatically to Microsoft Teams

✅ Fully automated using n8n

---

# 🛠 Technologies Used

- Microsoft Entra ID
- Viva Engage (Yammer API)
- Microsoft Teams
- n8n
- OAuth 2.0
- JavaScript

---

# 📋 Prerequisites

Before starting, ensure you have:

- Microsoft 365 Tenant
- Viva Engage enabled
- Microsoft Teams enabled
- Azure Subscription (optional)
- n8n instance running
- Global Administrator permissions (recommended)

---

# 🔐 Step 1 - Create an App Registration

Navigate to:

```text
Microsoft Entra ID
→ App Registrations
→ New Registration
```

Name:

```text
n8n Viva Engage Integration
```

Supported account type:

```text
Accounts in this organizational directory only
```

---

# 🔑 Step 2 - Create a Client Secret

Navigate to:

```text
Certificates & Secrets
→ New Client Secret
```

Copy the generated:

```text
Client Secret Value
```

⚠️ Store it securely.

---

# 🌐 Step 3 - Configure Redirect URI

Navigate to:

```text
Authentication
```

Add:

```text
Web
```

Redirect URL:

```text
https://oauth.n8n.cloud/oauth2/callback
```

For self-hosted n8n:

```text
https://YOUR-N8N-DOMAIN/rest/oauth2-credential/callback
```

---

# 🔒 Step 4 - Configure API Permissions

Navigate to:

```text
API Permissions
→ Add Permission
```

Add permissions for:

## Viva Engage / Yammer

```text
Community.Read.All
```

```text
EngagementConversation.Read.All
```

```text
engage_access
```

```text
access_as_user
```

After adding permissions:

```text
Grant Admin Consent
```

---

# ⚙️ Step 5 - Configure OAuth2 in n8n

Create a new credential:

```text
OAuth2 API
```

## Grant Type

```text
Authorization Code
```

## Authorization URL

```text
https://login.microsoftonline.com/YOUR-TENANT-ID/oauth2/v2.0/authorize
```

## Access Token URL

```text
https://login.microsoftonline.com/YOUR-TENANT-ID/oauth2/v2.0/token
```

## Client ID

Use the Application (Client) ID from the App Registration.

## Client Secret

Use the secret created earlier.

## Scope

```text
openid profile offline_access https://api.yammer.com/access_as_user
```

Click:

```text
Connect my account
```

and sign in with your Microsoft 365 account.

---

# 📡 Step 6 - Retrieve Viva Engage Messages

Add an HTTP Request node.

Configuration:

## Method

```text
GET
```

## URL

```text
https://api.yammer.com/api/v1/messages.json
```

## Authentication

```text
OAuth2 API
```

Select the credential created previously.

Run the node.

Expected response:

```json
{
  "messages": [],
  "references": [],
  "meta": {}
}
```

---

# 🧹 Step 7 - Process Messages

Add a Code node and use the following script.

## Weekly Summary

```javascript
const messages = $json.messages || [];

const now = new Date();

const oneWeekAgo = new Date();
oneWeekAgo.setDate(now.getDate() - 7);

const filteredMessages = messages
  .filter(m => m.body?.plain)
  .filter(m => m.body.plain.trim() !== '')
  .filter(m => new Date(m.created_at) >= oneWeekAgo);

const posts = filteredMessages.map((m, index) => {

  const cleanText = m.body.plain
    .replace(/https?:\/\/\S+/g, '')
    .replace(/\n/g, ' ')
    .trim();

  return `${index + 1}. ${cleanText}`;
});

const summary = `
📊 WEEKLY VIVA ENGAGE SUMMARY

📌 Total analyzed messages: ${posts.length}

📝 Highlights:

${posts.join('\n\n')}
`;

return [{
  summary
}];
```

---

# 📅 Daily Summary Version

```javascript
const messages = $json.messages || [];

const now = new Date();

const oneDayAgo = new Date();
oneDayAgo.setDate(now.getDate() - 1);

const filteredMessages = messages
  .filter(m => m.body?.plain)
  .filter(m => m.body.plain.trim() !== '')
  .filter(m => new Date(m.created_at) >= oneDayAgo);

const posts = filteredMessages.map((m, index) => {

  const cleanText = m.body.plain
    .replace(/https?:\/\/\S+/g, '')
    .replace(/\n/g, ' ')
    .trim();

  return `${index + 1}. ${cleanText}`;
});

const summary = `
📊 DAILY VIVA ENGAGE SUMMARY

📌 Total analyzed messages: ${posts.length}

📝 Highlights:

${posts.join('\n\n')}
`;

return [{
  summary
}];
```

---

# 💬 Step 8 - Post to Microsoft Teams

Add a Microsoft Teams node.

Action:

```text
Create Channel Message
```

Message:

```text
{{$json.summary}}
```

Example output:

```text
📊 WEEKLY VIVA ENGAGE SUMMARY

📌 Total analyzed messages: 12

📝 Highlights:

1. Welcome to the company!

2. Thanks for helping with the project!

3. New onboarding process is now available.
```

---

# ⏰ Step 9 - Automate Execution

Add a:

```text
Schedule Trigger
```

Example:

```text
Every Friday
5:00 PM
```

Workflow:

```text
Schedule Trigger
        │
        ▼
HTTP Request
(Viva Engage)
        │
        ▼
Code
(Process Messages)
        │
        ▼
Microsoft Teams
(Post Summary)
```

---

# 📂 Recommended Repository Structure

```text
viva-engage-weekly-summary
│
├── README.md
│
├── docs
│   ├── app-registration.md
│   ├── oauth-configuration.md
│   └── teams-integration.md
│
├── code
│   ├── daily-summary.js
│   └── weekly-summary.js
│
├── images
│   ├── architecture.png
│   ├── workflow.png
│   └── teams-result.png
│
└── LICENSE
```

---

# 🔮 Future Enhancements

- Azure OpenAI Integration
- Sentiment Analysis
- Topic Extraction
- Employee Feedback Categorization
- Risk Identification
- Engagement Scoring
- Power BI Dashboard
- Viva Engage Community Analytics
- Executive Summaries
- Adaptive Cards for Teams

---

# 🎯 Business Value

This solution helps organizations:

- Increase visibility into employee discussions
- Reduce manual monitoring effort
- Improve internal communications
- Promote knowledge sharing
- Surface relevant discussions automatically
- Create an employee experience analytics foundation

---

# 📜 License

MIT License

---

# 👤 Author

**Milena Almeida**

Microsoft 365 | Viva | Teams | Azure | Automation | AI Enthusiast

Built with ❤️ using Microsoft technologies and n8n.
