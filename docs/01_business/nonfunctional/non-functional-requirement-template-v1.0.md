# Non‑Functional Requirements (NFR) Markup Template — CHAMBRS

> **How to use:** Duplicate this doc, keep the structure, and fill each placeholder. Prefer measurable targets (SLOs), explicit owners, and verifiable tests. Use the YAML blocks as your system‑of‑record for automation.

---

## 0) Document Meta

```yaml
nfr_doc:
  project: "CHAMBRS App"
  version: "0.1"
  last_updated: "YYYY‑MM‑DD"
  owner: "Name, Role"
  reviewers: ["Name, Security" , "Name, SRE" , "Name, Product"]
  status: "Draft | In Review | Approved"
  scope: "Web app, APIs, worker jobs, mobile app, data pipeline, admin console"
  out_of_scope: ["<list>"]
  references:
    - "Architecture diagram vX"
    - "Security policy"
    - "Runbooks & Playbooks"
```

---

## 1) Global Quality Targets (SLO Summary)

| SLI             | Definition                                     | Target                    | Measurement              | Error Budget |
| --------------- | ---------------------------------------------- | ------------------------- | ------------------------ | ------------ |
| Availability    | % of successful requests (core surfaces)       | ≥ 99.9% monthly           | Uptime probe + 5xx ratio | 43m/month    |
| Latency p95     | Time to first actionable paint (web) / API p95 | Web ≤ 2.0s, API ≤ 300ms   | RUM + APM                | 5%           |
| Error rate      | 5xx + app errors                               | ≤ 0.5%                    | Logs + APM               | 0.5%         |
| Data durability | Probability of data loss                       | ≥ 11 nines                | Storage vendor reports   | n/a          |
| RPO/RTO         | Recovery point / time objectives               | RPO ≤ 5 min, RTO ≤ 30 min | DR tests                 | n/a          |
| Accessibility   | WCAG conformance                               | ≥ WCAG 2.2 AA             | AXE/PA11Y audits         | n/a          |

> Map each SLO to alerts and dashboards.

---

## 2) NFR Item Template (reuse for each requirement)

```yaml
- id: NFR-XXX
  category: "Performance | Availability | Security | Privacy | Compliance | Scalability | Reliability | Observability | Maintainability | Operability | UX | Accessibility | i18n | Portability | DR/Backup | Data Mgmt | API Quality | Release Mgmt | Cost | Sustainability | AI Governance"
  statement: "<Concise, testable statement>"
  business_value: "<Why this matters>"
  metric: "<What we measure>"
  target: "<Numeric target & window>"
  baseline: "<Current or assumed>"
  scope: "<Components, endpoints, roles>"
  environment: "prod | staging | dev"
  constraints: ["<tech/policy limits>"]
  dependencies: ["<systems/teams>"]
  priority: "Must | Should | Could | Won’t"
  verification: "<Test/monitoring/audit method>"
  evidence: "<Links to dashboards, test runs, audits>"
  owner: "<team/role>"
  status: "Planned | In‑progress | Verified"
  risks: "<known risks>"
  notes: "<freeform>"
```

---

## 3) Category Sections

> For each section, add one or more NFR items using the template above. Use the checklists to ensure coverage.

### 3.1 Performance & Capacity

**Checklist**

* [ ] Define peak/steady load profile (RPS, DAU/MAU, concurrency, payload sizes).
* [ ] Web TTFB/TTI, API p95/p99, background job SLAs.
* [ ] Throughput limits, connection pools, queue depths.
* [ ] Capacity headroom ≥ 30% under p95 load.

**Example metrics**

* API p95 ≤ 300ms for `/investments/*` at 500 RPS.
* Web TTI ≤ 2s on 3G Fast, Core Web Vitals: LCP ≤ 2.5s, CLS ≤ 0.1.

### 3.2 Availability & Resilience

**Checklist**

* [ ] SLOs per user‑critical journey (login, deposit, invest, withdraw).
* [ ] Multi‑AZ, graceful degradation, circuit breakers, retries with backoff.
* [ ] Health checks (liveness/readiness) + dependency health.
* [ ] Chaos/DR game days scheduled.

**Example metrics**

* 99.9% monthly availability for invest/withdraw flows.

### 3.3 Scalability

**Checklist**

* [ ] Horizontal scaling strategy (stateless services, auto‑scale policies).
* [ ] Data partitioning/indices for large tables (holdings, transactions).
* [ ] Async work via queues; idempotent consumers.

### 3.4 Security

**Checklist**

* [ ] RBAC (investor/admin/compliance_officer) with server‑side checks only.
* [ ] PII encryption at rest (pgp_sym_encrypt) + KMS/Vault for keys.
* [ ] TLS 1.2+ in transit; HSTS; secure cookies; OAuth/OIDC.
* [ ] RLS policies for tenant/owner isolation.
* [ ] Secrets management, rotation policy, no secrets in client.
* [ ] Audit logging (who/what/when/where) immutable and queryable.
* [ ] Dependency and container scanning in CI.

**Example metrics**

* 100% sensitive columns encrypted; 0 critical vulns outstanding > 7 days.

### 3.5 Privacy & POPIA/GDPR Compliance (South Africa focus)

**Checklist**

* [ ] Lawful basis recorded for each data category.
* [ ] Data minimisation: collect only necessary fields.
* [ ] Retention schedule + automated deletion/anonymisation.
* [ ] Data subject rights: access, rectify, erase, portability.
* [ ] Processor/third‑party disclosures maintained.

