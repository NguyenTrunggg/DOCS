# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA03-6 | Tương ứng UCA03-6 |
| **Related UC ID** | UCA03-6 | Thêm nguyên liệu mới |
| **SD Name** | Luồng thêm nguyên liệu mới |
| **Description** | Admin thêm nguyên liệu: nhập liệu, validate duy nhất, lưu vào CSDL, hỗ trợ alias. |
| **Primary Actor** | Admin |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC-A3/UCA03-6_Them_nguyen_lieu_moi.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Ingredient` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Form tạo nguyên liệu |
| L2 | IngredientAdminController | Control | Core | Internal | v1 | Điều phối |
| L3 | IngredientAdminService | Service | Core | Internal | v1 | Nghiệp vụ tạo |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `Ingredient.Create` |
| L5 | IngredientRepository | Entity/DAO | Data | SQL | n/a | Lưu nguyên liệu |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as IngredientAdminController
  participant L4 as AuthZService
  participant L3 as IngredientAdminService
  participant L5 as IngredientRepository

  L1->>L2: createIngredient(payload)
  L2->>L4: checkPermission(adminId, "Ingredient.Create")
  alt allowed
    L2->>L3: createIngredient(payload)
    L3->>L5: insertIngredient(name, unit, category, alias[])
    L5-->>L3: ingredientId
    L3-->>L2: { ingredientId }
    L2-->>L1: showToast("Đã thêm nguyên liệu")
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Submit tạo | `createIngredient(payload)` | Sync | `{ name, unit, category, alias? }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Ingredient.Create")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Ghi DB | `insertIngredient(...)` | Sync | `{ ... }` | `{ ingredientId }` | L3 | L5 | 409/5xx | Ghi |
| 4 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-4 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Tên trùng | Thay thế 3-4 | DUPLICATE_NAME | Sửa tên | "Nguyên liệu đã tồn tại" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 4 | DB_ERROR | Retry | "Không thể lưu" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | Tên duy nhất (case-insensitive, không dấu); hỗ trợ alias |
| Performance | Lưu < 1s |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Ingredient` | INSERT | `name`, `unit`, `categoryId`, `alias` |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Alias dùng cho gợi ý tìm kiếm.
- Câu hỏi mở: Có import CSV hàng loạt?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





