# UCS04-3: Xem danh sách công thức Yêu thích [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập xem lại tất cả các công thức mình đã lưu vào danh sách yêu thích, với khả năng lọc, sắp xếp và quản lý danh sách một cách tiện lợi.

**Trigger:** Người dùng đã đăng nhập nhấn vào menu "Công thức yêu thích", "Favorites" hoặc truy cập từ trang profile cá nhân.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng đã có ít nhất một công thức trong danh sách yêu thích (hoặc hiển thị trang trống nếu chưa có)

**Post-Condition:**
- Danh sách tất cả công thức yêu thích của người dùng được hiển thị
- Người dùng có thể thực hiện các hành động khác như gỡ yêu thích, xem chi tiết, chia sẻ
- Thống kê tổng quan về danh sách yêu thích được hiển thị

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn vào menu "Công thức yêu thích" hoặc "Favorites"
2. Hệ thống truy vấn cơ sở dữ liệu để lấy tất cả công thức yêu thích của người dùng
3. Hệ thống hiển thị trang "Công thức yêu thích" với layout:

   **Header với thống kê:**
   - a. Tổng số công thức yêu thích
   - b. Số công thức đã xem gần đây
   - c. Số công thức chưa xem

   **Bộ lọc và sắp xếp:**
   - d. Bộ lọc theo danh mục món ăn
   - e. Bộ lọc theo độ khó (Dễ, Trung bình, Khó)
   - f. Bộ lọc theo thời gian nấu
   - g. Sắp xếp theo (Thời gian thêm, Tên A-Z, Điểm đánh giá, Thời gian nấu)
   - h. Tìm kiếm trong danh sách yêu thích

   **Danh sách công thức:**
   - i. Mỗi công thức hiển thị dạng card với:
      - Ảnh đại diện
      - Tên món ăn
      - Thời gian thêm vào yêu thích
      - Thời gian nấu và độ khó
      - Điểm đánh giá trung bình
      - Số lượt xem từ người dùng
      - Các nút hành động (Xem chi tiết, Gỡ yêu thích, Chia sẻ)

4. Hệ thống hiển thị danh sách với phân trang (20 công thức/trang)
5. Người dùng có thể:
   - a. Nhấp vào công thức để xem chi tiết (UC2.5)
   - b. Nhấn "Gỡ yêu thích" để loại bỏ khỏi danh sách (UC4.2)
   - c. Nhấn "Chia sẻ" để chia sẻ công thức (UC4.6)
   - d. Sử dụng bộ lọc để tìm công thức cụ thể
   - e. Sắp xếp danh sách theo tiêu chí mong muốn

**Alternative Flow:**
- **Xem từ trang profile:**
  - Người dùng có thể truy cập từ trang thông tin cá nhân (UC1.4)
  - Nhấn vào tab "Công thức yêu thích" hoặc số lượng yêu thích
  - Tiếp tục luồng từ bước 2 của Basic Flow

- **Xem danh sách rút gọn:**
  - Hiển thị danh sách yêu thích dạng compact với ít thông tin hơn
  - Phù hợp cho việc xem nhanh và quản lý

- **Xuất danh sách yêu thích:**
  - Người dùng có thể xuất danh sách yêu thích ra file PDF hoặc Excel
  - Bao gồm thông tin cơ bản và link đến từng công thức

- **Chia sẻ danh sách yêu thích:**
  - Người dùng có thể chia sẻ toàn bộ danh sách yêu thích với bạn bè
  - Tạo link chia sẻ hoặc gửi qua email

- **Đồng bộ danh sách yêu thích:**
  - Nếu người dùng đăng nhập trên nhiều thiết bị
  - Danh sách yêu thích được đồng bộ tự động

**Exception Flow:**
- **Chưa có công thức yêu thích nào:**
  - Nếu người dùng chưa thêm công thức nào vào yêu thích
  - Hệ thống hiển thị trang trống với thông báo: "Bạn chưa có công thức yêu thích nào."
  - Hiển thị gợi ý: "Khám phá các công thức phổ biến" với link đến trang chủ
  - Cung cấp nút "Khám phá công thức" để tìm kiếm

- **Lỗi tải dữ liệu:**
  - Nếu có lỗi khi truy vấn cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể tải danh sách yêu thích. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới" để thử lại

- **Session hết hạn:**
  - Nếu session hết hạn trong khi xem danh sách
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Không tìm thấy kết quả lọc:**
  - Nếu bộ lọc không trả về kết quả nào
  - Hệ thống hiển thị: "Không có công thức nào phù hợp với bộ lọc đã chọn."
  - Cung cấp nút "Xóa bộ lọc" để xem tất cả công thức

- **Lỗi hiển thị ảnh:**
  - Nếu ảnh đại diện của công thức không tải được
  - Hệ thống hiển thị ảnh placeholder mặc định
  - Không ảnh hưởng đến việc hiển thị thông tin khác

- **Công thức đã bị xóa:**
  - Nếu một số công thức trong danh sách yêu thích đã bị xóa khỏi hệ thống
  - Hệ thống tự động loại bỏ khỏi danh sách hiển thị
  - Hiển thị thông báo: "Đã tự động loại bỏ [số] công thức không còn tồn tại."

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị công thức yêu thích của chính người dùng đang đăng nhập
  - Danh sách được sắp xếp theo thời gian thêm vào yêu thích (mới nhất trước) theo mặc định
  - Chỉ hiển thị công thức có trạng thái "Đã duyệt"
  - Phân trang tối đa 20 công thức/trang để đảm bảo hiệu suất
  - Thống kê được cập nhật real-time khi có thay đổi
  - Danh sách yêu thích được cache trong 10 phút để tăng tốc độ

- **Non-Functional Requirement:**
  - Performance: Thời gian tải danh sách yêu thích phải dưới 2 giây
  - Usability: Giao diện phải rõ ràng, dễ sử dụng, có thể lọc và sắp xếp linh hoạt
  - Security: Chỉ hiển thị danh sách yêu thích của chính người dùng
  - Reliability: Hệ thống phải hoạt động ổn định ngay cả khi có nhiều công thức yêu thích

**Priority:** Low  
**CRUD:** Read


