# UCS03-3: Chỉnh sửa công thức đã tạo [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập chỉnh sửa lại thông tin của các công thức do mình tạo, với các hạn chế khác nhau tùy theo trạng thái duyệt của công thức (chỉ có thể sửa khi ở trạng thái "Chờ duyệt" hoặc "Bị từ chối").

**Trigger:** Người dùng đã đăng nhập nhấn nút "Sửa" hoặc "Chỉnh sửa" trên một công thức trong danh sách "Công thức của tôi" (UC3.2).

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại và thuộc về người dùng hiện tại
- Công thức có trạng thái "Chờ duyệt", "Bị từ chối" hoặc "Nháp"
- Công thức chưa được admin khóa chỉnh sửa

**Post-Condition:**
- Thông tin công thức được cập nhật trong cơ sở dữ liệu
- Nếu công thức từ "Bị từ chối" chuyển về "Chờ duyệt" để admin xem xét lại
- Thời gian chỉnh sửa cuối được ghi lại
- Lịch sử chỉnh sửa được lưu để audit (nếu có hệ thống version control)

**Basic Flow:**
1. Người dùng đã đăng nhập truy cập trang "Công thức của tôi" (UC3.2)
2. Người dùng tìm và nhấn nút "Sửa" trên công thức muốn chỉnh sửa
3. Hệ thống kiểm tra quyền chỉnh sửa công thức:
   - a. Công thức thuộc về người dùng hiện tại
   - b. Công thức có trạng thái cho phép chỉnh sửa
4. Hệ thống hiển thị form chỉnh sửa với tất cả thông tin hiện tại đã được điền sẵn:

   **Thông tin cơ bản:**
   - a. Tên món ăn (có thể chỉnh sửa)
   - b. Mô tả ngắn (có thể chỉnh sửa)
   - c. Ảnh đại diện hiện tại (có thể thay thế)
   - d. Danh mục món ăn (có thể thay đổi)

   **Thông tin nấu ăn:**
   - e. Thời gian chuẩn bị (có thể chỉnh sửa)
   - f. Thời gian nấu (có thể chỉnh sửa)
   - g. Độ khó (có thể thay đổi)
   - h. Khẩu phần ăn (có thể chỉnh sửa)

   **Nguyên liệu:**
   - i. Danh sách nguyên liệu hiện tại (có thể thêm/bớt/sửa)
   - j. Các nút thêm/xóa nguyên liệu

   **Các bước thực hiện:**
   - k. Danh sách các bước hiện tại (có thể chỉnh sửa/sắp xếp lại)
   - l. Có thể thêm/xóa/sửa từng bước
   - m. Có thể thay đổi ảnh minh họa cho từng bước

5. Người dùng chỉnh sửa các thông tin cần thiết:
   - a. Thay đổi tên, mô tả, thông tin nấu ăn
   - b. Thay thế ảnh đại diện (nếu cần)
   - c. Điều chỉnh danh sách nguyên liệu
   - d. Chỉnh sửa các bước thực hiện
6. Hệ thống hiển thị preview công thức đã chỉnh sửa
7. Người dùng xem lại và nhấn "Lưu thay đổi" hoặc "Cập nhật công thức"
8. Hệ thống kiểm tra và xác thực thông tin đầu vào (giống UC3.1)
9. Hệ thống cập nhật thông tin công thức trong cơ sở dữ liệu
10. Nếu công thức từ trạng thái "Bị từ chối":
    - a. Hệ thống chuyển trạng thái về "Chờ duyệt"
    - b. Gửi thông báo cho admin có công thức cần xem xét lại
11. Hệ thống cập nhật thời gian chỉnh sửa cuối
12. Hệ thống hiển thị thông báo: "Công thức đã được cập nhật thành công!"

**Alternative Flow:**
- **Chỉnh sửa từ trang chi tiết:**
  - Người dùng đang xem chi tiết công thức (UC2.5)
  - Nhấn nút "Chỉnh sửa" (chỉ hiển thị nếu có quyền)
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Lưu nháp trong quá trình chỉnh sửa:**
  - Người dùng có thể nhấn "Lưu nháp" để lưu thay đổi chưa hoàn thành
  - Hệ thống lưu với trạng thái "Nháp" và có thể tiếp tục chỉnh sửa sau

- **Hoàn nguyên thay đổi:**
  - Người dùng có thể nhấn "Hoàn nguyên" để quay về phiên bản trước
  - Hệ thống khôi phục thông tin gốc và hủy tất cả thay đổi

- **Chỉnh sửa một phần:**
  - Người dùng có thể chọn chỉnh sửa một section cụ thể (VD: chỉ nguyên liệu)
  - Hệ thống hiển thị form chỉnh sửa cho section đó

**Exception Flow:**
- **Không có quyền chỉnh sửa:**
  - Nếu công thức đã được duyệt hoặc không thuộc về người dùng
  - Hệ thống hiển thị thông báo: "Bạn không có quyền chỉnh sửa công thức này."
  - Chuyển hướng về trang danh sách công thức

- **Công thức đã bị khóa:**
  - Nếu admin đã khóa chỉnh sửa công thức này
  - Hệ thống hiển thị: "Công thức này đã bị khóa chỉnh sửa. Vui lòng liên hệ admin."
  - Không cho phép chỉnh sửa

- **Thông tin bắt buộc thiếu sau chỉnh sửa:**
  - Nếu sau khi chỉnh sửa, công thức thiếu thông tin bắt buộc
  - Hệ thống hiển thị: "Vui lòng điền đầy đủ thông tin bắt buộc: [danh sách trường thiếu]"
  - Highlight các trường thiếu

- **Lỗi upload ảnh mới:**
  - Nếu không thể upload ảnh thay thế
  - Hệ thống hiển thị: "Không thể upload ảnh mới. Ảnh cũ sẽ được giữ nguyên."
  - Tiếp tục lưu các thay đổi khác

- **Lỗi cập nhật cơ sở dữ liệu:**
  - Nếu không thể cập nhật thông tin vào cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể cập nhật công thức. Vui lòng thử lại sau."
  - Không lưu thay đổi

- **Session hết hạn trong quá trình chỉnh sửa:**
  - Nếu session hết hạn khi đang chỉnh sửa
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."
  - Các thay đổi chưa lưu sẽ bị mất

- **Xung đột chỉnh sửa:**
  - Nếu có người khác đang chỉnh sửa cùng lúc (trường hợp hiếm)
  - Hệ thống hiển thị: "Công thức đang được chỉnh sửa. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới" để xem phiên bản mới nhất

**Additional Information:**
- **Business Rule:**
  - Chỉ có thể chỉnh sửa công thức ở trạng thái "Chờ duyệt", "Bị từ chối", "Nháp"
  - Công thức "Đã duyệt" không thể chỉnh sửa để đảm bảo tính ổn định
  - Sau khi chỉnh sửa công thức "Bị từ chối", trạng thái chuyển về "Chờ duyệt"
  - Mỗi lần chỉnh sửa phải ghi lại thời gian và người chỉnh sửa
  - Thay đổi tên món ăn phải đảm bảo không trùng với công thức khác
  - Có thể chỉnh sửa nhiều lần cho đến khi được duyệt

- **Non-Functional Requirement:**
  - Performance: Thời gian tải form chỉnh sửa phải dưới 3 giây
  - Usability: Form phải giữ nguyên layout và validation như form tạo mới
  - Security: Chỉ cho phép chỉnh sửa công thức của chính người tạo
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi cập nhật

**Priority:** Medium  
**CRUD:** Update


