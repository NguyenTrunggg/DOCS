# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA03-8 | Tương ứng UCA03-8 |
| **Related UC ID** | UCA03-8 | Xóa nguyên liệu |
| **SD Name** | Luồng xóa nguyên liệu |
| **Description** | Admin xóa nguyên liệu: kiểm tra tham chiếu công thức, xác nhận, xóa/soft delete hoặc thay thế. |
| **Primary Actor** | Admin |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC-A3/UCA03-8_Xoa_nguyen_lieu.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Ingredient`, `Recipe` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | UI xác nhận xóa |
| L2 | IngredientAdminController | Control | Core | Internal | v1 | Điều phối |
| L3 | IngredientAdminService | Service | Core | Internal | v1 | Nghiệp vụ xóa/thay thế |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `Ingredient.Delete` |
| L5 | IngredientRepository | Entity/DAO | Data | SQL | n/a | Xóa nguyên liệu |
| L6 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Kiểm tra/thay thế tham chiếu |
| L7 | AuditLogService | Service | Core | Internal | v1 | Audit |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as IngredientAdminController
  participant L4 as AuthZService
  participant L3 as IngredientAdminService
  participant L6 as RecipeRepository
  participant L5 as IngredientRepository
  participant L7 as AuditLogService

  L1->>L2: deleteIngredient(ingredientId)
  L2->>L4: checkPermission(adminId, "Ingredient.Delete")
  alt allowed
    L2->>L3: deleteIngredient(ingredientId)
    L3->>L6: countRecipesUsingIngredient(ingredientId)
    alt no references
      L3->>L1: showConfirm()
      L1-->>L3: confirm()
      L3->>L5: hardDeleteIngredient(ingredientId)
      L5-->>L3: OK
    else has references
      L3->>L1: showReplaceDialog(listRecipes)
      L1-->>L3: confirmReplace(toIngredientId)
      L3->>L6: replaceIngredientInRecipes(ingredientId, toIngredientId)
      L6-->>L3: OK
      L3->>L5: softDeleteIngredient(ingredientId)
      L5-->>L3: OK
    end
    L3->>L7: writeAudit("INGREDIENT_DELETE", ingredientId, adminId)
    L7-->>L3: OK
    L3-->>L2: Success
    L2-->>L1: showToast("Đã xóa nguyên liệu")
  else denied
    L2-->>L1: showError("Bạn không có quyền xóa nguyên liệu")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Yêu cầu xóa | `deleteIngredient(ingredientId)` | Sync | `{ ingredientId }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Ingredient.Delete")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Kiểm tra tham chiếu | `countRecipesUsingIngredient(...)` | Sync | `{ ingredientId }` | `{ count }` | L3 | L6 | 5xx | Đọc |
| 4 | Xóa/Thay thế | `hardDeleteIngredient`/`replaceIngredientInRecipes` | Sync | `{ ... }` | `OK` | L3 | L5/L6 | 5xx | Ghi |
| 5 | Audit | `writeAudit(...)` | Sync | `{ action }` | `OK` | L3 | L7 | 5xx | Ghi |
| 6 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-6 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Còn tham chiếu, không thay thế | Thay thế 4-6 | FK_CONSTRAINT | Bắt buộc thay thế | "Nguyên liệu đang được sử dụng" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 6 | DB_ERROR | Retry | "Không thể xóa" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Security | Audit xóa/thay thế |
| Reliability | Bảo toàn toàn vẹn dữ liệu |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Ingredient` | DELETE/UPDATE | all/`deletedAt` |
| `Recipe` | UPDATE | thay ingredientId |
| `AuditLog` | INSERT | delete action |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Có UI thay thế nguyên liệu hàng loạt.
- Câu hỏi mở: Chính sách soft delete bao lâu?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





