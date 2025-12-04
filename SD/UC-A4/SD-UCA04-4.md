# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA04-4 | Tương ứng UCA04-4 |
| **Related UC ID** | UCA04-4 | Xóa tài khoản Admin |
| **SD Name** | Luồng xóa tài khoản Admin |
| **Description** | Super Admin xóa tài khoản Admin: xác nhận, xóa CSDL, kiểm tra ràng buộc Super Admin cuối, audit. |
| **Primary Actor** | Super Admin |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC-A4/UCA04-4_Xoa_tai_khoan_admin.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `AdminAccount`, `AuditLog` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | SuperAdmin GUI | Boundary | Web Admin | HTTP | n/a | UI xác nhận xóa |
| L2 | AdminAccountController | Control | Core | Internal | v1 | Điều phối |
| L3 | AdminAccountService | Service | Core | Internal | v1 | Nghiệp vụ xóa |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `AdminAccount.Delete` |
| L5 | AdminAccountRepository | Entity/DAO | Data | SQL | n/a | Xóa tài khoản |
| L6 | AuditLogService | Service | Core | Internal | v1 | Audit |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as SuperAdmin GUI
  participant L2 as AdminAccountController
  participant L4 as AuthZService
  participant L3 as AdminAccountService
  participant L5 as AdminAccountRepository
  participant L6 as AuditLogService

  L1->>L2: deleteAdmin(adminId)
  L2->>L4: checkPermission(superAdminId, "AdminAccount.Delete")
  alt allowed
    L2->>L3: deleteAdminAccount(adminId)
    L3->>L3: ensureNotLastSuperAdmin(adminId)
    alt not last super admin
      L2->>L1: showConfirm()
      L1-->>L2: confirm()
      L3->>L5: hardDeleteAdmin(adminId)
      L5-->>L3: OK
      L3->>L6: writeAudit("ADMIN_DELETE", adminId, superAdminId)
      L6-->>L3: OK
      L3-->>L2: Success
      L2-->>L1: showToast("Đã xóa tài khoản Admin")
    else last super admin
      L3-->>L2: Error(SUPERADMIN_LAST)
      L2-->>L1: showError("Không thể xóa Super Admin cuối cùng")
    end
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Yêu cầu xóa | `deleteAdmin(adminId)` | Sync | `{ adminId }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "AdminAccount.Delete")` | Sync | `{ superAdminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Ràng buộc Super Admin | `ensureNotLastSuperAdmin(...)` | Sync | `{ adminId }` | `{ ok }` | L3 | L3 | 409 | Đang mở |
| 4 | Xóa DB | `hardDeleteAdmin(...)` | Sync | `{ adminId }` | `OK` | L3 | L5 | 5xx | Ghi |
| 5 | Audit | `writeAudit("ADMIN_DELETE", ...)` | Sync | `{ ... }` | `OK` | L3 | L6 | 5xx | Ghi |
| 6 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-6 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Super Admin cuối cùng | Thay thế 4-6 | SUPERADMIN_LAST | Chặn | "Không thể xóa Super Admin cuối cùng" | log: error |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 6 | DB_ERROR | Retry | "Không thể xóa" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | Không để hệ thống thiếu Super Admin |
| Security | Audit xóa |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `AdminAccount` | DELETE | all |
| `AuditLog` | INSERT | delete action |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Có cảnh báo khi admin tự xóa chính mình.
- Câu hỏi mở: Có hỗ trợ soft delete thay vì hard delete?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





