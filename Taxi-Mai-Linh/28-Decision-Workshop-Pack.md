# Business Decision Workshop Pack
This pack converts unknown policy into explicit decisions. No default value becomes production policy without owner approval.

| ID | Decision | Required owner(s) | Options to evaluate | Evidence required |
|---|---|---|---|---|
| DEC-001 | MVP channels | Business/Product | hotline only; hotline+app; all channels | volume, launch scope |
| DEC-002 | DIRECT/PARTNER eligibility | Operations/Fleet/Legal | common pool with constraints; zoned pool; service-specific | contracts, fleet rules |
| DEC-003 | Dispatch ranking | Operations | ETA-first; fairness; zone balancing; weighted score | simulation/backtest |
| DEC-004 | Offer mode and timeout | Operations | sequential/batched; timeout by service/zone | driver response data |
| DEC-005 | Search radius/retry | Operations | fixed/expanding radius; bounded retries | coverage and demand data |
| DEC-006 | Fare tariff | Pricing/Finance | distance/time/base/other approved components | signed tariff examples |
| DEC-007 | Cancellation/no-show | Operations/Finance/Legal | fee rules and evidence | customer policy/contracts |
| DEC-008 | PARTNER economics | Finance/Legal/Partner Ops | commission/share/deductions/tax treatment | signed contract templates |
| DEC-009 | Payment methods/refunds | Finance | cash/card/QR/wallet if approved | provider and reconciliation |
| DEC-010 | Settlement period/approval | Finance | daily/weekly/monthly; maker-checker | finance SOP |
| DEC-011 | SLA/KPI | Business/Operations | booking, dispatch, pickup, trip, complaint | baseline performance |
| DEC-012 | Scale targets | IT/Business | active drivers, bookings peak, concurrent users | forecast |
| DEC-013 | Availability/RPO/RTO | IT/Business | criticality tiers | BIA |
| DEC-014 | Map/GPS/SMS/voice vendors | IT/Procurement | candidate providers | cost, coverage, SLA |
| DEC-015 | ERP/invoice integration | Finance/IT | API, batch, manual first release | system capability |
| DEC-016 | Retention/privacy | Legal/Security | data-class retention schedule | legal assessment |

## Workshop output
For every decision record: final choice, rejected options, rationale, effective date, owner, approver, impacted requirements/APIs/tables/tests, and change history. Update Decision Register and Traceability Matrix after approval.
