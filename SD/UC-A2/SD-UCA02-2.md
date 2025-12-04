# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA02-2 | Tương ứng UCA02-2 |
| **Related UC ID** | UCA02-2 | Thêm công thức hệ thống |
| **SD Name** | Luồng thêm công thức hệ thống | - |
| **Description** | Admin tạo công thức mới: nhập liệu, validate, upload media, lưu và xuất bản ở trạng thái "Đã duyệt". | - |
| **Primary Actor** | Admin | - |
| **Phiên bản (Version)** | 0.1.0 | - |
| **Trạng thái (Status)** | Draft | - |
| **Tác giả (Author)** |  | - |
| **Ngày (Date)** |  | Ngày cập nhật gần nhất |
| **Liên kết UC/BR/NFR** | `UC/UC-A2/UCA02-2_Them_cong_thuc_he_thong.md` | BR/NFR trong UC |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid | Lưu kèm trong file |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Recipe`, `Media` | - |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng (Lifeline) | Vai trò/Loại (Stereotype) | Chủ quản (Ownership) | Giao thức/Interface (Protocol) | Phiên bản API | Mô tả chi tiết |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Form tạo công thức |
| L2 | RecipeAdminController | Control | Core | Internal | v1 | Điều phối tạo/sửa |
| L3 | RecipeAdminService | Service | Core | Internal | v1 | Nghiệp vụ tạo công thức |
| L4 | AuthZService | Service | Core | Internal | v1 | Kiểm tra quyền `Recipe.Create` |
| L5 | MediaService | Service | Core | Internal | v1 | Upload/validate media |
| L6 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Lưu công thức |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as RecipeAdminController
  participant L4 as AuthZService
  participant L3 as RecipeAdminService
  participant L5 as MediaService
  participant L6 as RecipeRepository

  L1->>L2: createRecipe(draftOrPublish, payload)
  L2->>L4: checkPermission(adminId, "Recipe.Create")
  alt allowed
    L2->>L3: createRecipe(payload, publish=true)
    opt has media
      L3->>L5: uploadAndValidate(media)
      L5-->>L3: mediaUrls
    end
    L3->>L6: insertRecipe({...payload, mediaUrls, status:"Approved"})
    L6-->>L3: recipeId
    L3-->>L2: { recipeId }
    L2-->>L1: showToast("Đã tạo công thức")
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Thông điệp (Message) | Sync/Async | Input | Output | Nguồn | Đích | Lỗi/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Submit tạo | `createRecipe(...)` | Sync | `{ payload }` | `202/200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Recipe.Create")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Xử lý media | `uploadAndValidate(media)` | Sync | `{ files }` | `{ urls }` | L3 | L5 | timeout | Đang mở |
| 4 | Ghi CSDL | `insertRecipe(...)` | Sync | `{ data }` | `{ recipeId }` | L3 | L6 | 5xx | Ghi |
| 5 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Luồng Thay thế / Ngoại lệ (Alternative / Exception Flows)

| Fragment ID | Loại | Guard Condition | Ảnh hưởng bước | Error Code/Type | Chiến lược khôi phục | Thông điệp hiển thị | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AF-1 | [opt] | Lưu nháp | Thay thế 4 | DRAFT | Lưu trạng thái nháp | "Đã lưu nháp" | log: info |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-5 | PERMISSION_DENIED | Dừng luồng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Lỗi media | Thay thế 4-5 | MEDIA_INVALID | Yêu cầu chọn lại | "Media không hợp lệ" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 5 | DB_ERROR | Cho phép thử lại | "Không thể lưu" | log: error |

---

## V. Ghi Chú và Ràng Buộc (Additional Information)

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | Tên món duy nhất; ≥3 nguyên liệu và ≥3 bước |
| Performance | Lưu < 3s (không tính upload lớn) |
| Security | Chỉ Admin/Super Admin tạo; audit |

---

## VI. Tác Động Dữ Liệu (Data Impact)

| Entity/Bảng | Hành động | Trường bị ảnh hưởng | Ràng buộc |
| :--- | :--- | :--- | :--- |
| `Recipe` | INSERT | all core fields | `status="Approved"` khi publish |
| `Media` | INSERT | url, type, size | Validate định dạng/kích thước |

---

## VII. Giả Định & Câu Hỏi Mở (Assumptions & Open Questions)

- Giả định: MediaService trả url công khai bền vững.
- Câu hỏi mở: Có workflow duyệt nội bộ trước khi publish?

---

## VIII. Nguồn Biểu Đồ (Diagram Source)

- Mermaid embedded ở mục III.





