<div align="center">

# 🚀 Collabry

### AI-Powered Influencer Marketing Platform

[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com)
[![Java](https://img.shields.io/badge/Java-17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.org)

**Collabry** is a full-stack platform that connects brands with influencers through **AI-powered semantic matching**, secure milestone-based payments, and admin-controlled identity verification.

[Features](#-features) · [Tech Stack](#-tech-stack) · [Quick Start](#-quick-start) · [Architecture](#-architecture) · [AI Engine](#-ai-engine) · [Documentation](#-documentation)

</div>

---

## ✨ Features

### 🎯 For Brands
- **AI Smart Matchmaker** — Groq LLM-powered influencer recommendations with numerical Match Scores and reasoning
- **AI Campaign Brief Generator** — Auto-generate professional campaign descriptions from a simple title
- **Campaign Lifecycle** — Create → Publish → Complete campaigns with full status tracking
- **Invitation Management** — Send, withdraw, and negotiate collaboration terms with counter-offers
- **Payment Ledger** — Milestone-based financial tracking with invoice generation
- **Influencer Search** — Filter by niche, location, follower count, and engagement rate

### 🌟 For Influencers
- **AI Bio Enhancement** — Transform a basic bio into a professional, brand-ready portfolio description
- **Smart Dashboard** — At-a-glance view of pending invitations and active collaborations
- **Negotiation Tools** — Accept, reject, or counter-offer on brand invitations
- **Deliverable Submission** — Submit content URLs with notes for brand review
- **Availability Toggle** — Control visibility in brand searches
- **Cloudinary Photo Upload** — Professional profile image hosting

### 🛡️ For Administrators
- **Identity Verification Queue** — Manual review and approval of Blue Checkmark requests
- **User Safety Controls** — Flag suspicious accounts or deactivate confirmed violators
- **Platform Analytics** — Real-time stats on users, campaigns, and payment status
- **Recent Signups Monitor** — Track new registrations with role and status info

---

## 🛠 Tech Stack

| Layer | Technology |
|:------|:-----------|
| **Frontend** | React 19, TypeScript 5.9, Vite 7, Ant Design 5, Recharts |
| **Backend** | Spring Boot 3.5, Spring Security, Spring Data JPA |
| **AI Engine** | Groq LLM API (Semantic Matching + Content Generation) |
| **Database** | H2 (Development) / MySQL 8 (Production) |
| **Auth** | JWT + Google OAuth 2.0 |
| **Media** | Cloudinary (Image Upload & CDN) |
| **Email** | SMTP (Gmail App Passwords) with Console Fallback |
| **DevOps** | Docker Compose, JaCoCo, DesigniteJava |
| **Testing** | JUnit 5, Mockito, Vitest, React Testing Library |

---

## 🚀 Quick Start

### Prerequisites
- **Docker & Docker Compose** (recommended), OR
- Java 17+, Node.js 18+, and Maven 3.6+

### Option 1: Docker (One Command)

```bash
# Clone the repo
git clone https://github.com/YourUsername/collabry.git
cd collabry

# Set up environment
cp .env.example .env
# Edit .env with your API keys (see .env.example for details)

# Build and run
docker compose up --build
```

The app will be live at `http://localhost:9090`

### Option 2: Manual Setup

```bash
# Terminal 1 — Backend
cd backend
./mvnw spring-boot:run

# Terminal 2 — Frontend
cd frontend
npm install
cp .env.example .env
npm run dev
```

Backend: `http://localhost:8080` | Frontend: `http://localhost:5173`

### Test Accounts (Auto-seeded)

| Email | Password | Role |
|:------|:---------|:-----|
| admin@collabry.com | password123 | 🛡️ Admin |
| brand@collabry.com | password123 | 🎯 Brand |
| influencer@collabry.com | password123 | 🌟 Influencer |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────┐
│                    React Frontend                    │
│         (Vite + TypeScript + Ant Design)             │
└───────────────────────┬─────────────────────────────┘
                        │ Axios + JWT
┌───────────────────────▼─────────────────────────────┐
│               Spring Security Filter                 │
│            (JWT Validation + RBAC Guard)              │
└───────────────────────┬─────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────┐
│              REST Controllers (Thin)                 │
│   Auth │ Campaign │ Invitation │ Payment │ Admin     │
└───────────────────────┬─────────────────────────────┘
                        │
┌───────────────────────▼─────────────────────────────┐
│                Service Layer                         │
│  AuthService │ CampaignService │ AiRecommendation    │
│  InvitationService │ PaymentService │ AdminService   │
└──────┬────────────────┬─────────────────────────────┘
       │                │
┌──────▼──────┐  ┌──────▼──────┐
│  JPA Repos  │  │  Groq API   │
│  (H2/MySQL) │  │  (LLM AI)   │
└─────────────┘  └─────────────┘
```

### Key Design Decisions
- **Layered Architecture** — Controllers → Services → Repositories → Models
- **Role-Based Access Control** — `BRAND`, `INFLUENCER`, `ADMIN` roles enforced at security filter level
- **State Machine** — Campaign and Invitation statuses managed through enums with strict transition rules
- **Failover Design** — AI features gracefully degrade to deterministic matching when the API is unavailable

---

## 🤖 AI Engine

Collabry integrates three AI-powered features via the **Groq LLM API**:

### 1. Semantic Influencer Matchmaker
```
Brand selects Campaign → Backend assembles Context (niche, budget, goals)
→ System Prompt + Influencer Data sent to Groq → AI returns Match Scores
→ Backend enriches with profile data → Frontend renders ranked cards
```
- **60% weight** on Niche Relevance (semantic, not keyword-based)
- **40% weight** on Budget Alignment
- Automatic **fallback to deterministic matching** if the API is unavailable

### 2. AI Campaign Brief Generator
Brands enter a rough title → AI generates a professional, SEO-friendly campaign description optimized for influencer appeal.

### 3. AI Bio Enhancement
Influencers provide basic keywords → AI crafts a polished, brand-ready professional biography.

---

## 🧪 Testing

```bash
# Backend unit tests
cd backend && ./mvnw test

# Backend integration tests + coverage report
cd backend && ./mvnw verify
# Coverage report: backend/target/site/jacoco/index.html

# Frontend tests
cd frontend && npm test
```

### TDD: Image Upload Feature
The Cloudinary image upload was built using strict **Test-Driven Development** (Red → Green → Refactor):

| Step | Description |
|:-----|:------------|
| 🔴 Red | Wrote failing tests for `CloudinaryService` and `ImageUploadController` |
| 🟢 Green | Implemented Cloudinary integration to pass all tests |
| 🔵 Refactor | Replaced URL text inputs with direct file upload UI |

---

## 📖 Documentation

| Document | Description |
|:---------|:------------|
| [Usage Scenarios](./docs/USAGE_SCENARIOS.md) | Complete walkthrough of all user flows |
| [Usage Scenarios (with Screenshots)](./Usage%20scenario.docx) | Visual walkthrough with annotated screenshots |
| [Design Principles](./DESIGN_PRINCIPLES.md) | SOLID principles, cohesion/coupling metrics |
| [Integration Tests](./docs/INTEGRATION_TESTS.md) | Full list of integration test coverage |
| [Code Quality](./quality/README.md) | Quality assurance processes and standards |
| [Code Smells Analysis](./CodeSmells_Designite/) | DesigniteJava reports with resolved justifications |

---

## ⚙️ Environment Variables

Copy `.env.example` to `.env` and configure:

| Variable | Required | Description |
|:---------|:---------|:------------|
| `VITE_GOOGLE_CLIENT_ID` | Yes | Google OAuth Client ID |
| `VITE_API_BASE_URL` | Yes | Backend API URL (`http://localhost:9090/api/auth`) |
| `SPRING_MAIL_USERNAME` | Optional | Gmail address for sending emails |
| `SPRING_MAIL_PASSWORD` | Optional | Gmail App Password (16-char) |
| `GROQ_API_KEY` | Optional | Groq API key for AI features |
| `CLOUDINARY_CLOUD_NAME` | Optional | Cloudinary cloud name for image uploads |
| `CLOUDINARY_API_KEY` | Optional | Cloudinary API key |
| `CLOUDINARY_API_SECRET` | Optional | Cloudinary API secret |

> Without SMTP configured, confirmation links print to the backend console. Without Groq, AI features fall back to deterministic matching.

---

## 🔧 Troubleshooting

| Problem | Solution |
|:--------|:---------|
| Blank screen on frontend | Set `VITE_GOOGLE_CLIENT_ID` in your `.env` |
| 401/403 errors | Ensure `VITE_API_BASE_URL` matches the backend port |
| CORS errors | Add `http://localhost:5173` to allowed origins in `ApplicationConfig` |
| Google login fails | Verify OAuth Client ID and authorized JavaScript origins |
| Emails not sending | Expected in local dev — links print to backend console |

---

<div align="center">

**Built with ❤️ using Spring Boot, React, and Groq AI**

</div>
