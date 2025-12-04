# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA03-1 | Tương ứng UCA03-1 |
| **Related UC ID** | UCA03-1 | Xem danh sách danh mục |
| **SD Name** | Luồng xem danh sách danh mục | - |
| **Description** | Admin mở trang danh mục; hệ thống kiểm tra quyền, truy vấn và hiển thị danh mục có tìm kiếm, sắp xếp, phân trang. | - |
| **Primary Actor** | Admin | - |
| **Phiên bản (Version)** | 0.1.0 | - |
| **Trạng thái (Status)** | Draft | - |
| **Tác giả (Author)** |  | - |
| **Ngày (Date)** |  | Ngày cập nhật gần nhất |
| **Liên kết UC/BR/NFR** | `UC/UC-A3/UCA03-1_Xem_danh_sach_danh_muc.md` | BR/NFR trong UC |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid | Lưu kèm trong file |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Category` | - |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng (Lifeline) | Vai trò/Loại (Stereotype) | Chủ quản (Ownership) | Giao thức/Interface (Protocol) | Phiên bản API | Mô tả chi tiết |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Trang "Quản lý Danh mục" |
| L2 | CategoryController | Control | Core | Internal | v1 | Điều phối |
| L3 | CategoryQueryService | Service | Core | Internal | v1 | Truy vấn danh mục |
| L4 | AuthZService | Service | Core | Internal | v1 | Quyền `Category.Read` |
| L5 | CategoryRepository | Entity/DAO | Data | SQL | n/a | Truy cập `Category` |
| L6 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Đếm số công thức theo danh mục |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as CategoryController
  participant L4 as AuthZService
  participant L3 as CategoryQueryService
  participant L5 as CategoryRepository
  participant L6 as RecipeRepository

  L1->>L2: openCategoryList(page, filters, sort)
  L2->>L4: checkPermission(adminId, "Category.Read")
  alt allowed
    L2->>L3: getCategories(query)
    par categories
      L3->>L5: findCategories(query)
      L5-->>L3: { items, total }
    and counts
      L3->>L6: countRecipesByCategory(ids)
      L6-->>L3: { categoryId: count }[]
    end
    L3-->>L2: { itemsWithCounts, total }
    L2-->>L1: renderTable(itemsWithCounts, total)
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Thông điệp (Message) | Sync/Async | Input | Output | Nguồn | Đích | Lỗi/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Mở danh sách | `openCategoryList(...)` | Sync | `{ page, filters, sort }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Category.Read")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Truy vấn | `getCategories(query)` | Sync | `{ ... }` | `{ items, total }` | L2 | L3 | 5xx | Đọc |
| 4 | DB categories | `findCategories(query)` | Sync | `{ ... }` | `{ items, total }` | L3 | L5 | 5xx | Đọc |
| 5 | Đếm công thức | `countRecipesByCategory(ids)` | Sync | `{ ids[] }` | `{ id:count }[]` | L3 | L6 | 5xx | Đọc |
| 6 | Render UI | `renderTable(...)` | Sync | `{ itemsWithCounts, total }` | UI updated | L2 | L1 | - | N/A |

### B. Luồng Thay thế / Ngoại lệ (Alternative / Exception Flows)

| Fragment ID | Loại | Guard Condition | Ảnh hưởng bước | Error Code/Type | Chiến lược khôi phục | Thông điệp hiển thị | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AF-1 | [alt] | Không có dữ liệu | Thay thế 6 | EMPTY | Bảng rỗng | "Chưa có danh mục" | log: info |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-6 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Lỗi tải dữ liệu | Thay thế 6 | SERVER_ERROR | Retry | "Không thể tải danh sách" | log: error |

---

## V. Ghi Chú và Ràng Buộc (Additional Information)

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | Tên danh mục duy nhất |
| Security | Chỉ Admin; audit truy cập |

---

## VI. Tác Động Dữ Liệu (Data Impact)

| Entity/Bảng | Hành động | Trường bị ảnh hưởng | Ràng buộc |
| :--- | :--- | :--- | :--- |
| `Category` | READ | n/a | - |
| `Recipe` | READ | count by categoryId | - |

---

## VII. Giả Định & Câu Hỏi Mở (Assumptions & Open Questions)

- Giả định: Có index hỗ trợ đếm nhanh.
- Câu hỏi mở: Có cần export kèm số công thức?

---

## VIII. Nguồn Biểu Đồ (Diagram Source)

- Mermaid embedded ở mục III.





