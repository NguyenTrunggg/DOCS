# UCS04-6: Chia sẻ công thức [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng chia sẻ một công thức nấu ăn lên các nền tảng mạng xã hội, gửi qua email, hoặc sao chép link để chia sẻ với bạn bè và gia đình.

**Trigger:** Người dùng nhấp vào nút "Chia sẻ" hoặc biểu tượng chia sẻ trên trang chi tiết công thức (UC2.5) hoặc trong danh sách công thức.

**Pre-Condition:**
- Công thức tồn tại và có trạng thái "Đã duyệt"
- Hệ thống đang hoạt động bình thường
- Công thức có thể truy cập công khai

**Post-Condition:**
- Công thức được chia sẻ theo phương thức người dùng đã chọn
- Link chia sẻ được tạo (nếu cần)
- Thông báo xác nhận được hiển thị
- Thống kê chia sẻ của công thức được cập nhật

**Basic Flow:**
1. Người dùng đang xem trang chi tiết công thức (UC2.5) hoặc danh sách công thức
2. Người dùng nhấp vào nút "Chia sẻ" hoặc biểu tượng chia sẻ (📤)
3. Hệ thống hiển thị modal/popup chia sẻ với các tùy chọn:

   **Chia sẻ qua mạng xã hội:**
   - a. Facebook - Chia sẻ lên Facebook
   - b. Instagram - Chia sẻ story hoặc post
   - c. Twitter - Tweet công thức
   - d. Pinterest - Pin công thức
   - e. WhatsApp - Gửi qua WhatsApp
   - f. Telegram - Gửi qua Telegram

   **Chia sẻ qua email:**
   - g. Gmail - Mở Gmail với nội dung sẵn
   - h. Email khác - Sao chép nội dung để gửi email

   **Chia sẻ khác:**
   - i. Sao chép link - Copy URL công thức
   - j. QR Code - Tạo mã QR để chia sẻ
   - k. In công thức - In ra giấy

4. Người dùng chọn một phương thức chia sẻ
5. Hệ thống thực hiện hành động tương ứng:

   **Chia sẻ mạng xã hội:**
   - a. Tạo link chia sẻ với thông tin công thức
   - b. Mở cửa sổ popup của mạng xã hội
   - c. Tự động điền nội dung: tên món, mô tả, ảnh, link

   **Chia sẻ email:**
   - d. Mở ứng dụng email với subject và nội dung sẵn
   - e. Bao gồm link công thức và thông tin cơ bản

   **Sao chép link:**
   - f. Copy URL công thức vào clipboard
   - g. Hiển thị thông báo "Đã sao chép link"

   **QR Code:**
   - h. Tạo mã QR chứa link công thức
   - i. Hiển thị mã QR để người dùng screenshot

6. Hệ thống cập nhật thống kê chia sẻ của công thức
7. Hệ thống hiển thị thông báo xác nhận: "Đã chia sẻ '[tên công thức]' thành công!"
8. Modal chia sẻ tự động đóng sau 2 giây hoặc người dùng đóng thủ công

**Alternative Flow:**
- **Chia sẻ từ danh sách:**
  - Người dùng có thể chia sẻ từ danh sách công thức
  - Hiển thị modal chia sẻ tương tự

- **Chia sẻ nhiều công thức:**
  - Người dùng có thể chọn nhiều công thức và chia sẻ cùng lúc
  - Tạo link tổng hợp hoặc danh sách các công thức

- **Chia sẻ với nội dung tùy chỉnh:**
  - Người dùng có thể thêm tin nhắn cá nhân khi chia sẻ
  - Tùy chỉnh nội dung chia sẻ trước khi gửi

- **Chia sẻ qua ứng dụng di động:**
  - Trên mobile, có thể chia sẻ qua các ứng dụng đã cài đặt
  - Hiển thị danh sách ứng dụng có thể chia sẻ

- **Chia sẻ công thức từ "Yêu thích":**
  - Từ danh sách yêu thích, người dùng có thể chia sẻ nhiều công thức
  - Tạo collection công thức để chia sẻ

**Exception Flow:**
- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa hoặc không tồn tại
  - Hệ thống hiển thị: "Công thức không tồn tại. Không thể chia sẻ."
  - Đóng modal chia sẻ

- **Công thức chưa được duyệt:**
  - Nếu công thức chưa được duyệt
  - Hệ thống hiển thị: "Công thức chưa được duyệt. Chỉ có thể chia sẻ sau khi được duyệt."
  - Đóng modal chia sẻ

- **Lỗi tạo link chia sẻ:**
  - Nếu không thể tạo link chia sẻ
  - Hệ thống hiển thị: "Không thể tạo link chia sẻ. Vui lòng thử lại."
  - Cung cấp nút "Thử lại"

- **Lỗi mở mạng xã hội:**
  - Nếu không thể mở ứng dụng mạng xã hội
  - Hệ thống hiển thị: "Không thể mở [tên mạng xã hội]. Vui lòng kiểm tra ứng dụng."
  - Cung cấp tùy chọn "Sao chép link" thay thế

- **Lỗi sao chép link:**
  - Nếu không thể copy link vào clipboard
  - Hệ thống hiển thị: "Không thể sao chép link. Vui lòng copy thủ công: [link]"
  - Hiển thị link để người dùng copy thủ công

- **Lỗi tạo QR Code:**
  - Nếu không thể tạo mã QR
  - Hệ thống hiển thị: "Không thể tạo mã QR. Vui lòng sử dụng phương thức khác."
  - Ẩn tùy chọn QR Code

- **Lỗi cập nhật thống kê:**
  - Nếu không thể cập nhật số lượt chia sẻ
  - Hệ thống vẫn thực hiện chia sẻ nhưng không cập nhật thống kê
  - Hiển thị cảnh báo: "Chia sẻ thành công nhưng không thể cập nhật thống kê."

- **Mạng xã hội bị chặn:**
  - Nếu mạng xã hội bị chặn hoặc không khả dụng
  - Hệ thống hiển thị: "[Tên mạng xã hội] hiện không khả dụng."
  - Cung cấp tùy chọn chia sẻ khác

**Additional Information:**
- **Business Rule:**
  - Chỉ có thể chia sẻ công thức có trạng thái "Đã duyệt"
  - Link chia sẻ phải dẫn trực tiếp đến trang chi tiết công thức
  - Nội dung chia sẻ bao gồm: tên món, mô tả ngắn, ảnh đại diện, link
  - Thống kê chia sẻ được cập nhật real-time
  - Link chia sẻ có thời hạn vô thời hạn (không expire)
  - QR Code chứa link ngắn gọn để dễ scan
  - Có thể chia sẻ mà không cần đăng nhập

- **Non-Functional Requirement:**
  - Performance: Thời gian tạo link chia sẻ phải dưới 1 giây
  - Usability: Giao diện chia sẻ phải trực quan, dễ sử dụng
  - Security: Link chia sẻ phải an toàn, không chứa thông tin nhạy cảm
  - Reliability: Hệ thống phải hoạt động ổn định với nhiều yêu cầu chia sẻ đồng thời

**Priority:** Low  
**CRUD:** Read


