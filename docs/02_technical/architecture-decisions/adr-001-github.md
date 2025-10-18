# ADR-001: GitHub for Version Control Management
**Status:** Accepted  
**Date:** 2025-10-06  
**Owner:** Technical Automation Lead  
**Related:** ADR-00X CI/CD, ADR-00X Secrets, Compliance Register

## Context
- Need a reliable, low-overhead VCS that integrates with future CI/CD, code review, and policy controls.  
- Early-stage budget constraints; security + auditability required (POPIA/FSCA expectations).  
- Team/collaborators are already familiar with GitHub.

## Decision
Use **GitHub (cloud, org account)** as the primary VCS for all source code and docs during MVP and early growth.  
Scope: public website assets allowed; regulated data must **not** be checked in.

## Options considered
- **GitHub (cloud)** — ✅ ubiquitous, integrations, marketplace; ❌ vendor lock-in risk; Actions not the strongest for self-host control.  
- **GitLab (SaaS/self-host)** — ✅ tight CI/CD; ❌ higher ops overhead now.  
- **Bitbucket (cloud)** — ✅ Jira integration; ❌ smaller community and plugin ecosystem.

## Consequences
### Positive
- Private repos, PR reviews, branch protection, CODEOWNERS, 2FA/SSO.  
- Broad ecosystem (Actions, Dependabot, Secret Scanning).

### Trade-offs / Mitigations
- Lock-in risk → keep code portable; use **mono-repo with clear module boundaries**; regular **org export**.  
