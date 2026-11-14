# Role & Permission Matrix
**Principle:** least privilege, separation of duties, explicit scope by branch/zone/partner, and auditable privileged actions.

| Capability | Call Center | Dispatcher | Supervisor | Fleet Admin | Finance | Partner User | Security Admin |
|---|---:|---:|---:|---:|---:|---:|---:|
| Create booking | Yes | Limited | Yes | No | No | Scoped TBD | No |
| View customer PII | Minimum | Minimum | Scoped | No | Masked | Own bookings only TBD | No by default |
| View dispatch queue | No | Yes | Yes | No | No | Own scope TBD | No |
| Manual assign | No | Yes | Override approval | No | No | No | No |
| Cancel booking | Request | Policy | Approve exception | No | No | Own request TBD | No |
| Manage driver/vehicle | No | Read | Read | Yes | Read limited | Own fleet TBD | No |
| Manage tariff | No | No | Read | No | Review | No | No |
| Approve tariff | No | No | Business approver TBD | No | Finance approver TBD | No | No |
| View payment | No | Limited | Read | No | Yes | Own statement only TBD | No |
| Refund | No | No | Request/approve TBD | No | Dual control TBD | No | No |
| Generate settlement | No | No | Read | No | Yes | Own statement view TBD | No |
| Approve settlement | No | No | No | No | Separate approver TBD | No | No |
| Manage users/roles | No | No | No | No | No | No | Yes |
| View audit logs | No | Scoped | Scoped | Scoped | Finance scope | Own scope TBD | Yes |

## Authorization rules
- Backend authorization is mandatory; hiding a UI button is not security.
- Enforce tenant/partner/branch/zone scope at query and command level.
- Sensitive reads are audited where policy requires.
- Break-glass access is time-bound, reason-coded, alerted and reviewed.
- User provisioning, role change and deactivation require audit.
- Finance maker/checker: creator cannot approve own tariff, refund or settlement unless a formally approved exception exists.
- Service accounts have narrow scopes, rotation and owner.

## Decisions required
Business owners must approve exact roles, data scopes, refund limits, tariff approval chain, settlement approval chain, emergency access and partner portal capabilities.
