# Delivery Backlog & Sprint Plan
**Planning basis:** proposed work breakdown, not a committed schedule. Team capacity and vendor dependencies are not yet supplied.

## Epic breakdown
| Epic | Scope | Dependency | Exit evidence |
|---|---|---|---|
| EP-01 Platform foundation | repo, CI/CD, environments, secrets, observability | architecture | deployable service, pipeline evidence |
| EP-02 Identity & RBAC | login, MFA, roles, scopes, audit | EP-01 | authorization tests |
| EP-03 Supply registry | driver, vehicle, partner, eligibility | EP-02 | approved master data workflow |
| EP-04 Booking | channels, stops, validation, lifecycle | EP-02 | booking E2E tests |
| EP-05 Pricing | tariff versioning, quote, simulation | policy decisions | Finance-approved examples |
| EP-06 Dispatch | candidate, offer, accept, retries, manual fallback | EP-03/04 | race/concurrency tests |
| EP-07 Driver app | online, offer, milestones, offline states | EP-03/06 | device/UAT tests |
| EP-08 Trip | state, evidence, exceptions | EP-06/07 | trip lifecycle E2E |
| EP-09 Payment | collection, callback, refund, reconciliation | EP-05/08 | provider sandbox and reconciliation |
| EP-10 Ledger & settlement | journal, partner contract snapshot, statements | EP-09 | Finance sign-off |
| EP-11 Control Tower | queue, map, filters, manual dispatch | EP-04/06/08 | dispatcher UAT |
| EP-12 Reporting | operational/financial read models | domain events | metric reconciliation |
| EP-13 Hardening & launch | load, security, DR, cutover, support | all | go/no-go pack |

## Sprint planning rules
- Sprint duration and velocity TBD after team assessment.
- Prioritize vertical slices that can be tested end-to-end.
- No story enters sprint without acceptance criteria, API/data dependency and UX state.
- Each story includes security, observability and audit considerations.
- Definition of Done: code review, automated tests, contract tests, migration reviewed, telemetry, documentation, QA evidence, no critical defect.

## Example user stories
- As a call-center agent, I can create a booking and see a stable public reference.
- As a dispatcher, I can see eligible candidates and why a candidate is excluded.
- As a driver, I can accept an unexpired offer and receive a server-confirmed assignment.
- As Finance, I can reconcile payment and ledger totals by trip and period.
- As Finance, I can generate a partner statement using the contract version applicable to each trip.
- As an auditor, I can trace who changed a booking, tariff, payment or settlement and why.

## Planning outputs required
Team roster and skills, vendor lead times, migration inventory, MVP boundary, sprint capacity, dependency map, delivery calendar, risk register and budget approval.
