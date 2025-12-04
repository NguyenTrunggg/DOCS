# UCS01-6: Đổi mật khẩu [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập thay đổi mật khẩu hiện tại của tài khoản bằng cách xác thực mật khẩu cũ và nhập mật khẩu mới đảm bảo bảo mật.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Đổi mật khẩu" trên trang thông tin cá nhân hoặc trang cài đặt bảo mật.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng biết mật khẩu hiện tại của tài khoản
- Tài khoản người dùng không bị khóa

**Post-Condition:**
- Mật khẩu mới được lưu vào cơ sở dữ liệu (đã được mã hóa)
- Tất cả session hiện tại của người dùng bị vô hiệu hóa
- Người dùng cần đăng nhập lại với mật khẩu mới
- Thời gian thay đổi mật khẩu được ghi lại trong log bảo mật

**Basic Flow:**
1. Người dùng đã đăng nhập truy cập trang thông tin cá nhân hoặc trang "Cài đặt bảo mật"
2. Người dùng nhấn nút "Đổi mật khẩu" hoặc chọn tab "Bảo mật"
3. Hệ thống hiển thị form đổi mật khẩu với các trường:
   - a. Mật khẩu hiện tại (password field)
   - b. Mật khẩu mới (password field)
   - c. Xác nhận mật khẩu mới (password field)
4. Người dùng nhập thông tin:
   - a. Nhập mật khẩu hiện tại
   - b. Nhập mật khẩu mới (tối thiểu 8 ký tự, bao gồm chữ hoa, chữ thường, số)
   - c. Nhập lại mật khẩu mới để xác nhận
5. Người dùng nhấn nút "Đổi mật khẩu"
6. Hệ thống xác thực mật khẩu hiện tại
7. Hệ thống kiểm tra tính hợp lệ của mật khẩu mới
8. Hệ thống kiểm tra mật khẩu mới và xác nhận có khớp nhau
9. Hệ thống mã hóa mật khẩu mới bằng thuật toán bcrypt
10. Hệ thống cập nhật mật khẩu mới vào cơ sở dữ liệu
11. Hệ thống vô hiệu hóa tất cả session hiện tại của người dùng
12. Hệ thống ghi lại thời gian thay đổi mật khẩu vào log bảo mật
13. Hệ thống hiển thị thông báo: "Đổi mật khẩu thành công! Vui lòng đăng nhập lại."
14. Hệ thống tự động chuyển hướng đến trang đăng nhập

**Alternative Flow:**
- **Truy cập từ email thông báo bảo mật:**
  - Người dùng nhận email cảnh báo bảo mật và click link "Đổi mật khẩu"
  - Hệ thống chuyển hướng trực tiếp đến form đổi mật khẩu
  - Tiếp tục luồng từ bước 3 của Basic Flow

- **Đổi mật khẩu từ trang quên mật khẩu:**
  - Nếu người dùng đang trong quá trình reset mật khẩu
  - Form sẽ không yêu cầu mật khẩu hiện tại
  - Tiếp tục luồng từ bước 6 của Basic Flow (bỏ qua xác thực mật khẩu cũ)

**Exception Flow:**
- **Mật khẩu hiện tại không chính xác:**
  - Nếu mật khẩu hiện tại không khớp với mật khẩu trong cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Mật khẩu hiện tại không chính xác. Vui lòng kiểm tra lại."
  - Người dùng có thể nhập lại mật khẩu hiện tại

- **Mật khẩu mới không đủ mạnh:**
  - Nếu mật khẩu mới không đáp ứng yêu cầu bảo mật
  - Hệ thống hiển thị thông báo: "Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ hoa, chữ thường và số."
  - Người dùng có thể chỉnh sửa mật khẩu mới

- **Mật khẩu xác nhận không khớp:**
  - Nếu mật khẩu mới và xác nhận không giống nhau
  - Hệ thống hiển thị thông báo: "Mật khẩu xác nhận không khớp. Vui lòng nhập lại."
  - Người dùng có thể chỉnh sửa trường xác nhận

- **Mật khẩu mới giống mật khẩu cũ:**
  - Nếu mật khẩu mới trùng với mật khẩu hiện tại
  - Hệ thống hiển thị thông báo: "Mật khẩu mới phải khác với mật khẩu hiện tại."
  - Người dùng cần chọn mật khẩu khác

- **Lỗi cập nhật cơ sở dữ liệu:**
  - Nếu không thể cập nhật mật khẩu mới vào cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Không thể cập nhật mật khẩu. Vui lòng thử lại sau."
  - Mật khẩu cũ vẫn được giữ nguyên

- **Session hết hạn trong quá trình đổi mật khẩu:**
  - Nếu session hết hạn trước khi hoàn thành việc đổi mật khẩu
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

**Additional Information:**
- **Business Rule:**
  - Mật khẩu mới phải có ít nhất 8 ký tự
  - Mật khẩu mới phải bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số
  - Mật khẩu mới không được trùng với mật khẩu hiện tại
  - Mật khẩu mới và xác nhận phải khớp nhau chính xác
  - Sau khi đổi mật khẩu, tất cả session hiện tại bị vô hiệu hóa
  - Mật khẩu phải được mã hóa bằng bcrypt với salt ngẫu nhiên
  - Mỗi lần đổi mật khẩu phải ghi lại trong log bảo mật

- **Non-Functional Requirement:**
  - Performance: Thời gian đổi mật khẩu phải dưới 3 giây
  - Usability: Form phải có gợi ý rõ ràng về yêu cầu mật khẩu
  - Security: Mật khẩu phải được mã hóa mạnh, không lưu plain text
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn khi cập nhật mật khẩu

**Priority:** Medium  
**CRUD:** Update

