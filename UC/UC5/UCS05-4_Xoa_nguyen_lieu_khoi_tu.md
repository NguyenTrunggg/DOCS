# UCS05-4: Xóa nguyên liệu khỏi tủ [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xóa một nguyên liệu không còn sử dụng hoặc đã hết khỏi "Tủ lạnh ảo", giúp danh sách luôn gọn gàng và chính xác.

**Trigger:** Người dùng nhấn nút "Xóa" tại một nguyên liệu trong danh sách tủ hoặc chọn nhiều nguyên liệu để xóa hàng loạt.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Nguyên liệu tồn tại trong tủ của người dùng

**Post-Condition:**
- Nguyên liệu bị xóa khỏi danh sách tủ
- Lịch sử xóa được ghi lại (audit trail)
- Danh sách hiển thị được cập nhật real-time

**Basic Flow:**
1. Người dùng mở trang "Tủ lạnh ảo" (UC5.2)
2. Người dùng nhấn "Xóa" tại nguyên liệu cần loại bỏ
3. Hệ thống hiển thị hộp thoại xác nhận xóa:
   - a. Tên nguyên liệu
   - b. Số lượng và đơn vị
   - c. Ngày hết hạn (nếu có)
4. Người dùng nhấn "Xác nhận xóa"
5. Hệ thống xóa bản ghi nguyên liệu khỏi CSDL
6. Hệ thống ghi lại lịch sử xóa (thời gian, người thực hiện)
7. Hệ thống cập nhật giao diện danh sách
8. Hiển thị thông báo: "Đã xóa nguyên liệu"

**Alternative Flow:**
- **Xóa hàng loạt:**
  - Người dùng chọn nhiều nguyên liệu và nhấn "Xóa đã chọn"
  - Hệ thống hiển thị danh sách các nguyên liệu sẽ xóa
  - Người dùng xác nhận, hệ thống xóa tất cả

- **Xóa tự động khi hết hạn:**
  - Hệ thống có tùy chọn tự động xóa các mục "Đã hết hạn" sau X ngày
  - Người dùng bật/tắt trong cài đặt

- **Chuyển sang danh sách mua sắm:**
  - Trước khi xóa, hệ thống gợi ý thêm vào "Danh sách mua sắm"
  - Nếu đồng ý, thêm vào danh sách mua sắm và sau đó xóa khỏi tủ

**Exception Flow:**
- **Người dùng hủy xóa:**
  - Người dùng nhấn "Hủy" trong hộp thoại xác nhận
  - Không có thay đổi nào được thực hiện

- **Nguyên liệu không tồn tại:**
  - Nếu nguyên liệu đã bị xóa từ trước
  - Hệ thống hiển thị: "Nguyên liệu không tồn tại"
  - Làm mới danh sách

- **Lỗi xóa cơ sở dữ liệu:**
  - Nếu không thể xóa do lỗi hệ thống
  - Hiển thị: "Không thể xóa. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình xóa
  - Hệ thống chuyển hướng đến trang đăng nhập

**Additional Information:**
- **Business Rule:**
  - Chỉ chủ sở hữu được xóa nguyên liệu trong tủ của mình
  - Có thể bật xác nhận xóa để tránh thao tác nhầm
  - Lưu lịch sử xóa để audit
  - Gợi ý chuyển sang danh sách mua sắm trước khi xóa

- **Non-Functional Requirement:**
  - Performance: Xóa < 500ms
  - Usability: Hộp thoại xác nhận rõ ràng, có chi tiết nguyên liệu
  - Security: Đảm bảo chỉ người dùng hợp lệ mới thao tác xóa
  - Reliability: Xóa an toàn, không để lại bản ghi mồ côi

**Priority:** Low  
**CRUD:** Delete

