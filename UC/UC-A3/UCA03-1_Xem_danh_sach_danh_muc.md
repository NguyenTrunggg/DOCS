# UCA03-1: Xem danh sách danh mục [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem tất cả danh mục món ăn hiện có trong hệ thống với khả năng tìm kiếm, lọc và phân trang.

**Trigger:** Admin truy cập mục "Quản lý Danh mục".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Read"

**Post-Condition:**
- Danh sách danh mục hiển thị theo bộ lọc/sắp xếp

**Basic Flow:**
1. Admin mở trang "Quản lý Danh mục"
2. Hệ thống truy vấn và hiển thị bảng danh mục gồm:
   - a. ID danh mục
   - b. Tên danh mục
   - c. Mô tả (nếu có)
   - d. Số công thức trong danh mục
   - e. Ngày tạo/cập nhật
3. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên danh mục
   - b. Sắp xếp theo tên/ngày tạo
   - c. Phân trang (20 mục/trang)
4. Admin có thể mở chi tiết/sửa/xóa danh mục (UCA03-3/UCA03-4)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV/Excel

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Chưa có danh mục nào"

- **Lỗi tải dữ liệu:** Hiển thị lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Tên danh mục là duy nhất

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây
  - Security: Chỉ Admin mới truy cập

**Priority:** Low  
**CRUD:** Read

