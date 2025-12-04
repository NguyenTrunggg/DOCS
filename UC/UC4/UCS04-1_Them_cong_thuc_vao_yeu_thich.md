# UCS04-1: Thêm công thức vào Yêu thích [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập lưu một công thức nấu ăn vào danh sách yêu thích cá nhân để có thể truy cập nhanh chóng sau này mà không cần tìm kiếm lại.

**Trigger:** Người dùng đã đăng nhập nhấp vào biểu tượng "Yêu thích" (♥ hoặc ☆) trên trang chi tiết công thức hoặc trong danh sách công thức.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại và có trạng thái "Đã duyệt"
- Công thức chưa được thêm vào danh sách yêu thích của người dùng
- Người dùng chưa đạt giới hạn số lượng công thức yêu thích

**Post-Condition:**
- Công thức được thêm vào danh sách yêu thích của người dùng
- Biểu tượng "Yêu thích" thay đổi trạng thái (đổi màu hoặc icon)
- Thông báo xác nhận được hiển thị
- Số lượng yêu thích của công thức tăng lên 1
- Công thức xuất hiện trong danh sách "Công thức yêu thích" (UC4.3)

**Basic Flow:**
1. Người dùng đã đăng nhập đang xem trang chi tiết công thức (UC2.5) hoặc danh sách công thức
2. Người dùng nhấp vào biểu tượng "Yêu thích" (♥ trống hoặc ☆ trống)
3. Hệ thống kiểm tra các điều kiện:
   - a. Người dùng đã đăng nhập
   - b. Công thức có trạng thái "Đã duyệt"
   - c. Công thức chưa có trong danh sách yêu thích của người dùng
   - d. Người dùng chưa đạt giới hạn yêu thích (tối đa 500 công thức)
4. Hệ thống thêm công thức vào danh sách yêu thích của người dùng:
   - a. Lưu ID công thức và ID người dùng vào bảng yêu thích
   - b. Ghi lại thời gian thêm vào yêu thích
5. Hệ thống cập nhật trạng thái hiển thị:
   - a. Biểu tượng "Yêu thích" đổi sang trạng thái "đã yêu thích" (♥ đỏ hoặc ☆ vàng)
   - b. Thêm hiệu ứng animation (nhỏ dần, đổi màu)
6. Hệ thống tăng số lượng yêu thích của công thức lên 1
7. Hệ thống hiển thị thông báo xác nhận: "Đã thêm '[tên công thức]' vào Yêu thích"
8. Thông báo tự động biến mất sau 3 giây hoặc người dùng có thể đóng thủ công

**Alternative Flow:**
- **Thêm yêu thích từ danh sách:**
  - Người dùng đang xem danh sách công thức (kết quả tìm kiếm, danh mục)
  - Nhấp vào biểu tượng yêu thích trên card công thức
  - Hệ thống thực hiện tương tự như Basic Flow

- **Thêm yêu thích hàng loạt:**
  - Người dùng có thể chọn nhiều công thức và thêm yêu thích cùng lúc
  - Hệ thống hiển thị số lượng công thức sẽ được thêm
  - Xác nhận và thực hiện thêm tất cả

- **Thêm yêu thích từ thông báo:**
  - Người dùng nhận được thông báo về công thức mới hoặc gợi ý
  - Có thể thêm yêu thích trực tiếp từ thông báo

**Exception Flow:**
- **Người dùng chưa đăng nhập:**
  - Nếu người dùng chưa đăng nhập và nhấp vào biểu tượng yêu thích
  - Hệ thống hiển thị popup: "Vui lòng đăng nhập để thêm công thức vào yêu thích"
  - Cung cấp nút "Đăng nhập" để chuyển đến trang đăng nhập
  - Sau khi đăng nhập, tự động quay lại trang hiện tại

- **Công thức đã có trong yêu thích:**
  - Nếu công thức đã được thêm vào yêu thích trước đó
  - Hệ thống hiển thị thông báo: "Công thức này đã có trong danh sách yêu thích của bạn"
  - Biểu tượng vẫn ở trạng thái "đã yêu thích"

- **Đạt giới hạn yêu thích:**
  - Nếu người dùng đã có 500 công thức yêu thích
  - Hệ thống hiển thị: "Bạn đã đạt giới hạn 500 công thức yêu thích. Vui lòng xóa một số công thức cũ để thêm mới."
  - Cung cấp link đến trang "Công thức yêu thích" để quản lý

- **Công thức chưa được duyệt:**
  - Nếu công thức có trạng thái "Chờ duyệt" hoặc "Bị từ chối"
  - Hệ thống hiển thị: "Công thức này chưa được duyệt. Không thể thêm vào yêu thích."
  - Biểu tượng yêu thích bị vô hiệu hóa

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu vào cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể thêm vào yêu thích. Vui lòng thử lại sau."
  - Biểu tượng không thay đổi trạng thái

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình thêm yêu thích
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa hoặc không tồn tại
  - Hệ thống hiển thị: "Công thức không tồn tại hoặc đã bị xóa."
  - Cập nhật trang để loại bỏ công thức không hợp lệ

**Additional Information:**
- **Business Rule:**
  - Mỗi người dùng chỉ có thể thêm tối đa 500 công thức vào yêu thích
  - Mỗi công thức chỉ có thể được thêm vào yêu thích 1 lần cho mỗi người dùng
  - Chỉ có thể thêm công thức có trạng thái "Đã duyệt" vào yêu thích
  - Thời gian thêm vào yêu thích được ghi lại để sắp xếp theo thời gian
  - Số lượng yêu thích của công thức được cập nhật real-time
  - Danh sách yêu thích được sắp xếp theo thời gian thêm (mới nhất trước)

- **Non-Functional Requirement:**
  - Performance: Thời gian thêm yêu thích phải dưới 1 giây
  - Usability: Biểu tượng yêu thích phải rõ ràng, dễ nhận biết trạng thái
  - Security: Chỉ người dùng đã đăng nhập mới có thể thêm yêu thích
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi thêm yêu thích

**Priority:** Low  
**CRUD:** Create


