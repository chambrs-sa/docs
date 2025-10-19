# ADR-002: Lovable as Initial Development Platform
**Status:** Accepted  
**Date:** 2025-10-02  
**Owner:** Product Engineering  
**Related:** ADR-Monolith-First, ADR-Data & Supabase, ADR-Compliance Banner, ADR-Observability

## Context
- Need fastest path to a usable MVP to validate investor workflows.  
- Team is small; infra capacity limited.  
- Desire to stay close to React/Supabase so code can be ported later.

## Decision
Use **Lovable** to generate the MVP (React front-end + hosted backend services it provisions), while **preserving portability** of generated code. The project is built with:
- Vite
- TypeScript
- React
- shadcn-ui
- Tailwind CSS  
Scope: MVP and early pilots; critical flows (auth, investor onboarding, portfolio views).

## Options considered
- **Lovable** — ✅ speed, scaffolding, hosting; ❌ ecosystem lock-in, advanced customization friction.  
- **Next.js + Supabase (hand-rolled)** — ✅ control; ❌ slower initial velocity.  
- **Flutter + Firebase** — ✅ fast mobile; ❌ diverges from web-first plan.

## Consequences
### Positive
- Rapid iteration from prompts; built-in auth, DB, hosting.  
- Focus on PMF vs boilerplate.

### Trade-offs / Mitigations
- Vendor dependence → **export code to GitHub repo after each milestone**; avoid proprietary features where alternatives exist.  
- Limited customization → plan **Phase-2 port** to standalone React/Next.js if needed.  
- Compliance nuance → add **custom compliance banner & disclosures**, ensure **data location settings** documented.

## Security & Compliance
- **Controls:** Configure auth providers, password policy, 2FA; secret management via platform vault; POPIA data minimization; display **FSCA/FSP/ODP** disclaimer across app; logging without PII by default.  
- **Evidence:** Environment configuration snapshots, banner text versioned, DPA/ToS archive, data-flow diagram.

## Revisit when
- Monthly Active Users > 2,000 **or** need custom IAM/roles/edge compute **or** FSCA due-diligence requires tighter infra control → plan migration to **React/Next.js + Supabase**.

## Implementation notes
- Lock a **“Lovable export”** GitHub repo; enforce PRs for prompt-generated changes.  
- Create a **compatibility checklist** (no platform-specific APIs without equivalents).  
- Add **telemetry hooks** (OpenTelemetry/HTTP logs) and feature flagging from day one.
