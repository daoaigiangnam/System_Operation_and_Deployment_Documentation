# Bitrix24 — Installation, Restore & Cut-over Documentation

Bộ tài liệu **Step-by-Step** triển khai, khôi phục, bảo mật, nghiệm thu và chuyển đổi Bitrix24.

## Documents

1. [01 — Step-by-Step Installation](./01-Step-by-Step-Installation.md)
2. [02 — Backup & Restore](./02-Backup-and-Restore.md)
3. [03 — Permission & Security](./03-Permission-and-Security.md)
4. [04 — Domain, SSL & Functional Test](./04-Domain-SSL-and-Functional-Test.md)
5. [05 — Cut-over & Rollback](./05-Cut-over-and-Rollback.md)
6. [06 — Troubleshooting](./06-Troubleshooting.md)
7. [07 — Final Acceptance Checklist](./07-Final-Acceptance-Checklist.md)
8. [Images / Evidence](./images/README.md)

## Execution flow

```text
Pre-flight
   ↓
Server Installation
   ↓
Security / SSH
   ↓
Storage & Backup
   ↓
Database Restore
   ↓
Bitrix Source Restore
   ↓
Permission Validation
   ↓
Domain / SSL
   ↓
Functional Test
   ↓
Cut-over
   ↓
Post-cut-over Monitoring
   ↓
Acceptance / Sign-off
```

## Important

- Các giá trị như IP, hostname, DB name, DB user, domain, mount point và PHP target phải được chốt theo môi trường thực tế.
- Không commit secret/password/private key/database dump/backup dữ liệu thật lên GitHub.
- Không tạo screenshot giả. Chỉ đưa ảnh thực tế sau khi command/test đã được thực hiện và kiểm chứng.
- Với backup Web lớn khoảng 131 GB, quy trình ưu tiên copy trực tiếp từ Server sang HDD rời kết nối với Server thay vì tải qua workstation.
- SSH quản trị dùng account riêng + `sudo`; không cho phép SSH root trực tiếp.

## Official references

- Bitrix server requirements: https://dev.1c-bitrix.ru/learning/course/?COURSE_ID=32&LESSON_ID=2593
- Bitrix package installation: https://dev.1c-bitrix.ru/learning/course/?COURSE_ID=32&LESSON_ID=15296
- Bitrix backup/restore: https://dev.1c-bitrix.ru/learning/course/index.php?COURSE_ID=48&LESSON_ID=6979
