# UCS01-7: Yêu cầu đặt lại mật khẩu [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng yêu cầu hệ thống gửi hướng dẫn để đặt lại mật khẩu khi họ quên mật khẩu hiện tại, đảm bảo tính bảo mật và khả năng phục hồi tài khoản.

**Trigger:** Người dùng nhấn liên kết "Quên mật khẩu?" trên trang đăng nhập hoặc trang cài đặt bảo mật.

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Người dùng có tài khoản hợp lệ trong hệ thống
- Email hoặc số điện thoại của người dùng đã được xác thực
- Tài khoản không bị khóa hoặc vô hiệu hóa

**Post-Condition:**
- Link đặt lại mật khẩu được gửi đến email hoặc mã OTP được gửi qua SMS
- Token đặt lại mật khẩu được tạo và lưu trong cơ sở dữ liệu với thời hạn 24 giờ
- Người dùng nhận được hướng dẫn về cách đặt lại mật khẩu
- Thời gian yêu cầu đặt lại mật khẩu được ghi lại trong log bảo mật

**Basic Flow:**
1. Người dùng truy cập trang đăng nhập
2. Người dùng nhấn liên kết "Quên mật khẩu?" hoặc "Đặt lại mật khẩu"
3. Hệ thống chuyển hướng đến trang "Đặt lại mật khẩu"
4. Hệ thống hiển thị form yêu cầu thông tin:
   - a. Email hoặc số điện thoại đã đăng ký
5. Người dùng nhập email hoặc số điện thoại của tài khoản
6. Người dùng nhấn nút "Gửi yêu cầu đặt lại mật khẩu"
7. Hệ thống kiểm tra email/số điện thoại có tồn tại trong cơ sở dữ liệu
8. Hệ thống tạo token đặt lại mật khẩu duy nhất với thời hạn 24 giờ
9. Hệ thống lưu token vào cơ sở dữ liệu với trạng thái "Chưa sử dụng"
10. Hệ thống gửi email/SMS chứa link đặt lại mật khẩu hoặc mã OTP:
    - a. Nếu bằng email: Gửi email với link chứa token
    - b. Nếu bằng SMS: Gửi SMS chứa mã OTP 6 chữ số
11. Hệ thống ghi lại thời gian yêu cầu vào log bảo mật
12. Hệ thống hiển thị thông báo: "Hướng dẫn đặt lại mật khẩu đã được gửi đến email/SMS của bạn. Vui lòng kiểm tra và làm theo hướng dẫn."

**Alternative Flow:**
- **Yêu cầu lại email/SMS:**
  - Nếu người dùng không nhận được email/SMS trong 5 phút
  - Người dùng có thể nhấn nút "Gửi lại email/SMS"
  - Hệ thống tạo token mới và gửi lại hướng dẫn
  - Token cũ bị vô hiệu hóa

- **Đặt lại mật khẩu bằng OTP:**
  - Nếu người dùng đăng ký bằng số điện thoại
  - Hệ thống gửi mã OTP 6 chữ số qua SMS
  - Người dùng nhập OTP để xác thực và đặt mật khẩu mới

**Exception Flow:**
- **Email/số điện thoại không tồn tại:**
  - Nếu email/số điện thoại không có trong cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Email/số điện thoại không tồn tại trong hệ thống. Vui lòng kiểm tra lại hoặc đăng ký tài khoản mới."
  - Không gửi email/SMS

- **Tài khoản bị khóa:**
  - Nếu tài khoản đã bị khóa hoặc vô hiệu hóa
  - Hệ thống hiển thị thông báo: "Tài khoản đã bị khóa. Vui lòng liên hệ quản trị viên để được hỗ trợ."
  - Không gửi email/SMS

- **Quá nhiều yêu cầu đặt lại:**
  - Nếu người dùng yêu cầu đặt lại mật khẩu quá nhiều lần (>3 lần/giờ)
  - Hệ thống hiển thị thông báo: "Bạn đã yêu cầu đặt lại mật khẩu quá nhiều lần. Vui lòng thử lại sau 1 giờ."
  - Không gửi email/SMS mới

- **Lỗi gửi email/SMS:**
  - Nếu không thể gửi email/SMS do lỗi server hoặc mạng
  - Hệ thống hiển thị thông báo: "Không thể gửi email/SMS. Vui lòng thử lại sau."
  - Token vẫn được tạo nhưng người dùng có thể yêu cầu gửi lại

- **Token đã hết hạn:**
  - Nếu người dùng click vào link sau 24 giờ
  - Hệ thống hiển thị thông báo: "Link đặt lại mật khẩu đã hết hạn. Vui lòng yêu cầu đặt lại mật khẩu mới."
  - Chuyển hướng về trang yêu cầu đặt lại mật khẩu

**Additional Information:**
- **Business Rule:**
  - Token đặt lại mật khẩu có thời hạn 24 giờ từ khi tạo
  - Mỗi token chỉ có thể sử dụng 1 lần
  - Tối đa 3 yêu cầu đặt lại mật khẩu trong 1 giờ
  - Email/SMS chỉ được gửi đến email/số điện thoại đã xác thực
  - Token phải được mã hóa và không thể đoán được
  - Mỗi lần yêu cầu đặt lại phải ghi lại trong log bảo mật
  - Link đặt lại mật khẩu phải chứa HTTPS để đảm bảo bảo mật

- **Non-Functional Requirement:**
  - Performance: Thời gian gửi email/SMS phải dưới 30 giây
  - Usability: Thông báo phải rõ ràng, hướng dẫn cụ thể
  - Security: Token phải được mã hóa, link phải sử dụng HTTPS
  - Reliability: Hệ thống phải đảm bảo email/SMS được gửi thành công

**Priority:** Medium  
**CRUD:** Update

