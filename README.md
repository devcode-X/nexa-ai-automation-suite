<p align="center">
  <img src="https://img.shields.io/badge/Workflows-25+-blue?style=for-the-badge" alt="Workflows">
  <img src="https://img.shields.io/badge/AI%20Powered-OpenAI%20GPT-blueviolet?style=for-the-badge" alt="AI Powered">
  <img src="https://img.shields.io/badge/Platforms-Make.com%20%7C%20n8n-orange?style=for-the-badge" alt="Platforms">
  <img src="https://img.shields.io/badge/Status-Production-brightgreen?style=for-the-badge" alt="Status">
</p>

# 🤖 Nexa AI Automation Suite

> **A complete, production-grade AI automation ecosystem that runs an entire consultancy's operations — from first lead touch to student enrollment — with zero manual intervention.**

This project is a collection of **25+ interconnected automation workflows** built for [Nexa Consultancy](https://nexaconsultancy.com), covering lead management, recruitment, student onboarding, partner security, and executive intelligence. Every workflow was originally built on **Make.com** and then **migrated to n8n** for full self-hosted control.

---

## 🧠 The Big Picture

Imagine a consultancy where:
- A lead fills out a form on Facebook → AI qualifies them in seconds → a personalized follow-up email goes out → the sales team gets a Slack ping with a priority score
- A candidate applies on Indeed → their application is parsed and logged → an interview is scheduled → reminders are sent automatically → a thank-you email goes out post-interview
- A student applies through an affiliate → they move through a 3-stage enrollment pipeline → counselors are assigned → approval emails fire automatically
- The CEO wakes up every morning to an **AI-generated briefing** summarizing everything that happened across all pipelines

**That's this project.** Zero manual data entry. Zero dropped leads. Zero forgotten follow-ups.

---

## 🏗️ Architecture

```
                        ┌─────────────────────────────────────────┐
                        │          NEXA AI AUTOMATION SUITE        │
                        └─────────────────┬───────────────────────┘
                                          │
            ┌─────────────────────────────┼─────────────────────────────┐
            │                             │                             │
   ┌────────▼────────┐          ┌────────▼────────┐          ┌────────▼────────┐
   │  LEAD PIPELINE  │          │ HIRING PIPELINE │          │STUDENT PIPELINE │
   │                 │          │                 │          │                 │
   │ • Capture       │          │ • Morning Scan  │          │ • Stage 1 Intake│
   │ • AI Qualify    │          │ • Reply Handler │          │ • Stage 2 Assign│
   │ • AI Score      │          │ • Scheduler     │          │ • Stage 3 Approve│
   │ • Follow-Up     │          │ • Reminders     │          │ • Portal Remind │
   │ • Call Summary  │          │ • Thank You     │          │ • Affiliate Sub │
   │ • Social Ads    │          │ • App Trigger   │          │                 │
   └────────┬────────┘          └────────┬────────┘          └────────┬────────┘
            │                             │                             │
            └─────────────────────────────┼─────────────────────────────┘
                                          │
            ┌─────────────────────────────┼─────────────────────────────┐
            │                             │                             │
   ┌────────▼────────┐          ┌────────▼────────┐          ┌────────▼────────┐
   │    SECURITY     │          │   OPERATIONS    │          │   EXECUTIVE     │
   │                 │          │                 │          │   INTELLIGENCE  │
   │ • New Device    │          │ • Affiliate     │          │                 │
   │   Login Alert   │          │   Signup Alert  │          │ • Daily AI      │
   │ • Password      │          │ • Content Ready │          │   Briefing      │
   │   Change Alert  │          │ • Multihexa     │          │   (CEO Report)  │
   │                 │          │   Launch        │          │                 │
   └─────────────────┘          └─────────────────┘          └─────────────────┘
```

---

## 📂 Project Structure

```
nexa-ai-automation-suite/
│
├── workflows/
│   ├── make.com/                    # Original Make.com blueprints
│   │   ├── lead-management/         # 6 workflows — AI-powered lead ops
│   │   ├── recruitment/             # 6 workflows — Indeed hiring pipeline
│   │   ├── student-pipeline/        # 5 workflows — College enrollment
│   │   ├── security/                # 2 workflows — Partner security alerts
│   │   └── operations/              # 5 workflows — Business operations
│   │
│   └── n8n/                         # Migrated n8n workflows (self-hosted)
│       ├── lead-management/         # 6 workflows
│       ├── recruitment/             # 5 workflows
│       ├── student-pipeline/        # 5 workflows
│       ├── security/                # 2 workflows
│       └── operations/              # 4 workflows
│
├── docs/
│   └── make-to-n8n-conversion-guide.md   # Complete migration playbook
│
├── data-schemas/
│   ├── Stage 1A_ Affiliate Submits Student.json
│   └── Stage 1A_ Affiliate Submits Student - Data Table.json
│
├── .gitignore
└── README.md
```

---

## 🔥 Workflow Catalog

### 1. 🎯 Lead Management & Sales (6 Workflows)

| Workflow | What It Does | Trigger | AI? |
|----------|-------------|---------|-----|
| **AI Lead Qualifier** | Receives lead data → GPT analyzes fit → categorizes as hot/warm/cold → updates CRM | Webhook | ✅ |
| **AI Lead Scoring** | Scores leads 0–100 based on budget, timeline, and fit → routes high-priority leads | Webhook | ✅ |
| **AI Call Summary** | Transcribes sales calls → extracts key points & action items → logs to Sheets | Webhook | ✅ |
| **AI Daily Follow-Up Engine** | Scans for unresponsive leads → generates personalized follow-up emails via GPT | Daily Schedule | ✅ |
| **Lead Capture & Follow-up** | Website form → validates → creates CRM contact → welcome email → follow-up sequence | Webhook | ❌ |
| **Facebook & Instagram Lead Ads** | Pulls leads from FB/IG ad campaigns → CRM → Sheets → Slack notification → welcome email | Facebook API | ❌ |

### 2. 👥 Recruitment & Hiring — Indeed Pipeline (6 Workflows)

| Workflow | What It Does | Trigger |
|----------|-------------|---------|
| **Morning Engine** | Scans overnight Indeed applications → parses candidates → daily digest to hiring manager | Daily Schedule |
| **Gmail Application Trigger** | Real-time trigger on new Indeed emails → extracts candidate info → logs → acknowledges | Gmail Trigger |
| **Candidate Reply Handler** | Monitors candidate replies → categorizes (interested/declined/question) → routes response | Gmail Trigger |
| **Interview Scheduler** | Generates time slots → sends calendar invite → updates tracking → notifies interviewer | Webhook |
| **Reminder Engine** | Checks for candidates awaiting response → sends follow-up reminders on overdue contacts | Daily Schedule |
| **Post-Interview Thank You** | Generates personalized thank-you emails after interviews → updates candidate status | Webhook |

### 3. 🎓 Student & College Pipeline (5 Workflows)

| Workflow | What It Does | Trigger |
|----------|-------------|---------|
| **College Stage 1 — Intake** | Processes initial student applications → validates → creates record → confirmation email | Webhook |
| **College Stage 2 — Assignment** | Assigns approved students to counselors/mentors → notifies both parties | Webhook |
| **Stage 3 — Application Approval** | Processes final approvals → sends acceptance email with next steps → triggers onboarding | Webhook |
| **Student Intake Form** | New student form submission → creates profile → sends portal credentials | Webhook |
| **Weekly Portal Login Reminder** | Sends Monday 12 PM reminders to all active students to log into learning portal | Weekly Cron |

### 4. 🔒 Security & Compliance (2 Workflows)

| Workflow | What It Does | Trigger |
|----------|-------------|---------|
| **Partner New Device Login** | Detects unrecognized device login → sends security alert with device fingerprint | Webhook |
| **Partner Password Changed** | Notifies partner immediately when password is changed → logs event with IP & timestamp | Webhook |

### 5. ⚙️ Operations & Executive Intelligence (5 Workflows)

| Workflow | What It Does | Trigger | AI? |
|----------|-------------|---------|-----|
| **Daily Karan Briefing** | Aggregates data from ALL pipelines → GPT generates executive summary → delivers to CEO | Daily Schedule | ✅ |
| **Affiliate Signup Alert** | New affiliate registration → welcome email → admin notification → Sheets logging | Webhook | ❌ |
| **Content Ready Notification** | Content marked complete → notifies stakeholders → updates status tracker | Webhook | ❌ |
| **Launch to Multihexa** | Orchestrates multi-channel product/service launch → notifications → tracking | Webhook | ❌ |

---

## 🛠️ Tech Stack & Integrations

| Technology | Role |
|-----------|------|
| **Make.com** | Original automation platform (24 blueprints) |
| **n8n** | Self-hosted automation platform (22 migrated workflows) |
| **OpenAI GPT** | Lead qualification, scoring, call summarization, follow-up generation, executive briefings |
| **Google Sheets** | Central data store across all workflows |
| **Gmail** | Email sending — notifications, follow-ups, alerts, acknowledgments |
| **Slack** | Real-time team notifications and alerts |
| **Facebook Lead Ads API** | Social media lead capture from FB & Instagram campaigns |
| **Google Calendar** | Interview scheduling |
| **Webhooks** | Real-time event triggers (20+ workflows) |
| **CRM API** | Contact management and lead tracking |

---

## 📊 Project Stats

| Metric | Value |
|--------|-------|
| Total Unique Workflows | **25** |
| Make.com Blueprints | **24** |
| n8n Workflows (Migrated) | **22** |
| AI-Powered Workflows | **6** |
| Scheduled Automations | **5** (daily/weekly) |
| Webhook-Triggered | **20+** |
| Integration Points | **10+ platforms** |
| Business Domains Covered | **5** |

---

## 🚀 Platform Migration: Make.com → n8n

All workflows were originally built on **Make.com** and subsequently **migrated to n8n** for self-hosted control, lower costs, and infrastructure ownership.

The complete migration methodology — including module mapping tables, expression syntax conversion, connection patterns, error handling strategies, and a step-by-step checklist — is documented in:

📄 **[`docs/make-to-n8n-conversion-guide.md`](docs/make-to-n8n-conversion-guide.md)**

**Why the migration?**
- 🏠 **Self-hosted** — No vendor lock-in, full data control
- 💰 **Cost-effective** — No per-operation pricing at scale
- 🔧 **Customizable** — Direct access to Node.js runtime for complex logic
- 🔐 **Security** — Credentials never leave your infrastructure

---

## ⚡ Quick Start

### Import Make.com Blueprints
1. Open [Make.com](https://www.make.com) → **Scenarios**
2. Click **Create a new scenario** → ⚙️ → **Import Blueprint**
3. Upload any `.blueprint.json` file from `workflows/make.com/`

### Import n8n Workflows
1. Open your n8n instance → **Workflows**
2. Click **⋮** → **Import from File**
3. Upload any `.json` file from `workflows/n8n/`
4. Configure credentials (Google Sheets, Gmail, Slack, OpenAI)

---

## 🔐 Security Note

All workflow files have credentials **redacted**. Before running any workflow:
1. Replace placeholder API keys with your own
2. Configure OAuth connections (Google, Facebook, Slack)
3. Set up OpenAI API keys for AI-powered workflows
4. Update webhook URLs to match your endpoints

---

## 📋 How the Workflows Connect — The Full Story

```
 MORNING                                                              
 ┌──────────────────────────────────────────────────────────────┐     
 │                    CEO DAILY BRIEFING                        │     
 │    Aggregates data from ALL pipelines below ↓               │     
 └──────────────────────────┬───────────────────────────────────┘     
                            │ pulls data from                         
     ┌──────────────────────┼──────────────────────┐                  
     ▼                      ▼                      ▼                  
 ┌────────┐          ┌────────────┐         ┌────────────┐            
 │ LEADS  │          │  HIRING    │         │ STUDENTS   │            
 │        │          │            │         │            │            
 │ FB/IG  │          │  Indeed    │         │ Affiliate  │            
 │ Ads ──►│          │  Apps ───► │         │ Submit ──► │            
 │        │          │            │         │            │            
 │ Website│          │  Morning   │         │ Stage 1    │            
 │ Form──►│          │  Engine ──►│         │ Intake ──► │            
 │        │          │            │         │            │            
 │ AI     │          │  Reply     │         │ Stage 2    │            
 │Qualify─┤          │  Handler──►│         │ Assign ──► │            
 │        │          │            │         │            │            
 │ AI     │          │ Interview  │         │ Stage 3    │            
 │Score──►│          │ Scheduler─►│         │ Approve──► │            
 │        │          │            │         │            │            
 │ Daily  │          │  Reminder  │         │ Weekly     │            
 │Follow  │          │  Engine ──►│         │ Reminder ──│            
 │  Up    │          │            │         │            │            
 │        │          │ Thank You  │         │            │            
 │ Call   │          │  Email     │         │            │            
 │Summary │          │            │         │            │            
 └────┬───┘          └─────┬──────┘         └─────┬──────┘            
      │                    │                      │                   
      └────────────────────┼──────────────────────┘                   
                           │                                          
                    ┌──────▼──────┐                                   
                    │  SECURITY   │                                   
                    │ • Login     │                                   
                    │   Alerts    │                                   
                    │ • Password  │                                   
                    │   Alerts    │                                   
                    └─────────────┘                                   
```

1. **Leads flow in** from Facebook/Instagram ads, website forms, and affiliate referrals
2. **AI qualifies and scores** every lead automatically — the sales team only touches hot leads
3. **Candidates apply** through Indeed — the system processes, tracks, schedules, reminds, and thanks them
4. **Students enroll** through a 3-stage pipeline — from application to counselor assignment to approval
5. **Security monitors** partner accounts for suspicious logins and password changes
6. **The CEO gets a daily AI briefing** that synthesizes metrics from every pipeline into one report

**Every step is automated. Every handoff is instant. Nothing falls through the cracks.**

---

## 🤝 Built By

**Devansh** — AI Automation Engineer

Built as a full-service AI automation engagement for Nexa Consultancy, covering end-to-end business process automation across sales, recruitment, education, security, and executive operations.

---

## 📄 License

This project is shared for **portfolio and educational purposes**. The workflows are templates — you'll need to configure your own API keys, credentials, and business logic to use them.

MIT License — see individual files for details.
