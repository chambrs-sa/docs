# 🚀 CHAMBRS MVP Migration Plan (Lovable ➝ Independent Architecture)

## Author

**Ntethelelo Zikalala**
Technical Co-Founder, CHAMBRS
2025

---

# 📌 Overview

This document outlines the **complete migration plan** for moving CHAMBRS away from Lovable.dev to a secure, scalable, and independent architecture built with:

* **Frontend:** Next.js + TypeScript + Tailwind
* **Backend:** Spring Boot (Java 21)
* **Database:** Supabase Postgres (initial), AWS RDS (future)
* **Infrastructure:** Vercel (initial), AWS ECS/RDS/CloudFront (future)
* **Security:** ISO27001-aligned controls, documentation, and processes
* **Timeline:** 8–12 weeks, strict execution plan

This is designed for **lean startups** with <100 users and <10 active users, and focuses on rapid delivery without compromising security.

---

# 🧭 Table of Contents

* [🏗 1. Migration Overview](#-1-migration-overview)
* [📅 2. 12-Week Migration Timeline](#-2-12-week-migration-timeline)
* [📦 3. De-Scoped MVP](#-3-de-scoped-mvp)
* [⚖ 4. Must-Have vs Nice-to-Have](#-4-must-have-vs-nice-to-have)
* [🎯 5. Solo Founder Productivity Plan](#-5-solo-founder-productivity-plan)
* [🗂 6. Weekly Sprint Plan](#-6-weekly-sprint-plan)
* [💸 7. Cost Estimation (ZAR)](#-7-cost-estimation-zar)
* [🏁 8. Final Recommendations](#-8-final-recommendations)

---

# 🏗 1. Migration Overview

CHAMBRS is currently built using Lovable.dev, which provides automated frontend generation and tight integration with Supabase, but introduces:

* Hidden logic
* Lack of full control
* Vendor lock-in
* Limited security control
* Limited scalability

The migration objective is to rebuild the system using:

### Frontend

* **Next.js + TypeScript**
* TailwindCSS
* React Query
* Axios
* Vercel deployment

### Backend

* **Spring Boot (Java 21)**
* JWT Authentication
* RBAC (role-based access control)
* Flyway migrations
* Unit & integration tests

### Database

* **Supabase Postgres** (initial MVP)
* **AWS RDS** (post-funding, compliant architecture)

### Security

* ISO27001-aligned governance
* JWT rotation
* Secure cookies
* Logging & audit trails
* Risk register
* Secure SDLC

The target timeline is **12 weeks** with strict, disciplined execution.

---

# 📅 2. 12-Week Migration Timeline

## **WEEK 1–2 — Frontend Extraction & Rebuild**

* Export Lovable code
* Create Next.js project
* Configure Tailwind
* Rebuild UI layout and pages
* Rebuild navigation/routing
* Implement Supabase auth (temporary)
* Marketplace UI
* Profile UI
* Environment configs

---

## **WEEK 3–4 — Backend Build (Spring Boot)**

* Setup Spring Boot (Java 21)
* Setup Maven project
* Create Postgres schema
* Build user service
* Build asset service
* Build investment service
* Create portfolio service
* Implement JWT authentication
* Implement Flyway migrations
* Write integration tests

---

## **WEEK 5 — Frontend ↔ Backend Integration**

* Replace placeholder data
* Connect all frontend modules
* Axios/React Query integration
* Error handling & retries
* User session flows
* Investment creation flow

---

## **WEEK 6 — Infrastructure Setup**

* Frontend deploy → Vercel
* Backend deploy → Supabase / AWS
* GitHub Actions CI/CD
* Logging & monitoring setup (Sentry)
* Environment variable management

---

## **WEEK 7–8 — Security & ISO Alignment**

* JWT rotation
* Secure cookies
* RBAC
* Encryption policies
* Data classification
* Access control matrix
* Secure SDLC documentation
* Threat model
* Risk register
* Basic penetration testing

---

## **WEEK 9 — QA & Stabilisation**

* End-to-end testing
* UI polish
* Bug fixes
* Performance checks
* Improve error handling

---

## **WEEK 10–12 — Hardening & Go-Live Prep**

* Load testing
* Backup strategy
* Resilience testing
* Deployment rehearsal
* Admin improvements
* Portfolio logic refinement
* Final documentation

---

# 📦 3. De-Scoped MVP

To deliver in 8–12 weeks, the following are included:

### User Management

* Signup
* Login
* Logout
* Profile update

### Marketplace

* Asset list
* Asset details

### Investments

* Create investment
* View investments
* View portfolio summary

### Admin

* Upload new assets
* Edit assets
* View investors

### Infrastructure

* Vercel (frontend)
* Supabase Postgres (db)
* Spring Boot API (backend)
* GitHub Actions

### Security

* HTTPS
* JWT
* Basic RBAC
* Error monitoring
* Basic logs

---

# ⚖ 4. Must-Have vs Nice-to-Have

## Must-Have (Build NOW)

* Next.js frontend
* Spring Boot backend
* Postgres DB
* User auth (JWT)
* Marketplace listing
* Investment creation
* Portfolio view
* Basic admin portal
* Error handling
* CI/CD
* Basic security
* Basic monitoring

## Nice-to-Have (Build LATER)

* Full ISO27001 compliance
* Advanced audit logs
* Document uploads
* Full KYC/AML integration
* Payment rails
* Withdrawal module
* Notification center
* Elasticsearch search
* Kafka eventing
* Multi-region setup
* Advanced role hierarchy

---

# 🎯 5. Solo Founder Productivity Plan

### 1. Zero task switching

One task at a time.

### 2. Hard weekly deliverables

Must produce visible progress weekly.

### 3. Controlled working windows

* Weekdays: 19:00–22:00
* Weekends: 4–6 hour deep work block

### 4. Ruthless scope control

If it won’t help investor demos, it does not exist.

### 5. Architecture before coding

No improvisation. Plan → Build → Test.

### 6. Document as you go

ISO docs done gradually, not at the end.

### 7. Protect energy

A tired founder produces buggy code.

---

# 🗂 6. Weekly Sprint Plan

### **WEEK 1**

* Next.js setup
* Navigation + layout

### **WEEK 2**

* Rebuild major screens
* Basic Supabase auth

### **WEEK 3**

* Spring Boot setup
* Auth service
* User service

### **WEEK 4**

* Asset + investment + portfolio services
* Flyway migrations

### **WEEK 5**

* Integrate frontend ↔ backend
* Replace placeholder data

### **WEEK 6**

* Vercel deploy
* Backend deploy
* CI/CD
* Sentry

### **WEEK 7**

* Security hardening
* Logging
* RBAC

### **WEEK 8**

* SDLC docs
* Risk register
* Threat model

### **WEEK 9**

* Testing & polishing

### **WEEK 10–12**

* Load testing
* Hardening
* Final refinements
* Go-live prep

---

# 💸 7. Cost Estimation (ZAR)

## Exchange Rate Reference

1 USD ≈ **R18.50**

---

## Option A — Lean MVP (Supabase + Vercel)

*Recommended for current stage*

| Item           | USD | ZAR  |
| -------------- | --- | ---- |
| Supabase Pro   | $25 | R463 |
| Vercel Hobby   | $0  | R0   |
| Sentry         | $0  | R0   |
| GitHub         | $0  | R0   |
| Email (Resend) | $0  | R0   |

### **Total Monthly: ~R500**

### **Annual: ~R6,000**

---

## Option B — Fully Compliant AWS Backend (ECS + RDS)

| Item                  | USD   | ZAR    |
| --------------------- | ----- | ------ |
| AWS RDS (db.t3.small) | $55   | R1,017 |
| AWS Fargate           | $50   | R925   |
| ALB                   | $20   | R370   |
| CloudFront            | $5    | R92    |
| S3                    | $5    | R92    |
| Secrets Manager       | $0.40 | R7     |
| NAT Gateway           | $32   | R590   |
| CloudWatch Logs       | $10   | R185   |

### **Total Monthly: R3,200 – R3,500**

### **Annual: R38,400 – R42,000**

---

## Option C — Hybrid (Supabase DB + AWS Fargate Backend)

| Item            | Cost (ZAR) |
| --------------- | ---------- |
| Supabase Pro    | R463       |
| Fargate backend | R925       |
| CloudWatch      | R185       |

### **Total Monthly: ~R1,500 – R1,700**

### **Annual: ~R18,000**

---

# 🏁 8. Final Recommendations

### Choose this NOW:

**Next.js + Spring Boot + Supabase + Vercel**
Cost: **± R500/month**

### Migrate to this AFTER seed funding:

**Next.js + Spring Boot + AWS ECS + AWS RDS**
Cost: **± R3,200/month**

### Why:

* Fastest path to shipping
* Secure enough for MVP
* Scalable for enterprise later
* Low cost while pre-revenue
* Easy future migration path

---





