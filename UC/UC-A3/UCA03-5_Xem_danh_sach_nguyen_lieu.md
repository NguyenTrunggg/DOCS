# UCA03-5: Xem danh sách nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem danh sách tất cả nguyên liệu đã được chuẩn hóa trong CSDL, phục vụ cho việc tìm kiếm và thêm công thức.

**Trigger:** Admin truy cập mục "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Read"

**Post-Condition:**
- Danh sách nguyên liệu được hiển thị theo bộ lọc/sắp xếp

**Basic Flow:**
1. Admin mở trang "Quản lý Nguyên liệu"
2. Hệ thống hiển thị bảng nguyên liệu gồm:
   - a. ID
   - b. Tên nguyên liệu (chuẩn hóa, không dấu)
   - c. Đơn vị mặc định
   - d. Danh mục (Rau củ, Thịt, Gia vị,...)
   - e. Ngày tạo/cập nhật
3. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên (có gợi ý)
   - b. Lọc theo danh mục/đơn vị
   - c. Phân trang (50 mục/trang)
4. Admin có thể thêm/sửa/xóa nguyên liệu (UCA03-6/7/8)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV để đồng bộ với hệ thống khác

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Chưa có nguyên liệu nào"

- **Lỗi tải dữ liệu:** Hiển thị lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Tên nguyên liệu phải duy nhất (case-insensitive, bỏ dấu)

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây với 10k nguyên liệu (paging)
  - Security: Chỉ Admin mới truy cập

**Priority:** Low  
**CRUD:** Read

