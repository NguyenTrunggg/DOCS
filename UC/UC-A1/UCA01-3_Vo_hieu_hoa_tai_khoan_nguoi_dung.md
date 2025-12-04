# UCA01-3: Vô hiệu hóa tài khoản người dùng [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin khóa tài khoản của người dùng vi phạm để ngăn họ đăng nhập và sử dụng hệ thống, với quy trình xác nhận và lưu lý do.

**Trigger:** Admin nhấn nút "Khóa" tại trang danh sách người dùng (UCA01-1) hoặc trang chi tiết người dùng (UCA01-2).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "User.Disable"
- Tài khoản người dùng mục tiêu tồn tại và đang ở trạng thái Hoạt động

**Post-Condition:**
- Trạng thái tài khoản chuyển sang "Bị khóa"
- Người dùng không thể đăng nhập vào hệ thống
- Lý do khóa được lưu lại, có thể hiển thị cho người dùng khi đăng nhập thất bại

**Basic Flow:**
1. Admin nhấn "Khóa" tại người dùng mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận với các trường:
   - a. Lý do khóa (bắt buộc, tối đa 255 ký tự)
   - b. Thời hạn khóa (tùy chọn: 15 phút, 1 ngày, vĩnh viễn)
3. Admin nhập lý do và chọn thời hạn
4. Admin nhấn "Xác nhận"
5. Hệ thống cập nhật trạng thái tài khoản thành "Bị khóa" và lưu lý do/thời hạn
6. Hệ thống đăng xuất tất cả session hiện tại của tài khoản
7. Hệ thống ghi log audit (ai khóa, khi nào, lý do)
8. Hiển thị thông báo: "Đã khóa tài khoản thành công"

**Alternative Flow:**
- **Khóa tạm thời theo chính sách:**
  - Hệ thống có preset thời hạn (15m, 1h, 24h)

- **Khóa hàng loạt:**
  - Admin chọn nhiều người dùng và thực hiện khóa cùng lúc
  - Hệ thống lưu lý do đồng nhất hoặc riêng cho từng người dùng

- **Thông báo đến người dùng:**
  - Gửi email thông báo tài khoản bị khóa và lý do

**Exception Flow:**
- **Thiếu quyền:**
  - Hiển thị: "Bạn không có quyền khóa tài khoản"

- **Tài khoản đã bị khóa:**
  - Hiển thị: "Tài khoản đã ở trạng thái bị khóa"

- **Lỗi cập nhật CSDL:**
  - Hiển thị: "Không thể cập nhật trạng thái. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Khóa tài khoản phải ghi nhận lý do rõ ràng
  - Khi hết thời hạn khóa tạm thời, hệ thống tự mở khóa
  - Tài khoản bị khóa không thể thực hiện bất kỳ hành động yêu cầu đăng nhập

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ các hành động khóa/mở khóa
  - Reliability: Đảm bảo đăng xuất toàn bộ session ngay lập tức
  - Performance: Cập nhật trạng thái < 1 giây

**Priority:** Medium  
**CRUD:** Update

