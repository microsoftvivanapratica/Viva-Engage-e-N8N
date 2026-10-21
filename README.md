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

After adding 
