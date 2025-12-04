# UCA04-1: Xem danh sách tài khoản Admin [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin xem danh sách các tài khoản quản trị (Admin) khác, với khả năng tìm kiếm, lọc và phân trang.

**Trigger:** Super Admin truy cập mục "Quản lý Tài khoản Admin".

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.Read`

**Post-Condition:**
- Danh sách tài khoản Admin hiển thị theo bộ lọc/sắp xếp

**Basic Flow:**
1. Super Admin mở trang "Quản lý Tài khoản Admin"
2. Hệ thống truy vấn danh sách tài khoản Admin
3. Bảng hiển thị các cột:
   - a. ID
   - b. Ảnh đại diện
   - c. Tên hiển thị
   - d. Email
   - e. Vai trò (Role)
   - f. Ngày tạo
   - g. Trạng thái (Hoạt động/Bị khóa)
4. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên/email
   - b. Lọc theo vai trò/trạng thái
   - c. Phân trang (20 mục/trang)
5. Super Admin có thể mở chi tiết/phan quyền/xóa (UCA04-3/UCA04-4)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV/Excel

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Chưa có tài khoản Admin nào"

- **Lỗi tải dữ liệu:** Thông báo lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Chỉ Super Admin được truy cập màn hình này

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây
  - Security: Audit truy cập

**Priority:** Low  
**CRUD:** Read

