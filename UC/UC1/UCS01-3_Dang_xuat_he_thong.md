# UCS01-3: Đăng xuất khỏi hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập thoát khỏi phiên đăng nhập hiện tại một cách an toàn, đảm bảo thông tin cá nhân và dữ liệu được bảo vệ.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Đăng xuất" hoặc chọn "Đăng xuất" từ menu người dùng.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng đang ở bất kỳ trang nào trong hệ thống (trừ trang đăng nhập)

**Post-Condition:**
- Session của người dùng được hủy bỏ hoàn toàn
- Tất cả cookie liên quan đến phiên đăng nhập được xóa
- Người dùng được chuyển hướng đến trang đăng nhập hoặc trang chủ
- Thông tin đăng nhập được xóa khỏi bộ nhớ trình duyệt (nếu có)
- Thời gian đăng xuất được ghi lại trong log hệ thống

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn vào avatar/tên người dùng ở góc trên phải màn hình
2. Menu dropdown xuất hiện với các tùy chọn cá nhân
3. Người dùng chọn "Đăng xuất" từ menu
4. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn đăng xuất?"
5. Người dùng nhấn "Xác nhận" trong hộp thoại
6. Hệ thống gửi yêu cầu đăng xuất đến server
7. Server xóa session khỏi cơ sở dữ liệu
8. Server gửi lệnh xóa cookie về trình duyệt
9. Hệ thống ghi lại thời gian đăng xuất vào log
10. Người dùng được chuyển hướng đến trang đăng nhập với thông báo "Đã đăng xuất thành công"

**Alternative Flow:**
- **Đăng xuất từ menu hamburger (mobile):**
  - Trên thiết bị di động, người dùng nhấn menu hamburger
  - Chọn "Đăng xuất" từ menu mobile
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Đăng xuất tự động do hết hạn session:**
  - Khi session hết hạn, hệ thống tự động đăng xuất người dùng
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."
  - Chuyển hướng đến trang đăng nhập

**Exception Flow:**
- **Người dùng hủy bỏ đăng xuất:**
  - Trong hộp thoại xác nhận, người dùng nhấn "Hủy"
  - Hộp thoại đóng lại, người dùng vẫn ở trang hiện tại
  - Session tiếp tục hoạt động bình thường

- **Lỗi khi hủy session:**
  - Nếu server không thể xóa session do lỗi kết nối
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi khi đăng xuất. Vui lòng thử lại."
  - Người dùng vẫn có thể thử đăng xuất lại

- **Mất kết nối mạng:**
  - Nếu mất kết nối trong quá trình đăng xuất
  - Hệ thống hiển thị thông báo: "Mất kết nối. Phiên đăng nhập sẽ được hủy khi khôi phục kết nối."
  - Chuyển hướng đến trang đăng nhập

**Additional Information:**
- **Business Rule:**
  - Mỗi lần đăng xuất phải xóa hoàn toàn session và cookie
  - Thời gian đăng xuất phải được ghi lại để audit
  - Sau khi đăng xuất, người dùng không thể truy cập các trang yêu cầu xác thực
  - Nếu có nhiều tab đang mở cùng lúc, tất cả tab sẽ được đăng xuất
  - Đăng xuất không ảnh hưởng đến dữ liệu đã lưu của người dùng

- **Non-Functional Requirement:**
  - Performance: Thời gian đăng xuất phải dưới 1 giây
  - Usability: Quá trình đăng xuất phải đơn giản, chỉ cần 1-2 click
  - Security: Đảm bảo session được xóa hoàn toàn, không để lại thông tin nhạy cảm
  - Reliability: Hệ thống phải đảm bảo đăng xuất thành công ngay cả khi có lỗi nhỏ

**Priority:** Low  
**CRUD:** N/A

