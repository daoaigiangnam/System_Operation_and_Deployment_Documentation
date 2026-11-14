# Implementation Blueprint — Taxi Mai Linh
**Version:** 1.0-draft | **Status:** Development planning baseline, pending business/architecture approval

## 1. Purpose and delivery boundary
This blueprint turns the v0.9 business and system documentation into implementation work packages. It does not authorize production configuration where a decision remains TBD.

### In scope
- Unified booking intake from hotline, customer app, web and approved partner/API channels.
- Unified dispatch orchestration for DIRECT and PARTNER supply.
- Driver mobile workflow, trip lifecycle, fare calculation, payment recording.
- Separate DIRECT and PARTNER contractual/financial accounting.
- Control Tower, customer/driver/vehicle/partner administration, reporting, audit and integrations.

### Explicitly out of scope until approved
- Autonomous dynamic pricing, surge pricing, loyalty, wallet/stored value, cashless acquiring vendor, tax invoice provider, third-party fleet onboarding rules, cross-border trips.
- Any legal or contractual interpretation not approved by Mai Linh Legal/Finance/Operations.

## 2. Bounded contexts and ownership
| Context | Owns | Must not own |
|---|---|---|
| Identity & Access | users, roles, sessions, access policy | driver employment/partner contracts |
| Customer | customer profile, consent, saved locations | trip pricing |
| Supply Registry | fleet type, driver, vehicle, eligibility | dispatch ranking |
| Booking | request, channel, pickup/dropoff, lifecycle | settlement |
| Dispatch | candidate evaluation, offers, assignment | fare ledger |
| Trip | accepted assignment, milestones, trip evidence | partner payout calculation |
| Pricing | tariff versions, quote, fare calculation | payment collection |
| Payment | payment intent/attempt/status/reconciliation | tariff authoring |
| Finance Ledger | immutable financial entries, adjustments | operational dispatch |
| Partner Settlement | partner statements, deductions, payable | direct payroll |
| Notification | templates, delivery attempts | source-of-truth trip state |
| Reporting | read models and aggregates | transactional writes |

Each context owns its data. Cross-context changes use versioned APIs/events, not direct database writes.

## 3. Canonical identifiers
Use opaque UUID/ULID identifiers. Public booking reference is a separate human-readable code and must not expose sequential database IDs.
- booking_id, dispatch_id, offer_id, assignment_id, trip_id
- customer_id, driver_id, vehicle_id, fleet_owner_id, partner_id
- tariff_version_id, quote_id, payment_id, ledger_entry_id, settlement_id
- correlation_id, idempotency_key, event_id

Identifier format and database-native type are architecture decisions; UUID is a recommendation, not a confirmed constraint.

## 4. Mandatory invariants
1. A booking channel never implies fleet type.
2. Every assignment references one eligible driver and one eligible vehicle at assignment time.
3. One active assignment at most per booking; one active trip at most per driver/vehicle unless a specifically approved multi-stop model says otherwise.
4. A quote stores the tariff version and calculation inputs used.
5. A completed trip's financial facts are immutable; corrections are compensating entries.
6. PARTNER settlement is derived from approved contract snapshots and ledger entries, never from mutable current contract settings.
7. Every state transition records actor, timestamp, reason, correlation ID and source.
8. Retried commands must be idempotent.
9. A payment provider callback is authenticated, deduplicated and reconciled.
10. No production launch while critical policy decisions are unresolved.

## 5. Reference service topology
Clients → API Gateway/BFF → domain services → relational transaction stores; asynchronous events → message broker → notification/integration/reporting consumers. Geospatial search may use a dedicated geo index, but authoritative driver/vehicle eligibility remains in Supply Registry.
- Synchronous calls: commands requiring immediate validation and response.
- Asynchronous events: notifications, analytics, partner export, non-blocking integrations.
- Outbox pattern: publish committed domain events reliably.
- Inbox/deduplication: prevent duplicate event side effects.
- Saga/process manager: booking-dispatch-trip-payment workflows across contexts.

## 6. Environment and release gates
| Environment | Purpose | Data |
|---|---|---|
| Local | developer loop | synthetic |
| DEV | shared integration | synthetic |
| SIT | cross-service testing | synthetic/masked |
| UAT | business acceptance | approved masked/test |
| Staging | production-like rehearsal | masked |
| Production | live operations | production only |

Secrets are environment-specific. No production PII or payment secrets in lower environments.

## 7. Definition of Development Ready
- Business rules are approved or explicitly excluded from the first release.
- API schemas and event contracts are versioned.
- ERD and migration scripts reviewed.
- UI states include loading, empty, error, permission denied, offline and conflict.
- Acceptance criteria are testable.
- Observability, audit and rollback are designed.
- Each TBD has owner and target decision date.

## 8. Release slicing (proposed)
R0 Foundation: identity, audit, reference data, supply registry.
R1 Booking: intake, quote placeholder, booking lifecycle.
R2 Dispatch: candidate search, offer, accept/decline, manual dispatch.
R3 Trip: driver app, trip milestones, GPS evidence.
R4 Fare/payment: tariff engine, payment recording, reconciliation.
R5 Finance: DIRECT accounting, PARTNER contract snapshot and settlement.
R6 Operations: Control Tower, reports, alerts, runbooks, hardening.
Release order is proposed; dependencies and business priority must be confirmed.

## 9. Go/no-go
No-go if: unresolved critical security finding; no tested restore; payment/ledger reconciliation fails; duplicate assignment can occur; dispatch cannot be manually recovered; critical UAT cases fail; support/on-call owner absent.
