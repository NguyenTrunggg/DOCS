# UC Implementation Checklist

Tài liệu dùng để theo dõi tiến độ các chức năng @UC cho cả backend và frontend.
Tick vào từng đầu việc khi hoàn tất để đảm bảo đồng bộ giữa hai phía.

> Quy ước: `Backend` đề cập API, logic, dữ liệu; `Frontend` đề cập UI/UX và tích hợp client.

---

## UC1 – Người dùng cuối

### UCS01-1 Đăng ký tài khoản
- [ ] Backend – API đăng ký, xác thực OTP/email, lưu user trạng thái chờ _(thiếu luồng đăng ký bằng SĐT & OTP SMS theo UC1 verification 2025-11-20)_
- [ ] Frontend – Form đa kênh (email/SĐT/OAuth), hiển thị lỗi & success

### UCS01-2 Đăng nhập
- [x] Backend – Xác thực, session/token, giới hạn sai _(PASS – UC1 verification 2025-11-20)_
- [ ] Frontend – Form login, tùy chọn ghi nhớ, cảnh báo khóa

### UCS01-3 Đăng xuất
- [x] Backend – Thu hồi session/cookie, log hoạt động _(PASS – UC1 verification 2025-11-20)_
- [ ] Frontend – Menu/logout, confirm modal, xử lý hết hạn

### UCS01-4 Xem thông tin cá nhân
- [x] Backend – API profile + thống kê, bảo vệ quyền riêng tư _(PASS – UC1 verification 2025-11-20)_
- [ ] Frontend – Trang profile, avatar fallback, refresh state

### UCS01-5 Cập nhật thông tin cá nhân
- [x] Backend – Validate dữ liệu, upload ảnh, audit update _(PASS – UC1 verification 2025-11-20)_
- [ ] Frontend – Form edit inline, preview ảnh, toast kết quả

### UCS01-6 Đổi mật khẩu
- [x] Backend – Xác thực mật khẩu cũ, cập nhật bcrypt, log bảo mật _(PASS – UC1 verification 2025-11-20)_
- [ ] Frontend – Form đổi mật khẩu, kiểm tra độ mạnh, cảnh báo lỗi

### UCS01-7 Quên mật khẩu
- [x] Backend – Phát sinh token reset, gửi email/SMS, quản lý hạn _(PASS – UC1 verification 2025-11-20)_
- [ ] Frontend – Form yêu cầu/reset, hướng dẫn OTP/link

---

## UC2 – Khám phá & AI

### UCS02-1 Tìm kiếm theo nguyên liệu
- [x] Backend – Query theo danh sách nguyên liệu, ưu tiên phù hợp _(PASS – UC2 verification 2025-11-20)_
- [ ] Frontend – Multi-select nguyên liệu, hiển thị kết quả lọc

### UCS02-2 Tìm kiếm theo tên món
- [x] Backend – Full-text search, gợi ý autocomplete _(PASS – UC2 verification 2025-11-20)_
- [ ] Frontend – Ô tìm kiếm realtime, hiển thị gợi ý

### UCS02-3 Tạo công thức bằng AI
- [x] Backend – Tích hợp dịch vụ AI, lưu phiên bản draft _(PASS – UC2 verification 2025-11-20)_
- [ ] Frontend – Form prompt, hiển thị kết quả AI, cho phép chỉnh

### UCS02-4 Lọc & sắp xếp
- [x] Backend – Tham số hóa filter/sort, phân trang _(PASS – UC2 verification 2025-11-20)_
- [ ] Frontend – Bộ lọc đa tiêu chí, giữ trạng thái filter

### UCS02-5 Xem chi tiết công thức
- [x] Backend – Endpoint chi tiết + liên quan, tăng view count _(PASS – UC2 verification 2025-11-20)_
- [ ] Frontend – Trang chi tiết, hiển thị nguyên liệu/bước/rating

---

## UC3 – Quản lý công thức cá nhân

### UCS03-1 Thêm công thức mới
- [x] Backend – API tạo, upload media, chuẩn hóa dữ liệu _(PASS – UC3 verification 2025-11-20)_
- [ ] Frontend – Form nhiều bước, validator, chỉ báo tiến trình

### UCS03-2 Xem danh sách công thức đã tạo
- [x] Backend – Lấy danh sách theo user, filter trạng thái _(PASS – UC3 verification 2025-11-20)_
- [ ] Frontend – Trang bảng/thẻ, sort/paginate

