# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA01-3 | Tương ứng UCA01-3 |
| **Related UC ID** | UCA01-3 | Vô hiệu hóa tài khoản người dùng |
| **SD Name** | Luồng khóa tài khoản người dùng | - |
| **Description** | Admin thực hiện khóa tài khoản: xác nhận lý do/thời hạn, cập nhật trạng thái, đăng xuất phiên, audit và thông báo. | - |
| **Primary Actor** | Admin | - |
| **Phiên bản (Version)** | 0.1.0 | - |
| **Trạng thái (Status)** | Draft | - |
| **Tác giả (Author)** |  | - |
| **Ngày (Date)** |  | Ngày cập nhật gần nhất |
| **Liên kết UC/BR/NFR** | `UC/UC-A1/UCA01-3_Vo_hieu_hoa_tai_khoan_nguoi_dung.md` | BR/NFR trong UC |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid | Lưu kèm trong file |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB Schema `User`, `AuditLog`, `Session` | - |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng (Lifeline) | Vai trò/Loại (Stereotype) | Chủ quản (Ownership) | Giao thức/Interface (Protocol) | Phiên bản API | Mô tả chi tiết |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Giao diện hành động khóa |
| L2 | UserAdminController | Control | Core | Internal | v1 | Điều phối xác nhận và gọi service |
| L3 | UserAdminService | Service | Core | Internal | v1 | Nghiệp vụ khóa tài khoản |
| L4 | AuthZService | Service | Core | Internal | v1 | Kiểm tra quyền `User.Disable` |
| L5 | UserRepository | Entity/DAO | Data | SQL | n/a | Cập nhật trạng thái `locked`, lý do, hạn |
| L6 | SessionService | Service | Core | Internal | v1 | Đăng xuất toàn bộ phiên của người dùng |
| L7 | AuditLogService | Service | Core | Internal | v1 | Ghi nhận sự kiện khóa |
| L8 | NotificationService | Service | Core | Internal | v1 | Gửi email thông báo (tuỳ chọn) |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as UserAdminController
  participant L4 as AuthZService
  participant L3 as UserAdminService
  participant L5 as UserRepository
  participant L6 as SessionService
  participant L7 as AuditLogService
  participant L8 as NotificationService

  L1->>L2: lockUser(userId)
  L2->>L4: checkPermission(adminId, "User.Disable")
  alt allowed
    L2->>L1: showConfirm(reason, ttl)
    L1-->>L2: confirm(reason, ttl)
    L2->>L3: disableUser(userId, reason, ttl)
    L3->>L5: updateUserLock(userId, reason, ttl)
    L5-->>L3: OK
    L3->>L6: revokeAllSessions(userId)
    L6-->>L3: OK
    L3->>L7: writeAudit("LOCK", userId, adminId, reason)
    L7-->>L3: OK
    opt notify user
      L3->>L8: sendLockEmail(userId, reason, ttl)
      L8-->>L3: OK
    end
    L3-->>L2: Success
    L2-->>L1: showToast("Đã khóa tài khoản")
  else denied
    L2-->>L1: showError("Bạn không có quyền khóa tài khoản")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Thông điệp (Message) | Sync/Async | Định nghĩa Input | Định nghĩa Output | Nguồn (Source) | Đích (Target) | Lỗi/Timeout | Giao dịch (Txn) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Admin yêu cầu khóa | `lockUser(userId)` | Sync | `{ userId }` | `200 OK` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(adminId, "User.Disable")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Xác nhận | `showConfirm(...)` | Sync | `{ reason, ttl }` | `confirm` | L2 | L1 | - | N/A |
| 4 | Gọi service khóa | `disableUser(userId, reason, ttl)` | Sync | `{ userId, reason, ttl }` | `OK` | L2 | L3 | 5xx | Bắt đầu |
| 5 | Cập nhật DB | `updateUserLock(...)` | Sync | `{ ... }` | `OK` | L3 | L5 | 5xx | Ghi |
| 6 | Thu hồi phiên | `revokeAllSessions(userId)` | Sync | `{ userId }` | `OK` | L3 | L6 | 5xx | Ghi |
| 7 | Audit | `writeAudit("LOCK", ...)` | Sync | `{ ... }` | `OK` | L3 | L7 | 5xx | Ghi |
| 8 | Thông báo (opt) | `sendLockEmail(...)` | Async | `{ ... }` | `Accepted` | L3 | L8 | timeout | N/A |
| 9 | Trả về UI | `showToast("Đã khóa tài khoản")` | Sync | `-` | UI updated | L2 | L1 | - | Kết thúc |

### B. Luồng Thay thế / Ngoại lệ (Alternative / Exception Flows)

| Fragment ID | Loại | Guard Condition | Ảnh hưởng bước | Error Code/Type | Chiến lược khôi phục | Thông điệp hiển thị | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-9 | PERMISSION_DENIED | Dừng luồng | "Bạn không có quyền khóa tài khoản" | log: warn |
| EF-2 | [alt] | Tài khoản đã bị khóa | Thay thế 4-9 | ALREADY_LOCKED | Dừng luồng | "Tài khoản đã ở trạng thái bị khóa" | log: info |
| EF-3 | [alt] | Lỗi cập nhật CSDL | Thay thế 6-9 | DB_ERROR | Cho phép thử lại | "Không thể cập nhật trạng thái" | log: error |

---

## V. Ghi Chú và Ràng Buộc (Additional Information)

| Trường | Chi tiết |
| :--- | :--- |
| Security | Audit đầy đủ; lý do khóa bắt buộc; TTL tự mở khóa |
| Reliability | Thu hồi phiên đảm bảo ngay lập tức |
| Performance | Cập nhật trạng thái < 1s |

---

## VI. Tác Động Dữ Liệu (Data Impact)

| Entity/Bảng | Hành động | Trường bị ảnh hưởng | Ràng buộc/Quy tắc |
| :--- | :--- | :--- | :--- |
| `User` | UPDATE | `status`, `lockReason`, `lockUntil` | Ghi nhận lý do rõ ràng |
| `Session` | DELETE/REVOKE | all sessions by user | Bắt buộc |
| `AuditLog` | INSERT | `action`, `actor`, `target`, `reason` | - |

---

## VII. Giả Định & Câu Hỏi Mở (Assumptions & Open Questions)

- Giả định: SessionService hỗ trợ revoke theo `userId`.
- Câu hỏi mở: Có áp dụng preset TTL theo chính sách hệ thống?

---

## VIII. Nguồn Biểu Đồ (Diagram Source)

- Mermaid embedded ở mục III.





