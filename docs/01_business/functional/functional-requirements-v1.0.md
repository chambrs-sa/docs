# Investor Dashboard – Functional Requirements (Heavy Machinery Investment Platform MVP)

---

## 0) Scope & Assumptions

* **MVP Scope:** Web app for Investors & Admins to manage investments in **heavy machinery assets** (e.g., dump trucks, excavators, tipper trucks used in mining).
* **Assets:** Real-world physical machines; investors may hold full or fractional ownership.
* **Revenue Source:** Assets generate returns via rental contracts, operational usage, or resale value.
* **Funding:** Deposits/withdrawals via bank transfer (manual workflow in MVP).
* **Geography:** South Africa-first (mining context, compliance: POPIA, FICA).

---

## 1) Roles & Permissions Matrix

| Capability                                      | Investor |  Admin/Operations |       Support |
| ----------------------------------------------- | -------: | ----------------: | ------------: |
| View asset catalogue & holdings                 |        ✓ |                 ✓ | ✓ (read-only) |
| View machinery attributes (make, model, status) |        ✓ |                 ✓ |             ✓ |
| Download asset docs (maintenance, insurance)    |        ✓ |                 ✓ |             ✓ |
| Update asset records & valuation                |        – |                 ✓ |             – |
| Create maintenance event/attach certificate     |        – |                 ✓ |             – |
| Request deposit/withdrawal                      |        ✓ | ✓ (manual assist) |             – |

---

## 2) Authentication & Onboarding
**FR-2.1** During onboarding, investors must declare source of funds and investment purpose (compliance requirement for machinery investments).
FR-2.2 Login with email + password; support MFA (TOTP) at login.
FR-2.3 Password reset (email magic link).
FR-2.4 Session management (list active sessions, revoke).
FR-2.5 CSRF protection for state-changing actions; same-site cookies (HttpOnly, Secure).
API (v1):

POST /auth/signup

POST /auth/login

POST /auth/mfa/setup (generate secret + QR)

POST /auth/mfa/verify

POST /auth/password/forgot

POST /auth/password/reset

GET /auth/sessions

DELETE /auth/sessions/{sessionId}

Acceptance: MFA required if enabled; invalid TOTP returns 401 with error code MFA_INVALID.

---

## 3) Asset Catalogue & Portfolio Overview

**FR-3.1** Portfolio summary includes:

* Total investment value
* Cash balance
* Rental income received to date
* Active assets vs. inactive assets (maintenance/downtime)

**FR-3.2** Show an **Asset Catalogue** filtered to holdings:

* Asset name (e.g., CAT 777 Dump Truck)
* Asset type (tipper truck, excavator, loader)
* Make, model, year
* Current market valuation
* Ownership % (for fractional investors)
* Operational status: `ACTIVE | UNDER_MAINTENANCE | IDLE | RETIRED`

**API:**

* `GET /portfolio/summary`
* `GET /holdings` (adds `ownershipPercent`, `status`, `location`)

**Acceptance:** Assets display a **thumbnail photo** + quick stats in table view.

---

## 4) Asset Detail Page

**FR-4.1** Asset detail view shows:

* **Specifications:** Make, model, VIN/serial number, purchase date, current mileage/operating hours.
* **Valuation:** Historical price trend + current appraisal.
* **Revenue Metrics:** Rental income per month, expenses (fuel, maintenance, insurance).
* **Maintenance History:** Scheduled/unscheduled maintenance logs.
* **Documents:** Ownership certificate, maintenance certificates, insurance, safety compliance docs.
* **Location:** Site or region where the machine operates (mine location, depot).

**API:**

* `GET /assets/{assetId}` → includes metadata, specs, documents.
* `GET /assets/{assetId}/maintenance`
* `GET /assets/{assetId}/income`

**Acceptance:** Investors can see **how their money is performing through physical asset use** (utilization %, downtime %).

---

## 5) Transactions (Machinery Context)

**FR-5.1** Transaction types:

* **Investment Buy-in** (purchase of machinery stake)
* **Exit/Resale** (partial or full)
* **Rental Income Distribution** (monthly, quarterly)
* **Maintenance Expense Allocation**
* **Insurance Premium Allocation**

**API:**

* `GET /transactions?type=buy|income|expense|sale`
* `GET /transactions/export`

**Acceptance:** Timeline clearly distinguishes **income vs. expense** flows linked to each machine.

---

## 6) Cash & Funding (Instruction-Based)

Same as before, but wording updated:

* **Deposits:** Funds to be used for machinery acquisition.
* **Withdrawals:** Rental income withdrawals or proceeds from asset liquidation.

Statuses remain `SUBMITTED → AWAITING_FUNDS → COMPLETED`.

---

## 7) Documents & Compliance

**FR-7.1** Document categories:

* **Investor docs:** Statements, tax certificates.
* **Asset docs:** Ownership certificate, maintenance logs, inspection certificates, insurance policies.

**API:**

* `GET /documents?type=asset|investor`
* `GET /assets/{id}/documents`

**Acceptance:** Machine documents must be linked to the specific asset in UI.

---

## 8) Notifications

**FR-8.1** Notifications triggered by:

* New machinery added to investor’s holdings.
* Rental income distribution posted.
* Asset under maintenance or downtime.
* New compliance or inspection certificate uploaded.

---

## 9) Profile & Settings

As before, but include:

* **Investor Risk Profile** (optional field – aggressive, balanced, conservative) to tailor machine investment suggestions.
FR-9.1 Edit profile (name, phone, address), contact verification via OTP/email link.
FR-9.2 Security: change password, manage MFA devices, view/revoke sessions.
API:

GET /me

PUT /me

POST /me/verify-email

POST /me/verify-phone

(Auth endpoints from section 2)

Acceptance: Email/phone updates remain “unverified” until confirmed.

---

## 10) Admin Console (Machinery Context)

**FR-10.1** Admin can:

* Add new machine (make, model, serial, purchase cost, acquisition date).
* Update operational status (active, maintenance).
* Upload maintenance/inspection docs.
* Update current valuation and utilization stats.
* Allocate rental income distributions to investors.

**API:**

* `POST /admin/assets`
* `PATCH /admin/assets/{id}/status`
* `POST /admin/assets/{id}/documents`
* `POST /admin/assets/{id}/income-distribution`

---

## 11) Audit & Compliance

Audit extended to cover:

* Who updated machine valuation/status.
* Who uploaded maintenance/inspection docs.
* When rental income was allocated.

---

## 12) Website (Lovable AI – Machinery Context)

**Pages:**

* `/` Portfolio Summary (cash, machinery assets, income)
* `/holdings` (table of machines)
* `/holdings/:id` (machine detail: specs, income, maintenance history)
* `/transactions` (buy-in, income distributions, expenses)
* `/documents` (investor + machine docs)
* `/funding` (deposit/withdrawal requests)
* `/admin/assets` (add/manage machines)
* `/admin/funding` (approve deposits/withdrawals)
* `/admin/distributions` (allocate income)
* `/admin/maintenance` (logs, docs)

---

## 13) Data Model (Machinery-Specific)

* **Asset** `{ id, name, type, make, model, year, serialNo, acquisitionDate, purchaseCost, status, location }`
* **Valuation** `{ assetId, date, appraisedValue }`
* **Holding** `{ id, investorId, assetId, ownershipPercent }`
* **IncomeDistribution** `{ assetId, amount, period, investorAllocations[] }`
* **MaintenanceLog** `{ id, assetId, type, description, cost, performedAt, docId }`
* **Document** `{ id, type:ownership|maintenance|insurance|investor, assetId?, investorId?, fileKey }`

---
