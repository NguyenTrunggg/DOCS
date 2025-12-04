# UCA03-8: Xóa nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xóa một nguyên liệu khỏi CSDL chuẩn hoá. Nếu nguyên liệu đang được tham chiếu bởi công thức, cần xử lý ràng buộc trước khi xóa.

**Trigger:** Admin nhấn "Xóa" tại một nguyên liệu trong trang "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Delete"
- Nguyên liệu tồn tại

**Post-Condition:**
- Nguyên liệu bị xóa hoặc được đánh dấu xóa/ẩn tuỳ chính sách

**Basic Flow:**
1. Admin nhấn "Xóa" tại nguyên liệu mục tiêu
2. Hệ thống kiểm tra ràng buộc (công thức sử dụng nguyên liệu này)
3. Nếu không có ràng buộc: Hiển thị xác nhận, Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa nguyên liệu khỏi CSDL
5. Hiển thị: "Đã xóa nguyên liệu"

**Alternative Flow:**
- **Có ràng buộc:**
  - Hiển thị danh sách công thức đang sử dụng nguyên liệu
  - Yêu cầu thay thế bằng nguyên liệu khác hoặc huỷ xóa

- **Soft Delete:** Đánh dấu ẩn để không phá vỡ tham chiếu

**Exception Flow:**
- **Không đủ quyền:** Thông báo "Bạn không có quyền xóa nguyên liệu"

- **Lỗi ràng buộc CSDL:** Thông báo lỗi khóa ngoại

- **Nguyên liệu không tồn tại:** Thông báo "Nguyên liệu không tồn tại"

**Additional Information:**
- **Business Rule:**
  - Không được xóa nếu còn tham chiếu, trừ khi đã thay thế/soft delete

- **Non-Functional Requirement:**
  - Security: Audit hành động xóa
  - Reliability: Đảm bảo toàn vẹn dữ liệu

**Priority:** Low  
**CRUD:** Delete

