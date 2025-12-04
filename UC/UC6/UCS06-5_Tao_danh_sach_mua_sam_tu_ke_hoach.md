# UCS06-5: Tạo danh sách mua sắm từ kế hoạch [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép hệ thống tự động tổng hợp danh sách nguyên liệu cần mua dựa trên các món ăn trong kế hoạch bữa ăn được chọn, gộp các nguyên liệu trùng và quy đổi đơn vị về chuẩn.

**Trigger:** Người dùng nhấn nút "Tạo danh sách mua sắm" khi đang xem kế hoạch bữa ăn.

**Pre-Condition:**
- Người dùng đã đăng nhập
- Có ít nhất một kế hoạch bữa ăn hợp lệ với các món và nguyên liệu
- Hệ thống có bảng quy đổi đơn vị chuẩn

**Post-Condition:**
- Danh sách mua sắm được tạo và hiển thị cho người dùng
- Người dùng có thể chỉnh sửa, in, xuất file, hoặc lưu danh sách

**Basic Flow:**
1. Người dùng mở kế hoạch bữa ăn (UC6.2)
2. Nhấn "Tạo danh sách mua sắm"
3. Hệ thống duyệt tất cả món trong khoảng thời gian của kế hoạch
4. Hệ thống tổng hợp toàn bộ nguyên liệu theo từng món
5. Hệ thống gộp các nguyên liệu trùng tên và quy đổi đơn vị về chuẩn (vd: g -> kg)
6. Hệ thống nhân định lượng theo khẩu phần đã thiết lập
7. Hệ thống loại bỏ các nguyên liệu đã có trong "Tủ lạnh ảo" (UC5) nếu người dùng chọn
8. Hệ thống hiển thị danh sách mua sắm theo nhóm danh mục (Rau củ, Thịt, Gia vị...)
9. Người dùng có thể chỉnh sửa số lượng, thêm/xóa mục
10. Người dùng có thể lưu danh sách mua sắm, in, hoặc xuất file (PDF/Excel)

**Alternative Flow:**
- **Bỏ qua nguyên liệu có sẵn:**
  - Tự động trừ đi số lượng nguyên liệu đã có trong "Tủ lạnh ảo"

- **Chọn phạm vi ngày:**
  - Tạo danh sách mua sắm chỉ cho một phạm vi ngày trong kế hoạch

- **Tách danh sách theo cửa hàng:**
  - Tách các mục theo nơi mua (siêu thị, chợ, cửa hàng gia vị)

**Exception Flow:**
- **Kế hoạch không có món/không có nguyên liệu:**
  - Hiển thị: "Không có dữ liệu để tạo danh sách mua sắm"

- **Lỗi quy đổi đơn vị:**
  - Một số nguyên liệu không quy đổi được
  - Hệ thống đánh dấu và yêu cầu người dùng nhập thủ công

- **Lỗi tạo danh sách:**
  - Nếu có lỗi hệ thống
  - Hiển thị: "Không thể tạo danh sách mua sắm. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Quy đổi đơn vị sử dụng bảng quy đổi chuẩn nội bộ
  - Gộp nguyên liệu theo tên chuẩn hoá (case-insensitive, bỏ dấu)
  - Có thể loại trừ các nguyên liệu nền (muối, đường) nếu bật tuỳ chọn
  - Lưu danh sách để tái sử dụng/đánh dấu đã mua

- **Non-Functional Requirement:**
  - Performance: Tạo danh sách cho 2 tuần < 3 giây
  - Usability: Cho phép chỉnh sửa inline, kéo/thả sắp xếp nhóm
  - Reliability: Kết quả ổn định và nhất quán sau khi chỉnh sửa

**Priority:** Medium  
**CRUD:** Create

