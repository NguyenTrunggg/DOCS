# UCA02-4: Xóa công thức hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xóa vĩnh viễn một công thức ra khỏi hệ thống, với xác nhận để tránh xóa nhầm.

**Trigger:** Admin chọn một công thức và nhấn nút "Xóa" trong mục "Quản lý Công thức" (UCA02-1).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Delete"
- Công thức tồn tại trong hệ thống

**Post-Condition:**
- Công thức bị xóa vĩnh viễn khỏi CSDL
- Không còn xuất hiện trong tìm kiếm công khai

**Basic Flow:**
1. Admin nhấn "Xóa" tại công thức mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận với tên công thức
3. Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa công thức khỏi CSDL và file media liên quan
5. Hiển thị thông báo: "Đã xóa công thức"

**Alternative Flow:**
- **Soft Delete:** Đánh dấu xóa và ẩn khỏi tìm kiếm, có thể khôi phục trong 30 ngày

- **Xóa hàng loạt:** Chọn nhiều công thức và xóa cùng lúc

**Exception Flow:**
- **Thiếu quyền:** Hiển thị "Bạn không có quyền xóa công thức"

- **Công thức không tồn tại:** Thông báo "Công thức không tồn tại hoặc đã bị xóa"

- **Lỗi CSDL/Storage:** Thông báo lỗi, đề nghị thử lại

**Additional Information:**
- **Business Rule:**
  - Xóa phải kèm xác nhận; với công thức có tương tác cao, yêu cầu nhập lại tên để xác nhận

- **Non-Functional Requirement:**
  - Security: Audit hành động xóa
  - Reliability: Xóa an toàn, không để dữ liệu mồ côi

**Priority:** Low  
**CRUD:** Delete

