# Production Readiness & Go-Live Checklist

## Governance
- [ ] BRD scope and MVP signed by Business Owner.
- [ ] DIRECT/PARTNER operating model signed by Operations, Fleet, Finance and Legal.
- [ ] Tariff, cancellation, no-show, refund, partner commission and deductions approved.
- [ ] SLA/KPI and customer communication policy approved.
- [ ] Data retention/privacy/legal assessment approved.

## Product and engineering
- [ ] All critical requirements traced to code and tests.
- [ ] API/event contracts versioned and backward compatibility checked.
- [ ] Database migrations reviewed, rehearsed and rollback/forward-fix plan documented.
- [ ] Concurrency/idempotency tests pass.
- [ ] No known critical/high security defects.
- [ ] Load test passes approved peak and growth assumptions.
- [ ] Mobile app tested on supported devices/OS versions.
- [ ] Accessibility, localization and error states reviewed.

## Finance and integrations
- [ ] Payment provider certification/sandbox evidence complete.
- [ ] Callback signature, replay defense and reconciliation verified.
- [ ] Fare examples signed by Pricing/Finance.
- [ ] Ledger balanced; refunds/reversals tested.
- [ ] Partner settlement sample statements signed by Finance and partner representative.
- [ ] Invoice/tax integration approved if in scope.

## Operations
- [ ] Control Tower staffing and escalation roster published.
- [ ] Manual dispatch and call-center fallback rehearsed.
- [ ] Monitoring dashboards and actionable alerts enabled.
- [ ] On-call rota, incident severity and communication templates published.
- [ ] Backup restore test and DR exercise passed.
- [ ] Runbooks and access break-glass process reviewed.
- [ ] Support training and knowledge base completed.

## Data and cutover
- [ ] Source-to-target mapping signed by data owners.
- [ ] Trial migration reconciled.
- [ ] Freeze window, delta migration and rollback criteria approved.
- [ ] DNS/app release and vendor coordination confirmed.
- [ ] Customer/driver/partner communication plan approved.
- [ ] Hypercare owners and exit criteria assigned.

## Go/no-go record
Decision: GO / NO-GO
Date/time:
Business Owner:
Operations:
Finance:
IT/Architecture:
Security:
Outstanding risks accepted:
Rollback trigger and decision owner:
