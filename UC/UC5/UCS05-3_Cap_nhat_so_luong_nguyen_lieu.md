# UCS05-3: Cập nhật số lượng nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng thay đổi số lượng hoặc đơn vị của một nguyên liệu đã có trong "Tủ lạnh ảo", giúp quản lý tồn kho chính xác theo thời gian.

**Trigger:** Người dùng nhấn nút "Sửa" hoặc biểu tượng chỉnh sửa trên một nguyên liệu trong danh sách tủ.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Nguyên liệu cần cập nhật tồn tại trong tủ của người dùng

**Post-Condition:**
- Số lượng/đơn vị/ngày hết hạn/ghi chú được cập nhật
- Lịch sử thay đổi được ghi lại (audit trail)
- Danh sách hiển thị được cập nhật real-time

**Basic Flow:**
1. Người dùng mở trang "Tủ lạnh ảo" (UC5.2)
2. Người dùng nhấn "Sửa" tại nguyên liệu cần cập nhật
3. Hệ thống hiển thị form chỉnh sửa với các trường:
   - a. Số lượng (>= 0)
   - b. Đơn vị (gram, kg, ml, lít, trái, bó...)
   - c. Ngày hết hạn (tùy chọn)
   - d. Ghi chú (tùy chọn)
4. Người dùng thay đổi thông tin và nhấn "Lưu"
5. Hệ thống validate dữ liệu (đảm bảo số lượng hợp lệ, đơn vị hợp lệ)
6. Hệ thống cập nhật bản ghi nguyên liệu trong CSDL
7. Hệ thống ghi lại lịch sử thay đổi (trước và sau)
8. Hệ thống cập nhật giao diện danh sách
9. Hiển thị thông báo: "Cập nhật thành công"

**Alternative Flow:**
- **Cộng trừ nhanh:**
  - Người dùng dùng nút + / - để thay đổi nhanh số lượng
  - Hệ thống cập nhật ngay (inline update)

- **Quy đổi đơn vị:**
  - Khi người dùng đổi đơn vị (vd: g -> kg)
  - Hệ thống tự động quy đổi theo tỉ lệ chuẩn (1000g = 1kg)

- **Cập nhật hàng loạt:**
  - Người dùng chọn nhiều nguyên liệu và cập nhật cùng lúc (vd: thêm 10%)
  - Hệ thống hiển thị preview trước khi xác nhận

**Exception Flow:**
- **Nguyên liệu không tồn tại:**
  - Nếu nguyên liệu đã bị xóa/không còn
  - Hiển thị: "Nguyên liệu không tồn tại"

- **Giá trị không hợp lệ:**
  - Nếu số lượng âm hoặc không phải số
  - Hiển thị lỗi và yêu cầu nhập lại

- **Xung đột cập nhật:**
  - Nếu nguyên liệu được cập nhật từ thiết bị khác cùng lúc
  - Hệ thống hiển thị cảnh báo xung đột và yêu cầu làm mới dữ liệu

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể cập nhật do lỗi hệ thống
  - Hiển thị: "Không thể cập nhật. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Số lượng phải >= 0, nếu = 0 có thể gợi ý xóa (UC5.4)
  - Đơn vị phải trong danh mục cho phép, có quy đổi chuẩn
  - Lưu lại lịch sử thay đổi để audit
  - Cảnh báo khi ngày hết hạn sắp tới (< 3 ngày)

- **Non-Functional Requirement:**
  - Performance: Cập nhật < 500ms
  - Usability: Hỗ trợ thao tác nhanh +/-, keyboard friendly
  - Security: Chỉ chủ tủ được cập nhật nguyên liệu của mình
  - Reliability: Xử lý xung đột cập nhật an toàn

**Priority:** Low  
**CRUD:** Update

