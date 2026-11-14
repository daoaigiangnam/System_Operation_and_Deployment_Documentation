# QA Test Catalog & Acceptance Scenarios
Detailed executable test data and expected results must be maintained in the test-management tool selected by Mai Linh.

## 1. Critical end-to-end tests
| ID | Scenario | Expected result |
|---|---|---|
| E2E-001 | Hotline creates booking | booking persisted with channel HOTLINE; fleet type remains undecided |
| E2E-002 | APP creates booking | same booking lifecycle; channel APP recorded |
| E2E-003 | Eligible DIRECT candidate exists | candidate evaluated under approved policy |
| E2E-004 | Eligible PARTNER candidate exists | partner contract eligibility validated and recorded |
| E2E-005 | DIRECT and PARTNER both eligible | selection follows approved neutral/configured policy; reason traceable |
| E2E-006 | Two drivers accept simultaneously | exactly one assignment succeeds |
| E2E-007 | Offer expires before accept | accept rejected as expired; retry/manual queue follows policy |
| E2E-008 | Driver becomes ineligible after offer | accept rejected; reason recorded |
| E2E-009 | Dispatcher manually assigns | hard gates enforced; override reason audited |
| E2E-010 | Pickup changed after quote | old quote invalidated or explicitly revalidated |
| E2E-011 | Trip completed twice by retry | one completion effect and one financial posting |
| E2E-012 | Payment callback delivered twice | duplicate ignored; one payment state transition |
| E2E-013 | Payment amount mismatch | reconciliation exception; no false success |
| E2E-014 | PARTNER settlement generated | contract snapshot and ledger lines reconcile |
| E2E-015 | Settlement approved twice | second approval safely rejected/idempotently returns result |
| E2E-016 | Unauthorized partner reads another partner data | denied and security event logged |
| E2E-017 | GPS stale/offline | state visibly stale; candidate eligibility policy applied |
| E2E-018 | Message broker unavailable | outbox persists events; eventual delivery after recovery |
| E2E-019 | Database failover | transaction integrity maintained within approved RTO/RPO |
| E2E-020 | Restore backup | documented restore passes data-integrity and application smoke tests |

## 2. Security tests
- IDOR/BOLA across booking, trip, driver, partner and settlement IDs.
- Privilege escalation and role-scope bypass.
- Session fixation, token expiry/revocation, MFA enforcement.
- Injection, XSS, CSRF where applicable, SSRF, unsafe file upload.
- Webhook signature forgery and replay.
- Rate-limit bypass and enumeration.
- PII leakage in logs, exports, errors and analytics.
- Secrets in source/build artifacts.

## 3. Financial reconciliation tests
- Fare breakdown sum equals trip fare.
- Payment captures/refunds reconcile to provider statement.
- Ledger debits equal credits per journal and currency.
- Partner gross, commission/deductions, adjustments and net payable reconcile to contract version.
- Reversal entries reference original entries.
- Rounding and currency precision tested.
- Dispute/hold excludes affected amount according to approved policy.

## 4. Acceptance gate
- 100% pass for approved critical severity test cases.
- No open critical/high security defect.
- No duplicate assignment or duplicate financial posting.
- Financial reconciliation has signed Finance evidence.
- Restore test and DR tabletop completed.
- UAT sign-off from Operations, Finance, Customer Service, Fleet/Partner and IT.
- Known limitations accepted with owner and due date.
