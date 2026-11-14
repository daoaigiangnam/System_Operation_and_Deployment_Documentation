# MAI LINH TAXI — SYSTEM IMPLEMENTATION DOCUMENTATION

> **Bộ tài liệu triển khai nền tảng Taxi Mai Linh** · Version 0.9 · 2026-11-14  
> **Mô hình:** DIRECT + PARTNER trên một nền tảng điều phối thống nhất.

## Mục tiêu

Bộ tài liệu này là cơ sở để Business Owner, BA, UX/UI, Architect, Developer, QA và IT Operations cùng phân tích, thiết kế, xây dựng, kiểm thử, nghiệm thu và vận hành hệ thống. Nội dung ở trạng thái Draft for Business Validation; các chính sách chưa được phê duyệt được ghi TBD.

## Nguyên tắc nền tảng

1. Một dispatch platform phục vụ xe/tài xế Mai Linh quản lý trực tiếp (**DIRECT**) và xe/tài xế đối tác (**PARTNER**).
2. `booking_channel` khác `fleet_type`. Kênh HOTLINE/APP/WEB/API không quyết định nguồn lực DIRECT/PARTNER.
3. Dispatch engine có thể dùng chung; điều kiện hợp đồng, doanh thu, công nợ và settlement phải phân biệt.
4. Fare và settlement lưu snapshot phiên bản tariff/contract/policy tại thời điểm nghiệp vụ.
5. Event vận hành và ledger phải truy vết; không xóa lịch sử để sửa dữ liệu.

## Danh mục tài liệu

| # | Tài liệu | Phạm vi |
|---|---|---|
| 00 | [Executive Overview](./00-Executive-Overview.md) | Mục tiêu, scope, nguyên tắc, approval gates |
| 01 | [BRD](./01-BRD-Business-Requirements.md) | Business objectives, stakeholders, capabilities, KPI |
| 02 | [Operating Model](./02-Operating-Model.md) | DIRECT/PARTNER, roles, eligibility |
| 03 | [End-to-End Workflows](./03-End-to-End-Workflows.md) | Booking, dispatch, trip, payment, settlement, incident |
| 04 | [SRS](./04-SRS-Functional-Requirements.md) | Requirement IDs, business rules, state model |
| 05 | [UI/UX](./05-UIUX-Specification.md) | Screen inventory, fields, interactions, states |
| 06 | [Dispatch & Control Tower](./06-Dispatch-Control-Tower.md) | Candidate logic, map, queue, exceptions |
| 07 | [Driver App](./07-Driver-App.md) | Mobile journey, states, offline/safety |
| 08 | [Fare/Payment/Settlement](./08-Fare-Payment-Settlement.md) | Tariff, payment, ledger, partner settlement |
| 09 | [Data Model & ERD](./09-Data-Model-ERD.md) | Domain entities, relationships, constraints |
| 10 | [API & Integration](./10-API-Integration.md) | API, events, contracts, integrations |
| 11 | [Architecture/NFR/Security](./11-Architecture-NFR-Security.md) | Logical architecture, resilience, security, DR |
| 12 | [QA/UAT/Acceptance](./12-QA-UAT-Acceptance.md) | Test strategy, test cases, acceptance gates |
| 13 | [Delivery/Deployment](./13-Delivery-Deployment-Plan.md) | Workstreams, environments, migration, cutover |
| 14 | [Operations/Runbooks](./14-Operations-Runbooks.md) | Monitoring, incident, backup/restore |
| 15 | [Decision Register](./15-Decision-Register.md) | TBD, owners, priorities, approvals |
| 16 | [Traceability](./16-Traceability-Matrix.md) | BRD → SRS → Flow → UI/API → Test |
| 17 | [Glossary](./17-Glossary.md) | Thuật ngữ thống nhất |
| Assets | [Wireframes & SVG](./assets/README.md) | Hình hệ thống, wireframe tổng đài, điều hành, tài xế |

## Visual assets

- ![System context](./assets/01-system-context.svg)
- ![Control Tower](./assets/02-control-tower-wireframe.svg)
- ![Call Center](./assets/03-call-center-booking-wireframe.svg)
- ![Driver App](./assets/04-driver-app-wireframe.svg)

## Cách sử dụng

- Business Owner/Operations xác nhận 00–03 và Decision Register.
- BA/PO quản lý requirement IDs tại 04 và traceability tại 16.
- UX/UI chuyển 05–07 thành Figma prototype có trạng thái tương tác.
- Architect/Developer dùng 09–11 để chốt kiến trúc, dữ liệu và API.
- QA sử dụng 12/16 làm căn cứ test coverage và UAT.
- IT Operations dùng 13–14 cho release và vận hành.

## Trạng thái tài liệu

Draft → In Review → Approved → Baseline. Khi đã Baseline, mọi thay đổi phải qua Change Request và đánh giá ảnh hưởng.

> **Không được tự giả định:** biểu giá, phụ phí, phí hủy/no-show, tỷ lệ chia PARTNER, ưu tiên DIRECT/PARTNER, bán kính tìm xe, offer timeout, SLA/KPI, tải thiết kế, RPO/RTO và nhà cung cấp tích hợp. Các mục này phải được owner phê duyệt trước khi đưa vào production.
