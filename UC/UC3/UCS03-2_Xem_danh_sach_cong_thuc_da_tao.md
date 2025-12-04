# UCS03-2: Xem danh sách công thức đã tạo [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập xem lại danh sách tất cả các công thức mình đã tạo, bao gồm trạng thái duyệt (Chờ duyệt, Đã duyệt, Bị từ chối), với khả năng lọc và sắp xếp theo các tiêu chí khác nhau.

**Trigger:** Người dùng đã đăng nhập nhấn vào menu "Công thức của tôi" hoặc truy cập trực tiếp từ trang profile cá nhân.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng đã tạo ít nhất một công thức trong hệ thống (hoặc hiển thị trang trống nếu chưa có)

**Post-Condition:**
- Danh sách tất cả công thức của người dùng được hiển thị với đầy đủ thông tin trạng thái
- Người dùng có thể thực hiện các hành động khác như chỉnh sửa, xóa, xem chi tiết
- Thống kê tổng quan về công thức được hiển thị (tổng số, số đã duyệt, số chờ duyệt, số bị từ chối)

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn vào menu "Công thức của tôi" hoặc "My Recipes"
2. Hệ thống truy vấn cơ sở dữ liệu để lấy tất cả công thức của người dùng hiện tại
3. Hệ thống hiển thị trang "Công thức của tôi" với layout:

   **Header với thống kê:**
   - a. Tổng số công thức đã tạo
   - b. Số công thức đã được duyệt
   - c. Số công thức đang chờ duyệt
   - d. Số công thức bị từ chối

   **Bộ lọc và sắp xếp:**
   - e. Bộ lọc theo trạng thái (Tất cả, Chờ duyệt, Đã duyệt, Bị từ chối)
   - f. Sắp xếp theo (Mới nhất, Cũ nhất, Tên A-Z, Điểm đánh giá)
   - g. Tìm kiếm trong công thức của mình

   **Danh sách công thức:**
   - h. Mỗi công thức hiển thị dạng card với:
      - Ảnh đại diện
      - Tên món ăn
      - Ngày tạo
      - Trạng thái (với màu sắc phân biệt)
      - Số lượt xem (nếu đã được duyệt)
      - Điểm đánh giá trung bình (nếu có)
      - Các nút hành động (Xem, Sửa, Xóa)

4. Hệ thống hiển thị danh sách với phân trang (20 công thức/trang)
5. Người dùng có thể:
   - a. Nhấp vào công thức để xem chi tiết (UC2.5)
   - b. Nhấn "Sửa" để chỉnh sửa công thức (UC3.3)
   - c. Nhấn "Xóa" để xóa công thức (UC3.4)
   - d. Sử dụng bộ lọc để xem công thức theo trạng thái
   - e. Tìm kiếm công thức theo tên

**Alternative Flow:**
- **Xem từ trang profile:**
  - Người dùng có thể truy cập từ trang thông tin cá nhân (UC1.4)
  - Nhấn vào tab "Công thức của tôi" hoặc số lượng công thức
  - Tiếp tục luồng từ bước 2 của Basic Flow

- **Lọc theo trạng thái cụ thể:**
  - Người dùng chọn một trạng thái cụ thể từ bộ lọc
  - Hệ thống chỉ hiển thị công thức có trạng thái đó
  - URL được cập nhật để có thể bookmark

- **Sắp xếp nâng cao:**
  - Người dùng có thể sắp xếp theo nhiều tiêu chí
  - Kết hợp sắp xếp với bộ lọc để tìm công thức mong muốn

- **Xuất danh sách:**
  - Người dùng có thể xuất danh sách công thức ra file PDF hoặc Excel
  - Bao gồm thông tin cơ bản và trạng thái của từng công thức

**Exception Flow:**
- **Chưa có công thức nào:**
  - Nếu người dùng chưa tạo công thức nào
  - Hệ thống hiển thị trang trống với thông báo: "Bạn chưa tạo công thức nào."
  - Hiển thị nút "Tạo công thức đầu tiên" để chuyển đến UC3.1

- **Lỗi tải dữ liệu:**
  - Nếu có lỗi khi truy vấn cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể tải danh sách công thức. Vui lòng thử lại sau."
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

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị công thức của chính người dùng đang đăng nhập
  - Công thức ở trạng thái "Chờ duyệt" có thể được chỉnh sửa và xóa
  - Công thức đã "Đã duyệt" chỉ có thể xem và báo cáo lỗi
  - Công thức "Bị từ chối" có thể xem lý do từ chối và tạo lại
  - Thống kê được cập nhật real-time khi có thay đổi trạng thái
  - Phân trang tối đa 20 công thức/trang để đảm bảo hiệu suất

- **Non-Functional Requirement:**
  - Performance: Thời gian tải danh sách phải dưới 2 giây
  - Usability: Giao diện phải rõ ràng, dễ phân biệt trạng thái bằng màu sắc
  - Security: Chỉ hiển thị công thức của chính người dùng
  - Reliability: Hệ thống phải hoạt động ổn định ngay cả khi có nhiều công thức

**Priority:** Low  
**CRUD:** Read


