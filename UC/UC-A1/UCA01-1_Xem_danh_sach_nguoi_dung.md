# UCA01-1: Xem danh sách người dùng [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem danh sách tất cả người dùng trong hệ thống, kèm tìm kiếm, lọc, phân trang để quản trị hiệu quả.

**Trigger:** Admin truy cập mục "Quản lý Người dùng" trong bảng điều khiển quản trị.

**Pre-Condition:**
- Admin đã đăng nhập bằng tài khoản quản trị
- Admin có quyền "User.Read"

**Post-Condition:**
- Danh sách người dùng được hiển thị theo các tiêu chí lọc/sắp xếp
- Admin có thể chuyển sang xem chi tiết một người dùng

**Basic Flow:**
1. Admin mở trang "Quản lý Người dùng"
2. Hệ thống truy vấn danh sách người dùng (theo trang)
3. Hệ thống hiển thị bảng gồm các cột:
   - a. ID
   - b. Ảnh đại diện
   - c. Tên hiển thị
   - d. Email
   - e. Ngày tham gia
   - f. Trạng thái (Hoạt động/Bị khóa)
4. Thanh công cụ gồm:
   - a. Ô tìm kiếm theo tên/email
   - b. Bộ lọc trạng thái (Tất cả/Hoạt động/Bị khóa)
   - c. Bộ lọc thời gian tham gia
   - d. Sắp xếp theo tên/ngày tham gia
5. Hệ thống hiển thị phân trang (mặc định 20 người dùng/trang)
6. Admin nhấp vào hàng người dùng để xem chi tiết (UCA01-2)

**Alternative Flow:**
- **Xuất danh sách:**
  - Admin xuất danh sách ra CSV/Excel

- **Lưu bộ lọc:**
  - Lưu bộ lọc thường dùng cho lần truy cập sau

**Exception Flow:**
- **Không có dữ liệu:**
  - Hiển thị: "Không tìm thấy người dùng nào phù hợp"

- **Lỗi tải dữ liệu:**
  - Hiển thị: "Không thể tải danh sách người dùng. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Chuyển hướng đến trang đăng nhập quản trị

**Additional Information:**
- **Business Rule:**
  - Chỉ tài khoản có quyền thích hợp mới truy cập được trang này
  - Phân trang 20 mục/trang, tối đa tải 1000 mục khi xuất file
  - Tìm kiếm không phân biệt hoa thường, hỗ trợ không dấu

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây cho 10k người dùng (server-side paging)
  - Usability: Bảng có cố định header, cột co giãn
  - Security: Endpoint chỉ dành cho admin; audit truy cập

**Priority:** Low  
**CRUD:** Read

