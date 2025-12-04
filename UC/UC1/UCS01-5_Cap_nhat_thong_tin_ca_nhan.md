# UCS01-5: Cập nhật thông tin cá nhân [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập thay đổi các thông tin cá nhân của mình như tên hiển thị và ảnh đại diện, đảm bảo thông tin luôn được cập nhật và chính xác.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Chỉnh sửa" hoặc "Cập nhật thông tin" trên trang thông tin cá nhân.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng đang ở trang thông tin cá nhân hoặc trang cài đặt
- Tài khoản người dùng không bị khóa

**Post-Condition:**
- Thông tin cá nhân được cập nhật thành công trong cơ sở dữ liệu
- Giao diện hiển thị thông tin mới ngay lập tức
- Thời gian cập nhật cuối được ghi lại
- Thông báo thành công được hiển thị cho người dùng

**Basic Flow:**
1. Người dùng đã đăng nhập truy cập trang thông tin cá nhân (UC1.4)
2. Người dùng nhấn nút "Chỉnh sửa" hoặc "Cập nhật thông tin"
3. Hệ thống chuyển sang chế độ chỉnh sửa, các trường thông tin trở thành có thể chỉnh sửa:
   - a. Họ và tên hiển thị (text input)
   - b. Ảnh đại diện (file upload)
4. Người dùng chỉnh sửa thông tin:
   - a. Nhập tên hiển thị mới (nếu muốn thay đổi)
   - b. Chọn ảnh mới từ thiết bị (nếu muốn thay đổi)
5. Hệ thống hiển thị preview ảnh mới (nếu có)
6. Người dùng nhấn nút "Lưu thay đổi"
7. Hệ thống kiểm tra và xác thực thông tin đầu vào
8. Hệ thống upload ảnh mới lên server (nếu có)
9. Hệ thống cập nhật thông tin trong cơ sở dữ liệu
10. Hệ thống cập nhật thời gian chỉnh sửa cuối
11. Hệ thống hiển thị thông báo "Cập nhật thông tin thành công!"
12. Trang tự động chuyển về chế độ xem (không chỉnh sửa)

**Alternative Flow:**
- **Chỉ cập nhật ảnh đại diện:**
  - Người dùng chỉ thay đổi ảnh đại diện, không thay đổi tên
  - Hệ thống chỉ xử lý upload ảnh và cập nhật URL ảnh mới
  - Tiếp tục luồng từ bước 9 của Basic Flow

- **Chỉ cập nhật tên hiển thị:**
  - Người dùng chỉ thay đổi tên hiển thị, không thay đổi ảnh
  - Hệ thống chỉ cập nhật trường tên trong cơ sở dữ liệu
  - Tiếp tục luồng từ bước 9 của Basic Flow

- **Hủy bỏ thay đổi:**
  - Người dùng nhấn nút "Hủy" trong khi đang chỉnh sửa
  - Hệ thống hủy bỏ tất cả thay đổi và quay về trạng thái ban đầu
  - Không cập nhật gì vào cơ sở dữ liệu

**Exception Flow:**
- **Tên hiển thị không hợp lệ:**
  - Nếu tên hiển thị quá ngắn (< 2 ký tự) hoặc chứa ký tự đặc biệt không được phép
  - Hệ thống hiển thị thông báo lỗi: "Tên hiển thị phải có ít nhất 2 ký tự và không chứa ký tự đặc biệt."
  - Người dùng có thể chỉnh sửa lại tên

- **Ảnh không hợp lệ:**
  - Nếu file ảnh quá lớn (> 5MB) hoặc không đúng định dạng (không phải JPG/PNG)
  - Hệ thống hiển thị thông báo: "Ảnh phải có định dạng JPG hoặc PNG và kích thước dưới 5MB."
  - Người dùng có thể chọn ảnh khác

- **Lỗi upload ảnh:**
  - Nếu quá trình upload ảnh bị lỗi do mạng hoặc server
  - Hệ thống hiển thị thông báo: "Không thể upload ảnh. Vui lòng thử lại."
  - Người dùng có thể thử upload lại hoặc bỏ qua việc thay đổi ảnh

- **Lỗi cập nhật cơ sở dữ liệu:**
  - Nếu không thể cập nhật thông tin vào cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Không thể cập nhật thông tin. Vui lòng thử lại sau."
  - Thông tin vẫn giữ nguyên trạng thái cũ

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình chỉnh sửa
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

**Additional Information:**
- **Business Rule:**
  - Tên hiển thị phải có ít nhất 2 ký tự và không được để trống
  - Tên hiển thị không được chứa ký tự đặc biệt (@, #, $, %, &, *)
  - Ảnh đại diện phải có định dạng JPG hoặc PNG
  - Kích thước ảnh tối đa 5MB
  - Hệ thống tự động resize ảnh về kích thước chuẩn (150x150px)
  - Email và số điện thoại không thể thay đổi từ trang này (cần có use case riêng)
  - Mỗi lần cập nhật phải ghi lại thời gian thay đổi

- **Non-Functional Requirement:**
  - Performance: Thời gian cập nhật thông tin phải dưới 3 giây
  - Usability: Giao diện chỉnh sửa phải trực quan, có preview ảnh
  - Security: Ảnh upload phải được kiểm tra để đảm bảo không chứa mã độc
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi cập nhật

**Priority:** Medium  
**CRUD:** Update

