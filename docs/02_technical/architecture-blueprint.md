+---------------------------------------------------------------+
|                Governance & Compliance Layer                  |
|   (POPIA/GDPR, RBAC, RLS, Key Mgmt, Audit, BC/DR, RTO/RPO)    |
+---------------------------+-----------------------------------+
|     Platform & Edge       |         Observability             |
|  (Lovable Cloud Runtime)  | (Sentry, Logflare, Metrics/Alrt)  |
|  - CDN / Edge routing     | - App logs & DB logs              |
|  - WAF / TLS / Custom DNS | - Traces, uptime, error budgets   |
|  - Serverless Functions   |                                   |
|  - CI/CD & Env Vars       |                                   |
+---------------------------+-----------------------------------+
|                        Application Layer                      |
|  - Web App (MVP Monolith now; microservices-ready later)     |
|  - API Routes / BFF (on Lovable functions)                    |
|  - Background jobs / schedulers (Lovable cron or 3rd-party)   |
|  - Feature flags & config                                     |
+---------------------------------------------------------------+
|                         Integration Layer                     |
|  - Auth (Supabase Auth)   - Email/SMS (e.g., Resend/Twilio)   |
|  - KYC/FICA provider      - Payments/Payouts                  |
|  - File AV scanning       - Caching (e.g., Upstash Redis)     |
+---------------------------+-----------------------------------+
|           Data Layer             |      Storage & Backups     |
|  - PostgreSQL (Supabase)         |  - Supabase Storage        |
|  - RLS + PII encryption (Vault)  |  - Object lifecycle + AV   |
|  - Read replicas (if needed)     |  - Point-in-time restore   |
+----------------------------------+----------------------------+
