# UCA02-1: Xem danh sách công thức hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem, tìm kiếm và lọc danh sách tất cả công thức có trên hệ thống (bao gồm công thức hệ thống và công thức người dùng đã được duyệt), có phân trang và sắp xếp.

**Trigger:** Admin truy cập mục "Quản lý Công thức" trong bảng điều khiển quản trị.

**Pre-Condition:**
- Admin đã đăng nhập bằng tài khoản quản trị
- Admin có quyền "Recipe.Read"

**Post-Condition:**
- Danh sách công thức được hiển thị theo tiêu chí lọc/sắp xếp
- Admin có thể chuyển sang xem/sửa/xóa công thức

**Basic Flow:**
1. Admin mở trang "Quản lý Công thức"
2. Hệ thống truy vấn danh sách công thức (server-side paging)
3. Hệ thống hiển thị bảng gồm các cột:
   - a. ID
   - b. Tên công thức
   - c. Người tạo (Hệ thống/User: Tên)
   - d. Trạng thái (Đã duyệt/Chờ duyệt/Bị từ chối)
   - e. Ngày tạo
   - f. Điểm đánh giá trung bình
4. Thanh công cụ gồm:
   - a. Ô tìm kiếm theo tên
   - b. Bộ lọc theo trạng thái
   - c. Bộ lọc theo người tạo (Hệ thống/User)
   - d. Sắp xếp theo ngày tạo/điểm đánh giá/tên
5. Hệ thống hiển thị phân trang (20 công thức/trang)
6. Admin nhấp vào một công thức để xem chi tiết/sửa/xóa (UCA02-3/UCA02-4)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV/Excel toàn bộ công thức theo bộ lọc hiện tại

- **Lưu bộ lọc thường dùng:** Lưu preset lọc/sắp xếp

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Không tìm thấy công thức nào phù hợp"

- **Lỗi tải dữ liệu:** Hiển thị "Không thể tải danh sách. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Chỉ Admin/Super Admin có thể truy cập
  - Phân trang 20 mục/trang, xuất tối đa 5000 dòng
  - Tìm kiếm không dấu, không phân biệt hoa thường

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây với 100k công thức (paging)
  - Usability: Bảng có cố định header, cột tuỳ biến
  - Security: Audit truy cập quản trị

**Priority:** Low  
**CRUD:** Read

