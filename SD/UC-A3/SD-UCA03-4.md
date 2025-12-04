# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA03-4 | Tương ứng UCA03-4 |
| **Related UC ID** | UCA03-4 | Xóa danh mục |
| **SD Name** | Luồng xóa danh mục |
| **Description** | Admin xóa danh mục: kiểm tra ràng buộc tham chiếu, xác nhận, xóa hoặc chuyển công thức, audit. |
| **Primary Actor** | Admin |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC-A3/UCA03-4_Xoa_danh_muc.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Category`, `Recipe` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | UI xác nhận xóa |
| L2 | CategoryAdminController | Control | Core | Internal | v1 | Điều phối |
| L3 | CategoryAdminService | Service | Core | Internal | v1 | Nghiệp vụ xóa/chuyển |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `Category.Delete` |
| L5 | CategoryRepository | Entity/DAO | Data | SQL | n/a | Xóa danh mục |
| L6 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Kiểm tra/chuyển tham chiếu |
| L7 | AuditLogService | Service | Core | Internal | v1 | Audit |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as CategoryAdminController
  participant L4 as AuthZService
  participant L3 as CategoryAdminService
  participant L6 as RecipeRepository
  participant L5 as CategoryRepository
  participant L7 as AuditLogService

  L1->>L2: deleteCategory(categoryId)
  L2->>L4: checkPermission(adminId, "Category.Delete")
  alt allowed
    L2->>L3: deleteCategory(categoryId)
    L3->>L6: countRecipesByCategory(categoryId)
    alt no references
      L3->>L1: showConfirm()
      L1-->>L3: confirm()
      L3->>L5: hardDeleteCategory(categoryId)
      L5-->>L3: OK
    else has references
      L3->>L1: showMoveDialog(listRecipes)
      L1-->>L3: confirmMove(toCategoryId)
      L3->>L6: moveRecipes(categoryId, toCategoryId)
      L6-->>L3: OK
      L3->>L5: hardDeleteCategory(categoryId)
      L5-->>L3: OK
    end
    L3->>L7: writeAudit("CATEGORY_DELETE", categoryId, adminId)
    L7-->>L3: OK
    L3-->>L2: Success
    L2-->>L1: showToast("Đã xóa danh mục")
  else denied
    L2-->>L1: showError("Bạn không có quyền xóa danh mục")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Yêu cầu xóa | `deleteCategory(categoryId)` | Sync | `{ categoryId }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Category.Delete")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Kiểm tra tham chiếu | `countRecipesByCategory(...)` | Sync | `{ categoryId }` | `{ count }` | L3 | L6 | 5xx | Đọc |
| 4 | Xóa/Chuyển | `hardDeleteCategory`/`moveRecipes` | Sync | `{ ... }` | `OK` | L3 | L5/L6 | 5xx | Ghi |
| 5 | Audit | `writeAudit(...)` | Sync | `{ action }` | `OK` | L3 | L7 | 5xx | Ghi |
| 6 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-6 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Còn tham chiếu, không chuyển | Thay thế 4-6 | FK_CONSTRAINT | Bắt buộc chuyển | "Danh mục đang được sử dụng" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 6 | DB_ERROR | Retry | "Không thể xóa" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Security | Audit xóa/chuyển |
| Reliability | Bảo toàn toàn vẹn dữ liệu |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Category` | DELETE | all |
| `Recipe` | UPDATE | `categoryId` khi chuyển |
| `AuditLog` | INSERT | delete action |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Có UI hỗ trợ chuyển danh mục hàng loạt.
- Câu hỏi mở: Có hỗ trợ soft delete danh mục?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





