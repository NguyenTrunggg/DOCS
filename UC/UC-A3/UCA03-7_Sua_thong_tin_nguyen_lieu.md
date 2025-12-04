# UCA03-7: Sửa thông tin nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin chỉnh sửa tên, đơn vị và thông tin liên quan của một nguyên liệu đã có trong CSDL chuẩn hoá.

**Trigger:** Admin chọn một nguyên liệu và nhấn "Sửa" trong trang "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Update"
- Nguyên liệu tồn tại

**Post-Condition:**
- Thông tin nguyên liệu được cập nhật
- Thời gian cập nhật được ghi nhận

**Basic Flow:**
1. Admin nhấn "Sửa" tại nguyên liệu mục tiêu
2. Hệ thống hiển thị form với dữ liệu hiện tại
3. Admin chỉnh sửa các trường:
   - a. Tên nguyên liệu (chuẩn hoá, không dấu)
   - b. Đơn vị mặc định
   - c. Danh mục
   - d. Alias (đồng nghĩa)
4. Admin nhấn "Lưu"
5. Hệ thống validate (tên duy nhất)
6. Hệ thống cập nhật CSDL
7. Hiển thị: "Cập nhật nguyên liệu thành công"

**Alternative Flow:**
- **Đổi tên có alias:** Tự động thêm tên cũ vào alias

**Exception Flow:**
- **Tên trùng:** Hiển thị "Nguyên liệu đã tồn tại"

- **Lỗi CSDL:** Thông báo lỗi

**Additional Information:**
- **Business Rule:**
  - Tên duy nhất (case-insensitive, không dấu)

- **Non-Functional Requirement:**
  - Performance: Cập nhật < 1 giây

**Priority:** Low  
**CRUD:** Update

