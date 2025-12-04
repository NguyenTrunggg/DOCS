# UCA04-4: Xóa tài khoản Admin [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin xóa một tài khoản quản trị khỏi hệ thống, có xác nhận để tránh xóa nhầm.

**Trigger:** Super Admin nhấn "Xóa" tại tài khoản Admin trong danh sách quản trị.

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.Delete`
- Tài khoản mục tiêu tồn tại

**Post-Condition:**
- Tài khoản Admin bị xóa khỏi hệ thống
- Lịch sử/audit ghi nhận sự kiện

**Basic Flow:**
1. Super Admin nhấn "Xóa" tại tài khoản mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận với thông tin tài khoản
3. Super Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa tài khoản khỏi CSDL
5. Hiển thị: "Đã xóa tài khoản Admin"

**Alternative Flow:**
- **Soft Delete:** Đánh dấu vô hiệu hóa thay vì xóa vĩnh viễn

- **Xóa hàng loạt:** Chọn nhiều tài khoản và xóa cùng lúc

**Exception Flow:**
- **Không thể xóa Super Admin cuối cùng:** Hệ thống ngăn chặn, hiển thị cảnh báo

- **Thiếu quyền:** "Bạn không có quyền xóa tài khoản Admin"

- **Lỗi CSDL:** Thông báo lỗi, không xóa

**Additional Information:**
- **Business Rule:**
  - Không được xóa khi tài khoản là Super Admin cuối cùng
  - Mọi thao tác xóa phải được audit

- **Non-Functional Requirement:**
  - Security: Kiểm soát truy cập nghiêm ngặt
  - Reliability: Đảm bảo toàn vẹn dữ liệu và quan hệ

**Priority:** Low  
**CRUD:** Delete

