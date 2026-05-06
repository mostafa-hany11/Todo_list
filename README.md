# ✅ ToDoApp — OutSystems ODC Full-Stack Task Management System

<div align="center">

![OutSystems ODC](https://img.shields.io/badge/OutSystems-ODC-red?style=for-the-badge&logo=outsystems&logoColor=white)
![REST API](https://img.shields.io/badge/REST-API-blue?style=for-the-badge)
![SendGrid](https://img.shields.io/badge/SendGrid-Email-009BDE?style=for-the-badge&logo=sendgrid&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FCM-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Google Calendar](https://img.shields.io/badge/Google-Calendar-4285F4?style=for-the-badge&logo=googlecalendar&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**A production-grade, full-stack To-Do application built on OutSystems Developer Cloud (ODC)**  
featuring a fully documented REST API, third-party integrations, OAuth 2.0, push notifications, and scheduled reminders.

[📖 API Docs](#-api-reference) · [🚀 Getting Started](#-getting-started) · [🏗️ Architecture](#️-architecture) · [🔌 Integrations](#-third-party-integrations) · [🤝 Contributing](#-contributing)

</div>

---

## 📋 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [Architecture](#️-architecture)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [ODC Environment Setup](#odc-environment-setup)
  - [Configuration & Secrets](#configuration--secrets)
- [API Reference](#-api-reference)
  - [Authentication](#authentication)
  - [Task Endpoints](#task-endpoints)
- [Third-Party Integrations](#-third-party-integrations)
  - [SendGrid (Email)](#sendgrid-email)
  - [Firebase FCM (Push Notifications)](#firebase-fcm-push-notifications)
  - [Google Calendar (Sync)](#google-calendar-sync)
- [Database Schema](#-database-schema)
- [Project Structure](#-project-structure)
- [Deployment](#-deployment)
- [Testing](#-testing)
- [Known Issues & Limitations](#-known-issues--limitations)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 🧩 About the Project

ToDoApp is a fully functional task management system built entirely on **OutSystems Developer Cloud (ODC)** — the cloud-native, container-based evolution of the OutSystems platform. This project was designed to demonstrate how ODC handles enterprise-grade concerns like:

- Clean **multi-app/library architecture** following the ODC 4-Layer Canvas
- A **secure, token-based REST API** consumable by any frontend or mobile client
- **Real-world third-party integrations** using both the ODC REST wizard and .NET External Libraries
- **OAuth 2.0** flows for Google Calendar synchronization
- **Scheduled background processing** via ODC Timers

This is not a tutorial project — it is structured the way a real team would build and maintain it in production.

---

## ✨ Features

- **Task Management** — Create, read, update, delete, and toggle completion on tasks
- **Due Date Tracking** — Assign due dates with automatic reminders 24 hours before
- **Priority System** — Three-level priority (High / Medium / Low) with visual cues
- **Token-Based REST API** — Full CRUD API secured with Bearer token authentication
- **Email Reminders** — Automated due date notifications via SendGrid
- **Push Notifications** — Mobile/web push alerts via Firebase Cloud Messaging (FCM)
- **Google Calendar Sync** — Tasks with due dates automatically create Google Calendar events
- **OAuth 2.0** — Full Google authorization flow with access token refresh handling
- **Rate Limiting** — Per-token request throttling (100 req/hour default)
- **Pagination** — All list endpoints support page/pageSize query parameters
- **Centralized Error Logging** — Every integration failure is logged with severity, module, and timestamp
- **Swagger Documentation** — Auto-generated OpenAPI spec at `/rest/TaskAPI/swagger.json`

---

## 🏗️ Architecture

This project follows the **ODC 4-Layer Canvas** pattern, split across four separate ODC apps/libraries:

```
┌─────────────────────────────────────────────────────────────┐
│                        ODC PORTAL                           │
│                                                             │
│   ┌─────────────────┐      ┌────────────────────────────┐   │
│   │  ToDoCore (Lib) │      │  ToDoIntegrations (Lib)    │   │
│   │                 │      │                            │   │
│   │ • Task Entity   │      │ • SendGrid REST consumer   │   │
│   │ • APIToken      │      │ • Firebase REST consumer   │   │
│   │ • CRUD Actions  │      │ • Google Calendar (ExtLib) │   │
│   │ • Exceptions    │      │ • OAuth token manager      │   │
│   │ • Structures    │      │ • Email/Push wrappers      │   │
│   └────────┬────────┘      └─────────────┬──────────────┘   │
│            └──────────────┬──────────────┘                  │
│                           │                                 │
│          ┌────────────────▼──────────────┐                  │
│          │         ToDoAPI (App)         │                  │
│          │                               │                  │
│          │  • Exposed REST API           │                  │
│          │  • Token auth + rate limit    │                  │
│          │  • All HTTP endpoints         │                  │
│          │  • ODC Timers (reminders)     │                  │
│          └───────────────────────────────┘                  │
│                                                             │
│          ┌───────────────────────────────┐                  │
│          │         ToDoWeb (App)         │                  │
│          │                               │                  │
│          │  • Reactive Web screens       │                  │
│          │  • Built-in ODC identity      │                  │
│          │  • Google OAuth callback      │                  │
│          └───────────────────────────────┘                  │
└─────────────────────────────────────────────────────────────┘
```

### Why This Structure?

| Concern | Where It Lives | Why |
|---|---|---|
| Database entities | `ToDoCore` | Shared across all apps without duplication |
| External API calls | `ToDoIntegrations` | Swap providers without touching business logic |
| HTTP endpoints | `ToDoAPI` | Isolated — can be scaled/versioned independently |
| User interface | `ToDoWeb` | Decoupled from API; could be replaced with React/mobile |

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Platform** | OutSystems Developer Cloud (ODC) |
| **IDE** | ODC Service Studio (latest) |
| **Runtime** | Containerized .NET / ODC Cloud |
| **Database** | ODC-managed Aurora PostgreSQL |
| **API Style** | REST (JSON, ISO 8601 dates) |
| **Authentication** | Bearer Token (custom) + ODC built-in OIDC |
| **Email** | SendGrid v3 API |
| **Push Notifications** | Firebase Cloud Messaging (FCM) |
| **Calendar** | Google Calendar API v3 |
| **External Library** | .NET 8 + `Google.Apis.Calendar.v3` NuGet |
| **Auth Flow** | OAuth 2.0 Authorization Code + PKCE |
| **Docs** | Swagger / OpenAPI (auto-generated by ODC) |

---

## 🚀 Getting Started

### Prerequisites

Before cloning or importing this project, make sure you have:

- [ ] An **OutSystems ODC account** — [Register free here](https://www.outsystems.com/ODC)
- [ ] **ODC Service Studio** installed on your machine
- [ ] A **SendGrid account** with an API key — [sendgrid.com](https://sendgrid.com)
- [ ] A **Firebase project** with Cloud Messaging enabled — [console.firebase.google.com](https://console.firebase.google.com)
- [ ] A **Google Cloud project** with the Calendar API enabled and OAuth 2.0 credentials created
- [ ] **.NET 8 SDK** (only needed if you modify the External Library)
- [ ] **Postman** (recommended for API testing)

---

### ODC Environment Setup

#### Step 1 — Import the Applications

> **Note:** OutSystems ODC does not use `.osp` packages like O11. Source is managed via ODC Portal directly. Each app/library listed below must be created manually in your ODC organization and configured using the steps in this guide.

1. Log in to your **ODC Portal** at `https://your-org.outsystems.app`.
2. Click **Create App** → **Library** → name it exactly `ToDoCore`.
3. Click **Create App** → **Library** → name it exactly `ToDoIntegrations`.
4. Click **Create App** → **Web App** → name it exactly `ToDoAPI`.
5. Click **Create App** → **Web App** → name it exactly `ToDoWeb`.

#### Step 2 — Set Up Dependencies

Open each app in ODC Service Studio and wire the references:

| App / Library | Depends On |
|---|---|
| `ToDoIntegrations` | `ToDoCore` |
| `ToDoAPI` | `ToDoCore`, `ToDoIntegrations` |
| `ToDoWeb` | `ToDoCore`, `ToDoIntegrations` |

In Service Studio: **Dependencies** tab → search for the library → add all public elements.

#### Step 3 — Upload the External Library

The Google Calendar integration requires a pre-compiled .NET External Library:

1. In **ODC Portal** → navigate to **External Libraries**.
2. Click **Upload** → select `ToDoApp.GoogleCalendar.zip` from the `/external-library` folder in this repo.
3. ODC validates and registers it automatically.
4. In `ToDoIntegrations` → **Dependencies** → add the `ICalendarIntegration` reference.

---

### Configuration & Secrets

All sensitive values are managed as **ODC Settings** — never hardcoded. Configure them in ODC Portal per deployment stage.

#### Navigate to Settings

ODC Portal → select `ToDoIntegrations` library → **Configuration** tab → **Settings**

#### Required Settings

| Setting Name | Type | Secret | Where to Get It |
|---|---|---|---|
| `SendGrid_APIKey` | Text | ✅ Yes | SendGrid Dashboard → API Keys |
| `SendGrid_SenderEmail` | Text | No | Your verified sender email |
| `Firebase_ServerKey` | Text | ✅ Yes | Firebase Console → Project Settings → Cloud Messaging |
| `Firebase_ProjectId` | Text | No | Firebase Console → Project Settings |
| `GoogleCalendar_ClientId` | Text | No | Google Cloud Console → Credentials |
| `GoogleCalendar_ClientSecret` | Text | ✅ Yes | Google Cloud Console → Credentials |
| `API_RateLimitPerHour` | Integer | No | Default: `100` |
| `JWT_SecretKey` | Text | ✅ Yes | Any strong random string (min 32 chars) |

> ⚠️ **Secret settings** are encrypted at rest by ODC and never appear in logs or exports. Always mark API keys and secrets as Secret.

#### Google OAuth Redirect URI

Add this URI to your Google Cloud Console → OAuth 2.0 Credentials → Authorized redirect URIs:

```
https://your-org.outsystems.app/ToDoWeb/GoogleOAuthCallback
```

---

## 📡 API Reference

**Base URL:**
```
https://your-org.outsystems.app/ToDoAPI/rest/TaskAPI
```

**Swagger UI:**
```
https://your-org.outsystems.app/ToDoAPI/rest/TaskAPI/swagger.json
```

### Authentication

All endpoints except `/auth/token` require a Bearer token in the Authorization header.

```http
Authorization: Bearer <your_token>
```

#### POST /auth/token

Authenticate and receive an API token.

**Request:**
```http
POST /auth/token
Content-Type: application/json

{
  "Username": "user@example.com",
  "Password": "yourpassword",
  "Label": "My Mobile App"
}
```

**Response `200 OK`:**
```json
{
  "Success": true,
  "Data": {
    "Token": "eyJhbGciOiJIUzI1...",
    "ExpiresAt": "2025-08-10T09:00:00Z",
    "Label": "My Mobile App"
  },
  "Error": null,
  "Meta": {
    "RequestId": "f3a1b2c4-...",
    "Timestamp": "2025-07-10T09:00:00Z"
  }
}
```

**Error Responses:**

| Status | Code | Meaning |
|---|---|---|
| `401` | `INVALID_CREDENTIALS` | Wrong username or password |
| `400` | `VALIDATION_ERROR` | Missing required fields |

---

### Task Endpoints

All task responses follow this envelope structure:

```json
{
  "Success": true | false,
  "Data": { ... },
  "Error": { "Code": "...", "Message": "..." },
  "Meta": {
    "Page": 1,
    "PageSize": 20,
    "TotalCount": 47,
    "RequestId": "uuid",
    "Timestamp": "ISO8601"
  }
}
```

---

#### GET /tasks

Retrieve a paginated list of tasks.

**Query Parameters:**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `ShowCompleted` | Boolean | `false` | Include completed tasks |
| `Page` | Integer | `1` | Page number |
| `PageSize` | Integer | `20` | Items per page (max 100) |
| `SortBy` | Text | `CreatedAt` | Field to sort by |
| `SortOrder` | Text | `DESC` | `ASC` or `DESC` |

**Example Request:**
```http
GET /tasks?ShowCompleted=false&Page=1&PageSize=10
Authorization: Bearer <token>
```

**Response `200 OK`:**
```json
{
  "Success": true,
  "Data": {
    "Tasks": [
      {
        "Id": 1,
        "Description": "Review PR for sprint 3",
        "IsCompleted": false,
        "DueDate": "2025-07-15",
        "Priority": 1,
        "CreatedAt": "2025-07-10T08:30:00Z"
      }
    ]
  },
  "Meta": {
    "Page": 1,
    "PageSize": 10,
    "TotalCount": 34
  }
}
```

---

#### POST /tasks

Create a new task.

**Request Body:**
```json
{
  "Description": "Finish the README",
  "DueDate": "2025-07-12",
  "Priority": 1
}
```

| Field | Type | Required | Constraints |
|---|---|---|---|
| `Description` | Text | ✅ | 1–500 characters |
| `DueDate` | Date | ❌ | ISO 8601 (`YYYY-MM-DD`) |
| `Priority` | Integer | ❌ | `1`=High, `2`=Medium, `3`=Low |

**Response `201 Created`:**
```json
{
  "Success": true,
  "Data": {
    "Task": {
      "Id": 42,
      "Description": "Finish the README",
      "IsCompleted": false,
      "DueDate": "2025-07-12",
      "Priority": 1,
      "CreatedAt": "2025-07-10T10:15:00Z"
    }
  }
}
```

---

#### PUT /tasks/{id}

Update an existing task (full or partial update).

```http
PUT /tasks/42
Authorization: Bearer <token>
Content-Type: application/json

{
  "Description": "Finish the README and push to GitHub",
  "Priority": 2
}
```

> Fields not sent retain their existing values — no need to send the full object.

**Response:** `200 OK` with updated task object.

---

#### PATCH /tasks/{id}/complete

Toggle the completion status of a task.

```http
PATCH /tasks/42/complete
Authorization: Bearer <token>
```

**Response `200 OK`:**
```json
{
  "Success": true,
  "Data": {
    "TaskId": 42,
    "IsCompleted": true,
    "Message": "Task completed!"
  }
}
```

---

#### DELETE /tasks/{id}

Delete a task permanently.

```http
DELETE /tasks/42
Authorization: Bearer <token>
```

> Deleting an **incomplete** task requires the confirmation header:
> ```http
> X-Force-Delete: true
> ```

| Status | Meaning |
|---|---|
| `204 No Content` | Successfully deleted |
| `404 Not Found` | Task does not exist |
| `409 Conflict` | Task is incomplete — send `X-Force-Delete: true` to confirm |

---

#### POST /tokens/{id}/revoke

Revoke an API token immediately.

```http
POST /tokens/7/revoke
Authorization: Bearer <token>
```

**Response:** `200 OK` — token is invalidated and all future requests with it return `401`.

---

## 🔌 Third-Party Integrations

### SendGrid (Email)

**Trigger:** Automatically sends an HTML reminder email 24 hours before a task's due date.

**Configuration:** Set `SendGrid_APIKey` and `SendGrid_SenderEmail` in ODC Portal settings.

**How it works:**
1. The `ProcessDueDateReminders` timer runs daily at 09:00 UTC.
2. It queries all incomplete tasks due the next day.
3. For each task, it calls `Email_SendTaskReminder` in `ToDoIntegrations`.
4. The action builds an HTML email body and calls the SendGrid v3 `/mail/send` endpoint.
5. Failures are logged silently — email errors never affect task operations.

---

### Firebase FCM (Push Notifications)

**Trigger:** Sends a push notification alongside the email reminder.

**Configuration:** Set `Firebase_ServerKey` and `Firebase_ProjectId` in ODC Portal settings.

**Device Token Registration:**

To receive push notifications, register a device token via:

```http
POST /devices/register
Authorization: Bearer <token>
Content-Type: application/json

{
  "DeviceToken": "fcm_device_token_here",
  "Platform": "android"
}
```

**Platforms supported:** `android`, `ios`, `web`

---

### Google Calendar (Sync)

**Trigger:** When a task with a `DueDate` is created, a corresponding all-day Google Calendar event is created automatically.

**OAuth Flow:**

1. The user initiates connection from the web app → redirected to Google's consent screen.
2. After approval, Google redirects back to your ODC app with an authorization code.
3. The app exchanges the code for access + refresh tokens (stored encrypted in DB).
4. The `OAuth_GetValidAccessToken` action handles silent token refresh before every Calendar API call.

**Google Cloud Console Setup:**

```
APIs & Services → Enable:
  ✅ Google Calendar API

Credentials → OAuth 2.0 Client ID:
  Application type: Web application
  Authorized redirect URI: https://your-org.outsystems.app/ToDoWeb/GoogleOAuthCallback
```

**Note:** If a user has not connected their Google account, task creation proceeds normally — the Calendar sync step is skipped without error.

---

## 🗄 Database Schema

```
┌───────────────────────────────────┐
│              Task                 │
├───────────────────────────────────┤
│ Id            Long Integer (PK)   │
│ Description   Text (500)          │
│ IsCompleted   Boolean             │
│ DueDate       Date                │
│ Priority      Integer (1/2/3)     │
│ CreatedAt     DateTime            │
│ GoogleEventId Text (200)          │
└───────────────────────────────────┘

┌───────────────────────────────────┐
│            APIToken               │
├───────────────────────────────────┤
│ Id            Long Integer (PK)   │
│ UserId        User Identifier     │
│ TokenHash     Text (500)          │
│ ExpiresAt     DateTime            │
│ IsRevoked     Boolean             │
│ Scope         Text (200)          │
│ Label         Text (100)          │
│ CreatedAt     DateTime            │
│ LastUsedAt    DateTime            │
└───────────────────────────────────┘

┌───────────────────────────────────┐
│          UserOAuthToken           │
├───────────────────────────────────┤
│ Id            Long Integer (PK)   │
│ UserId        User Identifier     │
│ Provider      Text (50)           │
│ AccessToken   Text (2000)         │
│ RefreshToken  Text (2000)         │
│ ExpiresAt     DateTime            │
│ Scope         Text (500)          │
└───────────────────────────────────┘

┌───────────────────────────────────┐
│           DeviceToken             │
├───────────────────────────────────┤
│ Id            Long Integer (PK)   │
│ UserId        User Identifier     │
│ Token         Text (500)          │
│ Platform      Text (10)           │
│ RegisteredAt  DateTime            │
│ IsActive      Boolean             │
└───────────────────────────────────┘

┌───────────────────────────────────┐
│            ErrorLog               │
├───────────────────────────────────┤
│ Id            Long Integer (PK)   │
│ Module        Text (100)          │
│ Action        Text (100)          │
│ ErrorMessage  Text (2000)         │
│ StackTrace    Text                │
│ UserId        User Identifier     │
│ Severity      Text (10)           │
│ OccurredAt    DateTime            │
└───────────────────────────────────┘
```

---

## 📁 Project Structure

```
ToDoApp/
│
├── ToDoCore/                        # Library — data & business logic
│   ├── Data/
│   │   └── Entities/
│   │       ├── Task
│   │       ├── APIToken
│   │       ├── UserOAuthToken
│   │       ├── DeviceToken
│   │       └── ErrorLog
│   └── Logic/
│       ├── ServerActions/
│       │   ├── Task/                # CRUD operations
│       │   ├── APIToken/            # Token generation & validation
│       │   └── Logging/             # LogEntry action
│       ├── Exceptions/
│       │   ├── UnauthorizedException
│       │   ├── NotFoundException
│       │   ├── ValidationException
│       │   ├── RateLimitException
│       │   └── IntegrationException
│       └── Structures/
│           ├── APIEnvelope
│           ├── APIError
│           ├── APIMeta
│           └── TaskResponse
│
├── ToDoIntegrations/                # Library — external API consumers
│   ├── Logic/
│   │   ├── REST/
│   │   │   ├── SendGrid             # Consumed REST API
│   │   │   └── FirebaseFCM          # Consumed REST API
│   │   ├── ServerActions/
│   │   │   ├── Email_SendTaskReminder
│   │   │   ├── Push_SendTaskReminder
│   │   │   ├── OAuth_GetAuthorizationURL
│   │   │   ├── OAuth_ExchangeCodeForToken
│   │   │   ├── OAuth_GetValidAccessToken
│   │   │   └── Calendar_SyncTask
│   │   └── ExternalLibraries/
│   │       └── ICalendarIntegration # Google Calendar .NET External Lib
│   └── Data/
│       └── Settings/                # All API keys & config
│
├── ToDoAPI/                         # App — REST API exposure
│   └── Logic/
│       ├── REST/
│       │   └── TaskAPI/             # Exposed REST API
│       │       ├── GetTasks
│       │       ├── CreateTask
│       │       ├── UpdateTask
│       │       ├── ToggleComplete
│       │       ├── DeleteTask
│       │       ├── AuthToken
│       │       └── RevokeToken
│       └── Timers/
│           └── ProcessDueDateReminders
│
├── ToDoWeb/                         # App — Reactive Web UI
│   └── Interface/
│       ├── MainFlow/
│       │   ├── Tasks (screen)
│       │   └── TaskDetail (screen)
│       └── AuthFlow/
│           └── GoogleOAuthCallback (screen)
│
└── external-library/               # .NET External Library source
    ├── ToDoApp.GoogleCalendar/
    │   ├── ICalendarIntegration.cs
    │   ├── CalendarIntegration.cs
    │   └── ToDoApp.GoogleCalendar.csproj
    └── ToDoApp.GoogleCalendar.zip  # Pre-built, ready to upload to ODC
```

---

## 🚢 Deployment

### Stage Promotion in ODC Portal

ODC manages deployment across stages (Development → QA → Production):

1. In **ODC Portal** → **Delivery** → **Deploy**.
2. Select the apps to promote: `ToDoCore` → `ToDoIntegrations` → `ToDoAPI` → `ToDoWeb` (in this order — dependencies first).
3. Before promoting to Production, verify that **all Settings** are configured for the target stage.
4. Click **Deploy** — ODC handles containerization and zero-downtime deployment automatically.

### Stage-Specific Settings

ODC settings are configured **per stage**. Never share API keys between Development and Production:

| Setting | Development | Production |
|---|---|---|
| `SendGrid_APIKey` | Test/sandbox key | Live key |
| `API_RateLimitPerHour` | `1000` (relaxed for testing) | `100` |
| `SendGrid_SenderEmail` | `dev@yourapp.com` | `noreply@yourapp.com` |

---

## 🧪 Testing

### Postman Collection

Import the collection from `/postman/ToDoApp.postman_collection.json`.

Set up your environment variables in Postman:

| Variable | Value |
|---|---|
| `base_url` | `https://your-org.outsystems.app/ToDoAPI/rest/TaskAPI` |
| `test_username` | Your ODC test user email |
| `test_password` | Your ODC test user password |

The collection includes a **pre-request script** that automatically fetches and caches the auth token — no manual copying needed.

### Test Coverage Checklist

```
Authentication
  ✅ Valid credentials → returns token
  ✅ Invalid credentials → 401
  ✅ Missing Authorization header → 401
  ✅ Expired token → 401
  ✅ Revoked token → 401

Tasks — Happy Path
  ✅ Create task (all fields)
  ✅ Create task (only required fields)
  ✅ Get tasks (default params)
  ✅ Get tasks (with pagination)
  ✅ Get tasks (ShowCompleted=true)
  ✅ Update task (partial)
  ✅ Toggle complete (false → true)
  ✅ Toggle complete (true → false)
  ✅ Delete completed task

Tasks — Error Cases
  ✅ Create with empty description → 400
  ✅ Create with description > 500 chars → 400
  ✅ Create with invalid priority → 400
  ✅ Get non-existent task → 404
  ✅ Update non-existent task → 404
  ✅ Delete incomplete task without X-Force-Delete → 409
  ✅ Delete non-existent task → 404

Rate Limiting
  ✅ 101st request in one hour → 429
```

### ODC Built-In REST Tester

For quick iteration without Postman:

1. Open any REST method in ODC Service Studio.
2. Click the **Test** tab on the right panel.
3. Fill in parameters and headers.
4. Click **Send** — response appears with full status code and headers.

---

## ⚠️ Known Issues & Limitations

| Issue | Status | Workaround |
|---|---|---|
| Google Calendar sync fails if user token expires during batch processing | 🔧 In Progress | Re-authenticate via `/google/connect` |
| Firebase device tokens are not automatically cleaned up on `401` responses | 📋 Planned | Manually call `DELETE /devices/{id}` |
| ODC timer runs in UTC — reminder time may differ from user's local timezone | 📋 Planned | Add UserTimezone to user profile |
| Rate limit window resets at top of hour, not rolling 60 minutes | 📋 Planned | Migrate to sliding window algorithm |

---

## 🛣 Roadmap

- [ ] **v1.1** — User timezone support for accurate reminder timing
- [ ] **v1.2** — Task sharing and collaboration (multi-user tasks)
- [ ] **v1.3** — Recurring tasks (daily / weekly / monthly)
- [ ] **v1.4** — File attachments per task (ODC Object Storage)
- [ ] **v2.0** — Native mobile app (ODC Mobile) with offline sync
- [ ] **v2.1** — Microsoft Teams / Slack integration for task notifications
- [ ] **v2.2** — AI-powered task prioritization via Anthropic Claude API

---

## 🤝 Contributing

Contributions are welcome. Please follow this workflow:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes following the architecture patterns described above.
4. Test all affected endpoints using the Postman collection.
5. Update this README if you add new endpoints, settings, or integrations.
6. Open a Pull Request with a clear description of what changed and why.

### Code Conventions

- All Server Actions must have `OnException` handling — no unhandled exceptions.
- External API calls must be wrapped in a dedicated action inside `ToDoIntegrations`.
- New API keys go into **ODC Settings** as Secret — never hardcoded.
- All new endpoints must follow the `APIEnvelope` response structure.
- Log significant events and all integration errors using the `LogEntry` action.

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for full details.

---

## 📬 Contact

**Mostafa Ahmed**
- GitHub: [@mostafa-hany11](https://github.com/mostafa-hany11)
- LinkedIn: [linkedin.com/in/yourprofile](www.linkedin.com/in/mostafa-ahmed-0b2493243)
- Email: nasefmostafa05@gmail.com

---

<div align="center">
  Built with ❤️ using OutSystems ODC · <a href="#-todoapp--outsystems-odc-full-stack-task-management-system">Back to top ↑</a>
</div>
