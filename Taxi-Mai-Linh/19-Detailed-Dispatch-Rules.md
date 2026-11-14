# Detailed Dispatch Rules & State Handling
**Status:** Draft specification; ranking weights and operational thresholds require Operations approval.

## 1. Dispatch inputs
Required booking facts: booking_id, requested_at, channel, pickup coordinate/address, destination if known, service class, passenger count, accessibility/equipment needs, requested pickup time, customer constraints, quote reference.
Supply facts: driver and vehicle IDs, DIRECT/PARTNER, current availability, last GPS timestamp, location accuracy, vehicle/service eligibility, document/permit status, driver shift/status, active assignment/trip, partner operational status.

## 2. Eligibility gates (hard filters)
A candidate is excluded if any mandatory condition fails:
- Driver account/operational status is not eligible.
- Vehicle is inactive, unavailable, wrong service class, or compliance status is expired/blocked.
- Driver is not available or has an active conflicting assignment/trip.
- GPS fix is stale beyond the approved threshold.
- Candidate cannot satisfy passenger count, accessibility or required equipment.
- PARTNER/contract is suspended or not valid for the service/time/area.
- Candidate is outside configured search boundary.
Thresholds and legal eligibility matrix: TBD by Operations, Fleet and Legal.

## 3. Candidate scoring (configurable, explainable)
After hard filters, calculate a score from approved factors:
- estimated pickup time/distance;
- GPS freshness and location confidence;
- service/vehicle match;
- driver availability duration and workload fairness;
- zone balance and operational constraints;
- fleet type eligibility and contractual constraints.
Weights are TBD. The engine must return score components and exclusion reasons for audit; do not encode hidden preference for DIRECT or PARTNER without approved policy.

## 4. Offer workflow
1. Create dispatch attempt with immutable candidate snapshot and policy version.
2. Send offer to selected candidate(s) according to approved sequential/batched mode.
3. Offer has expiry timestamp and unique offer_id.
4. Accept command is atomic: booking still dispatchable, offer still valid, driver/vehicle still eligible, and no conflicting assignment.
5. First valid accept wins; all competing offers are closed as superseded.
6. Decline, timeout, unreachable or eligibility loss records reason and triggers retry policy.
7. Retry uses updated supply snapshot and records attempt number.
8. If automated attempts exhausted, move to manual intervention queue; do not silently cancel.

## 5. Concurrency and idempotency
- Enforce one active assignment per booking with database constraint/transaction.
- Enforce one active trip/assignment per driver and vehicle where policy requires.
- Use compare-and-set on booking version/state.
- Accept endpoint requires idempotency key. Repeated same key returns original result.
- Different accept requests racing for same booking: exactly one succeeds; others receive conflict response.
- Queue consumers must tolerate at-least-once delivery.

## 6. Manual dispatch
Dispatcher may select a candidate only if hard eligibility gates pass. Override of a soft score ordering requires a reason code and free-text note. Hard-gate override is prohibited unless a separate emergency policy is formally approved and audited.

## 7. State model
Booking: DRAFT → REQUESTED → QUOTED (optional) → DISPATCHING → ASSIGNED → DRIVER_EN_ROUTE → ARRIVED → IN_PROGRESS → COMPLETED.
Alternative terminal states: CANCELLED, EXPIRED, UNFULFILLED. A state may be skipped only where an approved workflow permits it; all transitions are validated server-side.
Offer: CREATED → SENT → ACCEPTED | DECLINED | EXPIRED | CANCELLED | SUPERSEDED.
Trip: CREATED → DRIVER_EN_ROUTE → ARRIVED → STARTED → COMPLETED; exception states: CANCELLED, DISPUTED, NEEDS_REVIEW.
Payment: INITIATED → PENDING → SUCCEEDED | FAILED | CANCELLED | REFUNDED | PARTIALLY_REFUNDED | RECONCILIATION_REQUIRED.

## 8. Exception matrix
| Scenario | System action | Human action |
|---|---|---|
| No eligible supply | retain booking in exception queue; record search coverage | dispatcher contacts customer / approved fallback |
| Driver no response | expire offer; record delivery status | none unless repeated failure |
| Driver cancels after accept | release assignment safely; create new attempt | dispatcher monitors customer impact |
| GPS stale | exclude or downgrade according to approved threshold | contact driver / manual location check |
| Customer changes pickup | version booking; invalidate stale quote/offer as needed | confirm changed terms |
| App offline | queue safe telemetry; prohibit unsafe duplicate command | call center fallback |
| Trip fare disputed | freeze settlement component under dispute policy | Finance/Operations review |

## 9. Operational metrics
Offer acceptance rate, time-to-assign, pickup ETA error, no-supply rate, driver cancellation rate, manual intervention rate, duplicate assignment count (target zero), GPS freshness distribution. Targets TBD.
