# Data Dictionary & Integrity Constraints
This document defines logical fields. Physical DB engine, partitioning and exact SQL types are Architecture decisions.

## 1. Core entity dictionary
| Entity | Required fields | Integrity / notes |
|---|---|---|
| Customer | id, status, created_at | PII access restricted; soft deletion policy TBD |
| Booking | id, public_reference, channel, customer_id, state, version, requested_at | channel enum independent of fleet_type |
| BookingStop | id, booking_id, sequence, type, address, lat, lng | sequence unique per booking |
| Driver | id, status, fleet_type, partner_id nullable | PARTNER requires partner_id; DIRECT partner_id null |
| Vehicle | id, plate_normalized, status, fleet_type, partner_id nullable | plate uniqueness scope TBD |
| DriverVehicleAssignment | id, driver_id, vehicle_id, valid_from, valid_to | no overlapping active assignment unless approved |
| DispatchAttempt | id, booking_id, attempt_no, policy_version, state | unique booking_id + attempt_no |
| DriverOffer | id, dispatch_attempt_id, driver_id, vehicle_id, expires_at, state | deduplicate active offer per attempt/driver |
| Assignment | id, booking_id, offer_id, driver_id, vehicle_id, assigned_at, state | one active assignment per booking |
| Trip | id, booking_id, assignment_id, state, started_at, completed_at | unique booking assignment relationship |
| TariffVersion | id, code, version, valid_from, valid_to, status | immutable after activation |
| FareQuote | id, booking_id, tariff_version_id, amount_minor, currency, expires_at | immutable input snapshot |
| FareBreakdown | id, quote/trip, component_code, amount_minor, metadata | sum must reconcile to total |
| Payment | id, trip_id, method, amount_minor, currency, status | provider reference unique where applicable |
| LedgerEntry | id, trip_id, account_code, debit_minor, credit_minor, currency, entry_type | append-only; balanced journal |
| PartnerContractVersion | id, partner_id, version, valid_from, valid_to, terms_snapshot | immutable once used |
| Settlement | id, partner_id, period_start, period_end, state, version | unique partner + period + version policy |
| SettlementLine | id, settlement_id, ledger_entry_id, amount_minor, line_type | no duplicate ledger entry in same statement |
| AuditEvent | id, actor_id, action, resource_type, resource_id, occurred_at, correlation_id | append-only, tamper-evident controls |

## 2. Required indexes (logical)
- Booking: public_reference unique; (state, requested_at); (customer_id, created_at).
- BookingStop: unique (booking_id, sequence).
- Driver/Vehicle: (status, fleet_type, partner_id).
- GPS latest-position store: geospatial index + observed_at; retention policy TBD.
- DispatchAttempt: unique (booking_id, attempt_no).
- Offer: (driver_id, state, expires_at), (dispatch_attempt_id, state).
- Assignment: partial/filtered unique active booking and driver/vehicle constraints as supported by DB.
- Trip: (state, started_at), unique assignment_id.
- LedgerEntry: (trip_id), (account_code, posted_at), unique source_event_id + entry_type.
- Settlement: (partner_id, period_start, period_end, state).

## 3. Money and precision
Store integer minor units and currency code. Currency scale must come from currency metadata; do not assume all currencies have two decimal places. Amount signs and debit/credit conventions must be documented in Finance chart of accounts.

## 4. Data lifecycle
Define retention per data class: customer PII, GPS, call recording, trip evidence, financial ledger, security logs. Retention, legal hold, deletion/anonymization and backup expiry require Legal/Compliance approval. Do not delete financial audit trail to satisfy a user deletion request; apply approved anonymization/retention process.

## 5. Integrity rules
- Foreign keys for core ownership relationships.
- Check constraints for non-negative amounts where applicable.
- State transitions validated in domain service and persisted atomically.
- Optimistic locking/version column on mutable aggregates.
- Append-only audit and ledger tables; correction via reversal/adjustment entries.
- Referential integrity for tariff and contract snapshots used by completed trips.
- Time ranges use half-open intervals [valid_from, valid_to).

## 6. Data migration
Map source IDs to target IDs using a migration crosswalk. Validate row counts, financial totals, referential integrity, duplicates, encoding, timezone, status mapping and PII handling. Reconciliation must be signed by data owner.
