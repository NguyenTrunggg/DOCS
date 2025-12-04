# UCS04-4: Gửi đánh giá và bình luận [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập chấm điểm (từ 1-5 sao) và viết bình luận cho một công thức nấu ăn, giúp cộng đồng chia sẻ trải nghiệm và đánh giá chất lượng công thức.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Đánh giá" hoặc cuộn xuống phần đánh giá trên trang chi tiết công thức và nhấn "Viết đánh giá".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại và có trạng thái "Đã duyệt"
- Người dùng chưa đánh giá công thức này trước đó (hoặc muốn cập nhật đánh giá cũ)

**Post-Condition:**
- Đánh giá và bình luận được lưu vào cơ sở dữ liệu
- Điểm đánh giá trung bình của công thức được cập nhật
- Bình luận được hiển thị ngay lập tức trong danh sách
- Thông báo xác nhận được hiển thị cho người dùng

**Basic Flow:**
1. Người dùng đã đăng nhập đang xem trang chi tiết công thức (UC2.5)
2. Người dùng cuộn xuống phần "Đánh giá và Bình luận" hoặc nhấn nút "Đánh giá"
3. Hệ thống hiển thị form đánh giá với các trường:
   - a. Chọn số sao đánh giá (1-5 sao) với giao diện interactive
   - b. Ô nhập bình luận (tối đa 1000 ký tự)
   - c. Các tag đánh giá nhanh (VD: "Dễ làm", "Ngon", "Nguyên liệu khó tìm")
4. Người dùng thực hiện đánh giá:
   - a. Chọn số sao bằng cách click vào các ngôi sao
   - b. Viết bình luận chi tiết về trải nghiệm nấu ăn
   - c. Chọn các tag phù hợp (tùy chọn)
5. Hệ thống hiển thị preview đánh giá để người dùng xem lại
6. Người dùng nhấn nút "Gửi đánh giá" hoặc "Đăng bình luận"
7. Hệ thống kiểm tra và xác thực thông tin:
   - a. Đảm bảo đã chọn số sao (bắt buộc)
   - b. Kiểm tra nội dung bình luận không vi phạm quy định
   - c. Kiểm tra người dùng chưa đánh giá công thức này
8. Hệ thống lưu đánh giá và bình luận vào cơ sở dữ liệu:
   - a. Lưu điểm số (1-5)
   - b. Lưu nội dung bình luận
   - c. Lưu các tag đã chọn
   - d. Ghi lại thời gian đánh giá
9. Hệ thống cập nhật điểm đánh giá trung bình của công thức
10. Hệ thống hiển thị đánh giá vừa gửi ngay lập tức trong danh sách
11. Hệ thống hiển thị thông báo: "Cảm ơn bạn đã đánh giá! Đánh giá của bạn đã được đăng."

**Alternative Flow:**
- **Cập nhật đánh giá đã có:**
  - Nếu người dùng đã đánh giá công thức này trước đó
  - Hệ thống hiển thị đánh giá cũ và cho phép chỉnh sửa
  - Người dùng có thể thay đổi số sao và nội dung bình luận
  - Hệ thống cập nhật thay vì tạo mới

- **Đánh giá nhanh chỉ với sao:**
  - Người dùng có thể chỉ chọn số sao mà không viết bình luận
  - Hệ thống vẫn lưu đánh giá với bình luận trống
  - Điểm trung bình vẫn được cập nhật

- **Đánh giá với ảnh:**
  - Người dùng có thể đính kèm ảnh món ăn đã nấu
  - Hệ thống upload ảnh và lưu đường dẫn
  - Ảnh được hiển thị cùng với bình luận

**Exception Flow:**
- **Người dùng chưa đăng nhập:**
  - Nếu người dùng chưa đăng nhập và nhấn "Đánh giá"
  - Hệ thống hiển thị popup: "Vui lòng đăng nhập để đánh giá công thức"
  - Cung cấp nút "Đăng nhập" để chuyển đến trang đăng nhập

- **Chưa chọn số sao:**
  - Nếu người dùng nhấn "Gửi" mà chưa chọn số sao
  - Hệ thống hiển thị: "Vui lòng chọn số sao đánh giá (1-5 sao)"
  - Highlight phần chọn sao để người dùng chú ý

- **Nội dung bình luận vi phạm:**
  - Nếu bình luận chứa nội dung spam, lăng mạ, hoặc không phù hợp
  - Hệ thống hiển thị: "Nội dung bình luận không phù hợp. Vui lòng kiểm tra lại."
  - Highlight các từ ngữ có vấn đề

- **Đã đánh giá trước đó:**
  - Nếu người dùng đã đánh giá công thức này
  - Hệ thống hiển thị: "Bạn đã đánh giá công thức này. Bạn có muốn cập nhật đánh giá không?"
  - Cung cấp tùy chọn "Cập nhật đánh giá" hoặc "Hủy"

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu đánh giá vào cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể gửi đánh giá. Vui lòng thử lại sau."
  - Không lưu đánh giá

- **Lỗi upload ảnh:**
  - Nếu có lỗi khi upload ảnh đính kèm
  - Hệ thống vẫn lưu đánh giá nhưng không có ảnh
  - Hiển thị thông báo: "Đánh giá đã được gửi nhưng không thể upload ảnh."

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình viết đánh giá
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa trong quá trình viết đánh giá
  - Hệ thống hiển thị: "Công thức không còn tồn tại. Không thể đánh giá."
  - Chuyển hướng về trang trước

**Additional Information:**
- **Business Rule:**
  - Mỗi người dùng chỉ có thể đánh giá một công thức một lần
  - Điểm đánh giá từ 1-5 sao (bắt buộc)
  - Bình luận tối đa 1000 ký tự (tùy chọn)
  - Nội dung bình luận không được chứa spam, lăng mạ, hoặc nội dung không phù hợp
  - Đánh giá được kiểm duyệt tự động bằng AI hoặc manual
  - Điểm đánh giá trung bình được tính real-time
  - Người dùng có thể cập nhật đánh giá của mình bất cứ lúc nào

- **Non-Functional Requirement:**
  - Performance: Thời gian gửi đánh giá phải dưới 3 giây
  - Usability: Giao diện chọn sao phải trực quan, dễ sử dụng
  - Security: Nội dung bình luận phải được validate để tránh XSS
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi lưu đánh giá

**Priority:** Medium  
**CRUD:** Create


