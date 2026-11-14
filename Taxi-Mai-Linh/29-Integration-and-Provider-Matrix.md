# External Integration & Provider Matrix

| Integration | Purpose | Direction | Failure behavior | Selection criteria | Provider |
|---|---|---|---|---|---|
| Maps/routing | geocoding, route, ETA | request/response | cache, fallback/manual address | Vietnam coverage, traffic, cost, SLA | TBD |
| Driver GPS | device location | mobile → platform | stale marker, queue safe telemetry | accuracy, battery, privacy | TBD |
| SMS/Voice/PBX | OTP/customer/driver notification, hotline | bidirectional | retry, alternate channel, call-center fallback | delivery, latency, reporting, SLA | TBD |
| Push notification | driver offers/status | platform → mobile | retry, polling fallback | Android/iOS delivery | TBD |
| Payment gateway | collection/refund status | bidirectional | pending/reconciliation queue | licensing, security, fees, settlement | TBD |
| ERP/accounting | finance posting | bidirectional/batch | outbox and reconciliation | API, chart of accounts, audit | TBD |
| E-invoice | invoice issuance | platform → provider | retry/manual exception | legal fit, API, archive | TBD |
| Identity provider | authentication/MFA | bidirectional | fail closed for privileged access | OIDC/SAML, lifecycle | TBD |
| Analytics/BI | operational and financial reporting | event/batch | replayable pipelines | access control, freshness | TBD |

## Integration contract requirements
For each selected provider record: legal entity, product/SKU, environment URLs, auth method, scopes, rate limits, timeout, retry policy, idempotency, webhook signature, IP allowlist if applicable, data residency, PII handling, SLA, support contact, incident escalation, pricing, contract expiry and exit/migration plan.

## Provider onboarding test
Connectivity → auth → happy path → invalid request → timeout → retry → duplicate callback → provider outage → reconciliation → credential rotation → production readiness sign-off.
