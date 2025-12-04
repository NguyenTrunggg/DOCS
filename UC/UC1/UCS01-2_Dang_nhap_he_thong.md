# UCS01-2: Đăng nhập vào hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng sử dụng thông tin tài khoản đã đăng ký (email/số điện thoại và mật khẩu) để truy cập vào hệ thống quản lý công thức nấu ăn.

**Trigger:** Người dùng truy cập trang đăng nhập và nhấn nút "Đăng nhập".

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Người dùng đã có tài khoản hợp lệ trong hệ thống
- Tài khoản không bị khóa hoặc vô hiệu hóa
- Tài khoản đã được xác thực (nếu đăng ký bằng email/SMS)

**Post-Condition:**
- Người dùng được xác thực thành công
- Session được tạo và lưu trữ
- Người dùng được chuyển hướng đến trang chủ hoặc trang được yêu cầu trước đó
- Thời gian đăng nhập cuối được cập nhật trong cơ sở dữ liệu

**Basic Flow:**
1. Người dùng truy cập trang đăng nhập từ trang chủ hoặc được chuyển hướng từ trang cần xác thực
2. Người dùng nhập thông tin đăng nhập:
   - a. Email hoặc số điện thoại
   - b. Mật khẩu
3. Người dùng có thể chọn "Ghi nhớ đăng nhập" (tùy chọn)
4. Người dùng nhấn nút "Đăng nhập"
5. Hệ thống kiểm tra và xác thực thông tin đăng nhập
6. Hệ thống kiểm tra trạng thái tài khoản (không bị khóa, đã xác thực)
7. Hệ thống tạo session mới và lưu vào cơ sở dữ liệu
8. Hệ thống cập nhật thời gian đăng nhập cuối của người dùng
9. Nếu có "Ghi nhớ đăng nhập": Hệ thống tạo cookie với thời hạn dài hơn
10. Hệ thống chuyển hướng người dùng đến trang chủ hoặc trang được yêu cầu trước đó

**Alternative Flow:**
- **Đăng nhập bằng Google/Facebook:**
  - Người dùng chọn "Đăng nhập bằng Google" hoặc "Đăng nhập bằng Facebook"
  - Hệ thống chuyển hướng đến trang xác thực của bên thứ ba
  - Sau khi xác thực thành công, hệ thống kiểm tra email trong cơ sở dữ liệu
  - Nếu tài khoản tồn tại: Tạo session và đăng nhập tự động
  - Nếu tài khoản không tồn tại: Chuyển hướng đến trang đăng ký với thông tin từ bên thứ ba

- **Quên mật khẩu:**
  - Người dùng nhấn liên kết "Quên mật khẩu?"
  - Hệ thống chuyển hướng đến trang đặt lại mật khẩu (UC1.7)

**Exception Flow:**
- **Thông tin đăng nhập không chính xác:**
  - Hệ thống hiển thị thông báo: "Email/số điện thoại hoặc mật khẩu không chính xác. Vui lòng kiểm tra lại."
  - Người dùng có thể nhập lại thông tin

- **Tài khoản chưa được xác thực:**
  - Hệ thống hiển thị thông báo: "Tài khoản chưa được xác thực. Vui lòng kiểm tra email/SMS để xác thực tài khoản."
  - Cung cấp tùy chọn "Gửi lại email/SMS xác thực"

- **Tài khoản bị khóa:**
  - Hệ thống hiển thị thông báo: "Tài khoản đã bị khóa. Vui lòng liên hệ quản trị viên để được hỗ trợ."
  - Cung cấp thông tin liên hệ hỗ trợ

- **Quá nhiều lần đăng nhập sai:**
  - Sau 5 lần đăng nhập sai liên tiếp, tài khoản bị tạm khóa 15 phút
  - Hệ thống hiển thị thông báo: "Tài khoản đã bị tạm khóa do quá nhiều lần đăng nhập sai. Vui lòng thử lại sau 15 phút."

- **Lỗi hệ thống:**
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Không tạo session mới

**Additional Information:**
- **Business Rule:**
  - Mật khẩu phải khớp chính xác với mật khẩu đã lưu (đã được mã hóa)
  - Session có thời hạn mặc định 24 giờ
  - Nếu chọn "Ghi nhớ đăng nhập", session có thời hạn 30 ngày
  - Tối đa 5 lần đăng nhập sai trước khi tài khoản bị tạm khóa
  - Thời gian tạm khóa là 15 phút, sau đó tự động mở khóa
  - Mỗi tài khoản chỉ có thể có 1 session hoạt động tại một thời điểm

- **Non-Functional Requirement:**
  - Performance: Thời gian xác thực và đăng nhập phải dưới 2 giây
  - Usability: Giao diện đăng nhập phải rõ ràng, có thể nhập bằng phím Enter
  - Security: Session phải được mã hóa và bảo mật, sử dụng HTTPS
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn của session

**Priority:** Low  
**CRUD:** Read

