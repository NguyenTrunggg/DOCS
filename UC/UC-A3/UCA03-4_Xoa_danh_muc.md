# UCA03-4: Xóa danh mục [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xóa một danh mục khỏi hệ thống. Nếu danh mục đang được sử dụng bởi các công thức, hệ thống cảnh báo và yêu cầu chuyển các công thức đó sang danh mục khác trước khi xóa.

**Trigger:** Admin nhấn "Xóa" tại một danh mục trong trang "Quản lý Danh mục".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Delete"
- Danh mục tồn tại

**Post-Condition:**
- Danh mục bị xóa khỏi hệ thống (nếu không còn được tham chiếu)
- Danh sách danh mục được cập nhật

**Basic Flow:**
1. Admin nhấn "Xóa" tại danh mục mục tiêu
2. Hệ thống kiểm tra ràng buộc tham chiếu (công thức đang gắn danh mục)
3. Nếu không có tham chiếu: Hiển thị xác nhận, Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa danh mục khỏi CSDL
5. Hiển thị thông báo: "Đã xóa danh mục"

**Alternative Flow:**
- **Có công thức đang tham chiếu:**
  - Hệ thống hiển thị danh sách công thức đang dùng danh mục này
  - Yêu cầu chọn danh mục thay thế
  - Sau khi xác nhận chuyển, thực hiện xóa danh mục cũ

- **Soft Delete:** Đánh dấu xóa để ẩn, có thể khôi phục

**Exception Flow:**
- **Không đủ quyền:** Thông báo "Bạn không có quyền xóa danh mục"

- **Lỗi ràng buộc CSDL:** Thông báo lỗi do ràng buộc khóa ngoại

- **Danh mục không tồn tại:** Thông báo "Danh mục không tồn tại"

**Additional Information:**
- **Business Rule:**
  - Không được xóa danh mục nếu còn công thức tham chiếu, trừ khi đã chuyển

- **Non-Functional Requirement:**
  - Security: Audit hành động xóa/chuyển danh mục
  - Reliability: Đảm bảo tính toàn vẹn dữ liệu khi chuyển

**Priority:** Low  
**CRUD:** Delete

