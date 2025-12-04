# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCA02-3 | Tương ứng UCA02-3 |
| **Related UC ID** | UCA02-3 | Sửa công thức hệ thống |
| **SD Name** | Luồng sửa công thức hệ thống | - |
| **Description** | Admin chỉnh sửa công thức: tải dữ liệu, cập nhật nội dung, validate, lưu thay đổi, ghi lịch sử. | - |
| **Primary Actor** | Admin | - |
| **Phiên bản (Version)** | 0.1.0 | - |
| **Trạng thái (Status)** | Draft | - |
| **Tác giả (Author)** |  | - |
| **Ngày (Date)** |  | Ngày cập nhật gần nhất |
| **Liên kết UC/BR/NFR** | `UC/UC-A2/UCA02-3_Sua_cong_thuc_he_thong.md` | BR/NFR trong UC |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid | Lưu kèm trong file |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Recipe`, `RecipeVersion` | - |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng (Lifeline) | Vai trò/Loại (Stereotype) | Chủ quản (Ownership) | Giao thức/Interface (Protocol) | Phiên bản API | Mô tả chi tiết |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | Admin GUI | Boundary | Web Admin | HTTP | n/a | Form sửa công thức |
| L2 | RecipeAdminController | Control | Core | Internal | v1 | Điều phối |
| L3 | RecipeAdminService | Service | Core | Internal | v1 | Nghiệp vụ cập nhật |
| L4 | AuthZService | Service | Core | Internal | v1 | Kiểm tra quyền `Recipe.Update` |
| L5 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Đọc/ghi công thức |
| L6 | MediaService | Service | Core | Internal | v1 | Upload/validate media |
| L7 | VersioningService | Service | Core | Internal | v1 | Ghi lịch sử/phiên bản |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as Admin GUI
  participant L2 as RecipeAdminController
  participant L4 as AuthZService
  participant L3 as RecipeAdminService
  participant L5 as RecipeRepository
  participant L6 as MediaService
  participant L7 as VersioningService

  L1->>L2: updateRecipe(recipeId, payload)
  L2->>L4: checkPermission(adminId, "Recipe.Update")
  alt allowed
    L2->>L3: updateRecipe(recipeId, payload)
    opt media changes
      L3->>L6: uploadAndValidate(media)
      L6-->>L3: mediaUrls
    end
    L3->>L7: snapshotVersion(recipeId, beforeChange)
    L7-->>L3: versionId
    L3->>L5: updateRecipeDB(recipeId, mergedPayload)
    L5-->>L3: OK
    L3-->>L2: Success
    L2-->>L1: showToast("Cập nhật thành công")
  else denied
    L2-->>L1: showError("Bạn không có quyền")
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Thông điệp (Message) | Sync/Async | Input | Output | Nguồn | Đích | Lỗi/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Submit sửa | `updateRecipe(...)` | Sync | `{ recipeId, payload }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra quyền | `checkPermission(..., "Recipe.Update")` | Sync | `{ adminId }` | `{ allowed }` | L2 | L4 | 403 | N/A |
| 3 | Ảnh/media | `uploadAndValidate(...)` | Sync | `{ files }` | `{ urls }` | L3 | L6 | timeout | Đang mở |
| 4 | Versioning | `snapshotVersion(...)` | Sync | `{ recipeId }` | `{ versionId }` | L3 | L7 | 5xx | Ghi |
| 5 | Cập nhật DB | `updateRecipeDB(...)` | Sync | `{ recipeId, data }` | `OK` | L3 | L5 | 5xx | Ghi |
| 6 | Phản hồi UI | `showToast(...)` | Sync | `{ message }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Luồng Thay thế / Ngoại lệ (Alternative / Exception Flows)

| Fragment ID | Loại | Guard Condition | Ảnh hưởng bước | Error Code/Type | Chiến lược khôi phục | Thông điệp hiển thị | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AF-1 | [opt] | Lưu nháp | Thay thế 5 | DRAFT | Lưu draft | "Đã lưu nháp" | log: info |
| EF-1 | [alt] | Thiếu quyền | Thay thế 3-6 | PERMISSION_DENIED | Dừng | "Bạn không có quyền" | log: warn |
| EF-2 | [alt] | Dữ liệu không hợp lệ | Thay thế 5-6 | VALIDATION_ERROR | Sửa lỗi | "Dữ liệu không hợp lệ" | log: warn |
| EF-3 | [alt] | Lỗi CSDL | Thay thế 6 | DB_ERROR | Retry | "Không thể cập nhật" | log: error |

---

## V. Ghi Chú và Ràng Buộc (Additional Information)

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | ≥3 nguyên liệu và ≥3 bước; media đạt chuẩn |
| Reliability | Lưu lịch sử thay đổi để audit |
| Usability | Giữ layout/validation nhất quán |

---

## VI. Tác Động Dữ Liệu (Data Impact)

| Entity/Bảng | Hành động | Trường bị ảnh hưởng | Ràng buộc |
| :--- | :--- | :--- | :--- |
| `Recipe` | UPDATE | fields được chỉnh sửa | - |
| `RecipeVersion` | INSERT | snapshot trước khi sửa | Phục vụ rollback |

---

## VII. Giả Định & Câu Hỏi Mở (Assumptions & Open Questions)

- Giả định: VersioningService lưu diff/snapshot đầy đủ.
- Câu hỏi mở: Có hỗ trợ rollback từng phần?

---

## VIII. Nguồn Biểu Đồ (Diagram Source)

- Mermaid embedded ở mục III.





