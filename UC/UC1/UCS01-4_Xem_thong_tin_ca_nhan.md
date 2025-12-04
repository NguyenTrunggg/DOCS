# UCS01-4: Xem thông tin cá nhân [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập xem các thông tin cá nhân của mình như tên hiển thị, email, ảnh đại diện và các thông tin tài khoản khác.

**Trigger:** Người dùng đã đăng nhập nhấn vào avatar/tên người dùng và chọn "Thông tin cá nhân" hoặc "Profile".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Thông tin cá nhân của người dùng tồn tại trong cơ sở dữ liệu

**Post-Condition:**
- Thông tin cá nhân của người dùng được hiển thị đầy đủ trên màn hình
- Người dùng có thể thấy trạng thái tài khoản và các thông tin liên quan
- Không có thay đổi nào được thực hiện đối với dữ liệu

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn vào avatar/tên người dùng ở góc trên phải màn hình
2. Menu dropdown xuất hiện với các tùy chọn cá nhân
3. Người dùng chọn "Thông tin cá nhân" hoặc "Profile"
4. Hệ thống truy vấn cơ sở dữ liệu để lấy thông tin cá nhân của người dùng
5. Hệ thống hiển thị trang thông tin cá nhân với các thông tin sau:
   - a. Ảnh đại diện (avatar)
   - b. Họ và tên hiển thị
   - c. Email đăng ký
   - d. Số điện thoại (nếu có)
   - e. Ngày đăng ký tài khoản
   - f. Lần đăng nhập cuối
   - g. Trạng thái tài khoản (Đã xác thực/Chờ xác thực)
6. Hệ thống hiển thị thống kê hoạt động cơ bản:
   - a. Số công thức đã đăng
   - b. Số công thức đã yêu thích
   - c. Số bình luận đã viết
7. Người dùng có thể xem thông tin và quyết định có muốn chỉnh sửa hay không

**Alternative Flow:**
- **Truy cập từ trang cài đặt:**
  - Người dùng vào trang "Cài đặt" trước
  - Chọn tab "Thông tin cá nhân"
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Truy cập từ liên kết trực tiếp:**
  - Người dùng có thể bookmark hoặc truy cập trực tiếp URL profile
  - Hệ thống kiểm tra quyền truy cập và hiển thị thông tin

**Exception Flow:**
- **Không tìm thấy thông tin người dùng:**
  - Nếu thông tin người dùng bị thiếu hoặc không tồn tại
  - Hệ thống hiển thị thông báo: "Không thể tải thông tin cá nhân. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới" để thử lại

- **Lỗi tải ảnh đại diện:**
  - Nếu ảnh đại diện không tải được
  - Hệ thống hiển thị ảnh mặc định (avatar mặc định)
  - Không ảnh hưởng đến việc hiển thị các thông tin khác

- **Session hết hạn:**
  - Nếu session hết hạn trong khi xem thông tin
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Lỗi hệ thống:**
  - Nếu có lỗi server khi truy vấn thông tin
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Cung cấp nút "Quay lại" để trở về trang trước

**Additional Information:**
- **Business Rule:**
  - Chỉ người dùng đã đăng nhập mới có thể xem thông tin cá nhân của mình
  - Thông tin email và số điện thoại được hiển thị đầy đủ để người dùng xác nhận
  - Ảnh đại diện được hiển thị với kích thước chuẩn (150x150px)
  - Thống kê hoạt động được cập nhật theo thời gian thực
  - Thông tin nhạy cảm như mật khẩu không bao giờ được hiển thị

- **Non-Functional Requirement:**
  - Performance: Thời gian tải trang thông tin cá nhân phải dưới 2 giây
  - Usability: Giao diện phải rõ ràng, dễ đọc, có thể phân biệt rõ các loại thông tin
  - Security: Thông tin cá nhân chỉ được hiển thị cho chính chủ sở hữu
  - Reliability: Hệ thống phải đảm bảo tính chính xác của thông tin hiển thị

**Priority:** Low  
**CRUD:** Read

