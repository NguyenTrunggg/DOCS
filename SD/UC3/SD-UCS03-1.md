# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCS03-1 | Tương ứng UCS03-1 |
| **Related UC ID** | UCS03-1 | Thêm công thức mới |
| **SD Name** | Luồng người dùng tạo công thức mới |
| **Description** | Người dùng điền form, upload media, validate và gửi công thức ở trạng thái "Chờ duyệt"; thông báo tới admin. |
| **Primary Actor** | User |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC3/UCS03-1_Them_cong_thuc_moi.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Recipe`, `Media`, Notification |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | User App/Web | Boundary | Client | HTTP | n/a | UI tạo công thức |
| L2 | UserRecipeController | Control | Core | Internal | v1 | Điều phối |
| L3 | UserRecipeService | Service | Core | Internal | v1 | Nghiệp vụ tạo |
| L4 | MediaService | Service | Core | Internal | v1 | Upload/validate media |
| L5 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Lưu công thức |
| L6 | NotificationService | Service | Core | Internal | v1 | Thông báo admin |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as User App/Web
  participant L2 as UserRecipeController
  participant L3 as UserRecipeService
  participant L4 as MediaService
  participant L5 as RecipeRepository
  participant L6 as NotificationService

  L1->>L2: createUserRecipe(payload, media[])
  L2->>L3: create(payload)
  opt has media
    L3->>L4: uploadAndValidate(media[])
    L4-->>L3: mediaUrls
  end
  L3->>L5: insertRecipe({...payload, mediaUrls, status:"Pending", author:userId})
  L5-->>L3: recipeId
  L3->>L6: notifyAdminNewRecipe(recipeId)
  L6-->>L3: Accepted
  L3-->>L2: { recipeId }
  L2-->>L1: showSuccess("Gửi duyệt thành công")

  %% Save draft
  L1->>L2: saveDraft(payload)
  L2->>L3: saveDraft(payload)
  L3->>L5: upsertDraft(userId, payload)
  L5-->>L3: draftId
  L3-->>L2: { draftId }
  L2-->>L1: showSavedDraft()
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Submit | `createUserRecipe(...)` | Sync | `{ payload, media? }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Upload | `uploadAndValidate(...)` | Sync | `{ files }` | `{ urls }` | L3 | L4 | timeout | Đang mở |
| 3 | Ghi DB | `insertRecipe(...)` | Sync | `{ data }` | `{ recipeId }` | L3 | L5 | 5xx | Ghi |
| 4 | Notify | `notifyAdminNewRecipe(...)` | Async | `{ recipeId }` | `Accepted` | L3 | L6 | timeout | N/A |
| 5 | Phản hồi | `showSuccess(...)` | Sync | `{ msg }` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| AF-1 | [opt] | Lưu nháp | Thay thế 3-5 | DRAFT | Lưu draft | "Đã lưu nháp" | log: info |
| EF-1 | [alt] | Thiếu thông tin | Thay thế 3-5 | VALIDATION_ERROR | Sửa form | "Thiếu thông tin bắt buộc" | log: warn |
| EF-2 | [alt] | Ảnh không hợp lệ | Thay thế 3-5 | MEDIA_INVALID | Chọn lại | "Ảnh không hợp lệ" | log: warn |
| EF-3 | [alt] | Lỗi DB | Thay thế 5 | DB_ERROR | Retry | "Không thể lưu" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Business Rules | ≥3 nguyên liệu, ≥3 bước; ảnh ≤5MB JPG/PNG |
| Security | Scan media; sanitize nội dung |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Recipe` | INSERT | all fields, status=Pending |
| `DraftRecipe` | UPSERT | userId, payload |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Import từ AI/session khả dụng.
- Câu hỏi mở: Giới hạn số công thức/ngày?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





