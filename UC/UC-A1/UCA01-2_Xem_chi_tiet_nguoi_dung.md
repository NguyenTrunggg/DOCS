# UCA01-2: Xem chi tiết người dùng [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem thông tin chi tiết của một người dùng, bao gồm hồ sơ tài khoản, thống kê hoạt động và lịch sử gần đây.

**Trigger:** Admin nhấp vào một người dùng trong danh sách (UCA01-1) hoặc dùng chức năng tìm kiếm để mở trang chi tiết.

**Pre-Condition:**
- Admin đã đăng nhập bằng tài khoản quản trị
- Admin có quyền "User.Read"
- Người dùng mục tiêu tồn tại trong hệ thống

**Post-Condition:**
- Thông tin chi tiết người dùng được hiển thị đầy đủ
- Admin có thể chuyển sang các thao tác quản trị khác (khóa/mở khóa)

**Basic Flow:**
1. Admin từ trang danh sách người dùng (UCA01-1) chọn một người dùng
2. Hệ thống truy vấn dữ liệu chi tiết của người dùng gồm:
   - a. Thông tin tài khoản: Ảnh đại diện, Tên hiển thị, Email, ID, Ngày đăng ký, Lần đăng nhập cuối
   - b. Trạng thái tài khoản: Hoạt động/Bị khóa, Lý do khóa (nếu có)
   - c. Thống kê hoạt động: Tổng số công thức đã đăng, đã duyệt, bị từ chối, tổng số bình luận
   - d. Lịch sử hoạt động gần đây: Danh sách công thức đã đăng gần nhất (link đến chi tiết)
3. Hệ thống hiển thị trang chi tiết với bố cục rõ ràng
4. Admin có thể nhấn nút "Khóa/Mở khóa" tài khoản (chuyển sang UCA01-3/UCA01-4)
5. Admin có thể nhấn vào từng mục lịch sử để mở trang chi tiết liên quan

**Alternative Flow:**
- **Mở trực tiếp bằng ID:**
  - Admin nhập ID người dùng vào ô tìm kiếm nhanh
  - Hệ thống mở thẳng trang chi tiết

- **Xuất báo cáo người dùng:**
  - Admin xuất thông tin người dùng ra PDF để lưu trữ

**Exception Flow:**
- **Người dùng không tồn tại:**
  - Hiển thị: "Người dùng không tồn tại hoặc đã bị xóa"

- **Thiếu quyền:**
  - Hiển thị: "Bạn không có quyền xem chi tiết người dùng"

- **Lỗi tải dữ liệu:**
  - Hiển thị: "Không thể tải thông tin người dùng. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Dữ liệu hiển thị phải tuân thủ bảo vệ thông tin cá nhân (PII)
  - Chỉ Admin/Super Admin mới xem được thống kê đầy đủ

- **Non-Functional Requirement:**
  - Performance: Tải trang chi tiết < 2 giây
  - Usability: Bố cục rõ ràng, có breadcrumb quay lại danh sách
  - Security: Audit khi truy cập trang chi tiết

**Priority:** Low  
**CRUD:** Read

