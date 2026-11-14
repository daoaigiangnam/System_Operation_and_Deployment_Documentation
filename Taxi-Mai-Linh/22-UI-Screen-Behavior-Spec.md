# UI Screen Behavior Specification
Wireframes in assets are conceptual. This document defines behavior required before high-fidelity design.

## 1. Global UI standards
- Desktop Control Tower optimized for 1366×768 minimum; responsive layout rules to be confirmed.
- Driver app designed for supported Android/iOS versions to be approved.
- Every screen specifies loading, empty, error, offline, permission-denied and stale-data states.
- Destructive/financial actions require confirmation and reason.
- Display local time consistently; store UTC.
- Use role/permission-based navigation and field masking.
- Map markers include state, last update and accessible non-color labels.

## 2. Screen inventory
| ID | Screen | Primary role | Primary actions |
|---|---|---|---|
| UI-001 | Login/MFA | all users | sign in, recover, MFA |
| UI-002 | Control Tower | dispatcher/supervisor | queue, map, assign, escalate |
| UI-003 | Booking Workspace | call center | create/edit/confirm booking |
| UI-004 | Booking Detail | operations | view timeline, cancel, re-dispatch |
| UI-005 | Driver/Vehicle Registry | fleet admin | create, verify, suspend |
| UI-006 | Partner Registry | partner admin/finance | manage partner profile/status |
| UI-007 | Tariff Management | pricing admin | draft, simulate, approve, activate |
| UI-008 | Trip Detail | operations | milestones, evidence, dispute |
| UI-009 | Payment/Reconciliation | finance | review, reconcile, refund request |
| UI-010 | Partner Settlement | finance | generate, review, approve, export |
| UI-011 | Reports | manager | filter, export, drill-down |
| UI-012 | User/Roles/Audit | security admin | grant/revoke, inspect audit |
| UI-013 | Driver Home | driver | go online/offline, view status |
| UI-014 | Offer Detail | driver | accept/decline with countdown |
| UI-015 | Active Trip | driver | navigate, arrive, start, complete |

## 3. Control Tower (UI-002)
Layout: global header (environment/time/user), KPI strip, left exception/dispatch queue, central map, right selected booking/candidate detail, bottom event timeline.
Filters: zone, booking age, service class, channel, state, fleet type, partner (permission restricted).
Queue columns: reference, requested time, pickup zone, wait duration, service, state, assigned resource, exception.
Actions: open detail, initiate dispatch, manual candidate selection, reassign, escalate, cancel (permission + reason).
Guardrails: never expose unauthorized customer PII; manual selection cannot bypass hard eligibility; stale GPS visibly marked.
Acceptance: queue/map reflect server truth; simultaneous operator changes use optimistic version conflict; all overrides audited.

## 4. Booking Workspace (UI-003)
Sections: customer lookup/create; pickup/dropoff; service class; passenger/requirements; pickup time; notes; quote summary; channel/source metadata.
Validation: pickup required; coordinate/address confidence shown; passenger count within approved service limits; duplicate warning; contact consent captured where required.
Actions: save draft, request quote, confirm booking, cancel draft.
On address edit after quote: invalidate/recalculate quote. Do not silently preserve stale fare.

## 5. Driver app screens (UI-013–015)
Home: online/offline toggle, connectivity, GPS permission, vehicle and shift status.
Offer: pickup summary, service, estimated fare only if policy permits, expiry countdown, accept/decline and decline reason.
Active trip: route, call/mask contact, arrive, start, complete; emergency/support action.
Offline: clearly show queued vs server-confirmed actions. Accept/start/complete must not appear confirmed until server acknowledges; duplicate submissions use idempotency key.
Safety: minimize interaction while moving; no forced text entry during driving; legal review required.

## 6. Finance screens
Tariff: versioned draft, effective period, component table, simulation, four-eyes approval, activation and rollback by new version (never edit historical version).
Payment: provider status, internal status, amount, currency, reference, reconciliation status; refund is a separately authorized command.
Settlement: period, contract snapshot, gross, deductions, adjustments, net payable, disputes, approval chain, export checksum.

## 7. Design handoff package
Figma pages by role; component library; tokens; responsive breakpoints; interaction prototype; field-level validation; accessibility; localization; permission matrix; annotated API bindings; error-state designs. Wireframe SVGs are not production UI.
