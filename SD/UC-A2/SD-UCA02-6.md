# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA02-6 | Tương ứng UCA02-6 |
| **Related UC ID** | UCA02-6 | Phê duyệt công thức |
| **SD Name** | Luồng phê duyệt công thức | - |
| **Description** | Admin phê duyệt công thức chờ duyệt: kiểm tra quyền, cập nhật trạng thái, ghi log, thông báo người gửi. | - |
| **Primary Actor** | Admin | - |
| **Phiên bản (Version)** | 0.1.0 | - |
| **Trạng thái (Status)** | Draft | - |
| **Tác giả (Author)** |  | - |
| **Ngày (Date)** |  | Ngày cập nhật gần nhất |
| **Liên kết UC/BR/NFR** | `UC/UC-A2/UCA02-6_Phe_duyet_cong_thuc.md` | BR/NFR trong UC |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid | Lưu kèm trong file |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Recipe`, `AuditLog`, Notification | - |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng (Lifeline) | Vai trò/Loại (Stereotype) | Chủ quản (Ownership) | Giao thức/Interface (Protocol) | Phiên bản API | Mô tả chi tiết |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | UI chi tiết công thức |
| L2 | ModerationController | Control | Core | Internal | v1 | Điều phối |
| L3 | ModerationService | Service | Core | Internal | v1 | Nghiệp vụ phê duyệt/từ chối |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `Recipe.Moderate` |
| L5 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Cập nhật trạng thái |
| L6 | AuditLogService | Service | Core | Internal | v1 | Ghi log kiểm duyệt |
| L7 | NotificationService | Service | Core | Internal | v1 | Gửi thông báo |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as ModerationController
  participant L4 as AuthZService
  participant L3 as ModerationService
  participant L5 as RecipeRepository
  participant L6 as AuditLogService
  participant L7 as NotificationService

  L1->>L2: approveRecipe(recipeId)
  L2->>L4: checkPermission(adminId, "Recipe.Moderate")
  alt allowed
    L2->>L3: approve(recipeId)
    L3->>L5: updateStatus(recipeId, "Approved")
    L5-->>L3: OK
    L3->>L6: writeAudit("APPROVE", recipeId, adminId)
    L6-->>L3: OK
    L3->>L7: notifyUser(recipeIdOwner, "approved")
    L7-->>L3: OK
    L3-->>L2: Success
    L2-->>L1: showToast("Đã phê duyệt")
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Thông điệp (Message) | Sync/Async | Input | Output | Nguồn | Đích | Lỗi/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Yêu cầu phê duyệt | `approveRecipe(recipeId)` | Sync | `{ recipeId }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Recipe.Moderate")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Cập nhật trạng thái | `updateStatus(..., "Approved")` | Sync | `{ recipeId }` | `OK` | L3 | L5 | 5xx | Ghi |
| 4 | Audit | `writeAudit("APPROVE", ...)` | Sync | `{ ... }` | `OK` | L3 | L6 | 5xx | Ghi |
| 5 | Thông báo | `notifyUser(...)` | Async | `{ userId, type }` | `Accepted` | L3 | L7 | timeout | N/A |
| 6 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Luồng Thay thế / Ngoại lệ (Alternative / Exception Flows)

| Fragment ID | Loại | Guard Condition | Ảnh hưởng bước | Error Code/Type | Chiến lược khôi phục | Thông điệp hiển thị | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-6 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Trạng thái không hợp lệ | Thay thế 3-6 | INVALID_STATE | Dừng | "Trạng thái không hợp lệ" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 4-6 | DB_ERROR | Retry | "Không thể cập nhật" | log: error |

---

## V. Ghi Chú và Ràng Buộc (Additional Information)

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | Nội dung tuân thủ quy định cộng đồng |
| Security | Audit đầy đủ; phân quyền rõ ràng |

---

## VI. Tác Động Dữ Liệu (Data Impact)

| Entity/Bảng | Hành động | Trường bị ảnh hưởng | Ràng buộc |
| :--- | :--- | :--- | :--- |
| `Recipe` | UPDATE | `status` -> Approved | - |
| `AuditLog` | INSERT | approve action | - |

---

## VII. Giả Định & Câu Hỏi Mở (Assumptions & Open Questions)

- Giả định: NotificationService gửi thông báo realtime/email.
- Câu hỏi mở: Có cần phân công kiểm duyệt viên trước khi duyệt?

---

## VIII. Nguồn Biểu Đồ (Diagram Source)

- Mermaid embedded ở mục III.





