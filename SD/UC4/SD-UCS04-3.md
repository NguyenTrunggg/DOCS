# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCS04-3 | Tương ứng UCS04-3 |
| **Related UC ID** | UCS04-3 | Xem danh sách công thức Yêu thích |
| **SD Name** | Luồng xem danh sách yêu thích |
| **Description** | Người dùng mở trang yêu thích; hệ thống truy vấn danh sách yêu thích, hiển thị với filter/sort/paging và thống kê cơ bản. |
| **Primary Actor** | User |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC4/UCS04-3_Xem_danh_sach_cong_thuc_yeu_thich.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `Favorite`, `Recipe` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | User App/Web | Boundary | Client | HTTP | n/a | UI Favorites |
| L2 | FavoriteListController | Control | Core | Internal | v1 | Điều phối |
| L3 | FavoriteListService | Service | Core | Internal | v1 | Truy vấn danh sách & thống kê |
| L4 | FavoriteRepository | Entity/DAO | Data | SQL | n/a | Lấy favorite theo user |
| L5 | RecipeRepository | Entity/DAO | Data | SQL | n/a | Lấy thông tin công thức |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as User App/Web
  participant L2 as FavoriteListController
  participant L3 as FavoriteListService
  participant L4 as FavoriteRepository
  participant L5 as RecipeRepository

  L1->>L2: openFavorites(page, filters, sort)
  L2->>L3: getFavorites(userId, query)
  L3->>L4: findFavoritesByUser(userId, query)
  L4-->>L3: { favoriteRecipeIds, total }
  L3->>L5: getRecipes(favoriteRecipeIds)
  L5-->>L3: recipes
  L3-->>L2: { recipes, total }
  L2-->>L1: renderFavorites(recipes, total)
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Mở favorites | `openFavorites(...)` | Sync | `{ page, filters, sort }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Lấy favorites | `findFavoritesByUser(...)` | Sync | `{ userId, query }` | `{ ids, total }` | L3 | L4 | 5xx | Đọc |
| 3 | Lấy recipes | `getRecipes(ids)` | Sync | `{ ids[] }` | `{ recipes }` | L3 | L5 | 5xx | Đọc |
| 4 | Render | `renderFavorites(...)` | Sync | `{ recipes, total }` | UI updated | L2 | L1 | - | N/A |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Trống | Thay thế 4 | EMPTY | Gợi ý khám phá | "Chưa có yêu thích" | log: info |
| EF-2 | [alt] | Lỗi tải | Thay thế 4 | SERVER_ERROR | Retry | "Không thể tải" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Performance | Cache 10 phút; phân trang 20/trang |
| Security | Chỉ hiển thị của chính user |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `Favorite` | READ | by userId |
| `Recipe` | READ | by ids |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Có export danh sách.
- Câu hỏi mở: Có chia sẻ toàn bộ Favorites?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





