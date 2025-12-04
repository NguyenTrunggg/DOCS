# TÓM TẮT USE CASES - HỆ THỐNG QUẢN LÝ CÔNG THỨC NẤU ĂN

## MỤC LỤC

1. [Tổng quan Hệ thống](#1-tổng-quan-hệ-thống)
2. [Nhóm Chức năng Admin (UC-A)](#2-nhóm-chức-năng-admin-uc-a)
3. [Nhóm Chức năng User (UC1-UC6)](#3-nhóm-chức-năng-user-uc1-uc6)
4. [Bảng Tổng hợp](#4-bảng-tổng-hợp)
5. [Đặc điểm Nổi bật](#5-đặc-điểm-nổi-bật)
6. [Yêu cầu Phi chức năng Chính](#6-yêu-cầu-phi-chức-năng-chính)
7. [Use Case Diagrams](#7-use-case-diagrams)
8. [Sequence Diagrams](#8-sequence-diagrams)
9. [Class Diagrams](#9-class-diagrams)
10. [Entity Relationship Diagrams](#10-entity-relationship-diagrams)
11. [API Specification](#11-api-specification)
12. [Kết luận](#12-kết-luận)

---

## 1. TỔNG QUAN HỆ THỐNG

### 1.1. Giới thiệu
Hệ thống quản lý công thức nấu ăn là một nền tảng cho phép người dùng tìm kiếm, chia sẻ và quản lý các công thức nấu ăn một cách thông minh, tích hợp công nghệ AI và các tính năng tiên tiến.

### 1.2. Phân loại Actors
- **Admin**: Quản trị viên hệ thống, có quyền quản lý toàn bộ dữ liệu và người dùng
- **Super Admin**: Quản trị viên cấp cao, có thêm quyền quản lý tài khoản admin và phân quyền
- **User**: Người dùng cuối, sử dụng các chức năng tìm kiếm, tạo công thức, tương tác cộng đồng

### 1.3. Thống kê tổng quan
- **Tổng số Use Cases**: 51
- **Use Cases Admin**: 24 use cases (UC-A1 đến UC-A5)
- **Use Cases User**: 27 use cases (UC1 đến UC6)
- **Use Cases Priority High**: 4 use cases
- **Use Cases Priority Medium**: 15 use cases
- **Use Cases Priority Low**: 32 use cases

---

## 2. NHÓM CHỨC NĂNG ADMIN (UC-A)

### UC-A1: Quản lý Người dùng (4 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCA01-1 | Xem danh sách người dùng | Hiển thị danh sách tất cả người dùng với tìm kiếm, lọc, phân trang. Tìm kiếm theo tên/email, lọc theo trạng thái, sắp xếp. | Low | Read |
| UCA01-2 | Xem chi tiết người dùng | Xem thông tin chi tiết: profile, thống kê hoạt động, lịch sử công thức đã đăng. | Low | Read |
| UCA01-3 | Vô hiệu hóa tài khoản | Khóa tài khoản người dùng vi phạm với lý do, thời hạn (15 phút, 1 ngày, vĩnh viễn). Đăng xuất toàn bộ session, audit đầy đủ. | Medium | Update |
| UCA01-4 | Kích hoạt lại tài khoản | Mở khóa tài khoản đã bị khóa. Tự động mở khóa khi hết hạn, audit đầy đủ. | Medium | Update |

**Đặc điểm chính**: Audit trail đầy đủ, đăng xuất toàn bộ session ngay lập tức.

### UC-A2: Quản lý Công thức (7 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCA02-1 | Xem danh sách công thức hệ thống | Xem tất cả công thức (hệ thống + user đã duyệt). Lọc theo trạng thái, người tạo, sắp xếp theo điểm đánh giá. | Low | Read |
| UCA02-2 | Thêm công thức hệ thống | Tạo công thức mới với trạng thái "Đã duyệt". Import từ file, sao chép từ công thức có sẵn. | Medium | Create |
| UCA02-3 | Sửa công thức hệ thống | Chỉnh sửa toàn bộ thông tin công thức. Versioning để có thể hoàn nguyên. | Medium | Update |
| UCA02-4 | Xóa công thức hệ thống | Xóa vĩnh viễn, yêu cầu xác nhận đặc biệt cho công thức có tương tác cao. Soft delete với khôi phục trong 30 ngày. | Low | Delete |
| UCA02-5 | Xem danh sách công thức chờ duyệt | Hiển thị công thức do user gửi đang chờ duyệt. Phân công kiểm duyệt viên, tìm kiếm, lọc theo người gửi. | Low | Read |
| UCA02-6 | Phê duyệt công thức | Duyệt công thức của user. Có thể chỉnh sửa nhẹ trước khi duyệt. Gửi thông báo cho user. | Medium | Update |
| UCA02-7 | Từ chối công thức | Từ chối với lý do rõ ràng, có gợi ý chỉnh sửa. Cung cấp mẫu lý do nhanh. Gửi thông báo kèm lý do. | Medium | Update |

**Đặc điểm chính**: Workflow phê duyệt rõ ràng, gửi thông báo cho người tạo.

### UC-A3: Quản lý Danh mục & Nguyên liệu (8 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCA03-1 | Xem danh sách danh mục món ăn | Xem tất cả danh mục với tìm kiếm, lọc. Sắp xếp theo tên. | Low | Read |
| UCA03-2 | Thêm danh mục mới | Tạo danh mục mới với tên, mô tả. Kiểm tra trùng lặp. | Low | Create |
| UCA03-3 | Sửa tên danh mục | Chỉnh sửa tên và mô tả danh mục. Cập nhật tất cả công thức liên quan. | Low | Update |
| UCA03-4 | Xóa danh mục | Xóa danh mục với kiểm tra ràng buộc. Yêu cầu chuyển công thức sang danh mục khác nếu có ràng buộc. | Low | Delete |
| UCA03-5 | Xem danh sách nguyên liệu chuẩn hóa | Xem tất cả nguyên liệu trong DB chuẩn hóa. Tìm kiếm, lọc theo đơn vị, danh mục. | Low | Read |
| UCA03-6 | Thêm nguyên liệu mới vào DB chuẩn hóa | Thêm nguyên liệu với tên, đơn vị chuẩn, danh mục. Liên kết từ đồng nghĩa, kiểm tra trùng lặp. | Medium | Create |
| UCA03-7 | Sửa thông tin nguyên liệu | Chỉnh sửa tên, đơn vị, danh mục của nguyên liệu. Cập nhật tất cả công thức sử dụng nguyên liệu. | Low | Update |
| UCA03-8 | Xóa nguyên liệu | Xóa nguyên liệu với kiểm tra ràng buộc. Xử lý foreign key constraints, đề xuất giải pháp thay thế. | Low | Delete |

**Đặc điểm chính**: Chuẩn hóa dữ liệu, xử lý ràng buộc khóa ngoại trước khi xóa.

### UC-A4: Quản lý Tài khoản Admin (4 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCA04-1 | Xem danh sách tài khoản admin | Xem tất cả tài khoản admin khác. Lọc theo vai trò/trạng thái. | Low | Read |
| UCA04-2 | Tạo tài khoản admin mới | Tạo tài khoản mới ở trạng thái "Chờ kích hoạt", gửi email kích hoạt. Import hàng loạt từ CSV. | Medium | Create |
| UCA04-3 | Phân quyền cho tài khoản admin | **HIGH PRIORITY** - Gán roles/permissions theo nguyên tắc least privilege. Xem effective permissions, áp dụng role template. | High | Update |
| UCA04-4 | Xóa tài khoản admin | Xóa với xác nhận. Ngăn chặn xóa Super Admin cuối cùng. | Low | Delete |

**Đặc điểm chính**: RBAC/ABAC rõ ràng, audit đầy đủ các thay đổi quyền, không cho phép hệ thống không còn Super Admin.

### UC-A5: Báo cáo & Thống kê (1 Use Case)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCA05-1 | Xem báo cáo thống kê | **HIGH PRIORITY** - Dashboard với KPIs: người dùng, công thức, tỷ lệ phê duyệt, tương tác. Biểu đồ tương tác: đường (tăng trưởng), cột (công thức mới), tròn (trạng thái), heatmap (hoạt động). Drill-down, xuất PDF/CSV. | High | Read |

**Đặc điểm chính**: Dashboard tương tác với nhiều loại biểu đồ, xuất báo cáo, lưu preset.

---

## 3. NHÓM CHỨC NĂNG USER (UC1-UC6)

### UC1: Authentication & Profile (7 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCS01-1 | Đăng ký tài khoản | Đăng ký bằng email/SĐT, xác thực qua email/SMS. OAuth với Google/Facebook. Mật khẩu tối thiểu 8 ký tự (chữ hoa, thường, số). | Low | Create |
| UCS01-2 | Đăng nhập hệ thống | Đăng nhập email/SĐT/OAuth. "Ghi nhớ đăng nhập" (30 ngày). Rate limiting: khóa 15 phút sau 5 lần sai. | Low | Read |
| UCS01-3 | Đăng xuất hệ thống | Hủy session và cookie. Xác nhận trước khi đăng xuất. | Low | N/A |
| UCS01-4 | Xem thông tin cá nhân | Xem profile đầy đủ, thống kê hoạt động (số công thức đăng/yêu thích/bình luận). | Low | Read |
| UCS01-5 | Cập nhật thông tin cá nhân | Cập nhật tên hiển thị, ảnh đại diện. Preview ảnh, resize tự động 150x150px. | Medium | Update |
| UCS01-6 | Đổi mật khẩu | Đổi mật khẩu với xác thực mật khẩu cũ. Vô hiệu hóa tất cả session hiện tại, ghi log bảo mật. | Medium | Update |
| UCS01-7 | Đặt lại mật khẩu | Reset mật khẩu khi quên. Token 24 giờ, gửi email/SMS. Giới hạn 3 lần/giờ. | Medium | Update |

**Đặc điểm chính**: Bảo mật mật khẩu, rate limiting, audit đăng xuất toàn bộ session.

### UC2: Tìm kiếm & Khám phá Công thức (5 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCS02-1 | Tìm kiếm theo nguyên liệu | **HIGH PRIORITY** - Thuật toán 3 mức ưu tiên: (1) Tất cả nguyên liệu, (2) N-1 nguyên liệu, (3) Gợi ý thêm 1-2 nguyên liệu. Tích hợp "Tủ lạnh ảo", cache 30 phút. | High | Read |
| UCS02-2 | Tìm kiếm theo tên món ăn | Tìm kiếm exact/partial/fuzzy search, không dấu. Autocomplete real-time. | Medium | Read |
| UCS02-3 | Tạo công thức bằng AI | **HIGH PRIORITY** - GPT API tạo công thức từ nguyên liệu. Giới hạn 5 lần/ngày, thời gian 15s. Lưu tạm 24h, không lưu vào DB chính. | High | Create |
| UCS02-4 | Lọc và sắp xếp kết quả | Lọc theo loại món, thời gian, độ khó, khẩu phần, điểm đánh giá. Sắp xếp theo liên quan/thời gian/điểm/tên. Lưu preset 5 bộ lọc. | Medium | Read |
| UCS02-5 | Xem chi tiết công thức | Xem đầy đủ: nguyên liệu, bước thực hiện, ảnh minh họa, đánh giá, bình luận. Tăng lượt xem, lịch sử xem. | Medium | Read |

**Đặc điểm chính**: Thuật toán tìm kiếm thông minh, tích hợp AI, tối ưu performance.

### UC3: Quản lý Công thức Cá nhân (4 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCS03-1 | Thêm công thức mới | Tạo công thức ở trạng thái "Chờ duyệt". Form đầy đủ: tên, mô tả, ảnh, danh mục, thời gian, độ khó, nguyên liệu, bước thực hiện. Import từ AI, sao chép từ công thức khác. Giới hạn 10 công thức/ngày. | Medium | Create |
| UCS03-2 | Xem danh sách công thức đã tạo | Xem tất cả công thức của mình với trạng thái: Chờ duyệt/Đã duyệt/Bị từ chối. Thống kê tổng quan, tìm kiếm, lọc. | Low | Read |
| UCS03-3 | Chỉnh sửa công thức đã tạo | Chỉnh sửa khi ở trạng thái "Chờ duyệt"/"Bị từ chối"/"Nháp". Nếu từ "Bị từ chối" → chuyển về "Chờ duyệt". Giữ nguyên layout như tạo mới. | Medium | Update |
| UCS03-4 | Xóa công thức đã tạo | Xóa với xác nhận đơn giản cho công thức chưa duyệt. Xác nhận đặc biệt (nhập lại tên) cho công thức có tương tác cao. | Low | Delete |

**Đặc điểm chính**: Workflow duyệt rõ ràng, chỉ cho phép sửa khi chưa được duyệt.

### UC4: Tương tác Xã hội (6 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCS04-1 | Thêm công thức vào yêu thích | Thêm vào yêu thích, tối đa 500. Tăng số lượt yêu thích của công thức. Animation phản hồi. | Low | Create |
| UCS04-2 | Gỡ công thức khỏi yêu thích | Gỡ khỏi danh sách yêu thích, giảm số lượt. Gỡ hàng loạt, ghi log. | Low | Delete |
| UCS04-3 | Xem danh sách công thức yêu thích | Xem tất cả yêu thích, lọc theo danh mục/độ khó/thời gian. Sắp xếp theo thời gian thêm/tên/điểm. | Low | Read |
| UCS04-4 | Gửi đánh giá và bình luận | Đánh giá 1-5 sao, bình luận tối đa 1000 ký tự, tag đánh giá nhanh. Đính kèm ảnh món ăn đã nấu. Kiểm duyệt tự động. Cập nhật điểm TB real-time. | Medium | Create |
| UCS04-5 | Xem đánh giá và bình luận | Xem tất cả đánh giá với biểu đồ phân phối điểm, tỷ lệ %, lọc theo số sao, sắp xếp theo hữu ích. Đánh dấu "Hữu ích". | Low | Read |
| UCS04-6 | Chia sẻ công thức | Chia sẻ qua Facebook/Instagram/Twitter/Pinterest/WhatsApp/Telegram. Email, copy link, QR code, in công thức. Tự động điền nội dung. | Low | Read |

**Đặc điểm chính**: Tương tác cộng đồng đầy đủ, chia sẻ đa nền tảng.

### UC5: Tủ lạnh ảo (4 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCS05-1 | Thêm nguyên liệu vào tủ | Thêm nguyên liệu với số lượng, đơn vị, ngày hết hạn. Autocomplete từ danh mục chuẩn hóa. Quét mã vạch, nhập hàng loạt CSV. Gộp nếu trùng. | Low | Create |
| UCS05-2 | Xem danh sách nguyên liệu trong tủ | Xem danh sách với màu cảnh báo (sắp hết hạn/đã hết hạn). Lọc theo trạng thái, danh mục. Sắp xếp theo tên/ngày hết hạn. Phân trang 50 mục. | Low | Read |
| UCS05-3 | Cập nhật số lượng nguyên liệu | Cập nhật số lượng/đơn vị/ngày hết hạn. Nút +/- cập nhật nhanh. Quy đổi đơn vị tự động (1000g = 1kg). Cập nhật hàng loạt. | Low | Update |
| UCS05-4 | Xóa nguyên liệu khỏi tủ | Xóa với xác nhận. Gợi ý chuyển sang danh sách mua sắm. Xóa hàng loạt. Auto-delete khi hết hạn (nếu bật). | Low | Delete |

**Đặc điểm chính**: Quản lý tồn kho thông minh, cảnh báo hết hạn, quy đổi đơn vị tự động.

### UC6: Kế hoạch Bữa ăn (5 Use Cases)

| ID | Use Case | Mô tả | Priority | CRUD |
|---|---|---|---|---|
| UCS06-1 | Tạo kế hoạch bữa ăn | Tạo kế hoạch theo ngày/tuần/tháng. Thêm món cho từng bữa (sáng/trưa/tối/phụ). Chọn từ tìm kiếm, tùy chỉnh khẩu phần. Template thực đơn, tạo từ yêu thích, AI gợi ý thực đơn theo mục tiêu. | Medium | Create |
| UCS06-2 | Xem kế hoạch bữa ăn | Xem theo lịch (ngày/tuần/tháng). Kéo/thả đổi vị trí món. Thao tác nhanh: đổi bữa, xóa, sao chép. Lọc theo bữa/danh mục/độ khó. | Low | Read |
| UCS06-3 | Chỉnh sửa kế hoạch bữa ăn | Kéo/thả món giữa các bữa/ngày. Thay thế, thêm, xóa món. Đổi khẩu phần. Batch edit, undo/redo. Auto-save nháp. | Medium | Update |
| UCS06-4 | Xóa kế hoạch bữa ăn | Xóa kế hoạch với xác nhận. Xóa hàng loạt. Archive thay vì xóa để tham khảo. | Low | Delete |
| UCS06-5 | Tạo danh sách mua sắm từ kế hoạch | **MEDIUM PRIORITY** - Tổng hợp nguyên liệu từ tất cả món trong kế hoạch. Gộp trùng, quy đổi đơn vị về chuẩn. Nhân theo khẩu phần. Trừ đi nguyên liệu có trong "Tủ lạnh ảo". Nhóm theo danh mục. | Medium | Create |

**Đặc điểm chính**: Kế hoạch bữa ăn thông minh, tự động tạo danh sách mua sắm, tích hợp AI.

---

## 4. BẢNG TỔNG HỢP

### 4.1. Ma trận Use Cases theo Priority

| Priority | Số lượng | Tỷ lệ | Use Cases |
|---|---|---|---|
| **High** | 4 | 7.8% | UCA04-3, UCA05-1, UCS02-1, UCS02-3 |
| **Medium** | 15 | 29.4% | UCA01-3, UCA01-4, UCA02-2, UCA02-3, UCA02-6, UCA02-7, UCA03-6, UCA04-2, UCS01-5, UCS01-6, UCS01-7, UCS02-2, UCS02-4, UCS02-5, UCS03-1, UCS03-3, UCS04-4, UCS06-1, UCS06-3, UCS06-5 |
| **Low** | 32 | 62.8% | Các use cases còn lại |

### 4.2. Phân loại CRUD Operations

| Operation | Số lượng | Mô tả |
|---|---|---|
| **Create** | 13 | Tạo mới: đăng ký, tạo công thức, thêm yêu thích, tạo kế hoạch |
| **Read** | 21 | Xem danh sách, xem chi tiết, xem báo cáo, xem thống kê |
| **Update** | 13 | Cập nhật thông tin, đổi mật khẩu, sửa công thức, phân quyền |
| **Delete** | 4 | Xóa công thức, xóa nguyên liệu, xóa kế hoạch |

### 4.3. Actors và Permissions

| Actor | Quyền | Use Cases |
|---|---|---|
| **Super Admin** | Quản lý toàn bộ hệ thống | UCA04-1, UCA04-2, UCA04-3, UCA04-4, tất cả UC-A khác |
| **Admin** | Quản lý dữ liệu nội dung | Tất cả UC-A trừ UC-A4 |
| **User** | Sử dụng tính năng cơ bản | Tất cả UC1-UC6 |
| **Guest** | Xem công thức công khai | UCS02-1, UCS02-2, UCS02-4, UCS02-5, UCS04-3, UCS04-5, UCS04-6 |

---

## 5. ĐẶC ĐIỂM NỔI BẬT

### 5.1. Tích hợp Trí tuệ Nhân tạo (AI)
- **Tạo công thức bằng GPT**: Sử dụng GPT API để tạo công thức mới từ danh sách nguyên liệu
- **Gợi ý thực đơn AI**: Tạo kế hoạch bữa ăn tự động theo mục tiêu (giảm cân, tăng cơ)
- **Kiểm duyệt nội dung**: Tự động kiểm tra spam, lăng mạ trong bình luận

### 5.2. Thuật toán Tìm kiếm Thông minh
- **Tìm kiếm 3 mức ưu tiên**: Tất cả nguyên liệu → N-1 nguyên liệu → Gợi ý thêm nguyên liệu
- **Tích hợp Tủ lạnh ảo**: Import nguyên liệu trực tiếp từ tủ để tìm kiếm
- **Cache kết quả**: Tối ưu performance với cache 15-30 phút

### 5.3. Workflow Phê duyệt Công thức
- **Trạng thái rõ ràng**: Nháp → Chờ duyệt → Đã duyệt / Bị từ chối
- **Thông báo tự động**: Email/SMS khi công thức được duyệt/từ chối
- **Gợi ý chỉnh sửa**: Admin có thể đưa ra gợi ý khi từ chối

### 5.4. Quản lý Tồn kho Thông minh
- **Tủ lạnh ảo**: Quản lý nguyên liệu cá nhân với cảnh báo hết hạn
- **Quy đổi đơn vị tự động**: 1000g = 1kg, hỗ trợ nhiều đơn vị
- **Tạo danh sách mua sắm**: Tự động từ kế hoạch bữa ăn, trừ nguyên liệu có sẵn

### 5.5. Tính năng Xã hội
- **Yêu thích**: Lưu công thức yêu thích, tối đa 500
- **Đánh giá và bình luận**: Hệ thống đánh giá 5 sao với bình luận chi tiết
- **Chia sẻ đa nền tảng**: Facebook, Instagram, Twitter, WhatsApp, Email, QR code

---

## 6. YÊU CẦU PHI CHỨC NĂNG CHÍNH

### 6.1. Performance (Hiệu năng)
- Thời gian tải trang chủ: < 2 giây
- Thời gian tìm kiếm: < 5 giây cho 20 nguyên liệu
- AI tạo công thức: < 15 giây
- Response time API: < 1 giây
- Upload ảnh: < 10 giây
- Cache kết quả tìm kiếm: 15-30 phút

### 6.2. Security (Bảo mật)
- Mật khẩu: Hash bcrypt với salt ngẫu nhiên
- Session timeout: 24 giờ (mặc định), 30 ngày (ghi nhớ)
- Rate limiting: Khóa 15 phút sau 5 lần đăng nhập sai
- SQL Injection: Prepared statements, parameterized queries
- XSS: Sanitize tất cả input từ user
- RBAC/ABAC: Phân quyền rõ ràng, least privilege
- Audit trail: Ghi log tất cả hành động quan trọng
- HTTPS: Bắt buộc cho tất cả kết nối

### 6.3. Usability (Khả năng sử dụng)
- Responsive design: Hỗ trợ mobile, tablet, desktop
- Tự động lưu nháp: Tránh mất dữ liệu khi gián đoạn
- Undo/Redo: Hoàn nguyên thao tác gần nhất
- Real-time validation: Validate ngay khi nhập
- Autocomplete: Gợi ý real-time khi gõ
- Drag & Drop: Kéo thả trực quan
- Keyboard shortcuts: Hỗ trợ phím tắt cho thao tác thường dùng
- Dark mode: Chế độ tối cho mắt

### 6.4. Reliability (Độ tin cậy)
- Xử lý xung đột: Khi cập nhật đồng thời, báo cảnh báo và yêu cầu refresh
- Auto-save: Tự động lưu nháp định kỳ
- Transaction: Đảm bảo tính toàn vẹn dữ liệu
- Error handling: Xử lý lỗi graceful, thông báo rõ ràng
- Backup: Sao lưu dữ liệu định kỳ
- Monitoring: Theo dõi hệ thống real-time

### 6.5. Scalability (Khả năng mở rộng)
- Database indexing: Index các trường tìm kiếm thường dùng
- Pagination: Phân trang 20-50 mục/trang
- Caching: Cache kết quả tìm kiếm, danh sách phổ biến
- CDN: Phân phối tĩnh qua CDN
- Load balancing: Cân bằng tải khi có nhiều server

---

## 7. USE CASE DIAGRAMS

### 7.1. Use Case Diagram - Tổng quát Toàn Hệ thống

![Use Case Diagram - Tổng quát](diagrams/usecase/uc-general.png)

*Sơ đồ Use Case tổng quát mô tả toàn bộ hệ thống với các actors chính: Super Admin, Admin, User, Guest và các module chức năng tương ứng.*

### 7.2. Use Case Diagram - UC-A1: Quản lý Người dùng

![Use Case Diagram - UC-A1](diagrams/usecase/uc-a1-user-management.png)

*Sơ đồ Use Case cho module Quản lý Người dùng bao gồm: Xem danh sách, Xem chi tiết, Vô hiệu hóa và Kích hoạt lại tài khoản.*

### 7.3. Use Case Diagram - UC-A2: Quản lý Công thức

![Use Case Diagram - UC-A2](diagrams/usecase/uc-a2-recipe-management.png)

*Sơ đồ Use Case cho module Quản lý Công thức bao gồm: Xem danh sách, CRUD công thức hệ thống, Phê duyệt và Từ chối công thức.*

### 7.4. Use Case Diagram - UC-A3: Quản lý Danh mục & Nguyên liệu

![Use Case Diagram - UC-A3](diagrams/usecase/uc-a3-category-ingredient-management.png)

*Sơ đồ Use Case cho module Quản lý Danh mục & Nguyên liệu bao gồm: CRUD danh mục món ăn và CRUD nguyên liệu chuẩn hóa.*

### 7.5. Use Case Diagram - UC-A4: Quản lý Tài khoản Admin

![Use Case Diagram - UC-A4](diagrams/usecase/uc-a4-admin-account-management.png)

*Sơ đồ Use Case cho module Quản lý Tài khoản Admin bao gồm: Xem danh sách, Tạo mới, Phân quyền và Xóa tài khoản admin.*

### 7.6. Use Case Diagram - UC-A5: Báo cáo & Thống kê

![Use Case Diagram - UC-A5](diagrams/usecase/uc-a5-analytics-reporting.png)

*Sơ đồ Use Case cho module Báo cáo & Thống kê bao gồm: Xem dashboard với các KPIs và biểu đồ tương tác.*

### 7.7. Use Case Diagram - UC1: Authentication & Profile

![Use Case Diagram - UC1](diagrams/usecase/uc1-authentication-profile.png)

*Sơ đồ Use Case cho module Authentication & Profile bao gồm: Đăng ký, Đăng nhập, Đăng xuất, Xem/Update profile, Đổi mật khẩu, Reset mật khẩu.*

### 7.8. Use Case Diagram - UC2: Tìm kiếm & Khám phá Công thức

![Use Case Diagram - UC2](diagrams/usecase/uc2-search-explore.png)

*Sơ đồ Use Case cho module Tìm kiếm & Khám phá bao gồm: Tìm kiếm theo nguyên liệu/tên, Tạo công thức bằng AI, Lọc/Sắp xếp, Xem chi tiết.*

### 7.9. Use Case Diagram - UC3: Quản lý Công thức Cá nhân

![Use Case Diagram - UC3](diagrams/usecase/uc3-personal-recipe-management.png)

*Sơ đồ Use Case cho module Quản lý Công thức Cá nhân bao gồm: CRUD công thức cá nhân với workflow phê duyệt.*

### 7.10. Use Case Diagram - UC4: Tương tác Xã hội

![Use Case Diagram - UC4](diagrams/usecase/uc4-social-interaction.png)

*Sơ đồ Use Case cho module Tương tác Xã hội bao gồm: Yêu thích, Đánh giá & Bình luận, Chia sẻ công thức.*

### 7.11. Use Case Diagram - UC5: Tủ lạnh ảo

![Use Case Diagram - UC5](diagrams/usecase/uc5-virtual-fridge.png)

*Sơ đồ Use Case cho module Tủ lạnh ảo bao gồm: CRUD nguyên liệu trong tủ với cảnh báo hết hạn và quy đổi đơn vị.*

### 7.12. Use Case Diagram - UC6: Kế hoạch Bữa ăn

![Use Case Diagram - UC6](diagrams/usecase/uc6-meal-planning.png)

*Sơ đồ Use Case cho module Kế hoạch Bữa ăn bao gồm: CRUD kế hoạch bữa ăn, Tạo danh sách mua sắm tự động.*

---

## 8. SEQUENCE DIAGRAMS

### 8.1. Sequence Diagrams - UC-A1: Quản lý Người dùng

#### 9.1.1. UCA01-1: Xem danh sách người dùng
![Sequence Diagram - UCA01-1](diagrams/sequence/sd-uca01-1.png)

#### 9.1.2. UCA01-2: Xem chi tiết người dùng
![Sequence Diagram - UCA01-2](diagrams/sequence/sd-uca01-2.png)

#### 9.1.3. UCA01-3: Vô hiệu hóa tài khoản
![Sequence Diagram - UCA01-3](diagrams/sequence/sd-uca01-3.png)

#### 9.1.4. UCA01-4: Kích hoạt lại tài khoản
![Sequence Diagram - UCA01-4](diagrams/sequence/sd-uca01-4.png)

---

### 8.2. Sequence Diagrams - UC-A2: Quản lý Công thức

#### 9.2.1. UCA02-1: Xem danh sách công thức hệ thống
![Sequence Diagram - UCA02-1](diagrams/sequence/sd-uca02-1.png)

#### 9.2.2. UCA02-2: Thêm công thức hệ thống
![Sequence Diagram - UCA02-2](diagrams/sequence/sd-uca02-2.png)

#### 9.2.3. UCA02-3: Sửa công thức hệ thống
![Sequence Diagram - UCA02-3](diagrams/sequence/sd-uca02-3.png)

#### 9.2.4. UCA02-4: Xóa công thức hệ thống
![Sequence Diagram - UCA02-4](diagrams/sequence/sd-uca02-4.png)

#### 9.2.5. UCA02-5: Xem danh sách công thức chờ duyệt
![Sequence Diagram - UCA02-5](diagrams/sequence/sd-uca02-5.png)

#### 9.2.6. UCA02-6: Phê duyệt công thức
![Sequence Diagram - UCA02-6](diagrams/sequence/sd-uca02-6.png)

#### 9.2.7. UCA02-7: Từ chối công thức
![Sequence Diagram - UCA02-7](diagrams/sequence/sd-uca02-7.png)

---

### 8.3. Sequence Diagrams - UC-A3: Quản lý Danh mục & Nguyên liệu

#### 9.3.1. UCA03-1: Xem danh sách danh mục món ăn
![Sequence Diagram - UCA03-1](diagrams/sequence/sd-uca03-1.png)

#### 9.3.2. UCA03-2: Thêm danh mục mới
![Sequence Diagram - UCA03-2](diagrams/sequence/sd-uca03-2.png)

#### 9.3.3. UCA03-3: Sửa tên danh mục
![Sequence Diagram - UCA03-3](diagrams/sequence/sd-uca03-3.png)

#### 9.3.4. UCA03-4: Xóa danh mục
![Sequence Diagram - UCA03-4](diagrams/sequence/sd-uca03-4.png)

#### 9.3.5. UCA03-5: Xem danh sách nguyên liệu chuẩn hóa
![Sequence Diagram - UCA03-5](diagrams/sequence/sd-uca03-5.png)

#### 9.3.6. UCA03-6: Thêm nguyên liệu mới vào DB chuẩn hóa
![Sequence Diagram - UCA03-6](diagrams/sequence/sd-uca03-6.png)

#### 9.3.7. UCA03-7: Sửa thông tin nguyên liệu
![Sequence Diagram - UCA03-7](diagrams/sequence/sd-uca03-7.png)

#### 9.3.8. UCA03-8: Xóa nguyên liệu
![Sequence Diagram - UCA03-8](diagrams/sequence/sd-uca03-8.png)

---

### 8.4. Sequence Diagrams - UC-A4: Quản lý Tài khoản Admin

#### 9.4.1. UCA04-1: Xem danh sách tài khoản admin
![Sequence Diagram - UCA04-1](diagrams/sequence/sd-uca04-1.png)

#### 9.4.2. UCA04-2: Tạo tài khoản admin mới
![Sequence Diagram - UCA04-2](diagrams/sequence/sd-uca04-2.png)

#### 9.4.3. UCA04-3: Phân quyền cho tài khoản admin
![Sequence Diagram - UCA04-3](diagrams/sequence/sd-uca04-3.png)

#### 9.4.4. UCA04-4: Xóa tài khoản admin
![Sequence Diagram - UCA04-4](diagrams/sequence/sd-uca04-4.png)

---

### 8.5. Sequence Diagrams - UC-A5: Báo cáo & Thống kê

#### 9.5.1. UCA05-1: Xem báo cáo thống kê
![Sequence Diagram - UCA05-1](diagrams/sequence/sd-uca05-1.png)

---

### 8.6. Sequence Diagrams - UC1: Authentication & Profile

#### 9.6.1. UCS01-1: Đăng ký tài khoản
![Sequence Diagram - UCS01-1](diagrams/sequence/sd-ucs01-1.png)

#### 9.6.2. UCS01-2: Đăng nhập hệ thống
![Sequence Diagram - UCS01-2](diagrams/sequence/sd-ucs01-2.png)

#### 9.6.3. UCS01-3: Đăng xuất hệ thống
![Sequence Diagram - UCS01-3](diagrams/sequence/sd-ucs01-3.png)

#### 9.6.4. UCS01-4: Xem thông tin cá nhân
![Sequence Diagram - UCS01-4](diagrams/sequence/sd-ucs01-4.png)

#### 9.6.5. UCS01-5: Cập nhật thông tin cá nhân
![Sequence Diagram - UCS01-5](diagrams/sequence/sd-ucs01-5.png)

#### 9.6.6. UCS01-6: Đổi mật khẩu
![Sequence Diagram - UCS01-6](diagrams/sequence/sd-ucs01-6.png)

#### 9.6.7. UCS01-7: Đặt lại mật khẩu
![Sequence Diagram - UCS01-7](diagrams/sequence/sd-ucs01-7.png)

---

### 8.7. Sequence Diagrams - UC2: Tìm kiếm & Khám phá Công thức

#### 9.7.1. UCS02-1: Tìm kiếm theo nguyên liệu
![Sequence Diagram - UCS02-1](diagrams/sequence/sd-ucs02-1.png)

#### 9.7.2. UCS02-2: Tìm kiếm theo tên món ăn
![Sequence Diagram - UCS02-2](diagrams/sequence/sd-ucs02-2.png)

#### 9.7.3. UCS02-3: Tạo công thức bằng AI
![Sequence Diagram - UCS02-3](diagrams/sequence/sd-ucs02-3.png)

#### 9.7.4. UCS02-4: Lọc và sắp xếp kết quả
![Sequence Diagram - UCS02-4](diagrams/sequence/sd-ucs02-4.png)

#### 9.7.5. UCS02-5: Xem chi tiết công thức
![Sequence Diagram - UCS02-5](diagrams/sequence/sd-ucs02-5.png)

---

### 8.8. Sequence Diagrams - UC3: Quản lý Công thức Cá nhân

#### 9.8.1. UCS03-1: Thêm công thức mới
![Sequence Diagram - UCS03-1](diagrams/sequence/sd-ucs03-1.png)

#### 9.8.2. UCS03-2: Xem danh sách công thức đã tạo
![Sequence Diagram - UCS03-2](diagrams/sequence/sd-ucs03-2.png)

#### 9.8.3. UCS03-3: Chỉnh sửa công thức đã tạo
![Sequence Diagram - UCS03-3](diagrams/sequence/sd-ucs03-3.png)

#### 9.8.4. UCS03-4: Xóa công thức đã tạo
![Sequence Diagram - UCS03-4](diagrams/sequence/sd-ucs03-4.png)

---

### 8.9. Sequence Diagrams - UC4: Tương tác Xã hội

#### 9.9.1. UCS04-1: Thêm công thức vào yêu thích
![Sequence Diagram - UCS04-1](diagrams/sequence/sd-ucs04-1.png)

#### 9.9.2. UCS04-2: Gỡ công thức khỏi yêu thích
![Sequence Diagram - UCS04-2](diagrams/sequence/sd-ucs04-2.png)

#### 9.9.3. UCS04-3: Xem danh sách công thức yêu thích
![Sequence Diagram - UCS04-3](diagrams/sequence/sd-ucs04-3.png)

#### 9.9.4. UCS04-4: Gửi đánh giá và bình luận
![Sequence Diagram - UCS04-4](diagrams/sequence/sd-ucs04-4.png)

#### 9.9.5. UCS04-5: Xem đánh giá và bình luận
![Sequence Diagram - UCS04-5](diagrams/sequence/sd-ucs04-5.png)

#### 9.9.6. UCS04-6: Chia sẻ công thức
![Sequence Diagram - UCS04-6](diagrams/sequence/sd-ucs04-6.png)

---

### 8.10. Sequence Diagrams - UC5: Tủ lạnh ảo

#### 9.10.1. UCS05-1: Thêm nguyên liệu vào tủ
![Sequence Diagram - UCS05-1](diagrams/sequence/sd-ucs05-1.png)

#### 9.10.2. UCS05-2: Xem danh sách nguyên liệu trong tủ
![Sequence Diagram - UCS05-2](diagrams/sequence/sd-ucs05-2.png)

#### 9.10.3. UCS05-3: Cập nhật số lượng nguyên liệu
![Sequence Diagram - UCS05-3](diagrams/sequence/sd-ucs05-3.png)

#### 9.10.4. UCS05-4: Xóa nguyên liệu khỏi tủ
![Sequence Diagram - UCS05-4](diagrams/sequence/sd-ucs05-4.png)

---

### 8.11. Sequence Diagrams - UC6: Kế hoạch Bữa ăn

#### 9.11.1. UCS06-1: Tạo kế hoạch bữa ăn
![Sequence Diagram - UCS06-1](diagrams/sequence/sd-ucs06-1.png)

#### 9.11.2. UCS06-2: Xem kế hoạch bữa ăn
![Sequence Diagram - UCS06-2](diagrams/sequence/sd-ucs06-2.png)

#### 9.11.3. UCS06-3: Chỉnh sửa kế hoạch bữa ăn
![Sequence Diagram - UCS06-3](diagrams/sequence/sd-ucs06-3.png)

#### 9.11.4. UCS06-4: Xóa kế hoạch bữa ăn
![Sequence Diagram - UCS06-4](diagrams/sequence/sd-ucs06-4.png)

#### 9.11.5. UCS06-5: Tạo danh sách mua sắm từ kế hoạch
![Sequence Diagram - UCS06-5](diagrams/sequence/sd-ucs06-5.png)

---

## 9. CLASS DIAGRAMS

### 9.1. Class Diagram - UC-A1: Quản lý Người dùng
![Class Diagram - UC-A1](diagrams/class/cd-uc-a1.png)

*Class diagram mô tả các class liên quan đến quản lý người dùng: User, Admin, UserManagement, AuditLog, etc.*

---

### 9.2. Class Diagram - UC-A2: Quản lý Công thức
![Class Diagram - UC-A2](diagrams/class/cd-uc-a2.png)

*Class diagram mô tả các class liên quan đến quản lý công thức: Recipe, RecipeManagement, ApprovalWorkflow, NotificationService, etc.*

---

### 9.3. Class Diagram - UC-A3: Quản lý Danh mục & Nguyên liệu
![Class Diagram - UC-A3](diagrams/class/cd-uc-a3.png)

*Class diagram mô tả các class liên quan đến quản lý danh mục và nguyên liệu: Category, Ingredient, StandardizedIngredient, IngredientSynonym, etc.*

---

### 9.4. Class Diagram - UC-A4: Quản lý Tài khoản Admin
![Class Diagram - UC-A4](diagrams/class/cd-uc-a4.png)

*Class diagram mô tả các class liên quan đến quản lý tài khoản admin: AdminAccount, Role, Permission, RBACService, etc.*

---

### 9.5. Class Diagram - UC-A5: Báo cáo & Thống kê
![Class Diagram - UC-A5](diagrams/class/cd-uc-a5.png)

*Class diagram mô tả các class liên quan đến báo cáo và thống kê: AnalyticsService, ReportGenerator, ChartRenderer, KPICalculator, etc.*

---

### 9.6. Class Diagram - UC1: Authentication & Profile
![Class Diagram - UC1](diagrams/class/cd-uc1.png)

*Class diagram mô tả các class liên quan đến authentication và profile: User, AuthService, ProfileService, SessionManager, RateLimiter, etc.*

---

### 9.7. Class Diagram - UC2: Tìm kiếm & Khám phá Công thức
![Class Diagram - UC2](diagrams/class/cd-uc2.png)

*Class diagram mô tả các class liên quan đến tìm kiếm và khám phá: SearchService, RecipeSearchEngine, AIService, RecipeDetailService, etc.*

---

### 9.8. Class Diagram - UC3: Quản lý Công thức Cá nhân
![Class Diagram - UC3](diagrams/class/cd-uc3.png)

*Class diagram mô tả các class liên quan đến quản lý công thức cá nhân: PersonalRecipe, RecipeDraft, RecipeStatus, etc.*

---

### 9.9. Class Diagram - UC4: Tương tác Xã hội
![Class Diagram - UC4](diagrams/class/cd-uc4.png)

*Class diagram mô tả các class liên quan đến tương tác xã hội: Favorite, Rating, Comment, ShareService, etc.*

---

### 9.10. Class Diagram - UC5: Tủ lạnh ảo
![Class Diagram - UC5](diagrams/class/cd-uc5.png)

*Class diagram mô tả các class liên quan đến tủ lạnh ảo: VirtualFridge, FridgeIngredient, ExpirationWarningService, UnitConverter, etc.*

---

### 9.11. Class Diagram - UC6: Kế hoạch Bữa ăn
![Class Diagram - UC6](diagrams/class/cd-uc6.png)

*Class diagram mô tả các class liên quan đến kế hoạch bữa ăn: MealPlan, Meal, ShoppingList, ShoppingListGenerator, etc.*

---

## 10. ENTITY RELATIONSHIP DIAGRAMS

### 10.1. ERD - UC-A1: Quản lý Người dùng
![ERD - UC-A1](diagrams/erd/erd-uc-a1.png)

*ERD mô tả các entities và relationships trong module Quản lý Người dùng.*

---

### 10.2. ERD - UC-A2: Quản lý Công thức
![ERD - UC-A2](diagrams/erd/erd-uc-a2.png)

*ERD mô tả các entities và relationships trong module Quản lý Công thức.*

---

### 10.3. ERD - UC-A3: Quản lý Danh mục & Nguyên liệu
![ERD - UC-A3](diagrams/erd/erd-uc-a3.png)

*ERD mô tả các entities và relationships trong module Quản lý Danh mục & Nguyên liệu.*

---

### 10.4. ERD - UC-A4: Quản lý Tài khoản Admin
![ERD - UC-A4](diagrams/erd/erd-uc-a4.png)

*ERD mô tả các entities và relationships trong module Quản lý Tài khoản Admin.*

---

### 10.5. ERD - UC-A5: Báo cáo & Thống kê
![ERD - UC-A5](diagrams/erd/erd-uc-a5.png)

*ERD mô tả các entities và relationships trong module Báo cáo & Thống kê.*

---

### 10.6. ERD - UC1: Authentication & Profile
![ERD - UC1](diagrams/erd/erd-uc1.png)

*ERD mô tả các entities và relationships trong module Authentication & Profile.*

---

### 10.7. ERD - UC2: Tìm kiếm & Khám phá Công thức
![ERD - UC2](diagrams/erd/erd-uc2.png)

*ERD mô tả các entities và relationships trong module Tìm kiếm & Khám phá.*

---

### 10.8. ERD - UC3: Quản lý Công thức Cá nhân
![ERD - UC3](diagrams/erd/erd-uc3.png)

*ERD mô tả các entities và relationships trong module Quản lý Công thức Cá nhân.*

---

### 10.9. ERD - UC4: Tương tác Xã hội
![ERD - UC4](diagrams/erd/erd-uc4.png)

*ERD mô tả các entities và relationships trong module Tương tác Xã hội.*

---

### 10.10. ERD - UC5: Tủ lạnh ảo
![ERD - UC5](diagrams/erd/erd-uc5.png)

*ERD mô tả các entities và relationships trong module Tủ lạnh ảo.*

---

### 10.11. ERD - UC6: Kế hoạch Bữa ăn
![ERD - UC6](diagrams/erd/erd-uc6.png)

*ERD mô tả các entities và relationships trong module Kế hoạch Bữa ăn.*

---

## 11. API SPECIFICATION

### 11.1. Tổng quan API

Hệ thống cung cấp RESTful API được thiết kế theo chuẩn **OpenAPI 3.1.0**, hỗ trợ đầy đủ các chức năng quản lý công thức nấu ăn, tìm kiếm thông minh, tương tác xã hội và quản lý nội dung.

**Authentication**: Hầu hết các endpoints yêu cầu JWT Bearer token được lấy từ endpoint `/auth/login`. Token phải được gửi kèm trong header `Authorization: Bearer {token}`.

**Base URLs**:
- Production: `https://api.recipemanagement.com/v1`
- Staging: `https://staging-api.recipemanagement.com/v1`
- Development: `http://localhost:3000/api/v1`

**OpenAPI Specification**: [`dist/openapi-bundled.yaml`](../../dist/openapi-bundled.yaml)

---

### 11.2. API Endpoints theo Modules

#### 11.2.1. UC-A1: Admin - User Management APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/admin/users` | Xem danh sách người dùng |
| GET | `/admin/users/{userId}` | Xem chi tiết người dùng |
| POST | `/admin/users/{userId}/deactivate` | Vô hiệu hóa tài khoản |
| POST | `/admin/users/{userId}/activate` | Kích hoạt lại tài khoản |
| GET | `/admin/users/{userId}/activities` | Xem hoạt động người dùng |

---

#### 11.2.2. UC-A2: Admin - Recipe Management APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/admin/recipes` | Xem danh sách công thức hệ thống |
| POST | `/admin/recipes/system` | Thêm công thức hệ thống |
| GET | `/admin/recipes/pending` | Xem danh sách công thức chờ duyệt |
| GET | `/admin/recipes/{recipeId}` | Xem chi tiết công thức |
| PUT | `/admin/recipes/{recipeId}` | Sửa công thức hệ thống |
| DELETE | `/admin/recipes/{recipeId}` | Xóa công thức hệ thống |
| POST | `/admin/recipes/{recipeId}/approve` | Phê duyệt công thức |
| POST | `/admin/recipes/{recipeId}/reject` | Từ chối công thức |

---

#### 11.2.3. UC-A3: Admin - Category & Ingredient Management APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/admin/categories` | Xem danh sách danh mục |
| POST | `/admin/categories` | Thêm danh mục mới |
| PUT | `/admin/categories/{categoryId}` | Sửa danh mục |
| DELETE | `/admin/categories/{categoryId}` | Xóa danh mục |
| GET | `/admin/ingredients` | Xem danh sách nguyên liệu |
| POST | `/admin/ingredients` | Thêm nguyên liệu mới |
| PUT | `/admin/ingredients/{ingredientId}` | Sửa nguyên liệu |
| DELETE | `/admin/ingredients/{ingredientId}` | Xóa nguyên liệu |

---

#### 11.2.4. UC-A4: Admin - Account Management APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/admin/accounts` | Xem danh sách tài khoản admin |
| POST | `/admin/accounts` | Tạo tài khoản admin mới |
| GET | `/admin/accounts/{accountId}` | Xem chi tiết tài khoản admin |
| PUT | `/admin/accounts/{accountId}/permissions` | Phân quyền cho admin |
| DELETE | `/admin/accounts/{accountId}` | Xóa tài khoản admin |

---

#### 11.2.5. UC-A5: Admin - Analytics & Reporting APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/admin/analytics` | Xem báo cáo thống kê |

---

#### 11.2.6. UC1: Authentication & Profile APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| POST | `/auth/register` | Đăng ký tài khoản |
| POST | `/auth/login` | Đăng nhập hệ thống |
| POST | `/auth/logout` | Đăng xuất hệ thống |
| POST | `/auth/refresh` | Làm mới token |
| POST | `/auth/forgot-password` | Quên mật khẩu |
| POST | `/auth/reset-password` | Đặt lại mật khẩu |
| POST | `/auth/verify-email` | Xác thực email |
| GET | `/users/profile` | Xem thông tin cá nhân |
| PUT | `/users/profile` | Cập nhật thông tin cá nhân |
| POST | `/users/change-password` | Đổi mật khẩu |
| GET | `/users/sessions` | Xem danh sách sessions |

---

#### 11.2.7. UC2: Recipe Search & Discovery APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/recipes/search` | Tìm kiếm theo tên món ăn |
| GET | `/recipes/search/by-ingredients` | Tìm kiếm theo nguyên liệu |
| POST | `/recipes/ai-suggest` | Tạo công thức bằng AI |
| GET | `/recipes/{recipeId}` | Xem chi tiết công thức |
| GET | `/recipes/popular` | Xem công thức phổ biến |
| GET | `/categories` | Lấy danh sách danh mục |
| GET | `/ingredients/search` | Tìm kiếm nguyên liệu |

---

#### 11.2.8. UC3: Personal Recipe Management APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| POST | `/recipes` | Thêm công thức mới |
| GET | `/users/recipes` | Xem danh sách công thức đã tạo |
| PUT | `/recipes/{recipeId}/edit` | Chỉnh sửa công thức đã tạo |
| DELETE | `/recipes/{recipeId}` | Xóa công thức đã tạo |
| POST | `/recipes/{recipeId}/media` | Upload ảnh cho công thức |

---

#### 11.2.9. UC4: Social Interaction APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| POST | `/recipes/{recipeId}/favorite` | Thêm vào yêu thích |
| DELETE | `/recipes/{recipeId}/favorite` | Gỡ khỏi yêu thích |
| GET | `/users/favorites` | Xem danh sách yêu thích |
| POST | `/recipes/{recipeId}/rating` | Gửi đánh giá |
| GET | `/recipes/{recipeId}/comments` | Xem bình luận |
| POST | `/recipes/{recipeId}/comments` | Thêm bình luận |
| POST | `/comments/{commentId}/helpful` | Đánh dấu hữu ích |
| POST | `/recipes/{recipeId}/share` | Chia sẻ công thức |
| GET | `/share/channels` | Lấy danh sách kênh chia sẻ |

---

#### 11.2.10. UC5: Virtual Fridge & Collections APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/users/inventory` | Xem danh sách nguyên liệu trong tủ |
| POST | `/users/inventory` | Thêm nguyên liệu vào tủ |
| POST | `/users/inventory/bulk` | Thêm hàng loạt nguyên liệu |
| GET | `/users/inventory/expiring` | Xem nguyên liệu sắp hết hạn |
| GET | `/users/inventory/suggestions` | Gợi ý công thức từ tủ lạnh |
| PUT | `/users/inventory/{itemId}` | Cập nhật số lượng nguyên liệu |
| DELETE | `/users/inventory/{itemId}` | Xóa nguyên liệu khỏi tủ |
| GET | `/collections` | Xem danh sách collections |
| POST | `/collections` | Tạo collection mới |
| GET | `/collections/{collectionId}` | Xem chi tiết collection |
| PUT | `/collections/{collectionId}` | Cập nhật collection |
| DELETE | `/collections/{collectionId}` | Xóa collection |
| POST | `/collections/{collectionId}/recipes` | Thêm công thức vào collection |
| DELETE | `/collections/{collectionId}/recipes/{recipeId}` | Xóa công thức khỏi collection |

---

#### 11.2.11. UC6: Meal Planning APIs

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| GET | `/meal-plans` | Xem danh sách kế hoạch bữa ăn |
| POST | `/meal-plans` | Tạo kế hoạch bữa ăn |
| GET | `/meal-plans/weekly` | Xem kế hoạch theo tuần |
| GET | `/meal-plans/{mealPlanId}` | Xem chi tiết kế hoạch |
| PUT | `/meal-plans/{mealPlanId}` | Chỉnh sửa kế hoạch |
| DELETE | `/meal-plans/{mealPlanId}` | Xóa kế hoạch |
| GET | `/meal-plans/{mealPlanId}/shopping-list` | Tạo danh sách mua sắm |
| POST | `/meal-plans/suggest` | Gợi ý kế hoạch bằng AI |

---

## 12. KẾT LUẬN

Hệ thống quản lý công thức nấu ăn với **51 use cases** được phân bổ hợp lý giữa Admin (24 use cases) và User (27 use cases), đáp ứng đầy đủ các yêu cầu chức năng và phi chức năng. Các tính năng nổi bật bao gồm:

✅ **Tích hợp AI** để tạo công thức và gợi ý thực đơn  
✅ **Thuật toán tìm kiếm thông minh** với 3 mức ưu tiên  
✅ **Workflow phê duyệt** công thức rõ ràng và minh bạch  
✅ **Quản lý tồn kho thông minh** với tủ lạnh ảo  
✅ **Tương tác xã hội** đầy đủ với đánh giá, bình luận, chia sẻ  
✅ **Bảo mật cao** với RBAC, audit trail, rate limiting  
✅ **Performance tốt** với caching, pagination, CDN  

Hệ thống sẵn sàng để triển khai và đáp ứng nhu cầu của người dùng hiện đại.
