```mermaid
flowchart TD

subgraph Governance & Compliance
  A1(POPIA/GDPR<br>RBAC / RLS<br>Key Mgmt<br>Audit / BC-DR<br>RTO/RPO)
end

subgraph Platform & Edge
  B1(CDN / Edge routing)
  B2(WAF / TLS / DNS)
  B3(Serverless Functions)
  B4(CI/CD & Env Vars)
end

subgraph Observability
  C1(App logs & DB logs)
  C2(Traces / uptime / error budgets)
end

subgraph Application Layer
  D1(Web App - MVP Monolith)
  D2(API Routes / BFF)
  D3(Background jobs / schedulers)
  D4(Feature flags & config)
end

subgraph Integration Layer
  E1(Auth - Supabase)
  E2(Email/SMS - Resend/Twilio)
  E3(KYC/FICA provider)
  E4(Payments/Payouts)
  E5(File AV scanning)
  E6(Caching - Upstash Redis)
end

subgraph Data Layer
  F1(PostgreSQL - Supabase)
  F2(RLS + PII encryption)
  F3(Read replicas)
  F4(Supabase Storage)
  F5(Object lifecycle + AV)
  F6(Point-in-time restore)
end
