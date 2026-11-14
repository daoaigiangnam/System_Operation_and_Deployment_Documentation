# Non-Functional Requirements, Capacity & Resilience
**Status:** Target values are not invented. This is a measurable requirement worksheet.

## 1. SLO catalogue to approve
| Dimension | Measure | Target |
|---|---|---|
| Availability | monthly service availability by critical journey | TBD |
| Booking latency | p95 create booking response | TBD |
| Dispatch latency | p95 time from request to first offer | TBD |
| Map freshness | p95 location age displayed | TBD |
| API reliability | 5xx rate by endpoint | TBD |
| Mobile reliability | crash-free sessions | TBD |
| Recovery | RTO/RPO by service/data class | TBD |
| Support | incident response/restore time | TBD |

## 2. Capacity model inputs
Collect peak and average:
- bookings/hour and peak concurrent booking creation;
- active drivers and GPS update interval;
- offers per booking and retry distribution;
- concurrent Control Tower users and map subscriptions;
- trip events per second;
- payment callbacks and settlement batch size;
- retention volumes for GPS, logs, evidence and audit.
Capacity must include seasonal/event peak and growth horizon approved by business.

## 3. Load-test scenarios
1. Baseline steady-state for a representative peak period.
2. 2× and 3× forecast peak (final multiplier approved by architect/business).
3. Sudden booking spike with constrained supply.
4. GPS telemetry burst and delayed/out-of-order messages.
5. Provider latency/timeouts for map, SMS/voice and payment.
6. Database failover and broker backlog recovery.
7. Control Tower map/queue fan-out with many concurrent operators.
8. Settlement batch with production-like history volume.

## 4. Resilience patterns
- Timeouts, bounded retries with exponential backoff and jitter.
- Circuit breakers and bulkheads for external providers.
- Idempotent commands and deduplicated event consumers.
- Transactional outbox/inbox.
- Dead-letter queue with alert, replay approval and audit.
- Graceful degradation: manual dispatch and phone fallback if automation unavailable.
- No silent data loss for trip milestones, payments or ledger events.

## 5. Backup/DR
Define RPO/RTO by criticality. Backups encrypted, immutable/offline copy where feasible, separate credentials, monitored success and periodic restore test. DR runbook must cover DNS/network, database, broker, object storage, secrets, integrations and client access. A successful backup job is not proof of recoverability.

## 6. Observability
Structured logs with correlation_id; metrics by service/endpoint/state; distributed traces; audit stream; synthetic checks. Avoid PII/secrets in logs. Alert on booking failure, dispatch backlog, stale GPS, payment reconciliation mismatch, settlement imbalance, queue lag, DB saturation, certificate expiry and backup failure.

## 7. Security baseline
Threat model STRIDE; TLS in transit; encryption at rest; managed secrets; MFA for privileged roles; secure SDLC; dependency/container scanning; SAST/DAST; penetration test; patch SLAs; least privilege; audit retention; privacy impact assessment. Legal/Compliance approves applicable Vietnamese privacy, transport, payment and retention obligations.
