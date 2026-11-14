# API Contracts & Examples
**Contract style:** REST/JSON for synchronous commands and queries; asynchronous domain events for integration. Exact base URL, auth provider and gateway are TBD.

## 1. Common conventions
- Prefix: /api/v1
- Content-Type: application/json; UTF-8.
- Authentication: OAuth2/OIDC access token (provider TBD); service-to-service identity for internal calls.
- Every mutating request includes Idempotency-Key.
- Every request propagates X-Correlation-ID.
- Timestamps use ISO 8601 UTC; store UTC, render in configured local timezone.
- Money uses integer minor units plus ISO currency code; never binary floating point.
- Coordinates use WGS84 latitude/longitude with explicit validation.
- Pagination uses cursor; sort order must be stable.
- Error envelope: code, message, details, correlation_id, retryable.

## 2. Error envelope
```json
{
  "error": {
    "code": "BOOKING_STATE_CONFLICT",
    "message": "Booking is no longer dispatchable",
    "details": {"booking_id": "uuid", "current_state": "ASSIGNED"},
    "correlation_id": "uuid",
    "retryable": false
  }
}
```
Do not expose stack traces, SQL errors, secrets or internal hostnames.

## 3. Create booking
`POST /api/v1/bookings`
```json
{
  "channel": "HOTLINE",
  "customer_id": "uuid",
  "pickup": {"address": "string", "lat": 10.77, "lng": 106.69, "place_id": null},
  "dropoff": {"address": "string", "lat": 10.80, "lng": 106.70},
  "service_class": "STANDARD",
  "passenger_count": 2,
  "requested_pickup_at": null,
  "notes": null
}
```
Response: 201 with booking_id, public_reference, state, created_at, version. Fleet type is not supplied by channel and is decided by eligibility/dispatch policy.

## 4. Quote fare
`POST /api/v1/quotes`
Request: booking_id, pickup, dropoff, service_class, optional route_distance/duration only if sourced from trusted routing service. Response: quote_id, currency, estimated_amount_minor, breakdown[], tariff_version_id, valid_until, assumptions[].
A quote is an estimate unless policy explicitly marks it guaranteed.

## 5. Start dispatch
`POST /api/v1/bookings/{booking_id}/dispatch`
Request: policy_version, mode (AUTO|MANUAL), optional operator_note. Response: dispatch_id, state, attempt_no, candidate_count, created_at.

## 6. Driver offer response
`POST /api/v1/offers/{offer_id}/response`
Request: response (ACCEPT|DECLINE), reason_code. Response: offer state, booking state, assignment_id if accepted. Server validates expiry, eligibility and concurrency atomically.

## 7. Trip milestone
`POST /api/v1/trips/{trip_id}/milestones`
Request: milestone (EN_ROUTE|ARRIVED|STARTED|COMPLETED), occurred_at, location, odometer optional, evidence references optional. Server records received_at separately from occurred_at. Device time is untrusted.

## 8. Payment record
`POST /api/v1/payments`
Request: trip_id, method, amount_minor, currency, provider_reference optional, idempotency_key. Response: payment_id, status, amount_minor, currency. Provider callback is a separate authenticated endpoint.

## 9. Partner settlement
`POST /api/v1/partner-settlements/{settlement_id}/approve`
Request: approval_note, statement_version. Only authorized Finance role; approval checks statement totals, disputes and duplicate approval.

## 10. Webhook/event envelope
```json
{
  "event_id": "uuid",
  "event_type": "booking.assigned.v1",
  "occurred_at": "2026-11-14T04:00:00Z",
  "aggregate_id": "uuid",
  "aggregate_version": 7,
  "correlation_id": "uuid",
  "producer": "dispatch-service",
  "data": {}
}
```
Consumers deduplicate by event_id and reject stale aggregate versions where ordering matters.

## 11. Initial endpoint inventory
| Domain | Endpoint | Permission |
|---|---|---|
| Booking | POST/GET /bookings | booking:create/read |
| Booking | POST /bookings/{id}/cancel | booking:cancel |
| Pricing | POST /quotes | quote:create |
| Dispatch | POST /bookings/{id}/dispatch | dispatch:execute |
| Dispatch | GET /dispatch/queue | dispatch:read |
| Dispatch | POST /offers/{id}/response | driver:offer-response |
| Trip | POST /trips/{id}/milestones | trip:update |
| Payment | POST /payments | payment:create |
| Finance | GET /ledger/entries | finance:read |
| Settlement | GET /partner-settlements | settlement:read |
| Settlement | POST /partner-settlements/{id}/approve | settlement:approve |

## 12. Contract completion checklist
Before implementation freeze: OpenAPI 3.1 file, schemas, enum registry, auth scopes, rate limits, PII classification, error catalogue, pagination, idempotency retention, webhook signature/replay protection, versioning and backward compatibility tests.
