# UCA04-3: Phân quyền cho tài khoản Admin [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin gán hoặc thay đổi quyền hạn (roles/permissions) cho một tài khoản Admin, đảm bảo nguyên tắc phân quyền tối thiểu (least privilege).

**Trigger:** Super Admin mở chi tiết một tài khoản Admin và nhấn "Phân quyền".

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.ManageRoles`
- Tài khoản Admin mục tiêu tồn tại

**Post-Condition:**
- Quyền hạn/Vai trò của tài khoản Admin được cập nhật
- Hệ thống ghi log thay đổi quyền

**Basic Flow:**
1. Super Admin mở màn hình phân quyền của tài khoản Admin
2. Hệ thống hiển thị danh sách vai trò (Roles) và quyền (Permissions) khả dụng
3. Super Admin chọn/bỏ chọn các vai trò hoặc quyền cụ thể
4. Super Admin nhấn "Lưu phân quyền"
5. Hệ thống xác thực thay đổi (không tự hạ quyền của chính Super Admin nếu là tài khoản duy nhất)
6. Hệ thống cập nhật vai trò/quyền trong CSDL
7. Hệ thống ghi log audit thay đổi quyền (ai, khi nào, trước/sau)
8. Hiển thị: "Cập nhật phân quyền thành công"

**Alternative Flow:**
- **Gán quyền tạm thời:** Thiết lập thời hạn cho quyền tạm thời

- **Áp dụng từ mẫu (Role Template):** Chọn preset role để áp nhanh

- **Xem thử quyền hiệu lực (Effective Permissions):** Xem tổng hợp quyền sau khi áp

**Exception Flow:**
- **Thiếu quyền:** "Bạn không có quyền phân quyền cho Admin"

- **Xung đột vai trò:** Nếu vai trò mâu thuẫn, hệ thống cảnh báo và yêu cầu điều chỉnh

- **Không thể tự hạ quyền duy nhất:** Ngăn việc xoá role Super Admin cuối cùng

**Additional Information:**
- **Business Rule:**
  - Tuân thủ nguyên tắc least privilege
  - Mọi thay đổi quyền phải có audit
  - Không cho phép tình trạng hệ thống không còn Super Admin

- **Non-Functional Requirement:**
  - Security: RBAC/ABAC rõ ràng, audit đầy đủ
  - Reliability: Áp quyền có hiệu lực ngay lập tức

**Priority:** High  
**CRUD:** Update

