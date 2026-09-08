# 🚨 SIRENS

### Smart Incident Response & Emergency Notification System

> **An Agentic AI System for Real-Time Urban Crisis Detection, Planning, and Response**

SIRENS is an event-driven, multi-agent AI system designed to detect, verify, and coordinate responses to urban emergencies in real time.

Built for metropolitan cities in Pakistan, SIRENS combines **Google Gemini, Genkit, Firebase, Google Maps, real-time data sources, and citizen reports** to create an intelligent crisis-response workflow.

**Detect → Classify → Plan → Execute → Verify → Evaluate**

---

## 🎥 Demo

[![Watch Demo](https://img.shields.io/badge/▶_Watch_Demo-red?style=for-the-badge&logo=youtube)](YOUR_YOUTUBE_LINK_HERE)

> A short walkthrough of the complete SIRENS workflow, from incoming crisis signals to AI-driven response and citizen verification.

---

## 🔗 Links

| Resource | Link |
|---|---|
| 📱 Android App | Coming soon |
| 🖥️ Web Dashboard | Coming soon |
| 🎥 Demo Video | [Watch Demo](YOUR_YOUTUBE_LINK_HERE) |

---

# 🚨 The Problem

Pakistan's metropolitan cities face frequent localized crises such as:

- Urban flooding
- Heatwaves
- Road accidents
- Road blockages
- Fires
- Power outages
- Infrastructure failures

Emergency response is often:

- **Fragmented** — information is spread across different sources and agencies.
- **Reactive** — action begins after significant damage has already occurred.
- **Slow to coordinate** — manual processes delay emergency dispatch, traffic rerouting, and public communication.

At the same time, critical signals already exist across **weather APIs, news sources, maps, and citizen reports**.

The challenge is turning these scattered signals into **coordinated, actionable decisions in real time**.

---

# 💡 The Solution

SIRENS acts as an **agentic AI coordination layer** between incoming crisis signals and emergency response.

The system:

1. **Ingests** signals from weather APIs, news APIs, and citizen reports.
2. **Classifies** potential crises using an AI classification agent.
3. **Assesses** credibility and severity.
4. **Plans** coordinated response actions.
5. **Executes** response actions through AI-powered tools.
6. **Verifies** uncertain incidents using citizen feedback.
7. **Evaluates** the effectiveness of the response.
8. **Notifies** citizens and authorities in real time.

### Core Workflow

```text
Data Sources
     │
     ▼
Signal Ingestion
     │
     ▼
AI Classification
     │
     ▼
Confidence Assessment
     │
     ▼
Response Planning
     │
     ▼
Action Execution
     │
     ▼
Citizen Verification
     │
     ▼
Impact Evaluation
     │
     ▼
Updated Crisis State

## 🤖 Multi-Agent System

SIRENS uses five specialized AI agents, each responsible for a different stage of the crisis-response lifecycle.

| Agent | Role | Responsibility |
|---|---|---|
| 🧠 Agent 1 | Classifier | Processes signals, identifies crisis type, and evaluates credibility |
| 📋 Agent 2 | Planner | Analyzes the crisis and generates a prioritized response plan |
| ⚡ Agent 3 | Executor | Executes response actions using Genkit tools |
| 🛡️ Agent 4 | Verification | Uses citizen feedback to verify uncertain incidents |
| 📊 Agent 5 | Evaluator | Measures response effectiveness and determines the next action |

---

### 🧠 Agent 1 — Crisis Classifier

**File:** `backend/functions/src/ai/flows/classifySignals.ts`

Agent 1 processes incoming signals from weather services, news, and citizen reports.

**Responsibilities**
- Normalize incoming information
- Translate Roman Urdu and informal language
- Classify the crisis type
- Calculate a credibility score between 0.0 and 1.0
- Generate a structured crisis description
- Record the processing trace

**Supported Crisis Types**
- flood
- heatwave
- accident
- road_block
- power_outage
- infrastructure
- fire

**Confidence-Based Routing**

| Credibility | Action |
|---|---|
| < 0.5 | Send citizen alert |
| ≥ 0.5 | Continue to response planning |

---

### 📋 Agent 2 — Response Planner

**File:** `backend/functions/src/ai/flows/planCrisis.ts`

Agent 2 transforms a high-confidence crisis into a prioritized response plan.

**Responsibilities**
- Analyze the affected geographic area
- Estimate population impact
- Assess spread risk
- Identify infrastructure at risk
- Query available emergency resources
- Find alternate traffic routes using Google Maps
- Assign appropriate response resources
- Determine which authority should be notified

**Output**
- Impact analysis
- Prioritized action plan
- Rescue team assignment
- Medical unit assignment
- Authority notification
- Alternate route information

---

### ⚡ Agent 3 — Action Executor

**File:** `backend/functions/src/ai/flows/executePlan.ts`

Agent 3 uses Genkit tools to execute the response plan.

**Available Tools**

| Tool | Purpose |
|---|---|
| `sendAlert` | Sends alerts to citizens |
| `notifyAuthority` | Notifies relevant authorities |
| `dispatchResource` | Assigns emergency resources |
| `rerouteTraffic` | Generates an alternate route using Google Maps |
| `requestVerification` | Starts citizen verification |

> **Prototype scope:** Emergency interventions such as resource dispatch are simulated.

---

### 🛡️ Agent 4 — Human Verification

**File:** `backend/functions/src/verificationLoop.ts`

Low-confidence incidents are not immediately escalated into a full response.

SIRENS uses citizens in the affected area as an additional verification layer.

**Verification Flow**

```text
Low-Confidence Incident
          │
          ▼
Citizen Verification Request
          │
          ▼
     YES / NO Votes
          │
          ▼
   3 Unique YES Votes
          │
          ▼
 Confidence Escalated
          │
          ▼
    Agent 2 → Agent 3
```

When three unique citizens confirm an incident:

- The credibility score is escalated.
- The response plan is regenerated.
- The response execution stage is triggered.

This provides a human-in-the-loop safeguard against false positives.

---

### 📊 Agent 5 — Impact Evaluator

**File:** `backend/functions/src/ai/flows/evaluateImpact.ts`

Agent 5 evaluates whether the response was effective.

**Responsibilities**
- Analyze fresh signals
- Compare the situation before and after response
- Calculate an effectiveness score
- Update the danger zone
- Resolve the crisis when appropriate
- Notify citizens about the updated situation

**Possible Outcomes**
- `resolved`
- `requires_more_action`

---

## 🏗️ System Architecture

```text
┌──────────────────────────────────────────────────────────────┐
│                       DATA SOURCES                            │
│                                                                 │
│   🌦️ Open-Meteo     📰 GNews     📱 Citizen Reports           │
└───────────────────────────┬───────────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│                    SIGNAL INGESTION                            │
│                                                                 │
│                     Firebase / Firestore                       │
└───────────────────────────┬───────────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│                     AGENTIC AI                                 │
│                                                                 │
│   ┌──────────┐   ┌──────────┐   ┌──────────┐                  │
│   │ Agent 1  │ → │ Agent 2  │ → │ Agent 3  │                  │
│   │Classifier│   │ Planner  │   │ Executor │                  │
│   └──────────┘   └──────────┘   └────┬─────┘                  │
│                                       │                        │
│                                       ▼                        │
│                                ┌─────────────┐                 │
│                                │   Agent 4   │                 │
│                                │ Verification│                 │
│                                └──────┬──────┘                 │
│                                       │                        │
│                                       ▼                        │
│                                ┌─────────────┐                 │
│                                │   Agent 5   │                 │
│                                │  Evaluator  │                 │
│                                └─────────────┘                 │
└───────────────────────────┬────────────────────┬───────────────┘
                             │                    │
                             ▼                    ▼
┌──────────────────────────────────────────────────────────────┐
│                       FIRESTORE                                │
│                                                                 │
│  Crises │ Resources │ Danger Zones │ Alerts │ Agent Traces    │
└───────────────────────┬────────────────────┬───────────────────┘
                         │                    │
                         ▼                    ▼
                ┌─────────────────┐  ┌─────────────────┐
                │ Web Dashboard   │  │   Mobile App    │
                │   Authorities   │  │    Citizens     │
                └────────┬────────┘  └────────┬────────┘
                         │                    │
                         └─────────┬──────────┘
                                   ▼
                             Google Maps
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| AI Orchestration | Google Genkit |
| AI Model | Google Gemini |
| Backend | Node.js 20 |
| Backend Infrastructure | Firebase Cloud Functions Gen 2 |
| Database | Cloud Firestore |
| Storage | Firebase Storage |
| Scheduled Jobs | Firebase Cloud Scheduler |
| Realtime Events | Firestore Triggers |
| Mobile App | Expo / React Native |
| Web Dashboard | React / Next.js |
| Maps | Google Maps APIs |

---

## 🔌 APIs & Integrations

| Service | Purpose |
|---|---|
| Open-Meteo | Weather and environmental data |
| GNews | News signals for crisis detection |
| Google Maps Directions API | Alternate route generation |
| Google Maps JS / Native API | Interactive maps |
| Firebase Cloud Messaging | Push notifications |
| Google Gemini | AI classification, planning, execution, and evaluation |

---

## 📱 Citizen Mobile App

The mobile application gives citizens real-time situational awareness and allows them to contribute ground-level information.

**Features**
- 🗺️ Live crisis map
- 🚨 Incident reporting
- 🔔 Real-time alerts
- 🛡️ Crisis verification
- ⚙️ Alert preferences
- 👤 User onboarding

**Framework:** Expo / React Native

---

## 🖥️ Authority Command Center

The web dashboard gives authorities a centralized view of active crises and response activity.

**Features**
- 🗺️ Central live map
- 🚨 Active crisis monitoring
- 🚑 Emergency resource availability
- 🤖 AI activity and decision traces
- 📊 Crisis impact evaluation

**Framework:** React / Next.js

---

## 🚀 Getting Started

### Prerequisites
- Node.js 20+
- Firebase CLI
- Expo
- An active Firebase project
- Google Maps API key
- Gemini API key
- GNews API key

### 1. Clone the Repository
```bash
git clone https://github.com/your-team/SIRENS.git
cd SIRENS
```

### 2. Install Dependencies

**Mobile App**
```bash
npm install
```

**Backend**
```bash
cd backend/functions
npm install
```

### 3. Configure Firebase Secrets
```bash
firebase functions:secrets:set GOOGLE_GENAI_API_KEY
firebase functions:secrets:set GNEWS_API_KEY
firebase functions:secrets:set GOOGLE_MAPS_API_KEY
```

### 4. Configure Mobile Environment Variables

Create a `.env` file:

```env
EXPO_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=your_firebase_auth_domain
EXPO_PUBLIC_FIREBASE_PROJECT_ID=your_firebase_project_id
EXPO_PUBLIC_GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

> ⚠️ Never commit API keys, credentials, or `.env` files to GitHub.

### 5. Run the Mobile App
```bash
npx expo start
```
Scan the QR code using Expo Go.

### 6. Deploy Backend
```bash
cd backend/functions
firebase deploy --only functions
```

### 7. Build Android APK
```bash
npm install -g eas-cli
eas login
eas build --platform android --profile preview
```

### 8. Deploy Web Dashboard
```bash
npm install -g vercel
cd web-dashboard
vercel deploy --prod
```

---

## 🧪 Prototype Scope

SIRENS demonstrates a complete end-to-end agentic workflow:

```text
Multi-Source Signals
        ↓
AI Classification
        ↓
Confidence Assessment
        ↓
Response Planning
        ↓
Tool-Based Execution
        ↓
Citizen Verification
        ↓
Impact Evaluation
        ↓
Crisis Resolution
```

> The current prototype simulates real-world emergency interventions, such as resource dispatch.

---

## 🏆 Built For

**Agentic AI Hackathon**

SIRENS was built using Google Antigravity / Genkit to demonstrate how specialized AI agents can coordinate around a real-world emergency-response problem.

---

## 👥 Team

| Name | Role |
|---|---|
| Ayesha Zahid | Full Stack Engineer & Designer |
| Ayesha Noman | Full Stack AI Engineer |
| Maham Faisal | Frontend Developer |
| Maria Kousar | Full Stack AI Engineer |

---

## 🌍 Vision

Move emergency response from reactive to predictive, coordinated, and adaptive.

SIRENS demonstrates how agentic AI + real-time data + human verification + geographic intelligence can form an intelligent coordination layer for urban crisis management.

---

<div align="center">

### ⭐ SIRENS

**Detect faster. Coordinate smarter. Respond better.**

</div>
