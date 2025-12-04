# UCS03-1: Thêm công thức mới [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập tạo và gửi một công thức nấu ăn mới lên hệ thống để chờ admin duyệt, bao gồm đầy đủ thông tin về nguyên liệu, các bước thực hiện, ảnh minh họa và thông tin bổ sung.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Tạo công thức mới" hoặc "Thêm công thức" từ menu hoặc trang quản lý công thức cá nhân.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Tài khoản người dùng không bị khóa hoặc hạn chế
- Hệ thống có đủ dung lượng lưu trữ cho ảnh/video

**Post-Condition:**
- Công thức mới được lưu vào cơ sở dữ liệu với trạng thái "Chờ duyệt"
- Admin nhận được thông báo có công thức mới cần duyệt
- Người dùng nhận được xác nhận đã gửi công thức thành công
- Thời gian tạo công thức được ghi lại

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn nút "Tạo công thức mới" từ menu hoặc trang "Công thức của tôi"
2. Hệ thống hiển thị form tạo công thức mới với các section:

   **Thông tin cơ bản:**
   - a. Tên món ăn (bắt buộc, tối đa 100 ký tự)
   - b. Mô tả ngắn về món ăn (tối đa 500 ký tự)
   - c. Ảnh đại diện món ăn (upload file, JPG/PNG, tối đa 5MB)
   - d. Danh mục món ăn (dropdown chọn từ danh sách có sẵn)

   **Thông tin nấu ăn:**
   - e. Thời gian chuẩn bị (phút)
   - f. Thời gian nấu (phút)
   - g. Độ khó (Dễ/Trung bình/Khó)
   - h. Khẩu phần ăn (số người)

   **Nguyên liệu:**
   - i. Danh sách nguyên liệu với tên, định lượng, đơn vị
   - j. Có thể thêm/bớt nguyên liệu bằng nút +/-

   **Các bước thực hiện:**
   - k. Danh sách các bước với mô tả chi tiết
   - l. Mỗi bước có thể kèm ảnh minh họa
   - m. Có thể sắp xếp lại thứ tự các bước

3. Người dùng điền đầy đủ thông tin bắt buộc:
   - a. Nhập tên món ăn
   - b. Viết mô tả ngắn
   - c. Upload ít nhất 1 ảnh đại diện
   - d. Chọn danh mục
   - e. Nhập thời gian nấu và độ khó
   - f. Thêm ít nhất 3 nguyên liệu
   - g. Viết ít nhất 3 bước thực hiện

4. Hệ thống hiển thị preview công thức để người dùng xem lại
5. Người dùng có thể chỉnh sửa thông tin trước khi gửi
6. Người dùng nhấn nút "Gửi duyệt" hoặc "Tạo công thức"
7. Hệ thống kiểm tra và xác thực tất cả thông tin đầu vào
8. Hệ thống upload ảnh lên server và lưu đường dẫn
9. Hệ thống lưu công thức vào cơ sở dữ liệu với:
   - a. Trạng thái "Chờ duyệt"
   - b. ID người tạo
   - c. Thời gian tạo
   - d. Tất cả thông tin đã nhập
10. Hệ thống gửi thông báo cho admin có công thức mới cần duyệt
11. Hệ thống hiển thị thông báo: "Công thức đã được gửi duyệt thành công! Bạn sẽ nhận được thông báo khi admin duyệt xong."

**Alternative Flow:**
- **Lưu nháp:**
  - Người dùng có thể nhấn "Lưu nháp" để lưu công thức chưa hoàn thành
  - Hệ thống lưu với trạng thái "Nháp" và có thể chỉnh sửa sau
  - Người dùng có thể tiếp tục chỉnh sửa từ trang "Công thức của tôi"

- **Import từ AI:**
  - Nếu người dùng đã tạo công thức bằng AI (UC2.3)
  - Có thể import thông tin từ công thức AI đã tạo
  - Hệ thống tự động điền các thông tin cơ bản

- **Copy từ công thức khác:**
  - Người dùng có thể copy một công thức công khai làm template
  - Hệ thống tự động điền thông tin và người dùng chỉnh sửa
  - Đảm bảo không vi phạm bản quyền

- **Tạo công thức từ "Tủ lạnh ảo":**
  - Người dùng có thể chọn nguyên liệu từ "Tủ lạnh ảo" (UC5)
  - Hệ thống tự động điền danh sách nguyên liệu

**Exception Flow:**
- **Thông tin bắt buộc thiếu:**
  - Nếu thiếu thông tin bắt buộc (tên món, nguyên liệu, các bước)
  - Hệ thống hiển thị thông báo: "Vui lòng điền đầy đủ thông tin bắt buộc: [danh sách trường thiếu]"
  - Highlight các trường thiếu và yêu cầu người dùng điền

- **Tên món ăn trùng lặp:**
  - Nếu tên món ăn đã tồn tại trong hệ thống
  - Hệ thống hiển thị cảnh báo: "Tên món ăn này đã tồn tại. Bạn có muốn sử dụng tên khác không?"
  - Gợi ý một số tên thay thế

- **Ảnh không hợp lệ:**
  - Nếu file ảnh quá lớn (>5MB) hoặc không đúng định dạng
  - Hệ thống hiển thị: "Ảnh phải có định dạng JPG/PNG và kích thước dưới 5MB."
  - Người dùng có thể chọn ảnh khác hoặc nén ảnh

- **Lỗi upload ảnh:**
  - Nếu không thể upload ảnh lên server
  - Hệ thống hiển thị: "Không thể upload ảnh. Vui lòng thử lại hoặc chọn ảnh khác."
  - Cung cấp nút "Thử lại upload"

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu công thức vào cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể lưu công thức. Vui lòng thử lại sau."
  - Không tạo công thức mới

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình tạo công thức
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."
  - Công thức nháp (nếu có) được lưu để tiếp tục sau

**Additional Information:**
- **Business Rule:**
  - Tên món ăn phải duy nhất trong hệ thống (có thể có số thứ tự nếu trùng)
  - Mô tả không được chứa nội dung spam hoặc không phù hợp
  - Ảnh phải có kích thước tối thiểu 300x300px và tối đa 5MB
  - Phải có ít nhất 3 nguyên liệu và 3 bước thực hiện
  - Công thức được lưu với trạng thái "Chờ duyệt" và chỉ admin mới thấy
  - Người tạo có thể chỉnh sửa công thức khi còn ở trạng thái "Chờ duyệt"
  - Mỗi người dùng có thể tạo tối đa 10 công thức/ngày để tránh spam

- **Non-Functional Requirement:**
  - Performance: Thời gian upload ảnh và lưu công thức phải dưới 10 giây
  - Usability: Form phải có validation real-time và gợi ý rõ ràng
  - Security: Ảnh upload phải được kiểm tra để đảm bảo không chứa mã độc
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi lưu công thức

**Priority:** Medium  
**CRUD:** Create


