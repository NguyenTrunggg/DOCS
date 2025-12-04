# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA02-1 | Tương ứng UCA02-1 |
| **Related UC ID** | UCA02-1 | Xem danh sách công thức hệ thống |
| **SD Name** | Luồng xem danh sách công thức | - |
| **Description** | Admin truy cập trang quản lý công thức, hệ thống truy vấn, phân trang, lọc và hiển thị danh sách. | - |
| **Primary Actor** | Admin | - |
| **Phiên bản (Version)** | 0.1.0 | - |
| **Trạng thái (Status)** | Draft | - |
| **Tác giả (Author)** |  | - |
| **Ngày (Date)** |  | Ngày cập nhật gần nhất |
| **Liên kết UC/BR/NFR** | `UC/UC-A2/UCA02-1_Xem_danh_sach_cong_thuc_he_thong.md` | BR/NFR trong UC |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid | Lưu kèm trong file |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB Schema `Recipe` | - |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng (Lifeline) | Vai trò/Loại (Stereotype) | Chủ quản (Ownership) | Giao thức/Interface (Protocol) | Phiên bản API | Mô tả chi tiết |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Trang "Quản lý Công thức" |
| L2 | RecipeListController | Control | Core | Internal | v1 | Điều phối request/response |
| L3 | RecipeQueryService | Service | Core | Internal | v1 | Nghiệp vụ filter/sort/paging |
| L4 | AuthZService | Service | Core | Internal | v1 | Kiểm tra quyền `Recipe.Read` |
| L5 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Truy cập bảng `Recipe` |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as RecipeListController
  participant L4 as AuthZService
  participant L3 as RecipeQueryService
  participant L5 as RecipeRepository

  L1->>L2: openRecipeList(page, filters, sort)
  L2->>L4: checkPermission(adminId, "Recipe.Read")
  alt allowed
    L2->>L3: getRecipes(query)
    L3->>L5: findRecipes(query)
    L5-->>L3: { items, total }
    L3-->>L2: { items, total }
    L2-->>L1: renderTable(items, total)
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Thông điệp (Message) | Sync/Async | Input | Output | Nguồn | Đích | Lỗi/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Admin mở trang | `openRecipeList(...)` | Sync | `{ page, filters, sort }` | `200 OK` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Recipe.Read")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Gọi service | `getRecipes(query)` | Sync | `{ ... }` | `{ items, total }` | L2 | L3 | 5xx | Đọc |
| 4 | Truy vấn DB | `findRecipes(query)` | Sync | `{ ... }` | `{ items, total }` | L3 | L5 | 5xx | Đọc |
| 5 | Render UI | `renderTable(...)` | Sync | `{ items, total }` | UI updated | L2 | L1 | - | N/A |

### B. Luồng Thay thế / Ngoại lệ (Alternative / Exception Flows)

| Fragment ID | Loại | Guard Condition | Ảnh hưởng bước | Error Code/Type | Chiến lược khôi phục | Thông điệp hiển thị | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AF-1 | [alt] | Không có dữ liệu | Thay thế 5 | EMPTY | Hiển thị bảng rỗng | "Không tìm thấy công thức" | log: info |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-5 | PERMISSION_DENIED | Dừng luồng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Lỗi tải dữ liệu | Thay thế 5 | SERVER_ERROR | Cho phép thử lại | "Không thể tải danh sách" | log: error |

---

## V. Ghi Chú và Ràng Buộc (Additional Information)

| Trường | Chi tiết |
| :--- | :--- |
| Timing Constraint | Tải trang < 2s với 100k công thức (paging) |
| Business Rules | Tìm kiếm không dấu, không phân biệt hoa thường |
| Security | Chỉ Admin/Super Admin; audit truy cập |
| Observability | `recipe_list_latency_ms`, `recipe_list_error_rate` |

---

## VI. Tác Động Dữ Liệu (Data Impact)

| Entity/Bảng | Hành động | Trường bị ảnh hưởng | Ràng buộc |
| :--- | :--- | :--- | :--- |
| `Recipe` | READ | n/a | Lọc/sort theo tiêu chí |

---

## VII. Giả Định & Câu Hỏi Mở (Assumptions & Open Questions)

- Giả định: Có index phù hợp cho filter/sort.
- Câu hỏi mở: Có chức năng export CSV/Excel kèm?

---

## VIII. Nguồn Biểu Đồ (Diagram Source)

- Mermaid embedded ở mục III.