**Example metrics**

* Right‑to‑erasure request fulfilled ≤ 30 days; export ≤ 7 days.

### 3.6 Reliability & Data Integrity

**Checklist**

* [ ] ACID boundaries defined; idempotent writes; exactly‑once semantics where needed.
* [ ] Transactional outbox / saga patterns for cross‑service consistency.
* [ ] Checksums/hard deletes guarded; referential integrity enforced.

### 3.7 Observability (Logging, Metrics, Tracing)

**Checklist**

* [ ] Four Golden Signals instrumented: latency, traffic, errors, saturation.
* [ ] Trace coverage ≥ 90% of key paths; log redaction for PII.
* [ ] Alert runbooks with owner & escalation chain.

**Example metrics**

* MTTR ≤ 20 min; page rate ≤ 2/month on core services.

### 3.8 Maintainability & Code Health

**Checklist**

* [ ] Linting, formatting, type coverage thresholds.
* [ ] Unit/integration/E2E test coverage targets.
* [ ] Cyclomatic complexity caps; module boundaries documented.

**Example metrics**

* Unit coverage ≥ 80%; critical modules ≥ 90%.

### 3.9 Operability & Support

**Checklist**

* [ ] Runbooks/playbooks; on‑call rota; support SLAs.
* [ ] Feature flags + safe rollbacks.
* [ ] Config via env/params; no restarts for config changes where possible.

### 3.10 UX Quality & Accessibility

**Checklist**

* [ ] WCAG 2.2 AA; keyboard navigation; screen reader labels.
* [ ] Perceived performance (skeletons, optimistic UI, offline states).
* [ ] Error messages actionable; empty states helpful.

**Example metrics**

* Lighthouse Accessibility ≥ 95; task success ≥ 95% in usability tests.

### 3.11 Internationalisation (i18n) & Localisation (l10n)

**Checklist**

* [ ] Translation pipeline; fallback locales; number/date/currency formats.
* [ ] Avoid concatenated strings; keep placeholders.

### 3.12 Compatibility & Portability

**Checklist**

* [ ] Supported browsers/devices matrix.
* [ ] API versioning strategy; backward compatibility window.
* [ ] Infrastructure as Code; environment parity.

### 3.13 Backup, Disaster Recovery & Business Continuity

**Checklist**

* [ ] Automated, encrypted backups; verified restores.
* [ ] RPO/RTO targets; regional failover strategy.
* [ ] DR tests at least quarterly with reports.

**Example metrics**

* Restore validation success rate 100% of monthly tests.

### 3.14 Data Management & Quality

**Checklist**

* [ ] Data catalog; ownership; quality checks (freshness, completeness).
* [ ] Schema evolution policy; migration runbooks.

### 3.15 API Quality (External & Internal)

**Checklist**

* [ ] OpenAPI/JSON‑Schema source‑of‑truth.
* [ ] Idempotent endpoints; consistent error model; rate limits.
* [ ] N+1 query controls; pagination rules.

**Example metrics**

* Breaking changes require ≥ 30‑day deprecation notice.

### 3.16 Release & Change Management

**Checklist**

* [ ] Trunk‑based or GitFlow policy; CI gates; canary/blue‑green.
* [ ] DORA metrics tracked (Lead time, Deployment freq, MTTR, Change fail %).

**Example metrics**

* Change failure rate ≤ 10%; deploys ≥ 2/day.

### 3.17 Cost & Performance Efficiency

**Checklist**

* [ ] Unit economics dashboards (R/investor, R/transaction).
* [ ] Auto‑scaling and rightsizing policies.
* [ ] Budget alerts; cost anomaly detection.

### 3.18 Sustainability (Optional)

**Checklist**

* [ ] Carbon‑aware scheduling; efficient instance classes.
* [ ] Bundle size budgets; CDN offload.

### 3.19 AI/ML Governance (If Applicable)

**Checklist**

* [ ] Model/version registry; prompt/PII policies; audit trails.
* [ ] Evaluation harness; rollback plan; bias/robustness checks.

---

## 4) Risk Register (Top NFR Risks)

| Risk ID | Description                       | Impact | Likelihood | Mitigation           | Owner    | Status |
| ------- | --------------------------------- | ------ | ---------- | -------------------- | -------- | ------ |
| NFR‑R1  | Key compromise for PII encryption | High   | Low        | KMS + rotation + HSM | Security | Open   |

---

## 5) Verification Plan

```yaml
verification:
  performance_tests: "k6/Gatling scenarios; target RPS & p95/p99"
  security_tests: "SAST/DAST, dependency scans, RLS policy tests"
  accessibility_audits: "Lighthouse + AXE CI gate ≥ 90/95"
  chaos_drills: "Quarterly failure injections + DR restore tests"
  compliance_audits: "POPIA controls checklist, data‑retention verification"
  signoff: ["SRE lead", "Security lead", "Product"]
```

---

## 6) Traceability Matrix (Epics → NFRs → Tests → Dashboards)

| Epic     | NFR ID(s)               | Test ID(s)    | Dashboard/Alert               | Owner    |
| -------- | ----------------------- | ------------- | ----------------------------- | -------- |
| Payments | NFR‑PERF‑01, NFR‑SEC‑02 | PT‑12, SEC‑07 | "API Latency", "Fraud Alerts" | Platform |

---

## 7) Sign‑off

| Role     | Name | Date | Comments |
| -------- | ---- | ---- | -------- |
| Product  |      |      |          |
| Security |      |      |          |
| SRE      |      |      |          |
