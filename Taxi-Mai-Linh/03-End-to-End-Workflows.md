# 03 — END-TO-END BUSINESS WORKFLOWS

## 1. Booking → Dispatch → Trip → Payment

```mermaid
flowchart TD
 A["Customer / Hotline / App / Web / API"] --> B["Create booking"]
 B --> C{"Validation passed?"}
 C -- No --> C1["Field errors; retain entered data"]
 C1 --> B
 C -- Yes --> D["CONFIRMED / READY_FOR_DISPATCH"]
 D --> E["Dispatch candidate search"]
 E --> F{"Eligible candidate?"}
 F -- No --> G["Exception queue / Supervisor"]
 G --> E
 F -- Yes --> H["Driver offer"]
 H --> I{"Accept before expiry?"}
 I -- No --> E
 I -- Yes --> J["ASSIGNED"]
 J --> K["EN_ROUTE_TO_PICKUP"]
 K --> L["ARRIVED"]
 L --> M["IN_TRIP"]
 M --> N["COMPLETED"]
 N --> O["Fare finalization"]
 O --> P["Payment / Ledger"]
 P --> Q["Reporting / CS / Settlement"]
```

## 2. Booking tức thời — Tổng đài

1. Agent nhận cuộc gọi, tìm khách theo số liên hệ, tạo hồ sơ tối thiểu nếu chưa có.
2. Nhập pickup; chọn geocoding suggestion; xác nhận pin và hướng dẫn đón.
3. Nhập destination và ordered stop list nếu được phép.
4. Chọn service/vehicle type, thời gian, hành khách, hành lý, ghi chú.
5. Validation và duplicate warning; xác nhận booking.
6. Booking vào dispatch queue.
7. No candidate/offer timeout: booking vẫn tồn tại và vào exception queue.

## 3. Booking đặt trước

```mermaid
stateDiagram-v2
 [*] --> DRAFT
 DRAFT --> SCHEDULED: Validate scheduled time
 SCHEDULED --> READY_FOR_DISPATCH: Lead-time reached
 READY_FOR_DISPATCH --> SEARCHING
 SEARCHING --> ASSIGNED: Driver accepts
 SEARCHING --> EXCEPTION: No eligible candidate
 EXCEPTION --> SEARCHING: Retry / manual action
 SCHEDULED --> CANCELLED: Cancellation policy
 ASSIGNED --> CANCELLED: If policy permits
```

Lead-time, cutoff và xử lý booking không có xe là TBD. Không tự hủy booking khi thiếu nguồn lực.

## 4. Dispatch offer lifecycle

```mermaid
sequenceDiagram
 participant E as Dispatch Engine
 participant V as Eligibility Service
 participant A as Assignment Service
 participant D as Driver App
 E->>V: Find candidates
 V-->>E: Eligible ranked candidates
 E->>A: Create time-bound offer
 A-->>D: Push offer
 alt Driver accepts
  D->>A: ACCEPT
  A->>A: Atomic assignment lock
  A-->>E: ASSIGNED
 else Reject or timeout
  D->>A: REJECT / no response
  A->>A: Close offer and record reason
  A-->>E: Try next / exception
 end
```

Ranking weights, DIRECT/PARTNER priority, radius, timeout and retry count are TBD.

## 5. Trip state machine

```mermaid
stateDiagram-v2
 [*] --> ASSIGNED
 ASSIGNED --> EN_ROUTE_TO_PICKUP
 EN_ROUTE_TO_PICKUP --> ARRIVED
 ARRIVED --> IN_TRIP
 IN_TRIP --> COMPLETED
 ASSIGNED --> CANCELLED
 EN_ROUTE_TO_PICKUP --> CANCELLED
 ARRIVED --> NO_SHOW
 ARRIVED --> CANCELLED
 IN_TRIP --> INCIDENT
 INCIDENT --> IN_TRIP: Resume if approved
 INCIDENT --> COMPLETED: End under incident flow
```

Server validates every transition. Events store event ID, actor, server time, device time, source and reason.

## 6. Reassignment

- Check booking/trip state and dispatcher permission.
- Require reason (vehicle fault, driver reject, GPS issue, customer request, dispatcher action, other).
- Close prior assignment; never delete it.
- Re-evaluate eligibility and create next offer.
- Recalculate ETA and notify customer/operations according to policy.
- Preserve before/after and actor in audit.

## 7. Exception workflows

| Case | Evidence | Result |
|---|---|---|
| No-show | Arrival time, wait duration, reason/evidence | NO_SHOW + policy |
| Customer cancel | Actor, time, reason, state | CANCELLED |
| Driver cancel | Driver, reason, time | Re-dispatch/escalate |
| Vehicle failure | Vehicle, GPS, incident report | Reassign |
| SOS | Timestamp, location, trip, acknowledgement | High-severity incident |
| Payment exception | Provider ref, callback, status | Finance queue |

## 8. Partner settlement

```mermaid
flowchart LR
 A["Period close"] --> B["Partner + contract scope"]
 B --> C["Collect eligible completed trips"]
 C --> D["Apply policy snapshot"]
 D --> E["Gross / deductions / share / net"]
 E --> F["Finance review"]
 F --> G{"Variance?"}
 G -- Yes --> H["Evidence and adjustment"]
 H --> F
 G -- No --> I["Approve"]
 I --> J["Lock batch"]
 J --> K["Record payout"]
 K --> L["PAID"]
```

After LOCKED, use a linked adjustment batch instead of changing historical ledger lines.

## 9. Minimum audit events

BOOKING_CREATED/CONFIRMED/UPDATED/CANCELLED; DISPATCH_SEARCH_STARTED; DRIVER_OFFERED/ACCEPTED/REJECTED/EXPIRED; VEHICLE_ASSIGNED/REASSIGNED; DRIVER_EN_ROUTE/ARRIVED; TRIP_STARTED/COMPLETED/CANCELLED/NO_SHOW; FARE_CALCULATED; PAYMENT_INITIATED/SUCCEEDED/FAILED/REFUNDED; INCIDENT_CREATED/ACKNOWLEDGED/RESOLVED; SETTLEMENT_CALCULATED/APPROVED/LOCKED/PAID.
