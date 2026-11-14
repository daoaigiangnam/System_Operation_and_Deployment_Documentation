# Data Migration, Cutover & Rollback Plan

## 1. Migration principles
- Inventory all source systems and data owners before mapping.
- Migrate only approved data required for launch.
- Preserve source identifiers in crosswalk tables.
- Validate financial data independently with Finance.
- Mask personal data in non-production.
- Do not run destructive source changes during migration rehearsal.

## 2. Data domains
Customers, drivers, vehicles, partner organizations/contracts, tariffs, active bookings, trip history (if required), payment references, balances/settlement open items, users/roles, reference data and audit history. Each domain requires an explicit include/exclude decision.

## 3. Mapping template
| Source entity/field | Target entity/field | Transform | Null/default rule | Owner | Validation |
|---|---|---|---|---|---|
| TBD | TBD | TBD | no silent default | Data owner | count + sample + totals |

## 4. Rehearsal sequence
1. Freeze mapping and extraction scripts.
2. Snapshot source and record extraction watermark.
3. Load reference/master data.
4. Load contracts/tariffs with effective dates and version history.
5. Load customer/supply data and crosswalk IDs.
6. Load open operational/financial items only if approved.
7. Run referential, duplicate, format, timezone and status checks.
8. Reconcile record counts and financial totals.
9. Execute business smoke tests and obtain sign-off.
10. Record duration, defects and corrected scripts; repeat until stable.

## 5. Cutover pattern
- Define maintenance/freeze window and customer communication.
- Take final source snapshot and backup.
- Apply delta since last successful rehearsal.
- Switch traffic only after validation gates pass.
- Monitor booking, dispatch, payment and integration health.
- Keep source read-only during rollback decision window where feasible.

## 6. Rollback triggers
Examples requiring pre-approved thresholds: booking creation failure, assignment concurrency defect, financial imbalance, severe security issue, data corruption, unavailable critical provider, or restore/DR failure. Exact thresholds and decision authority are TBD.

## 7. Rollback safety
Do not blindly reverse external payment/notification effects. Use compensating actions and reconciliation. Preserve all audit evidence. A rollback plan must specify DNS/app rollback, database compatibility, event replay/parking, provider callback handling, and customer/driver communication.

## 8. Sign-off
Data owners, Finance, Operations, IT and Security sign reconciliation and cutover checklist before GO.
