# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA03-3 | Tương ứng UCA03-3 |
| **Related UC ID** | UCA03-3 | Sửa tên danh mục |
| **SD Name** | Luồng sửa danh mục |
| **Description** | Admin sửa tên/mô tả danh mục, validate tên duy nhất, cập nhật CSDL. |
| **Primary Actor** | Admin |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC-A3/UCA03-3_Sua_ten_danh_muc.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Category` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Form sửa danh mục |
| L2 | CategoryAdminController | Control | Core | Internal | v1 | Điều phối |
| L3 | CategoryAdminService | Service | Core | Internal | v1 | Nghiệp vụ cập nhật |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `Category.Update` |
| L5 | CategoryRepository | Entity/DAO | Data | SQL | n/a | Cập nhật danh mục |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as CategoryAdminController
  participant L4 as AuthZService
  participant L3 as CategoryAdminService
  participant L5 as CategoryRepository

  L1->>L2: updateCategory(categoryId, payload)
  L2->>L4: checkPermission(adminId, "Category.Update")
  alt allowed
    L2->>L3: updateCategory(categoryId, payload)
    L3->>L5: updateCategoryDB(categoryId, payload)
    L5-->>L3: OK
    L3-->>L2: Success
    L2-->>L1: showToast("Cập nhật danh mục thành công")
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Submit sửa | `updateCategory(...)` | Sync | `{ id, name?, description? }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Category.Update")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Cập nhật DB | `updateCategoryDB(...)` | Sync | `{ id, data }` | `OK` | L3 | L5 | 409/5xx | Ghi |
| 4 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-4 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Tên trùng | Thay thế 3-4 | DUPLICATE_NAME | Sửa tên | "Tên danh mục đã tồn tại" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 4 | DB_ERROR | Retry | "Không thể cập nhật" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | Tên duy nhất |
| Performance | Cập nhật < 1s |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Category` | UPDATE | `name`, `description` |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Tự động cập nhật slug nếu có.
- Câu hỏi mở: Có lock khi nhiều admin cùng sửa?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





