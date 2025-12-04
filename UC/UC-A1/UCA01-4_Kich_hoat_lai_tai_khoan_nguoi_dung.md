# UCA01-4: Kích hoạt lại tài khoản người dùng [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin mở khóa (kích hoạt lại) tài khoản người dùng đã bị khóa trước đó để họ có thể đăng nhập và sử dụng hệ thống bình thường.

**Trigger:** Admin nhấn nút "Mở khóa" tại trang danh sách người dùng (UCA01-1) hoặc trang chi tiết người dùng (UCA01-2).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "User.Enable"
- Tài khoản người dùng mục tiêu tồn tại và đang ở trạng thái "Bị khóa"

**Post-Condition:**
- Trạng thái tài khoản chuyển sang "Hoạt động"
- Người dùng có thể đăng nhập lại bình thường
- Lý do mở khóa được ghi log (nếu có)

**Basic Flow:**
1. Admin nhấn "Mở khóa" tại tài khoản mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận mở khóa
3. Admin có thể nhập ghi chú (tùy chọn)
4. Admin nhấn "Xác nhận"
5. Hệ thống cập nhật trạng thái tài khoản sang "Hoạt động"
6. Hệ thống ghi log audit (ai mở, khi nào, ghi chú)
7. Hiển thị thông báo: "Đã mở khóa tài khoản thành công"

**Alternative Flow:**
- **Tự động mở khóa khi hết hạn:**
  - Nếu tài khoản bị khóa tạm thời, hệ thống tự động mở khóa khi đến hạn

- **Mở khóa hàng loạt:**
  - Admin chọn nhiều tài khoản để mở khóa cùng lúc

- **Thông báo đến người dùng:**
  - Gửi email thông báo đã mở khóa tài khoản

**Exception Flow:**
- **Thiếu quyền:**
  - Hiển thị: "Bạn không có quyền mở khóa tài khoản"

- **Tài khoản đang hoạt động:**
  - Hiển thị: "Tài khoản đang ở trạng thái hoạt động"

- **Lỗi cập nhật CSDL:**
  - Hiển thị: "Không thể cập nhật trạng thái. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Mọi thao tác mở khóa phải được audit
  - Nếu tài khoản bị khóa do vi phạm nghiêm trọng, chỉ Super Admin mới được mở khóa

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ, phân quyền rõ ràng
  - Performance: Cập nhật trạng thái < 1 giây
  - Reliability: Trạng thái đồng bộ trên tất cả phiên

**Priority:** Medium  
**CRUD:** Update

