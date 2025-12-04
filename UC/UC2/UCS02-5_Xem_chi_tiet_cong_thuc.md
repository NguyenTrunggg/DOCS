# UCS02-5: Xem chi tiết Công thức [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xem thông tin chi tiết đầy đủ của một công thức nấu ăn bao gồm nguyên liệu, các bước thực hiện, ảnh minh họa, đánh giá và bình luận từ cộng đồng.

**Trigger:** Người dùng nhấp vào một công thức từ danh sách kết quả tìm kiếm (UC2.1, UC2.2) hoặc từ bất kỳ danh sách công thức nào trong hệ thống.

**Pre-Condition:**
- Công thức tồn tại trong cơ sở dữ liệu và có trạng thái "Đã duyệt"
- Hệ thống đang hoạt động bình thường
- Người dùng có quyền truy cập xem công thức (công thức công khai)

**Post-Condition:**
- Tất cả thông tin chi tiết của công thức được hiển thị đầy đủ
- Số lượt xem công thức được tăng lên 1
- Người dùng có thể thực hiện các hành động khác như yêu thích, đánh giá, bình luận
- Lịch sử xem công thức được ghi lại (nếu người dùng đã đăng nhập)

**Basic Flow:**
1. Người dùng nhấp vào một công thức từ danh sách kết quả tìm kiếm hoặc danh sách công thức
2. Hệ thống lấy ID của công thức được chọn
3. Hệ thống truy vấn cơ sở dữ liệu để lấy toàn bộ thông tin công thức:
   - a. Thông tin cơ bản: tên món, mô tả, ảnh/video đại diện
   - b. Thông tin nấu ăn: thời gian chuẩn bị, thời gian nấu, độ khó, khẩu phần
   - c. Danh sách nguyên liệu với định lượng và đơn vị
   - d. Các bước thực hiện chi tiết (có thể kèm ảnh cho từng bước)
   - e. Thông tin người tạo công thức
   - f. Thống kê đánh giá: điểm trung bình, số lượng đánh giá
4. Hệ thống hiển thị trang chi tiết công thức với các section:

   **Header:**
   - a. Ảnh/video đại diện món ăn (full size)
   - b. Tên món ăn (lớn, nổi bật)
   - c. Mô tả ngắn về món ăn
   - d. Các nút hành động: Yêu thích, Chia sẻ, In công thức

   **Thông tin nấu ăn:**
   - a. Thời gian chuẩn bị
   - b. Thời gian nấu
   - c. Tổng thời gian
   - d. Độ khó (với icon minh họa)
   - e. Khẩu phần ăn

   **Nguyên liệu:**
   - a. Danh sách nguyên liệu với định lượng chính xác
   - b. Đơn vị đo lường rõ ràng
   - c. Có thể chọn "Tạo danh sách mua sắm"

   **Các bước thực hiện:**
   - a. Đánh số thứ tự các bước
   - b. Mô tả chi tiết từng bước
   - c. Ảnh minh họa cho từng bước (nếu có)
   - d. Ghi chú đặc biệt cho từng bước

   **Thông tin tác giả:**
   - a. Ảnh đại diện người tạo
   - b. Tên người tạo
   - c. Ngày tạo công thức
   - d. Số công thức khác của tác giả

   **Đánh giá và bình luận:**
   - a. Điểm đánh giá trung bình (sao)
   - b. Biểu đồ phân phối điểm đánh giá
   - c. Danh sách bình luận từ người dùng khác
   - d. Form để đánh giá và bình luận (nếu đã đăng nhập)

5. Hệ thống tăng số lượt xem của công thức lên 1
6. Nếu người dùng đã đăng nhập, hệ thống ghi lại lịch sử xem công thức
7. Người dùng có thể thực hiện các hành động khác trên trang này

**Alternative Flow:**
- **Xem công thức từ liên kết chia sẻ:**
  - Người dùng truy cập qua link chia sẻ trực tiếp
  - Hệ thống kiểm tra công thức có tồn tại và công khai không
  - Tiếp tục luồng từ bước 3 của Basic Flow

- **Xem công thức trong chế độ in:**
  - Người dùng nhấn nút "In công thức"
  - Hệ thống hiển thị phiên bản tối ưu cho in ấn
  - Loại bỏ các element không cần thiết, chỉ giữ nội dung chính

- **Xem công thức trong chế độ tối:**
  - Người dùng có thể chuyển sang chế độ tối (dark mode)
  - Giao diện được điều chỉnh màu sắc phù hợp
  - Thiết lập được lưu trong preference của người dùng

- **Xem công thức với phông chữ lớn:**
  - Người dùng có thể tăng kích thước phông chữ
  - Phù hợp cho người cao tuổi hoặc khó đọc

**Exception Flow:**
- **Công thức không tồn tại:**
  - Nếu ID công thức không có trong cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Công thức không tồn tại hoặc đã bị xóa."
  - Cung cấp nút "Quay lại" hoặc "Về trang chủ"

- **Công thức chưa được duyệt:**
  - Nếu công thức có trạng thái "Chờ duyệt" hoặc "Bị từ chối"
  - Hệ thống hiển thị thông báo: "Công thức này chưa được duyệt hoặc không khả dụng."
  - Chỉ hiển thị nếu người xem là tác giả của công thức

- **Lỗi tải ảnh/video:**
  - Nếu ảnh/video không tải được do lỗi server
  - Hệ thống hiển thị ảnh placeholder hoặc icon mặc định
  - Không ảnh hưởng đến việc hiển thị các thông tin khác

- **Lỗi tải dữ liệu:**
  - Nếu có lỗi khi truy vấn thông tin công thức
  - Hệ thống hiển thị: "Không thể tải thông tin công thức. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới trang" để thử lại

- **Công thức bị báo cáo:**
  - Nếu công thức đang được xem xét do báo cáo từ cộng đồng
  - Hệ thống hiển thị thông báo: "Công thức này đang được xem xét. Một số nội dung có thể bị ẩn."
  - Chỉ hiển thị thông tin cơ bản, ẩn bình luận và đánh giá

- **Session hết hạn trong khi xem:**
  - Nếu người dùng đã đăng nhập nhưng session hết hạn
  - Hệ thống vẫn hiển thị công thức nhưng ẩn các tính năng cần đăng nhập
  - Hiển thị thông báo: "Vui lòng đăng nhập để đánh giá và bình luận."

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị công thức có trạng thái "Đã duyệt"
  - Tác giả công thức có thể xem công thức của mình dù chưa được duyệt
  - Số lượt xem được cập nhật mỗi khi có người xem (không tính refresh)
  - Lịch sử xem được lưu tối đa 50 công thức gần nhất cho mỗi người dùng
  - Ảnh/video được tối ưu hóa để tải nhanh trên mobile
  - Công thức có thể được xem mà không cần đăng nhập

- **Non-Functional Requirement:**
  - Performance: Thời gian tải trang chi tiết phải dưới 3 giây
  - Usability: Giao diện phải responsive, hiển thị tốt trên mobile và desktop
  - Security: Thông tin công thức phải được validate trước khi hiển thị
  - Reliability: Hệ thống phải hoạt động ổn định với nhiều người xem cùng lúc

**Priority:** Medium  
**CRUD:** Read


