# UCS06-4: Xóa kế hoạch bữa ăn [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xóa một kế hoạch bữa ăn đã tạo khi không còn sử dụng, giúp quản lý danh sách kế hoạch gọn gàng.

**Trigger:** Người dùng nhấn nút "Xóa" tại một kế hoạch trong danh sách kế hoạch hoặc khi đang xem chi tiết kế hoạch.

**Pre-Condition:**
- Người dùng đã đăng nhập
- Kế hoạch tồn tại và thuộc về người dùng

**Post-Condition:**
- Kế hoạch bị xóa khỏi hệ thống
- Danh sách kế hoạch được cập nhật

**Basic Flow:**
1. Người dùng mở danh sách kế hoạch bữa ăn
2. Nhấn biểu tượng "Xóa" tại kế hoạch cần xóa
3. Hệ thống hiển thị hộp thoại xác nhận xóa với tên kế hoạch và khoảng thời gian
4. Người dùng nhấn "Xác nhận xóa"
5. Hệ thống xóa kế hoạch khỏi CSDL
6. Hiển thị thông báo: "Đã xóa kế hoạch"

**Alternative Flow:**
- **Xóa hàng loạt:**
  - Chọn nhiều kế hoạch và xóa cùng lúc

- **Lưu trữ (Archive) thay vì xóa:**
  - Đưa kế hoạch vào danh sách lưu trữ để tham khảo sau

**Exception Flow:**
- **Người dùng hủy xóa:**
  - Nhấn "Hủy" trong hộp thoại xác nhận

- **Kế hoạch không tồn tại:**
  - Hiển thị: "Kế hoạch không tồn tại hoặc đã bị xóa"

- **Lỗi xóa cơ sở dữ liệu:**
  - Hiển thị: "Không thể xóa. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Chỉ chủ sở hữu được xóa kế hoạch của mình
  - Có thể yêu cầu nhập lại tên kế hoạch nếu cần xác nhận đặc biệt

- **Non-Functional Requirement:**
  - Performance: Xóa < 1 giây
  - Usability: Hộp thoại xác nhận rõ ràng
  - Security: Đảm bảo chỉ người có quyền mới thao tác xóa

**Priority:** Low  
**CRUD:** Delete

