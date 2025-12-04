# UCS04-2: Gỡ công thức khỏi Yêu thích [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập gỡ bỏ một công thức đã được thêm vào danh sách yêu thích, giúp quản lý danh sách yêu thích một cách linh hoạt và cập nhật theo sở thích thay đổi.

**Trigger:** Người dùng đã đăng nhập nhấp vào biểu tượng "Yêu thích" đang ở trạng thái "đã yêu thích" (♥ đỏ hoặc ☆ vàng) trên trang chi tiết công thức hoặc trong danh sách công thức.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại trong danh sách yêu thích của người dùng
- Biểu tượng "Yêu thích" đang ở trạng thái "đã yêu thích"

**Post-Condition:**
- Công thức được gỡ khỏi danh sách yêu thích của người dùng
- Biểu tượng "Yêu thích" trở về trạng thái ban đầu (♥ trống hoặc ☆ trống)
- Thông báo xác nhận được hiển thị
- Số lượng yêu thích của công thức giảm xuống 1
- Công thức không còn xuất hiện trong danh sách "Công thức yêu thích" (UC4.3)

**Basic Flow:**
1. Người dùng đã đăng nhập đang xem trang chi tiết công thức (UC2.5) hoặc danh sách công thức
2. Người dùng nhấp vào biểu tượng "Yêu thích" đang ở trạng thái "đã yêu thích" (♥ đỏ hoặc ☆ vàng)
3. Hệ thống kiểm tra các điều kiện:
   - a. Người dùng đã đăng nhập
   - b. Công thức tồn tại trong danh sách yêu thích của người dùng
4. Hệ thống gỡ bỏ công thức khỏi danh sách yêu thích:
   - a. Xóa bản ghi yêu thích từ bảng yêu thích
   - b. Ghi lại thời gian gỡ khỏi yêu thích (để audit)
5. Hệ thống cập nhật trạng thái hiển thị:
   - a. Biểu tượng "Yêu thích" đổi về trạng thái "chưa yêu thích" (♥ trống hoặc ☆ trống)
   - b. Thêm hiệu ứng animation (nhỏ dần, mờ đi)
6. Hệ thống giảm số lượng yêu thích của công thức xuống 1
7. Hệ thống hiển thị thông báo xác nhận: "Đã gỡ '[tên công thức]' khỏi Yêu thích"
8. Thông báo tự động biến mất sau 3 giây hoặc người dùng có thể đóng thủ công

**Alternative Flow:**
- **Gỡ yêu thích từ danh sách:**
  - Người dùng đang xem danh sách công thức yêu thích (UC4.3)
  - Nhấp vào biểu tượng yêu thích trên card công thức
  - Hệ thống thực hiện tương tự như Basic Flow

- **Gỡ yêu thích hàng loạt:**
  - Người dùng có thể chọn nhiều công thức và gỡ yêu thích cùng lúc
  - Hệ thống hiển thị số lượng công thức sẽ được gỡ
  - Xác nhận và thực hiện gỡ tất cả

- **Gỡ yêu thích từ trang quản lý:**
  - Người dùng vào trang "Quản lý yêu thích" với danh sách đầy đủ
  - Có thể gỡ yêu thích nhiều công thức cùng lúc
  - Có tùy chọn gỡ tất cả yêu thích

**Exception Flow:**
- **Người dùng chưa đăng nhập:**
  - Nếu người dùng chưa đăng nhập và nhấp vào biểu tượng yêu thích
  - Hệ thống hiển thị popup: "Vui lòng đăng nhập để quản lý yêu thích"
  - Cung cấp nút "Đăng nhập" để chuyển đến trang đăng nhập

- **Công thức không có trong yêu thích:**
  - Nếu công thức không có trong danh sách yêu thích
  - Hệ thống hiển thị thông báo: "Công thức này không có trong danh sách yêu thích của bạn"
  - Biểu tượng vẫn ở trạng thái "chưa yêu thích"

- **Lỗi xóa cơ sở dữ liệu:**
  - Nếu không thể xóa khỏi cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể gỡ khỏi yêu thích. Vui lòng thử lại sau."
  - Biểu tượng không thay đổi trạng thái

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình gỡ yêu thích
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa khỏi hệ thống
  - Hệ thống tự động gỡ khỏi danh sách yêu thích
  - Hiển thị thông báo: "Công thức đã bị xóa. Đã tự động gỡ khỏi danh sách yêu thích."

- **Lỗi cập nhật số lượng yêu thích:**
  - Nếu không thể cập nhật số lượng yêu thích của công thức
  - Hệ thống vẫn gỡ khỏi danh sách yêu thích của người dùng
  - Hiển thị cảnh báo: "Đã gỡ khỏi yêu thích nhưng có lỗi khi cập nhật thống kê."

**Additional Information:**
- **Business Rule:**
  - Chỉ có thể gỡ công thức khỏi danh sách yêu thích của chính mình
  - Việc gỡ yêu thích không ảnh hưởng đến công thức gốc
  - Thời gian gỡ yêu thích được ghi lại để audit
  - Số lượng yêu thích của công thức được cập nhật real-time
  - Sau khi gỡ, công thức có thể được thêm lại vào yêu thích bất cứ lúc nào
  - Danh sách yêu thích được cập nhật ngay lập tức

- **Non-Functional Requirement:**
  - Performance: Thời gian gỡ yêu thích phải dưới 1 giây
  - Usability: Biểu tượng yêu thích phải rõ ràng về trạng thái hiện tại
  - Security: Chỉ người dùng đã đăng nhập mới có thể gỡ yêu thích của mình
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi gỡ yêu thích

**Priority:** Low  
**CRUD:** Delete


