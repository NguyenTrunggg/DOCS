# UCA03-3: Sửa tên danh mục [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin chỉnh sửa tên hoặc mô tả của một danh mục món ăn hiện có.

**Trigger:** Admin chọn một danh mục và nhấn "Sửa".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Update"
- Danh mục tồn tại

**Post-Condition:**
- Tên/mô tả danh mục được cập nhật
- Thời gian cập nhật được ghi nhận

**Basic Flow:**
1. Admin nhấn "Sửa" tại danh mục mục tiêu
2. Hệ thống hiển thị form chỉnh sửa với thông tin hiện tại
3. Admin cập nhật tên/mô tả
4. Admin nhấn "Lưu"
5. Hệ thống validate (tên không trùng)
6. Hệ thống cập nhật CSDL
7. Hiển thị: "Cập nhật danh mục thành công"

**Alternative Flow:**
- **Đổi slug tự động:** Cập nhật slug theo tên mới (nếu có)

**Exception Flow:**
- **Tên trùng:** Hiển thị "Tên danh mục đã tồn tại"

- **Lỗi CSDL:** Thông báo lỗi và không cập nhật

**Additional Information:**
- **Business Rule:**
  - Tên danh mục duy nhất

- **Non-Functional Requirement:**
  - Performance: Cập nhật < 1 giây

**Priority:** Low  
**CRUD:** Update

