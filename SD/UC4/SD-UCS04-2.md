# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCS04-2 | Tương ứng UCS04-2 |
| **Related UC ID** | UCS04-2 | Gỡ công thức khỏi Yêu thích |
| **SD Name** | Luồng gỡ công thức khỏi Yêu thích |
| **Description** | Người dùng bỏ yêu thích; hệ thống kiểm tra tồn tại, xóa bản ghi, giảm đếm yêu thích và phản hồi UI. |
| **Primary Actor** | User |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC4/UCS04-2_Go_cong_thuc_khoi_yeu_thich.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Favorite`, `Recipe` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | User App/Web | Boundary | Client | HTTP | n/a | UI chi tiết/danh sách |
| L2 | FavoriteController | Control | Core | Internal | v1 | Điều phối remove favorite |
| L3 | AuthZService | Service | Core | Internal | v1 | Kiểm tra login |
| L4 | FavoriteService | Service | Core | Internal | v1 | Kiểm tra/ghi DB |
| L5 | FavoriteRepository | Entity/DAO | Data | SQL | n/a | Xóa bản ghi yêu thích |
| L6 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Giảm đếm yêu thích |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as User App/Web
  participant L2 as FavoriteController
  participant L3 as AuthZService
  participant L4 as FavoriteService
  participant L5 as FavoriteRepository
  participant L6 as RecipeRepository

  L1->>L2: removeFromFavorite(recipeId)
  L2->>L3: ensureAuthenticated(userId)
  alt authenticated
    L2->>L4: removeFavorite(userId, recipeId)
    L4->>L5: exists(userId, recipeId)?
    alt exists
      L4->>L5: deleteFavorite(userId, recipeId)
      L5-->>L4: OK
      L4->>L6: decrementFavoriteCount(recipeId)
      L6-->>L4: OK
      L4-->>L2: Success
      L2-->>L1: setUnfavoritedState()+toast
    else not exists
      L4-->>L2: NotFound
      L2-->>L1: showInfo("Không có trong yêu thích")
    end
  else not logged in
    L2-->>L1: promptLogin()
  end
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Yêu cầu gỡ | `removeFromFavorite(recipeId)` | Sync | `{ recipeId }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Kiểm tra login | `ensureAuthenticated(userId)` | Sync | `{ userId }` | `OK` | L2 | L3 | 401 | N/A |
| 3 | Kiểm tra tồn tại | `exists(userId, recipeId)` | Sync | `{ u, r }` | `{ bool }` | L4 | L5 | 5xx | Đọc |
| 4 | Xóa yêu thích | `deleteFavorite(...)` | Sync | `{ u, r }` | `OK` | L4 | L5 | 5xx | Ghi |
| 5 | Giảm đếm | `decrementFavoriteCount(...)` | Async | `{ recipeId }` | `OK` | L4 | L6 | 5xx | Ghi |
| 6 | Phản hồi UI | `setUnfavoritedState()` | Sync | `-` | UI updated | L2 | L1 | - | Kết thúc |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Không tồn tại | Thay thế 4-6 | NOT_FOUND | No-op | "Không có trong yêu thích" | log: info |
| EF-2 | [alt] | Lỗi DB | Thay thế 6 | DB_ERROR | Retry | "Không thể gỡ" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Observability | Telemetry: favorite_remove, not_found |
| Business Rules | Chỉ gỡ của chính chủ, realtime update |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Favorite` | DELETE | userId, recipeId |
| `Recipe` | UPDATE | favoritesCount-1 |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Giảm đếm bất đồng bộ.
- Câu hỏi mở: Có khóa cập nhật đếm để chống lệch số?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





