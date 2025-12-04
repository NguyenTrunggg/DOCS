# UCA04-2: Tạo tài khoản Admin mới [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin tạo một tài khoản quản trị mới để phân quyền quản lý hệ thống.

**Trigger:** Super Admin nhấn "Tạo tài khoản Admin" trong mục "Quản lý Tài khoản Admin".

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.Create`

**Post-Condition:**
- Tài khoản Admin mới được tạo và gửi email kích hoạt

**Basic Flow:**
1. Super Admin nhấn "Tạo tài khoản Admin"
2. Hệ thống hiển thị form:
   - a. Họ tên hiển thị
   - b. Email (bắt buộc, duy nhất)
   - c. Vai trò (Role) mặc định hoặc chọn từ danh sách
3. Super Admin nhập thông tin và nhấn "Tạo"
4. Hệ thống validate (email hợp lệ, chưa tồn tại)
5. Hệ thống tạo tài khoản ở trạng thái "Chờ kích hoạt" và gửi email kích hoạt
6. Hiển thị: "Đã tạo tài khoản Admin và gửi email kích hoạt"

**Alternative Flow:**
- **Gán role ngay khi tạo:** Chọn role cụ thể khi tạo mới

- **Nhập hàng loạt:** Import danh sách admin từ CSV, gửi email hàng loạt

**Exception Flow:**
- **Email trùng:** Hiển thị "Email đã tồn tại"

- **Lỗi gửi email:** Tạo tài khoản thành công nhưng hiển thị cảnh báo không gửi được email, cho phép gửi lại

- **Lỗi CSDL:** Thông báo lỗi và không tạo mới

**Additional Information:**
- **Business Rule:**
  - Email là danh tính đăng nhập duy nhất
  - Tài khoản mới phải kích hoạt qua email trước khi sử dụng

- **Non-Functional Requirement:**
  - Security: Chỉ Super Admin được phép tạo
  - Reliability: Hỗ trợ gửi lại email kích hoạt

**Priority:** Medium  
**CRUD:** Create

