# UCS01-1: Đăng ký tài khoản [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng mới tạo một tài khoản mới trong hệ thống quản lý công thức nấu ăn bằng cách sử dụng email hoặc số điện thoại cùng với mật khẩu bảo mật.

**Trigger:** Người dùng truy cập trang đăng ký và nhấn nút "Đăng ký tài khoản".

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Người dùng chưa có tài khoản trong hệ thống
- Email/số điện thoại chưa được sử dụng bởi tài khoản khác

**Post-Condition:**
- Tài khoản mới được tạo thành công trong cơ sở dữ liệu
- Người dùng nhận được email xác thực (nếu đăng ký bằng email)
- Trạng thái tài khoản là "Chờ xác thực" cho đến khi xác thực email/OTP
- Hệ thống tự động chuyển hướng đến trang đăng nhập

**Basic Flow:**
1. Người dùng truy cập trang đăng ký từ trang chủ hoặc trang đăng nhập
2. Người dùng chọn phương thức đăng ký (Email hoặc Số điện thoại)
3. Người dùng nhập thông tin đăng ký:
   - a. Email (nếu chọn đăng ký bằng email)
   - b. Số điện thoại (nếu chọn đăng ký bằng SĐT)
   - c. Mật khẩu (tối thiểu 8 ký tự, bao gồm chữ hoa, chữ thường, số)
   - d. Xác nhận mật khẩu
   - e. Họ và tên hiển thị
4. Người dùng tích chọn đồng ý với điều khoản sử dụng
5. Người dùng nhấn nút "Đăng ký"
6. Hệ thống kiểm tra và xác thực thông tin đầu vào
7. Hệ thống tạo tài khoản mới trong cơ sở dữ liệu với trạng thái "Chờ xác thực"
8. Nếu đăng ký bằng email: Hệ thống gửi email xác thực chứa link kích hoạt
9. Nếu đăng ký bằng SĐT: Hệ thống gửi mã OTP qua SMS
10. Hệ thống hiển thị thông báo thành công và chuyển hướng đến trang đăng nhập

**Alternative Flow:**
- **Đăng ký bằng Google/Facebook:**
  - Người dùng chọn "Đăng ký bằng Google" hoặc "Đăng ký bằng Facebook"
  - Hệ thống chuyển hướng đến trang xác thực của bên thứ ba
  - Sau khi xác thực thành công, hệ thống nhận thông tin cơ bản
  - Hệ thống tự động tạo tài khoản với trạng thái "Đã xác thực"
  - Người dùng được đăng nhập tự động

**Exception Flow:**
- **Email/SĐT đã tồn tại:**
  - Hệ thống hiển thị thông báo lỗi: "Email/Số điện thoại này đã được sử dụng. Vui lòng chọn email/SĐT khác hoặc đăng nhập nếu đã có tài khoản."
  - Người dùng có thể quay lại form để nhập thông tin mới

- **Mật khẩu không đủ mạnh:**
  - Hệ thống hiển thị thông báo: "Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ hoa, chữ thường và số."
  - Người dùng có thể chỉnh sửa mật khẩu

- **Lỗi gửi email/SMS:**
  - Hệ thống hiển thị thông báo: "Không thể gửi email/SMS xác thực. Vui lòng thử lại sau."
  - Tài khoản vẫn được tạo nhưng ở trạng thái "Chưa xác thực"
  - Người dùng có thể yêu cầu gửi lại email/SMS xác thực

- **Lỗi hệ thống:**
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Không tạo tài khoản mới

**Additional Information:**
- **Business Rule:**
  - Email phải có định dạng hợp lệ và chưa được sử dụng trong hệ thống
  - Số điện thoại phải có 10-11 chữ số và chưa được sử dụng
  - Mật khẩu phải có ít nhất 8 ký tự, bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số
  - Tên hiển thị phải có ít nhất 2 ký tự và không chứa ký tự đặc biệt
  - Người dùng phải đồng ý với điều khoản sử dụng mới có thể đăng ký
  - Email/SMS xác thực có hiệu lực trong 24 giờ

- **Non-Functional Requirement:**
  - Performance: Thời gian tạo tài khoản phải dưới 3 giây
  - Usability: Form đăng ký phải thân thiện, có gợi ý rõ ràng cho từng trường
  - Security: Mật khẩu phải được mã hóa bằng bcrypt trước khi lưu vào CSDL
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi tạo tài khoản

**Priority:** Low  
**CRUD:** Create