### UCS03-3 Chỉnh sửa công thức đã tạo
- [x] Backend – Endpoint update, versioning lịch sử _(PASS – UC3 verification 2025-11-20)_
- [ ] Frontend – Prefill form, cảnh báo mất dữ liệu

### UCS03-4 Xóa công thức đã tạo
- [x] Backend – Soft/hard delete, quản lý tài nguyên liên quan _(PASS – UC3 verification 2025-11-20)_
- [ ] Frontend – Confirm modal, cập nhật danh sách realtime

---

## UC4 – Tương tác cộng đồng

### UCS04-1 Thêm công thức vào yêu thích
- [ ] Backend – Toggle favorite, ngăn trùng
- [ ] Frontend – Nút favorite, trạng thái đồng bộ

### UCS04-2 Gỡ công thức khỏi yêu thích
- [ ] Backend – API remove, cập nhật thống kê
- [ ] Frontend – UI cập nhật tức thì, undo toast

### UCS04-3 Xem danh sách yêu thích
- [ ] Backend – Endpoint danh sách, filter theo tag
- [ ] Frontend – Trang hiển thị, sort/paginate

### UCS04-4 Gửi đánh giá & bình luận
- [ ] Backend – Lưu review, chấm điểm, chống spam
- [ ] Frontend – Form đánh giá sao + bình luận, realtime append

### UCS04-5 Xem đánh giá & bình luận
- [ ] Backend – API phân trang, tổng điểm trung bình
- [ ] Frontend – Component review list, filter theo sao

### UCS04-6 Chia sẻ công thức
- [ ] Backend – Sinh link share/public token
- [ ] Frontend – UI copy link, social share intent

---

## UC5 – Tủ nguyên liệu cá nhân

### UCS05-1 Thêm nguyên liệu vào tủ
- [ ] Backend – CRUD nguyên liệu user, sync tồn kho
- [ ] Frontend – Form thêm nhanh, gợi ý autocomplete

### UCS05-2 Xem danh sách nguyên liệu trong tủ
- [ ] Backend – Endpoint danh sách + sắp xếp theo hạn dùng
- [ ] Frontend – View dạng bảng/thẻ, highlight sắp hết hạn

### UCS05-3 Cập nhật số lượng nguyên liệu
- [ ] Backend – API update số lượng/đơn vị, log thay đổi
- [ ] Frontend – Inline edit, spinner kiểm soát

### UCS05-4 Xóa nguyên liệu khỏi tủ
- [ ] Backend – Delete entry, đảm bảo tham chiếu kế hoạch
- [ ] Frontend – Confirm, cập nhật UI tức thì

---

## UC6 – Kế hoạch bữa ăn & mua sắm

### UCS06-1 Tạo kế hoạch bữa ăn
- [ ] Backend – Model kế hoạch/ngày/bữa, kiểm tra nguyên liệu
- [ ] Frontend – Calendar builder, drag-drop công thức

### UCS06-2 Xem kế hoạch bữa ăn
- [ ] Backend – Lấy kế hoạch theo tuần/tháng
- [ ] Frontend – View lịch, switch phạm vi

### UCS06-3 Chỉnh sửa kế hoạch bữa ăn
- [ ] Backend – Update chi tiết bữa, đồng bộ tủ nguyên liệu
- [ ] Frontend – UI chỉnh sửa trực quan, cảnh báo xung đột

### UCS06-4 Xóa kế hoạch bữa ăn
- [ ] Backend – Hủy kế hoạch + liên kết danh sách mua sắm
- [ ] Frontend – Confirm, refresh calendar

### UCS06-5 Tạo danh sách mua sắm từ kế hoạch
- [ ] Backend – Tổng hợp nguyên liệu thiếu, group theo danh mục
- [ ] Frontend – Danh sách tích chọn, xuất/chia sẻ

---

## UCA1 – Quản trị người dùng

### UCA01-1 Xem danh sách người dùng
- [ ] Backend – Endpoint filter, phân trang, role-based access
- [ ] Frontend – Bảng quản trị, bộ lọc trạng thái

### UCA01-2 Xem chi tiết người dùng
- [ ] Backend – API chi tiết + log hoạt động
- [ ] Frontend – Drawer/Modal chi tiết, load on demand

### UCA01-3 Vô hiệu hóa tài khoản người dùng
- [ ] Backend – Thay đổi trạng thái, ghi audit, revoke sessions
- [ ] Frontend – Action kèm confirm, hiển thị badge khóa

