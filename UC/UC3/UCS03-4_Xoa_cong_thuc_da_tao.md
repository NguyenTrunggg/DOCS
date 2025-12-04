# UCS03-4: Xóa công thức đã tạo [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập xóa vĩnh viễn các công thức do mình tạo ra khỏi hệ thống, với các hạn chế khác nhau tùy theo trạng thái duyệt và mức độ tương tác của cộng đồng với công thức đó.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Xóa" trên một công thức trong danh sách "Công thức của tôi" (UC3.2) hoặc từ trang chi tiết công thức.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại và thuộc về người dùng hiện tại
- Công thức chưa bị admin khóa xóa
- Nếu công thức đã được duyệt và có tương tác cao, cần xác nhận đặc biệt

**Post-Condition:**
- Công thức được xóa vĩnh viễn khỏi cơ sở dữ liệu
- Tất cả dữ liệu liên quan (ảnh, đánh giá, bình luận) được xóa hoặc ẩn
- Thống kê của người dùng được cập nhật (giảm số công thức đã tạo)
- Admin nhận được thông báo về việc xóa công thức (nếu công thức đã được duyệt)

**Basic Flow:**
1. Người dùng đã đăng nhập truy cập trang "Công thức của tôi" (UC3.2)
2. Người dùng tìm và nhấn nút "Xóa" trên công thức muốn xóa
3. Hệ thống kiểm tra quyền xóa công thức:
   - a. Công thức thuộc về người dùng hiện tại
   - b. Công thức chưa bị admin khóa xóa
4. Hệ thống hiển thị hộp thoại xác nhận với thông tin:
   - a. Tên công thức sẽ bị xóa
   - b. Trạng thái hiện tại của công thức
   - c. Số lượt xem, đánh giá (nếu công thức đã được duyệt)
   - d. Cảnh báo về hậu quả của việc xóa
5. Hệ thống hiển thị các cảnh báo khác nhau tùy theo trạng thái:

   **Công thức chưa được duyệt (Chờ duyệt, Bị từ chối, Nháp):**
   - "Bạn có chắc chắn muốn xóa công thức '[tên món]'? Hành động này không thể hoàn tác."

   **Công thức đã được duyệt nhưng ít tương tác:**
   - "Công thức '[tên món]' đã được duyệt và có [số] lượt xem. Bạn có chắc chắn muốn xóa?"

   **Công thức có tương tác cao:**
   - "Công thức '[tên món]' đã được duyệt, có [số] lượt xem và [số] đánh giá. Việc xóa sẽ ảnh hưởng đến cộng đồng. Bạn có chắc chắn muốn xóa?"
   - Yêu cầu nhập lại tên công thức để xác nhận

6. Người dùng xác nhận việc xóa:
   - a. Nhấn "Xác nhận xóa" hoặc "Đồng ý"
   - b. Nếu có yêu cầu, nhập lại tên công thức
7. Hệ thống thực hiện xóa công thức:
   - a. Xóa thông tin công thức khỏi bảng chính
   - b. Xóa hoặc ẩn các đánh giá, bình luận liên quan
   - c. Xóa các file ảnh/video (hoặc đánh dấu để dọn dẹp sau)
   - d. Cập nhật thống kê của người dùng
8. Nếu công thức đã được duyệt:
   - a. Gửi thông báo cho admin về việc xóa
   - b. Ẩn công thức khỏi tìm kiếm công khai ngay lập tức
9. Hệ thống hiển thị thông báo: "Công thức '[tên món]' đã được xóa thành công."
10. Hệ thống chuyển hướng về trang "Công thức của tôi" với danh sách đã cập nhật

**Alternative Flow:**
- **Xóa từ trang chi tiết:**
  - Người dùng đang xem chi tiết công thức (UC2.5)
  - Nhấn nút "Xóa công thức" (chỉ hiển thị nếu có quyền)
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Xóa nhiều công thức cùng lúc:**
  - Người dùng có thể chọn nhiều công thức và xóa cùng lúc
  - Hệ thống hiển thị danh sách các công thức sẽ bị xóa
  - Yêu cầu xác nhận cho từng công thức hoặc xác nhận tổng thể

- **Xóa tạm thời (Soft Delete):**
  - Đối với công thức có tương tác cao, hệ thống có thể chỉ đánh dấu xóa
  - Công thức được ẩn khỏi tìm kiếm nhưng vẫn tồn tại trong CSDL
  - Có thể khôi phục trong vòng 30 ngày

**Exception Flow:**
- **Không có quyền xóa:**
  - Nếu công thức không thuộc về người dùng hoặc bị admin khóa
  - Hệ thống hiển thị thông báo: "Bạn không có quyền xóa công thức này."
  - Nút "Xóa" bị ẩn hoặc vô hiệu hóa

- **Người dùng hủy bỏ xóa:**
  - Trong hộp thoại xác nhận, người dùng nhấn "Hủy" hoặc "Đóng"
  - Hộp thoại đóng lại, không có hành động nào được thực hiện
  - Người dùng vẫn ở trang hiện tại

- **Nhập sai tên công thức:**
  - Nếu yêu cầu nhập lại tên công thức nhưng người dùng nhập sai
  - Hệ thống hiển thị: "Tên công thức không khớp. Vui lòng nhập lại chính xác."
  - Cho phép nhập lại hoặc hủy bỏ

- **Lỗi xóa cơ sở dữ liệu:**
  - Nếu có lỗi khi xóa dữ liệu khỏi cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể xóa công thức. Vui lòng thử lại sau."
  - Công thức vẫn tồn tại trong hệ thống

- **Lỗi xóa file ảnh:**
  - Nếu không thể xóa file ảnh/video khỏi server
  - Hệ thống vẫn xóa thông tin công thức nhưng ghi lại lỗi
  - Hiển thị thông báo: "Công thức đã được xóa nhưng có lỗi khi xóa file đính kèm."

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình xóa
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."
  - Không thực hiện xóa

- **Công thức đang được sử dụng:**
  - Nếu công thức đang được tham chiếu bởi các thành phần khác
  - Hệ thống hiển thị: "Công thức này đang được sử dụng. Không thể xóa."
  - Cung cấp thông tin về các thành phần đang sử dụng

**Additional Information:**
- **Business Rule:**
  - Chỉ có thể xóa công thức do chính mình tạo ra
  - Công thức ở trạng thái "Chờ duyệt", "Bị từ chối", "Nháp" có thể xóa tự do
  - Công thức "Đã duyệt" với tương tác thấp có thể xóa với xác nhận đơn giản
  - Công thức "Đã duyệt" với tương tác cao cần xác nhận đặc biệt (nhập lại tên)
  - Admin có thể khóa quyền xóa đối với công thức quan trọng
  - Việc xóa công thức đã duyệt phải được thông báo cho admin
  - Không thể xóa công thức đang trong quá trình kiểm duyệt

- **Non-Functional Requirement:**
  - Performance: Thời gian xóa công thức phải dưới 5 giây
  - Usability: Hộp thoại xác nhận phải rõ ràng về hậu quả của việc xóa
  - Security: Phải đảm bảo chỉ người tạo mới có thể xóa công thức của mình
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi xóa

**Priority:** Low  
**CRUD:** Delete


