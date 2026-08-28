# Bitrix24 — Installation, Restore & Cut-over Documentation

Bộ tài liệu Step-by-Step triển khai, khôi phục và nghiệm thu Bitrix24.

## Documents

- [01 — Step-by-Step Installation](./01-Step-by-Step-Installation.md)
- [02 — Backup & Restore](./02-Backup-and-Restore.md)
- [03 — Permission & Security](./03-Permission-and-Security.md)
- [04 — Domain, SSL & Functional Test](./04-Domain-SSL-and-Functional-Test.md)
- [05 — Cut-over & Rollback](./05-Cut-over-and-Rollback.md)
- [06 — Troubleshooting](./06-Troubleshooting.md)
- [07 — Final Acceptance Checklist](./07-Final-Acceptance-Checklist.md)

## Evidence

Ảnh minh họa nên được lưu trong thư mục `Bitrix24/images/` và được tham chiếu từ Markdown. Không lưu password, secret, private key hoặc file backup dữ liệu thật vào Git.

## Scope

Tài liệu tập trung vào môi trường self-hosted/box Bitrix24 trên Linux, với quy trình có thể áp dụng cho migration/restore sang server mới. Các giá trị môi trường cụ thể như hostname, IP, database name, PHP target version, domain và mount point phải được điền theo hệ thống thực tế trước khi execution.

## Source references

- Bitrix server requirements: https://dev.1c-bitrix.ru/learning/course/?COURSE_ID=32&LESSON_ID=2593
- Bitrix package installation: https://dev.1c-bitrix.ru/learning/course/?COURSE_ID=32&LESSON_ID=15296
- Bitrix backup/restore: https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=48&LESSON_ID=6979
