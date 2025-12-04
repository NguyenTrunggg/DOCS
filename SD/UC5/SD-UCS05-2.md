# Template Đặc Tả SEQUENCE DIAGRAM (SD)

## I. Thông Tin Tổng Quan (Header Information)

| Trường (Field) | Nội dung | Ghi chú/Ví dụ |
| :--- | :--- | :--- |
| **SD ID** | SD-UCS05-2 | Tương ứng UCS05-2 |
| **Related UC ID** | UCS05-2 | Xem danh sách nguyên liệu trong tủ |
| **SD Name** | Luồng xem danh sách nguyên liệu |
| **Description** | Người dùng mở "Tủ lạnh ảo"; hệ thống truy vấn kho cá nhân, hiển thị bảng/thẻ với filter/sort/paging và cảnh báo hết hạn. |
| **Primary Actor** | User |
| **Phiên bản (Version)** | 0.1.0 |
| **Trạng thái (Status)** | Draft |
| **Tác giả (Author)** |  |
| **Ngày (Date)** |  |
| **Liên kết UC/BR/NFR** | `UC/UC5/UCS05-2_Xem_danh_sach_nguyen_lieu_trong_tu.md` |
| **Nguồn biểu đồ (Diagram Source)** | Mermaid |
| **Tài liệu liên quan (Related Artifacts)** | API Spec, DB `PantryItem` |

---

## II. Danh Sách Đối Tượng Tham Gia (Participants / Lifelines)

| ID | Tên Đối tượng | Stereotype | Ownership | Protocol | API Ver | Mô tả |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| L1 | User App/Web | Boundary | Client | HTTP | n/a | UI tủ lạnh ảo |
| L2 | PantryController | Control | Core | Internal | v1 | Điều phối |
| L3 | PantryQueryService | Service | Core | Internal | v1 | Truy vấn danh sách & thống kê |
| L4 | PantryRepository | Entity/DAO | Data | SQL | n/a | Lấy `PantryItem` theo user |

---

## III. Biểu Đồ Sequence Diagram (Visual Model)

```mermaid
sequenceDiagram
  participant L1 as User App/Web
  participant L2 as PantryController
  participant L3 as PantryQueryService
  participant L4 as PantryRepository

  L1->>L2: openPantry(page, filters, sort)
  L2->>L3: getPantry(userId, query)
  L3->>L4: findPantryItems(userId, query)
  L4-->>L3: { items, total, stats }
  L3-->>L2: { items, total, stats }
  L2-->>L1: renderPantry(items, total, stats)
```

---

## IV. Đặc Tả Chi Tiết Luồng Tương Tác (Interaction Flow Specification)

### A. Luồng Thành công Chính (Basic Success Flow)

| STT | Hành động | Message | Sync/Async | Input | Output | Source | Target | Error/Timeout | Txn |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Mở tủ | `openPantry(...)` | Sync | `{ page, filters, sort }` | `200` | L1 | L2 | 401 | N/A |
| 2 | Truy vấn | `findPantryItems(...)` | Sync | `{ userId, query }` | `{ items, total, stats }` | L3 | L4 | 5xx | Đọc |
| 3 | Render | `renderPantry(...)` | Sync | `{ items, stats }` | UI updated | L2 | L1 | - | N/A |

### B. Alternative/Exception Flows

| ID | Type | Guard | Affect | Error | Recovery | UI Message | Telemetry |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| EF-1 | [alt] | Trống | Thay thế 3 | EMPTY | Gợi ý thêm | "Tủ trống" | log: info |
| EF-2 | [alt] | Lỗi tải | Thay thế 3 | SERVER_ERROR | Retry | "Không thể tải" | log: error |

---

## V. Ghi Chú & Ràng Buộc

| Trường | Chi tiết |
| :--- | :--- |
| Performance | Tải < 2s; phân trang 50/trang |
| Security | Dữ liệu theo userId |

---

## VI. Tác Động Dữ Liệu

| Bảng | Hành động | Trường |
| :--- | :--- | :--- |
| `PantryItem` | READ | by userId |

---

## VII. Giả Định & Câu Hỏi Mở

- Giả định: Nhớ chế độ hiển thị grid/list.
- Câu hỏi mở: Có export PDF/Excel?

---

## VIII. Nguồn Biểu Đồ

- Mermaid embedded ở mục III.