### UCA01-4 Kích hoạt lại tài khoản người dùng
- [ ] Backend – Restore trạng thái, gửi thông báo
- [ ] Frontend – Action hiển thị điều kiện bật lại

---

## UCA2 – Quản trị công thức hệ thống

### UCA02-1 Xem danh sách công thức hệ thống
- [ ] Backend – Endpoint với bộ lọc, thống kê trạng thái
- [ ] Frontend – Bảng quản trị, badge trạng thái

### UCA02-2 Thêm công thức hệ thống
- [ ] Backend – API create tiêu chuẩn, validate dinh dưỡng
- [ ] Frontend – Form biên tập chuyên sâu, upload media

### UCA02-3 Sửa công thức hệ thống
- [ ] Backend – Endpoint update, version history
- [ ] Frontend – Form edit, highlight thay đổi

### UCA02-4 Xóa công thức hệ thống
- [ ] Backend – Soft delete + logging
- [ ] Frontend – Confirm, filter hiển thị

### UCA02-5 Xem danh sách công thức chờ duyệt
- [ ] Backend – Endpoint filter pending, SLA timestamps
- [ ] Frontend – View hàng đợi, sort theo thời gian

### UCA02-6 Phê duyệt công thức
- [ ] Backend – Transition trạng thái, gửi thông báo tác giả
- [ ] Frontend – Action Approve, hiển thị ghi chú

### UCA02-7 Từ chối công thức
- [ ] Backend – Lưu lý do, trả công thức về tác giả
- [ ] Frontend – Modal nhập lý do, cập nhật danh sách

---

## UCA3 – Quản trị danh mục & nguyên liệu

### UCA03-1 Xem danh sách danh mục
- [ ] Backend – Endpoint categories + thống kê sử dụng
- [ ] Frontend – Trang danh mục, tìm kiếm

### UCA03-2 Thêm danh mục mới
- [ ] Backend – Validate tên, tránh trùng
- [ ] Frontend – Form thêm, cảnh báo trùng

### UCA03-3 Sửa tên danh mục
- [ ] Backend – Update + cascade cache
- [ ] Frontend – Inline edit, optimistic update

### UCA03-4 Xóa danh mục
- [ ] Backend – Kiểm tra ràng buộc công thức, soft delete
- [ ] Frontend – Confirm + cảnh báo ràng buộc

### UCA03-5 Xem danh sách nguyên liệu
- [ ] Backend – Endpoint + filter theo loại
- [ ] Frontend – Bảng nguyên liệu, tìm kiếm

### UCA03-6 Thêm nguyên liệu mới
- [ ] Backend – API create, mapping đơn vị
- [ ] Frontend – Form thêm với unit selector

### UCA03-7 Sửa thông tin nguyên liệu
- [ ] Backend – Update + lịch sử thay đổi
- [ ] Frontend – Form edit, validation cập nhật

### UCA03-8 Xóa nguyên liệu
- [ ] Backend – Kiểm tra ràng buộc công thức/tủ, soft delete
- [ ] Frontend – Confirm, báo các liên kết bị ảnh hưởng

---

## UCA4 – Quản trị admin

### UCA04-1 Xem danh sách tài khoản Admin
- [ ] Backend – Endpoint kèm role, bảo vệ truy cập
- [ ] Frontend – Bảng admin, lọc theo vai trò

### UCA04-2 Tạo tài khoản Admin mới
- [ ] Backend – Tạo user + phân quyền mặc định, gửi lời mời
- [ ] Frontend – Form tạo, gợi ý role

### UCA04-3 Phân quyền cho tài khoản Admin
- [ ] Backend – Module RBAC, lưu ma trận quyền
- [ ] Frontend – UI chọn quyền (checkbox tree)

### UCA04-4 Xóa tài khoản Admin
- [ ] Backend – Revoke quyền, bảo toàn audit
- [ ] Frontend – Confirm nâng cao (nhập “DELETE”), cập nhật bảng

---

## UCA5 – Báo cáo & thống kê

### UCA05-1 Xem báo cáo, thống kê
- [ ] Backend – Tổng hợp dữ liệu, cache KPI, endpoint filter thời gian
- [ ] Frontend – Dashboard biểu đồ, bộ lọc theo kỳ

---

> Gợi ý: Đồng bộ file này với tool quản lý công việc (Notion/Jira) bằng cách liên kết task/PR tương ứng cho từng checkbox để dễ tracking tiến độ.

