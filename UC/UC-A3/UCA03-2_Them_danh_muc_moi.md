# UCA03-2: Thêm danh mục mới [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin tạo một danh mục món ăn mới để phân loại công thức.

**Trigger:** Admin nhấn nút "Thêm mới" trong trang "Quản lý Danh mục".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Create"

**Post-Condition:**
- Danh mục mới được lưu và xuất hiện trong danh sách

**Basic Flow:**
1. Admin nhấn "Thêm mới"
2. Hệ thống hiển thị form nhập liệu:
   - a. Tên danh mục (bắt buộc)
   - b. Mô tả (tùy chọn)
3. Admin nhập thông tin và nhấn "Lưu"
4. Hệ thống validate dữ liệu (tên không trùng, độ dài hợp lệ)
5. Hệ thống lưu danh mục và hiển thị thông báo "Đã thêm danh mục mới"

**Alternative Flow:**
- **Nhập hàng loạt:** Import nhiều danh mục từ file CSV

**Exception Flow:**
- **Tên trùng:** Hiển thị "Tên danh mục đã tồn tại"

- **Lỗi lưu CSDL:** Hiển thị lỗi, không tạo danh mục

**Additional Information:**
- **Business Rule:**
  - Tên danh mục duy nhất, không phân biệt hoa thường

- **Non-Functional Requirement:**
  - Performance: Lưu < 1 giây
  - Security: Chỉ Admin được phép

**Priority:** Low  
**CRUD:** Create

