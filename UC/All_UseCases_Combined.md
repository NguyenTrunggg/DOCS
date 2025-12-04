# UCS01-1: Đăng ký tài khoản [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng mới tạo một tài khoản mới trong hệ thống quản lý công thức nấu ăn bằng cách sử dụng email hoặc số điện thoại cùng với mật khẩu bảo mật.

**Trigger:** Người dùng truy cập trang đăng ký và nhấn nút "Đăng ký tài khoản".

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Người dùng chưa có tài khoản trong hệ thống
- Email/số điện thoại chưa được sử dụng bởi tài khoản khác

**Post-Condition:**
- Tài khoản mới được tạo thành công trong cơ sở dữ liệu
- Người dùng nhận được email xác thực (nếu đăng ký bằng email)
- Trạng thái tài khoản là "Chờ xác thực" cho đến khi xác thực email/OTP
- Hệ thống tự động chuyển hướng đến trang đăng nhập

**Basic Flow:**
1. Người dùng truy cập trang đăng ký từ trang chủ hoặc trang đăng nhập
2. Người dùng chọn phương thức đăng ký (Email hoặc Số điện thoại)
3. Người dùng nhập thông tin đăng ký:
   - a. Email (nếu chọn đăng ký bằng email)
   - b. Số điện thoại (nếu chọn đăng ký bằng SĐT)
   - c. Mật khẩu (tối thiểu 8 ký tự, bao gồm chữ hoa, chữ thường, số)
   - d. Xác nhận mật khẩu
   - e. Họ và tên hiển thị
4. Người dùng tích chọn đồng ý với điều khoản sử dụng
5. Người dùng nhấn nút "Đăng ký"
6. Hệ thống kiểm tra và xác thực thông tin đầu vào
7. Hệ thống tạo tài khoản mới trong cơ sở dữ liệu với trạng thái "Chờ xác thực"
8. Nếu đăng ký bằng email: Hệ thống gửi email xác thực chứa link kích hoạt
9. Nếu đăng ký bằng SĐT: Hệ thống gửi mã OTP qua SMS
10. Hệ thống hiển thị thông báo thành công và chuyển hướng đến trang đăng nhập

**Alternative Flow:**
- **Đăng ký bằng Google/Facebook:**
  - Người dùng chọn "Đăng ký bằng Google" hoặc "Đăng ký bằng Facebook"
  - Hệ thống chuyển hướng đến trang xác thực của bên thứ ba
  - Sau khi xác thực thành công, hệ thống nhận thông tin cơ bản
  - Hệ thống tự động tạo tài khoản với trạng thái "Đã xác thực"
  - Người dùng được đăng nhập tự động

**Exception Flow:**
- **Email/SĐT đã tồn tại:**
  - Hệ thống hiển thị thông báo lỗi: "Email/Số điện thoại này đã được sử dụng. Vui lòng chọn email/SĐT khác hoặc đăng nhập nếu đã có tài khoản."
  - Người dùng có thể quay lại form để nhập thông tin mới

- **Mật khẩu không đủ mạnh:**
  - Hệ thống hiển thị thông báo: "Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ hoa, chữ thường và số."
  - Người dùng có thể chỉnh sửa mật khẩu

- **Lỗi gửi email/SMS:**
  - Hệ thống hiển thị thông báo: "Không thể gửi email/SMS xác thực. Vui lòng thử lại sau."
  - Tài khoản vẫn được tạo nhưng ở trạng thái "Chưa xác thực"
  - Người dùng có thể yêu cầu gửi lại email/SMS xác thực

- **Lỗi hệ thống:**
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Không tạo tài khoản mới

**Additional Information:**
- **Business Rule:**
  - Email phải có định dạng hợp lệ và chưa được sử dụng trong hệ thống
  - Số điện thoại phải có 10-11 chữ số và chưa được sử dụng
  - Mật khẩu phải có ít nhất 8 ký tự, bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số
  - Tên hiển thị phải có ít nhất 2 ký tự và không chứa ký tự đặc biệt
  - Người dùng phải đồng ý với điều khoản sử dụng mới có thể đăng ký
  - Email/SMS xác thực có hiệu lực trong 24 giờ

- **Non-Functional Requirement:**
  - Performance: Thời gian tạo tài khoản phải dưới 3 giây
  - Usability: Form đăng ký phải thân thiện, có gợi ý rõ ràng cho từng trường
  - Security: Mật khẩu phải được mã hóa bằng bcrypt trước khi lưu vào CSDL
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi tạo tài khoản

**Priority:** Low  
**CRUD:** Create

# UCS01-2: Đăng nhập vào hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng sử dụng thông tin tài khoản đã đăng ký (email/số điện thoại và mật khẩu) để truy cập vào hệ thống quản lý công thức nấu ăn.

**Trigger:** Người dùng truy cập trang đăng nhập và nhấn nút "Đăng nhập".

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Người dùng đã có tài khoản hợp lệ trong hệ thống
- Tài khoản không bị khóa hoặc vô hiệu hóa
- Tài khoản đã được xác thực (nếu đăng ký bằng email/SMS)

**Post-Condition:**
- Người dùng được xác thực thành công
- Session được tạo và lưu trữ
- Người dùng được chuyển hướng đến trang chủ hoặc trang được yêu cầu trước đó
- Thời gian đăng nhập cuối được cập nhật trong cơ sở dữ liệu

**Basic Flow:**
1. Người dùng truy cập trang đăng nhập từ trang chủ hoặc được chuyển hướng từ trang cần xác thực
2. Người dùng nhập thông tin đăng nhập:
   - a. Email hoặc số điện thoại
   - b. Mật khẩu
3. Người dùng có thể chọn "Ghi nhớ đăng nhập" (tùy chọn)
4. Người dùng nhấn nút "Đăng nhập"
5. Hệ thống kiểm tra và xác thực thông tin đăng nhập
6. Hệ thống kiểm tra trạng thái tài khoản (không bị khóa, đã xác thực)
7. Hệ thống tạo session mới và lưu vào cơ sở dữ liệu
8. Hệ thống cập nhật thời gian đăng nhập cuối của người dùng
9. Nếu có "Ghi nhớ đăng nhập": Hệ thống tạo cookie với thời hạn dài hơn
10. Hệ thống chuyển hướng người dùng đến trang chủ hoặc trang được yêu cầu trước đó

**Alternative Flow:**
- **Đăng nhập bằng Google/Facebook:**
  - Người dùng chọn "Đăng nhập bằng Google" hoặc "Đăng nhập bằng Facebook"
  - Hệ thống chuyển hướng đến trang xác thực của bên thứ ba
  - Sau khi xác thực thành công, hệ thống kiểm tra email trong cơ sở dữ liệu
  - Nếu tài khoản tồn tại: Tạo session và đăng nhập tự động
  - Nếu tài khoản không tồn tại: Chuyển hướng đến trang đăng ký với thông tin từ bên thứ ba

- **Quên mật khẩu:**
  - Người dùng nhấn liên kết "Quên mật khẩu?"
  - Hệ thống chuyển hướng đến trang đặt lại mật khẩu (UC1.7)

**Exception Flow:**
- **Thông tin đăng nhập không chính xác:**
  - Hệ thống hiển thị thông báo: "Email/số điện thoại hoặc mật khẩu không chính xác. Vui lòng kiểm tra lại."
  - Người dùng có thể nhập lại thông tin

- **Tài khoản chưa được xác thực:**
  - Hệ thống hiển thị thông báo: "Tài khoản chưa được xác thực. Vui lòng kiểm tra email/SMS để xác thực tài khoản."
  - Cung cấp tùy chọn "Gửi lại email/SMS xác thực"

- **Tài khoản bị khóa:**
  - Hệ thống hiển thị thông báo: "Tài khoản đã bị khóa. Vui lòng liên hệ quản trị viên để được hỗ trợ."
  - Cung cấp thông tin liên hệ hỗ trợ

- **Quá nhiều lần đăng nhập sai:**
  - Sau 5 lần đăng nhập sai liên tiếp, tài khoản bị tạm khóa 15 phút
  - Hệ thống hiển thị thông báo: "Tài khoản đã bị tạm khóa do quá nhiều lần đăng nhập sai. Vui lòng thử lại sau 15 phút."

- **Lỗi hệ thống:**
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Không tạo session mới

**Additional Information:**
- **Business Rule:**
  - Mật khẩu phải khớp chính xác với mật khẩu đã lưu (đã được mã hóa)
  - Session có thời hạn mặc định 24 giờ
  - Nếu chọn "Ghi nhớ đăng nhập", session có thời hạn 30 ngày
  - Tối đa 5 lần đăng nhập sai trước khi tài khoản bị tạm khóa
  - Thời gian tạm khóa là 15 phút, sau đó tự động mở khóa
  - Mỗi tài khoản chỉ có thể có 1 session hoạt động tại một thời điểm

- **Non-Functional Requirement:**
  - Performance: Thời gian xác thực và đăng nhập phải dưới 2 giây
  - Usability: Giao diện đăng nhập phải rõ ràng, có thể nhập bằng phím Enter
  - Security: Session phải được mã hóa và bảo mật, sử dụng HTTPS
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn của session

**Priority:** Low  
**CRUD:** Read

# UCS01-3: Đăng xuất khỏi hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập thoát khỏi phiên đăng nhập hiện tại một cách an toàn, đảm bảo thông tin cá nhân và dữ liệu được bảo vệ.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Đăng xuất" hoặc chọn "Đăng xuất" từ menu người dùng.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng đang ở bất kỳ trang nào trong hệ thống (trừ trang đăng nhập)

**Post-Condition:**
- Session của người dùng được hủy bỏ hoàn toàn
- Tất cả cookie liên quan đến phiên đăng nhập được xóa
- Người dùng được chuyển hướng đến trang đăng nhập hoặc trang chủ
- Thông tin đăng nhập được xóa khỏi bộ nhớ trình duyệt (nếu có)
- Thời gian đăng xuất được ghi lại trong log hệ thống

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn vào avatar/tên người dùng ở góc trên phải màn hình
2. Menu dropdown xuất hiện với các tùy chọn cá nhân
3. Người dùng chọn "Đăng xuất" từ menu
4. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn đăng xuất?"
5. Người dùng nhấn "Xác nhận" trong hộp thoại
6. Hệ thống gửi yêu cầu đăng xuất đến server
7. Server xóa session khỏi cơ sở dữ liệu
8. Server gửi lệnh xóa cookie về trình duyệt
9. Hệ thống ghi lại thời gian đăng xuất vào log
10. Người dùng được chuyển hướng đến trang đăng nhập với thông báo "Đã đăng xuất thành công"

**Alternative Flow:**
- **Đăng xuất từ menu hamburger (mobile):**
  - Trên thiết bị di động, người dùng nhấn menu hamburger
  - Chọn "Đăng xuất" từ menu mobile
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Đăng xuất tự động do hết hạn session:**
  - Khi session hết hạn, hệ thống tự động đăng xuất người dùng
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."
  - Chuyển hướng đến trang đăng nhập

**Exception Flow:**
- **Người dùng hủy bỏ đăng xuất:**
  - Trong hộp thoại xác nhận, người dùng nhấn "Hủy"
  - Hộp thoại đóng lại, người dùng vẫn ở trang hiện tại
  - Session tiếp tục hoạt động bình thường

- **Lỗi khi hủy session:**
  - Nếu server không thể xóa session do lỗi kết nối
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi khi đăng xuất. Vui lòng thử lại."
  - Người dùng vẫn có thể thử đăng xuất lại

- **Mất kết nối mạng:**
  - Nếu mất kết nối trong quá trình đăng xuất
  - Hệ thống hiển thị thông báo: "Mất kết nối. Phiên đăng nhập sẽ được hủy khi khôi phục kết nối."
  - Chuyển hướng đến trang đăng nhập

**Additional Information:**
- **Business Rule:**
  - Mỗi lần đăng xuất phải xóa hoàn toàn session và cookie
  - Thời gian đăng xuất phải được ghi lại để audit
  - Sau khi đăng xuất, người dùng không thể truy cập các trang yêu cầu xác thực
  - Nếu có nhiều tab đang mở cùng lúc, tất cả tab sẽ được đăng xuất
  - Đăng xuất không ảnh hưởng đến dữ liệu đã lưu của người dùng

- **Non-Functional Requirement:**
  - Performance: Thời gian đăng xuất phải dưới 1 giây
  - Usability: Quá trình đăng xuất phải đơn giản, chỉ cần 1-2 click
  - Security: Đảm bảo session được xóa hoàn toàn, không để lại thông tin nhạy cảm
  - Reliability: Hệ thống phải đảm bảo đăng xuất thành công ngay cả khi có lỗi nhỏ

**Priority:** Low  
**CRUD:** N/A

# UCS01-4: Xem thông tin cá nhân [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập xem các thông tin cá nhân của mình như tên hiển thị, email, ảnh đại diện và các thông tin tài khoản khác.

**Trigger:** Người dùng đã đăng nhập nhấn vào avatar/tên người dùng và chọn "Thông tin cá nhân" hoặc "Profile".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Thông tin cá nhân của người dùng tồn tại trong cơ sở dữ liệu

**Post-Condition:**
- Thông tin cá nhân của người dùng được hiển thị đầy đủ trên màn hình
- Người dùng có thể thấy trạng thái tài khoản và các thông tin liên quan
- Không có thay đổi nào được thực hiện đối với dữ liệu

**Basic Flow:**
1. Người dùng đã đăng nhập nhấn vào avatar/tên người dùng ở góc trên phải màn hình
2. Menu dropdown xuất hiện với các tùy chọn cá nhân
3. Người dùng chọn "Thông tin cá nhân" hoặc "Profile"
4. Hệ thống truy vấn cơ sở dữ liệu để lấy thông tin cá nhân của người dùng
5. Hệ thống hiển thị trang thông tin cá nhân với các thông tin sau:
   - a. Ảnh đại diện (avatar)
   - b. Họ và tên hiển thị
   - c. Email đăng ký
   - d. Số điện thoại (nếu có)
   - e. Ngày đăng ký tài khoản
   - f. Lần đăng nhập cuối
   - g. Trạng thái tài khoản (Đã xác thực/Chờ xác thực)
6. Hệ thống hiển thị thống kê hoạt động cơ bản:
   - a. Số công thức đã đăng
   - b. Số công thức đã yêu thích
   - c. Số bình luận đã viết
7. Người dùng có thể xem thông tin và quyết định có muốn chỉnh sửa hay không

**Alternative Flow:**
- **Truy cập từ trang cài đặt:**
  - Người dùng vào trang "Cài đặt" trước
  - Chọn tab "Thông tin cá nhân"
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Truy cập từ liên kết trực tiếp:**
  - Người dùng có thể bookmark hoặc truy cập trực tiếp URL profile
  - Hệ thống kiểm tra quyền truy cập và hiển thị thông tin

**Exception Flow:**
- **Không tìm thấy thông tin người dùng:**
  - Nếu thông tin người dùng bị thiếu hoặc không tồn tại
  - Hệ thống hiển thị thông báo: "Không thể tải thông tin cá nhân. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới" để thử lại

- **Lỗi tải ảnh đại diện:**
  - Nếu ảnh đại diện không tải được
  - Hệ thống hiển thị ảnh mặc định (avatar mặc định)
  - Không ảnh hưởng đến việc hiển thị các thông tin khác

- **Session hết hạn:**
  - Nếu session hết hạn trong khi xem thông tin
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Lỗi hệ thống:**
  - Nếu có lỗi server khi truy vấn thông tin
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Cung cấp nút "Quay lại" để trở về trang trước

**Additional Information:**
- **Business Rule:**
  - Chỉ người dùng đã đăng nhập mới có thể xem thông tin cá nhân của mình
  - Thông tin email và số điện thoại được hiển thị đầy đủ để người dùng xác nhận
  - Ảnh đại diện được hiển thị với kích thước chuẩn (150x150px)
  - Thống kê hoạt động được cập nhật theo thời gian thực
  - Thông tin nhạy cảm như mật khẩu không bao giờ được hiển thị

- **Non-Functional Requirement:**
  - Performance: Thời gian tải trang thông tin cá nhân phải dưới 2 giây
  - Usability: Giao diện phải rõ ràng, dễ đọc, có thể phân biệt rõ các loại thông tin
  - Security: Thông tin cá nhân chỉ được hiển thị cho chính chủ sở hữu
  - Reliability: Hệ thống phải đảm bảo tính chính xác của thông tin hiển thị

**Priority:** Low  
**CRUD:** Read

# UCS01-5: Cập nhật thông tin cá nhân [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập thay đổi các thông tin cá nhân của mình như tên hiển thị và ảnh đại diện, đảm bảo thông tin luôn được cập nhật và chính xác.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Chỉnh sửa" hoặc "Cập nhật thông tin" trên trang thông tin cá nhân.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng đang ở trang thông tin cá nhân hoặc trang cài đặt
- Tài khoản người dùng không bị khóa

**Post-Condition:**
- Thông tin cá nhân được cập nhật thành công trong cơ sở dữ liệu
- Giao diện hiển thị thông tin mới ngay lập tức
- Thời gian cập nhật cuối được ghi lại
- Thông báo thành công được hiển thị cho người dùng

**Basic Flow:**
1. Người dùng đã đăng nhập truy cập trang thông tin cá nhân (UC1.4)
2. Người dùng nhấn nút "Chỉnh sửa" hoặc "Cập nhật thông tin"
3. Hệ thống chuyển sang chế độ chỉnh sửa, các trường thông tin trở thành có thể chỉnh sửa:
   - a. Họ và tên hiển thị (text input)
   - b. Ảnh đại diện (file upload)
4. Người dùng chỉnh sửa thông tin:
   - a. Nhập tên hiển thị mới (nếu muốn thay đổi)
   - b. Chọn ảnh mới từ thiết bị (nếu muốn thay đổi)
5. Hệ thống hiển thị preview ảnh mới (nếu có)
6. Người dùng nhấn nút "Lưu thay đổi"
7. Hệ thống kiểm tra và xác thực thông tin đầu vào
8. Hệ thống upload ảnh mới lên server (nếu có)
9. Hệ thống cập nhật thông tin trong cơ sở dữ liệu
10. Hệ thống cập nhật thời gian chỉnh sửa cuối
11. Hệ thống hiển thị thông báo "Cập nhật thông tin thành công!"
12. Trang tự động chuyển về chế độ xem (không chỉnh sửa)

**Alternative Flow:**
- **Chỉ cập nhật ảnh đại diện:**
  - Người dùng chỉ thay đổi ảnh đại diện, không thay đổi tên
  - Hệ thống chỉ xử lý upload ảnh và cập nhật URL ảnh mới
  - Tiếp tục luồng từ bước 9 của Basic Flow

- **Chỉ cập nhật tên hiển thị:**
  - Người dùng chỉ thay đổi tên hiển thị, không thay đổi ảnh
  - Hệ thống chỉ cập nhật trường tên trong cơ sở dữ liệu
  - Tiếp tục luồng từ bước 9 của Basic Flow

- **Hủy bỏ thay đổi:**
  - Người dùng nhấn nút "Hủy" trong khi đang chỉnh sửa
  - Hệ thống hủy bỏ tất cả thay đổi và quay về trạng thái ban đầu
  - Không cập nhật gì vào cơ sở dữ liệu

**Exception Flow:**
- **Tên hiển thị không hợp lệ:**
  - Nếu tên hiển thị quá ngắn (< 2 ký tự) hoặc chứa ký tự đặc biệt không được phép
  - Hệ thống hiển thị thông báo lỗi: "Tên hiển thị phải có ít nhất 2 ký tự và không chứa ký tự đặc biệt."
  - Người dùng có thể chỉnh sửa lại tên

- **Ảnh không hợp lệ:**
  - Nếu file ảnh quá lớn (> 5MB) hoặc không đúng định dạng (không phải JPG/PNG)
  - Hệ thống hiển thị thông báo: "Ảnh phải có định dạng JPG hoặc PNG và kích thước dưới 5MB."
  - Người dùng có thể chọn ảnh khác

- **Lỗi upload ảnh:**
  - Nếu quá trình upload ảnh bị lỗi do mạng hoặc server
  - Hệ thống hiển thị thông báo: "Không thể upload ảnh. Vui lòng thử lại."
  - Người dùng có thể thử upload lại hoặc bỏ qua việc thay đổi ảnh

- **Lỗi cập nhật cơ sở dữ liệu:**
  - Nếu không thể cập nhật thông tin vào cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Không thể cập nhật thông tin. Vui lòng thử lại sau."
  - Thông tin vẫn giữ nguyên trạng thái cũ

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình chỉnh sửa
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

**Additional Information:**
- **Business Rule:**
  - Tên hiển thị phải có ít nhất 2 ký tự và không được để trống
  - Tên hiển thị không được chứa ký tự đặc biệt (@, #, $, %, &, *)
  - Ảnh đại diện phải có định dạng JPG hoặc PNG
  - Kích thước ảnh tối đa 5MB
  - Hệ thống tự động resize ảnh về kích thước chuẩn (150x150px)
  - Email và số điện thoại không thể thay đổi từ trang này (cần có use case riêng)
  - Mỗi lần cập nhật phải ghi lại thời gian thay đổi

- **Non-Functional Requirement:**
  - Performance: Thời gian cập nhật thông tin phải dưới 3 giây
  - Usability: Giao diện chỉnh sửa phải trực quan, có preview ảnh
  - Security: Ảnh upload phải được kiểm tra để đảm bảo không chứa mã độc
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi cập nhật

**Priority:** Medium  
**CRUD:** Update

# UCS01-6: Đổi mật khẩu [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập thay đổi mật khẩu hiện tại của tài khoản bằng cách xác thực mật khẩu cũ và nhập mật khẩu mới đảm bảo bảo mật.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Đổi mật khẩu" trên trang thông tin cá nhân hoặc trang cài đặt bảo mật.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Người dùng biết mật khẩu hiện tại của tài khoản
- Tài khoản người dùng không bị khóa

**Post-Condition:**
- Mật khẩu mới được lưu vào cơ sở dữ liệu (đã được mã hóa)
- Tất cả session hiện tại của người dùng bị vô hiệu hóa
- Người dùng cần đăng nhập lại với mật khẩu mới
- Thời gian thay đổi mật khẩu được ghi lại trong log bảo mật

**Basic Flow:**
1. Người dùng đã đăng nhập truy cập trang thông tin cá nhân hoặc trang "Cài đặt bảo mật"
2. Người dùng nhấn nút "Đổi mật khẩu" hoặc chọn tab "Bảo mật"
3. Hệ thống hiển thị form đổi mật khẩu với các trường:
   - a. Mật khẩu hiện tại (password field)
   - b. Mật khẩu mới (password field)
   - c. Xác nhận mật khẩu mới (password field)
4. Người dùng nhập thông tin:
   - a. Nhập mật khẩu hiện tại
   - b. Nhập mật khẩu mới (tối thiểu 8 ký tự, bao gồm chữ hoa, chữ thường, số)
   - c. Nhập lại mật khẩu mới để xác nhận
5. Người dùng nhấn nút "Đổi mật khẩu"
6. Hệ thống xác thực mật khẩu hiện tại
7. Hệ thống kiểm tra tính hợp lệ của mật khẩu mới
8. Hệ thống kiểm tra mật khẩu mới và xác nhận có khớp nhau
9. Hệ thống mã hóa mật khẩu mới bằng thuật toán bcrypt
10. Hệ thống cập nhật mật khẩu mới vào cơ sở dữ liệu
11. Hệ thống vô hiệu hóa tất cả session hiện tại của người dùng
12. Hệ thống ghi lại thời gian thay đổi mật khẩu vào log bảo mật
13. Hệ thống hiển thị thông báo: "Đổi mật khẩu thành công! Vui lòng đăng nhập lại."
14. Hệ thống tự động chuyển hướng đến trang đăng nhập

**Alternative Flow:**
- **Truy cập từ email thông báo bảo mật:**
  - Người dùng nhận email cảnh báo bảo mật và click link "Đổi mật khẩu"
  - Hệ thống chuyển hướng trực tiếp đến form đổi mật khẩu
  - Tiếp tục luồng từ bước 3 của Basic Flow

- **Đổi mật khẩu từ trang quên mật khẩu:**
  - Nếu người dùng đang trong quá trình reset mật khẩu
  - Form sẽ không yêu cầu mật khẩu hiện tại
  - Tiếp tục luồng từ bước 6 của Basic Flow (bỏ qua xác thực mật khẩu cũ)

**Exception Flow:**
- **Mật khẩu hiện tại không chính xác:**
  - Nếu mật khẩu hiện tại không khớp với mật khẩu trong cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Mật khẩu hiện tại không chính xác. Vui lòng kiểm tra lại."
  - Người dùng có thể nhập lại mật khẩu hiện tại

- **Mật khẩu mới không đủ mạnh:**
  - Nếu mật khẩu mới không đáp ứng yêu cầu bảo mật
  - Hệ thống hiển thị thông báo: "Mật khẩu phải có ít nhất 8 ký tự, bao gồm chữ hoa, chữ thường và số."
  - Người dùng có thể chỉnh sửa mật khẩu mới

- **Mật khẩu xác nhận không khớp:**
  - Nếu mật khẩu mới và xác nhận không giống nhau
  - Hệ thống hiển thị thông báo: "Mật khẩu xác nhận không khớp. Vui lòng nhập lại."
  - Người dùng có thể chỉnh sửa trường xác nhận

- **Mật khẩu mới giống mật khẩu cũ:**
  - Nếu mật khẩu mới trùng với mật khẩu hiện tại
  - Hệ thống hiển thị thông báo: "Mật khẩu mới phải khác với mật khẩu hiện tại."
  - Người dùng cần chọn mật khẩu khác

- **Lỗi cập nhật cơ sở dữ liệu:**
  - Nếu không thể cập nhật mật khẩu mới vào cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Không thể cập nhật mật khẩu. Vui lòng thử lại sau."
  - Mật khẩu cũ vẫn được giữ nguyên

- **Session hết hạn trong quá trình đổi mật khẩu:**
  - Nếu session hết hạn trước khi hoàn thành việc đổi mật khẩu
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

**Additional Information:**
- **Business Rule:**
  - Mật khẩu mới phải có ít nhất 8 ký tự
  - Mật khẩu mới phải bao gồm ít nhất 1 chữ hoa, 1 chữ thường, 1 số
  - Mật khẩu mới không được trùng với mật khẩu hiện tại
  - Mật khẩu mới và xác nhận phải khớp nhau chính xác
  - Sau khi đổi mật khẩu, tất cả session hiện tại bị vô hiệu hóa
  - Mật khẩu phải được mã hóa bằng bcrypt với salt ngẫu nhiên
  - Mỗi lần đổi mật khẩu phải ghi lại trong log bảo mật

- **Non-Functional Requirement:**
  - Performance: Thời gian đổi mật khẩu phải dưới 3 giây
  - Usability: Form phải có gợi ý rõ ràng về yêu cầu mật khẩu
  - Security: Mật khẩu phải được mã hóa mạnh, không lưu plain text
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn khi cập nhật mật khẩu

**Priority:** Medium  
**CRUD:** Update

# UCS01-7: Yêu cầu đặt lại mật khẩu [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng yêu cầu hệ thống gửi hướng dẫn để đặt lại mật khẩu khi họ quên mật khẩu hiện tại, đảm bảo tính bảo mật và khả năng phục hồi tài khoản.

**Trigger:** Người dùng nhấn liên kết "Quên mật khẩu?" trên trang đăng nhập hoặc trang cài đặt bảo mật.

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Người dùng có tài khoản hợp lệ trong hệ thống
- Email hoặc số điện thoại của người dùng đã được xác thực
- Tài khoản không bị khóa hoặc vô hiệu hóa

**Post-Condition:**
- Link đặt lại mật khẩu được gửi đến email hoặc mã OTP được gửi qua SMS
- Token đặt lại mật khẩu được tạo và lưu trong cơ sở dữ liệu với thời hạn 24 giờ
- Người dùng nhận được hướng dẫn về cách đặt lại mật khẩu
- Thời gian yêu cầu đặt lại mật khẩu được ghi lại trong log bảo mật

**Basic Flow:**
1. Người dùng truy cập trang đăng nhập
2. Người dùng nhấn liên kết "Quên mật khẩu?" hoặc "Đặt lại mật khẩu"
3. Hệ thống chuyển hướng đến trang "Đặt lại mật khẩu"
4. Hệ thống hiển thị form yêu cầu thông tin:
   - a. Email hoặc số điện thoại đã đăng ký
5. Người dùng nhập email hoặc số điện thoại của tài khoản
6. Người dùng nhấn nút "Gửi yêu cầu đặt lại mật khẩu"
7. Hệ thống kiểm tra email/số điện thoại có tồn tại trong cơ sở dữ liệu
8. Hệ thống tạo token đặt lại mật khẩu duy nhất với thời hạn 24 giờ
9. Hệ thống lưu token vào cơ sở dữ liệu với trạng thái "Chưa sử dụng"
10. Hệ thống gửi email/SMS chứa link đặt lại mật khẩu hoặc mã OTP:
    - a. Nếu bằng email: Gửi email với link chứa token
    - b. Nếu bằng SMS: Gửi SMS chứa mã OTP 6 chữ số
11. Hệ thống ghi lại thời gian yêu cầu vào log bảo mật
12. Hệ thống hiển thị thông báo: "Hướng dẫn đặt lại mật khẩu đã được gửi đến email/SMS của bạn. Vui lòng kiểm tra và làm theo hướng dẫn."

**Alternative Flow:**
- **Yêu cầu lại email/SMS:**
  - Nếu người dùng không nhận được email/SMS trong 5 phút
  - Người dùng có thể nhấn nút "Gửi lại email/SMS"
  - Hệ thống tạo token mới và gửi lại hướng dẫn
  - Token cũ bị vô hiệu hóa

- **Đặt lại mật khẩu bằng OTP:**
  - Nếu người dùng đăng ký bằng số điện thoại
  - Hệ thống gửi mã OTP 6 chữ số qua SMS
  - Người dùng nhập OTP để xác thực và đặt mật khẩu mới

**Exception Flow:**
- **Email/số điện thoại không tồn tại:**
  - Nếu email/số điện thoại không có trong cơ sở dữ liệu
  - Hệ thống hiển thị thông báo: "Email/số điện thoại không tồn tại trong hệ thống. Vui lòng kiểm tra lại hoặc đăng ký tài khoản mới."
  - Không gửi email/SMS

- **Tài khoản bị khóa:**
  - Nếu tài khoản đã bị khóa hoặc vô hiệu hóa
  - Hệ thống hiển thị thông báo: "Tài khoản đã bị khóa. Vui lòng liên hệ quản trị viên để được hỗ trợ."
  - Không gửi email/SMS

- **Quá nhiều yêu cầu đặt lại:**
  - Nếu người dùng yêu cầu đặt lại mật khẩu quá nhiều lần (>3 lần/giờ)
  - Hệ thống hiển thị thông báo: "Bạn đã yêu cầu đặt lại mật khẩu quá nhiều lần. Vui lòng thử lại sau 1 giờ."
  - Không gửi email/SMS mới

- **Lỗi gửi email/SMS:**
  - Nếu không thể gửi email/SMS do lỗi server hoặc mạng
  - Hệ thống hiển thị thông báo: "Không thể gửi email/SMS. Vui lòng thử lại sau."
  - Token vẫn được tạo nhưng người dùng có thể yêu cầu gửi lại

- **Token đã hết hạn:**
  - Nếu người dùng click vào link sau 24 giờ
  - Hệ thống hiển thị thông báo: "Link đặt lại mật khẩu đã hết hạn. Vui lòng yêu cầu đặt lại mật khẩu mới."
  - Chuyển hướng về trang yêu cầu đặt lại mật khẩu

**Additional Information:**
- **Business Rule:**
  - Token đặt lại mật khẩu có thời hạn 24 giờ từ khi tạo
  - Mỗi token chỉ có thể sử dụng 1 lần
  - Tối đa 3 yêu cầu đặt lại mật khẩu trong 1 giờ
  - Email/SMS chỉ được gửi đến email/số điện thoại đã xác thực
  - Token phải được mã hóa và không thể đoán được
  - Mỗi lần yêu cầu đặt lại phải ghi lại trong log bảo mật
  - Link đặt lại mật khẩu phải chứa HTTPS để đảm bảo bảo mật

- **Non-Functional Requirement:**
  - Performance: Thời gian gửi email/SMS phải dưới 30 giây
  - Usability: Thông báo phải rõ ràng, hướng dẫn cụ thể
  - Security: Token phải được mã hóa, link phải sử dụng HTTPS
  - Reliability: Hệ thống phải đảm bảo email/SMS được gửi thành công

**Priority:** Medium  
**CRUD:** Update

# UCS02-1: Tìm kiếm theo Nguyên liệu [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng nhập danh sách các nguyên liệu mình có, hệ thống sẽ thực hiện thuật toán tìm kiếm thông minh để trả về các công thức phù hợp nhất, sắp xếp theo độ ưu tiên từ cao đến thấp.

**Trigger:** Người dùng truy cập trang tìm kiếm và chọn tab "Tìm theo nguyên liệu" hoặc nhấn nút "Tìm kiếm theo nguyên liệu".

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Cơ sở dữ liệu công thức có sẵn các công thức với thông tin nguyên liệu
- Người dùng có ít nhất một nguyên liệu để tìm kiếm

**Post-Condition:**
- Danh sách các công thức phù hợp được hiển thị, sắp xếp theo độ ưu tiên
- Kết quả tìm kiếm được lưu vào session để có thể lọc/sắp xếp tiếp
- Thống kê tìm kiếm được ghi lại (số lượng kết quả, thời gian tìm kiếm)
- Nếu không có kết quả, hệ thống đề xuất sử dụng AI để tạo công thức mới

**Basic Flow:**
1. Người dùng truy cập trang tìm kiếm từ menu chính hoặc trang chủ
2. Người dùng chọn tab "Tìm theo nguyên liệu" hoặc nhấn nút tương ứng
3. Người dùng nhập danh sách nguyên liệu mình có bằng một trong các cách:
   - a. Gõ tên nguyên liệu và chọn từ danh sách gợi ý (autocomplete)
   - b. Chọn từ danh sách nguyên liệu phổ biến
   - c. Nhập nhiều nguyên liệu cách nhau bằng dấu phẩy
4. Hệ thống hiển thị danh sách nguyên liệu đã chọn với khả năng xóa từng nguyên liệu
5. Người dùng có thể thêm/bớt nguyên liệu và nhấn nút "Tìm kiếm"
6. Hệ thống thực hiện thuật toán tìm kiếm thông minh với 3 mức ưu tiên:
   - a. Ưu tiên 1: Tìm công thức chứa TẤT CẢ nguyên liệu người dùng nhập
   - b. Ưu tiên 2: Tìm công thức chứa NHIỀU NHẤT nguyên liệu có thể (N-1 nguyên liệu)
   - c. Ưu tiên 3: Gợi ý công thức chỉ cần thêm 1-2 nguyên liệu phổ biến
7. Hệ thống sắp xếp kết quả theo độ ưu tiên và hiển thị danh sách công thức
8. Mỗi công thức hiển thị: ảnh đại diện, tên món, thời gian nấu, độ khó, số sao đánh giá
9. Người dùng có thể xem chi tiết công thức bằng cách nhấp vào từng món

**Alternative Flow:**
- **Tìm kiếm từ "Tủ lạnh ảo":**
  - Người dùng có thể import nguyên liệu từ danh sách "Tủ lạnh ảo" đã lưu
  - Hệ thống tự động điền các nguyên liệu vào form tìm kiếm
  - Tiếp tục luồng từ bước 5 của Basic Flow

- **Tìm kiếm nâng cao:**
  - Người dùng chọn "Tìm kiếm nâng cao"
  - Có thể thêm bộ lọc: loại món ăn, thời gian nấu, độ khó, khẩu phần
  - Hệ thống áp dụng thêm các bộ lọc vào thuật toán tìm kiếm

- **Lưu tìm kiếm:**
  - Nếu kết quả tốt, người dùng có thể lưu tìm kiếm này
  - Hệ thống lưu danh sách nguyên liệu và kết quả vào "Tìm kiếm đã lưu"

**Exception Flow:**
- **Không tìm thấy công thức nào:**
  - Nếu không có công thức nào phù hợp với nguyên liệu đã nhập
  - Hệ thống hiển thị thông báo: "Không tìm thấy công thức phù hợp với nguyên liệu này."
  - Hiển thị nút "Sử dụng AI để tạo công thức mới" (kích hoạt UC2.3)
  - Gợi ý một số nguyên liệu phổ biến khác để thêm vào

- **Nguyên liệu không hợp lệ:**
  - Nếu người dùng nhập nguyên liệu không có trong hệ thống
  - Hệ thống hiển thị gợi ý: "Không tìm thấy '[tên nguyên liệu]'. Bạn có thể tham khảo các nguyên liệu gần giống: [danh sách gợi ý]"
  - Người dùng có thể chọn từ gợi ý hoặc nhập lại

- **Quá ít nguyên liệu:**
  - Nếu người dùng chỉ nhập 1 nguyên liệu
  - Hệ thống hiển thị cảnh báo: "Với 1 nguyên liệu, kết quả có thể rất nhiều. Bạn có muốn thêm nguyên liệu khác không?"
  - Vẫn thực hiện tìm kiếm nhưng hiển thị cảnh báo

- **Lỗi thuật toán tìm kiếm:**
  - Nếu có lỗi trong quá trình xử lý thuật toán
  - Hệ thống hiển thị thông báo: "Đã xảy ra lỗi trong quá trình tìm kiếm. Vui lòng thử lại."
  - Cung cấp nút "Thử lại" để thực hiện tìm kiếm lại

- **Timeout tìm kiếm:**
  - Nếu tìm kiếm mất quá 10 giây
  - Hệ thống hiển thị: "Tìm kiếm đang mất quá nhiều thời gian. Bạn có muốn thử với ít nguyên liệu hơn không?"
  - Cung cấp tùy chọn hủy hoặc tiếp tục chờ

**Additional Information:**
- **Business Rule:**
  - Thuật toán tìm kiếm ưu tiên công thức có tỷ lệ nguyên liệu khớp cao nhất
  - Công thức có tất cả nguyên liệu được đánh giá 100% phù hợp
  - Công thức thiếu 1 nguyên liệu được đánh giá 80-90% phù hợp
  - Công thức thiếu 2 nguyên liệu được đánh giá 60-80% phù hợp
  - Nguyên liệu phổ biến (gia vị, dầu ăn, muối) được ưu tiên thấp hơn
  - Kết quả tìm kiếm được cache trong 30 phút để tăng tốc độ
  - Tối đa hiển thị 50 kết quả mỗi trang, có phân trang

- **Non-Functional Requirement:**
  - Performance: Thời gian tìm kiếm phải dưới 5 giây cho tối đa 20 nguyên liệu
  - Usability: Giao diện tìm kiếm phải trực quan, có autocomplete và gợi ý
  - Security: Truy vấn tìm kiếm phải được sanitize để tránh SQL injection
  - Reliability: Hệ thống phải hoạt động ổn định ngay cả khi có nhiều người dùng tìm kiếm đồng thời

**Priority:** High  
**CRUD:** Read

# UCS02-2: Tìm kiếm theo Tên món ăn [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng nhập tên món ăn vào ô tìm kiếm, hệ thống sẽ tìm và hiển thị các công thức có tên khớp hoặc liên quan nhất đến từ khóa, hỗ trợ tìm kiếm bằng tiếng Việt có dấu và không dấu.

**Trigger:** Người dùng nhập tên món ăn vào ô tìm kiếm và nhấn nút "Tìm kiếm" hoặc nhấn phím Enter.

**Pre-Condition:**
- Hệ thống đang hoạt động bình thường
- Cơ sở dữ liệu công thức có sẵn các công thức với tên món ăn
- Người dùng có ít nhất một từ khóa để tìm kiếm

**Post-Condition:**
- Danh sách các công thức có tên khớp hoặc liên quan được hiển thị
- Kết quả được sắp xếp theo độ liên quan (khớp chính xác → khớp một phần → liên quan)
- Lịch sử tìm kiếm được lưu để gợi ý cho lần sau
- Thống kê tìm kiếm được ghi lại (số lượng kết quả, từ khóa tìm kiếm)

**Basic Flow:**
1. Người dùng truy cập trang tìm kiếm từ menu chính hoặc trang chủ
2. Người dùng nhập tên món ăn vào ô tìm kiếm (ví dụ: "thịt kho tàu", "bánh mì", "phở bò")
3. Hệ thống hiển thị gợi ý tìm kiếm real-time khi người dùng gõ (autocomplete)
4. Người dùng có thể:
   - a. Chọn từ gợi ý hiển thị
   - b. Tiếp tục gõ và nhấn "Tìm kiếm"
   - c. Nhấn phím Enter để tìm kiếm
5. Hệ thống thực hiện tìm kiếm với thuật toán:
   - a. Tìm kiếm chính xác (exact match)
   - b. Tìm kiếm một phần (partial match)
   - c. Tìm kiếm liên quan (fuzzy search)
   - d. Tìm kiếm không dấu (normalize Vietnamese text)
6. Hệ thống sắp xếp kết quả theo độ liên quan và hiển thị danh sách
7. Mỗi công thức hiển thị: ảnh đại diện, tên món, mô tả ngắn, thời gian nấu, độ khó, số sao
8. Người dùng có thể nhấp vào công thức để xem chi tiết (UC2.5)
9. Người dùng có thể lọc/sắp xếp kết quả (UC2.4)

**Alternative Flow:**
- **Tìm kiếm từ lịch sử:**
  - Người dùng nhấp vào biểu tượng lịch sử bên cạnh ô tìm kiếm
  - Hệ thống hiển thị danh sách các từ khóa đã tìm kiếm trước đó
  - Người dùng chọn từ khóa để tìm kiếm lại

- **Tìm kiếm từ gợi ý phổ biến:**
  - Hệ thống hiển thị danh sách "Tìm kiếm phổ biến" trên trang chủ
  - Người dùng nhấp vào một trong các gợi ý
  - Hệ thống tự động thực hiện tìm kiếm với từ khóa đó

- **Tìm kiếm nâng cao:**
  - Người dùng chọn "Tìm kiếm nâng cao"
  - Có thể thêm bộ lọc: loại món ăn, độ khó, thời gian nấu
  - Hệ thống kết hợp tìm kiếm tên với các bộ lọc

- **Tìm kiếm bằng giọng nói:**
  - Người dùng nhấn biểu tượng micro
  - Hệ thống chuyển đổi giọng nói thành text
  - Tự động thực hiện tìm kiếm với text đã chuyển đổi

**Exception Flow:**
- **Không tìm thấy kết quả:**
  - Nếu không có công thức nào khớp với từ khóa
  - Hệ thống hiển thị thông báo: "Không tìm thấy kết quả cho '[từ khóa]'"
  - Gợi ý: "Bạn có thể thử với từ khóa khác hoặc kiểm tra chính tả"
  - Hiển thị danh sách "Tìm kiếm phổ biến" để gợi ý

- **Từ khóa quá ngắn:**
  - Nếu từ khóa chỉ có 1-2 ký tự
  - Hệ thống hiển thị cảnh báo: "Từ khóa quá ngắn. Vui lòng nhập ít nhất 3 ký tự."
  - Không thực hiện tìm kiếm

- **Từ khóa chứa ký tự đặc biệt:**
  - Nếu từ khóa chứa ký tự không hợp lệ
  - Hệ thống tự động loại bỏ ký tự đặc biệt và thông báo
  - Tiếp tục tìm kiếm với từ khóa đã làm sạch

- **Quá nhiều kết quả:**
  - Nếu tìm thấy > 100 kết quả
  - Hệ thống hiển thị: "Tìm thấy [số] kết quả. Hãy thử từ khóa cụ thể hơn để thu hẹp kết quả."
  - Hiển thị kết quả đầu tiên và có phân trang

- **Lỗi hệ thống:**
  - Nếu có lỗi server khi tìm kiếm
  - Hệ thống hiển thị: "Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau."
  - Cung cấp nút "Thử lại" để tìm kiếm lại

- **Timeout tìm kiếm:**
  - Nếu tìm kiếm mất quá 5 giây
  - Hệ thống hiển thị: "Tìm kiếm đang mất quá nhiều thời gian. Vui lòng thử lại."
  - Cung cấp tùy chọn hủy hoặc tiếp tục chờ

**Additional Information:**
- **Business Rule:**
  - Tìm kiếm hỗ trợ tiếng Việt có dấu và không dấu
  - Tìm kiếm không phân biệt hoa thường
  - Ưu tiên hiển thị công thức có tên khớp chính xác trước
  - Sau đó là công thức có tên chứa từ khóa
  - Cuối cùng là công thức có tên liên quan (fuzzy search)
  - Kết quả tìm kiếm được cache trong 15 phút
  - Tối đa hiển thị 20 kết quả mỗi trang, có phân trang
  - Lịch sử tìm kiếm được lưu tối đa 50 từ khóa gần nhất

- **Non-Functional Requirement:**
  - Performance: Thời gian tìm kiếm phải dưới 2 giây
  - Usability: Autocomplete phải phản hồi trong vòng 300ms
  - Security: Từ khóa tìm kiếm phải được sanitize để tránh XSS
  - Reliability: Hệ thống phải hoạt động ổn định với nhiều yêu cầu tìm kiếm đồng thời

**Priority:** Medium  
**CRUD:** Read


# UCS02-3: Tạo công thức bằng AI [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép hệ thống sử dụng trí tuệ nhân tạo (AI) để tạo ra một công thức nấu ăn mới hoàn toàn từ danh sách nguyên liệu mà người dùng cung cấp, khi không tìm thấy công thức phù hợp trong cơ sở dữ liệu hiện có.

**Trigger:** Người dùng nhấn nút "Sử dụng AI để tạo công thức mới" sau khi tìm kiếm theo nguyên liệu (UC2.1) không trả về kết quả.

**Pre-Condition:**
- Việc tìm kiếm theo nguyên liệu (UC2.1) đã được thực hiện nhưng không trả về kết quả
- Hệ thống có kết nối đến API của mô hình AI (GPT hoặc tương tự)
- Người dùng có ít nhất 2-3 nguyên liệu chính để tạo công thức
- API AI đang hoạt động và có thể xử lý yêu cầu

**Post-Condition:**
- Một công thức mới được tạo ra bởi AI và hiển thị cho người dùng
- Công thức bao gồm: tên món, danh sách nguyên liệu chi tiết, các bước thực hiện, thời gian nấu, độ khó
- Công thức được lưu tạm thời trong session của người dùng
- Người dùng có thể lưu công thức vào yêu thích hoặc báo cáo nếu không hài lòng

**Basic Flow:**
1. Sau khi tìm kiếm theo nguyên liệu không có kết quả, hệ thống hiển thị thông báo: "Không tìm thấy công thức phù hợp. Bạn có muốn sử dụng AI để tạo một công thức mới từ những nguyên liệu này không?"
2. Người dùng nhấn nút "Đồng ý" hoặc "Sử dụng AI"
3. Hệ thống hiển thị màn hình loading với thông báo: "AI đang tạo công thức cho bạn..."
4. Hệ thống chuẩn bị prompt cho AI với thông tin:
   - a. Danh sách nguyên liệu chính người dùng có
   - b. Yêu cầu tạo công thức nấu ăn ngon, thực tế
   - c. Định dạng output mong muốn (JSON structure)
5. Hệ thống gửi yêu cầu đến API AI với prompt đã chuẩn bị
6. Hệ thống nhận phản hồi từ AI và xử lý dữ liệu:
   - a. Parse JSON response từ AI
   - b. Validate dữ liệu nhận được
   - c. Định dạng lại văn bản cho phù hợp với giao diện
7. Hệ thống hiển thị công thức vừa được tạo với các thông tin:
   - a. Tên món ăn (do AI đặt)
   - b. Mô tả ngắn về món ăn
   - c. Danh sách nguyên liệu chi tiết với định lượng
   - d. Các bước thực hiện từng bước
   - e. Thời gian nấu ước tính
   - f. Độ khó (Dễ/Trung bình/Khó)
   - g. Khẩu phần ăn
8. Hệ thống cung cấp các tùy chọn cho người dùng:
   - a. "Lưu vào Yêu thích"
   - b. "Chia sẻ công thức"
   - c. "Báo cáo không phù hợp"
   - d. "Tạo công thức khác"

**Alternative Flow:**
- **Tạo công thức từ "Tủ lạnh ảo":**
  - Người dùng có thể chọn "Tạo công thức từ Tủ lạnh ảo"
  - Hệ thống lấy danh sách nguyên liệu từ UC5 (Tủ lạnh ảo)
  - Tiếp tục luồng từ bước 3 của Basic Flow

- **Tạo công thức với yêu cầu đặc biệt:**
  - Người dùng có thể thêm yêu cầu: "Món chay", "Ít calo", "Nhanh chóng"
  - Hệ thống thêm yêu cầu này vào prompt cho AI
  - Tiếp tục luồng từ bước 4 của Basic Flow

- **Tạo nhiều công thức:**
  - Sau khi xem công thức đầu tiên, người dùng có thể nhấn "Tạo công thức khác"
  - Hệ thống gửi yêu cầu mới đến AI với cùng nguyên liệu
  - Tiếp tục luồng từ bước 5 của Basic Flow

**Exception Flow:**
- **API AI bị lỗi hoặc quá tải:**
  - Nếu API AI trả về lỗi hoặc timeout
  - Hệ thống hiển thị thông báo: "AI hiện đang quá tải. Vui lòng thử lại sau ít phút."
  - Cung cấp nút "Thử lại" và nút "Quay lại tìm kiếm"

- **Kết quả từ AI không đúng định dạng:**
  - Nếu AI trả về dữ liệu không đúng cấu trúc JSON mong muốn
  - Hệ thống hiển thị thông báo: "Không thể xử lý kết quả từ AI. Vui lòng thử lại."
  - Cung cấp nút "Thử lại với prompt khác"

- **Kết quả từ AI không hợp lý:**
  - Nếu AI tạo ra công thức có nguyên liệu không phù hợp hoặc hướng dẫn sai
  - Hệ thống hiển thị cảnh báo: "Công thức này có thể cần điều chỉnh. Vui lòng kiểm tra kỹ trước khi nấu."
  - Cung cấp nút "Báo cáo không phù hợp"

- **Mất kết nối mạng:**
  - Nếu mất kết nối trong quá trình gọi API AI
  - Hệ thống hiển thị: "Mất kết nối mạng. Vui lòng kiểm tra kết nối và thử lại."
  - Cung cấp nút "Thử lại" khi kết nối được khôi phục

- **Nguyên liệu quá ít:**
  - Nếu người dùng chỉ có 1 nguyên liệu
  - Hệ thống hiển thị: "Với 1 nguyên liệu, AI khó tạo ra công thức hoàn chỉnh. Bạn có muốn thêm nguyên liệu khác không?"
  - Cung cấp gợi ý các nguyên liệu phổ biến để thêm vào

- **Nguyên liệu không phù hợp:**
  - Nếu nguyên liệu quá kỳ lạ hoặc không phù hợp để nấu ăn
  - Hệ thống hiển thị: "Nguyên liệu này có thể không phù hợp để tạo công thức. Bạn có muốn thử nguyên liệu khác không?"
  - Cung cấp gợi ý các nguyên liệu thay thế

**Additional Information:**
- **Business Rule:**
  - AI chỉ được kích hoạt khi tìm kiếm theo nguyên liệu không có kết quả
  - Mỗi người dùng chỉ được sử dụng AI tối đa 5 lần/ngày để tránh lạm dụng
  - Prompt cho AI phải được chuẩn hóa để đảm bảo chất lượng output
  - Công thức do AI tạo không được lưu vào cơ sở dữ liệu chính
  - Công thức AI chỉ được lưu tạm thời trong session (24 giờ)
  - Người dùng có thể báo cáo công thức không phù hợp để cải thiện AI

- **Non-Functional Requirement:**
  - Performance: Thời gian AI tạo công thức phải dưới 15 giây
  - Usability: Giao diện phải hiển thị tiến trình loading rõ ràng
  - Security: API key của AI phải được bảo mật, không expose ra client
  - Reliability: Hệ thống phải có fallback khi AI không khả dụng

**Priority:** High  
**CRUD:** Create


# UCS02-4: Lọc và Sắp xếp kết quả [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng lọc và sắp xếp kết quả tìm kiếm công thức theo các tiêu chí khác nhau như loại món ăn, thời gian nấu, độ khó, điểm đánh giá để tìm được công thức phù hợp nhất với nhu cầu.

**Trigger:** Người dùng nhấn nút "Lọc" hoặc "Sắp xếp" sau khi có kết quả tìm kiếm từ UC2.1 hoặc UC2.2.

**Pre-Condition:**
- Người dùng đã thực hiện tìm kiếm và có kết quả hiển thị
- Kết quả tìm kiếm được lưu trong session của người dùng
- Hệ thống có đủ dữ liệu để thực hiện lọc và sắp xếp

**Post-Condition:**
- Kết quả tìm kiếm được lọc và sắp xếp theo tiêu chí người dùng chọn
- Giao diện hiển thị kết quả mới với thông tin về số lượng kết quả sau khi lọc
- Các bộ lọc được lưu để có thể áp dụng cho các tìm kiếm tiếp theo
- Trạng thái lọc/sắp xếp được lưu trong session

**Basic Flow:**
1. Người dùng đã có kết quả tìm kiếm từ trang tìm kiếm (UC2.1 hoặc UC2.2)
2. Người dùng nhấn nút "Lọc & Sắp xếp" hoặc biểu tượng bộ lọc
3. Hệ thống hiển thị panel lọc và sắp xếp với các tùy chọn:

   **Bộ lọc:**
   - a. Loại món ăn (Món chính, Món tráng miệng, Món chay, Món Hàn, Món Nhật, v.v.)
   - b. Thời gian nấu (Dưới 15 phút, 15-30 phút, 30-60 phút, Trên 60 phút)
   - c. Độ khó (Dễ, Trung bình, Khó)
   - d. Khẩu phần (1-2 người, 3-4 người, 5-6 người, Trên 6 người)
   - e. Điểm đánh giá (Từ 4 sao trở lên, Từ 3 sao trở lên)
   - f. Người tạo (Hệ thống, Người dùng)

   **Sắp xếp:**
   - a. Độ liên quan (mặc định)
   - b. Thời gian nấu (tăng dần/giảm dần)
   - c. Điểm đánh giá (cao đến thấp)
   - d. Ngày tạo (mới nhất/cũ nhất)
   - e. Tên món ăn (A-Z, Z-A)

4. Người dùng chọn các tiêu chí lọc và sắp xếp mong muốn
5. Người dùng nhấn nút "Áp dụng" hoặc "Lọc"
6. Hệ thống thực hiện lọc và sắp xếp kết quả theo tiêu chí đã chọn
7. Hệ thống hiển thị kết quả mới với thông tin:
   - a. Số lượng kết quả sau khi lọc (ví dụ: "Hiển thị 12 trong 45 kết quả")
   - b. Danh sách công thức đã được lọc và sắp xếp
   - c. Các bộ lọc đang được áp dụng
8. Người dùng có thể xem kết quả và thực hiện các hành động khác

**Alternative Flow:**
- **Lọc nhanh:**
  - Hệ thống hiển thị các nút lọc nhanh phổ biến (VD: "Nhanh", "Dễ làm", "Nổi tiếng")
  - Người dùng nhấp vào một trong các nút này
  - Hệ thống tự động áp dụng bộ lọc tương ứng

- **Lưu bộ lọc:**
  - Sau khi áp dụng bộ lọc thành công, người dùng có thể nhấn "Lưu bộ lọc này"
  - Hệ thống lưu bộ lọc với tên do người dùng đặt
  - Người dùng có thể sử dụng lại bộ lọc đã lưu cho các lần tìm kiếm sau

- **Xóa tất cả bộ lọc:**
  - Người dùng có thể nhấn "Xóa tất cả bộ lọc" để quay về kết quả ban đầu
  - Hệ thống hiển thị lại tất cả kết quả tìm kiếm gốc

- **Lọc theo nguyên liệu:**
  - Người dùng có thể thêm bộ lọc "Có nguyên liệu" hoặc "Không có nguyên liệu"
  - Chọn nguyên liệu cụ thể để lọc công thức có/không có nguyên liệu đó

**Exception Flow:**
- **Không có kết quả sau khi lọc:**
  - Nếu tất cả kết quả bị loại bỏ sau khi áp dụng bộ lọc
  - Hệ thống hiển thị thông báo: "Không có kết quả nào phù hợp với bộ lọc đã chọn."
  - Cung cấp nút "Xóa bộ lọc" để quay về kết quả ban đầu
  - Gợi ý các bộ lọc khác để thử

- **Bộ lọc không hợp lệ:**
  - Nếu người dùng chọn bộ lọc mâu thuẫn (VD: "Dưới 15 phút" và "Trên 60 phút")
  - Hệ thống hiển thị cảnh báo: "Bộ lọc đã chọn không hợp lệ. Vui lòng kiểm tra lại."
  - Highlight các bộ lọc mâu thuẫn và yêu cầu người dùng sửa

- **Lỗi xử lý bộ lọc:**
  - Nếu có lỗi server khi xử lý bộ lọc
  - Hệ thống hiển thị: "Đã xảy ra lỗi khi áp dụng bộ lọc. Vui lòng thử lại."
  - Cung cấp nút "Thử lại" để áp dụng bộ lọc lại

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình lọc
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Quá nhiều kết quả sau khi lọc:**
  - Nếu vẫn còn > 100 kết quả sau khi lọc
  - Hệ thống hiển thị: "Vẫn còn [số] kết quả. Bạn có thể thêm bộ lọc để thu hẹp kết quả."
  - Gợi ý các bộ lọc bổ sung

**Additional Information:**
- **Business Rule:**
  - Bộ lọc có thể được kết hợp nhiều tiêu chí cùng lúc
  - Kết quả lọc phải được sắp xếp theo thứ tự đã chọn
  - Bộ lọc được lưu trong session tối đa 2 giờ
  - Người dùng có thể lưu tối đa 5 bộ lọc tùy chỉnh
  - Bộ lọc nhanh được cập nhật dựa trên xu hướng tìm kiếm
  - Kết quả lọc phải giữ nguyên thông tin gốc của công thức

- **Non-Functional Requirement:**
  - Performance: Thời gian lọc và sắp xếp phải dưới 1 giây
  - Usability: Giao diện bộ lọc phải trực quan, dễ sử dụng
  - Security: Bộ lọc phải được validate để tránh SQL injection
  - Reliability: Hệ thống phải hoạt động ổn định với nhiều bộ lọc phức tạp

**Priority:** Medium  
**CRUD:** Read


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


# UCS04-1: Thêm công thức vào Yêu thích [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập lưu một công thức nấu ăn vào danh sách yêu thích cá nhân để có thể truy cập nhanh chóng sau này mà không cần tìm kiếm lại.

**Trigger:** Người dùng đã đăng nhập nhấp vào biểu tượng "Yêu thích" (♥ hoặc ☆) trên trang chi tiết công thức hoặc trong danh sách công thức.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại và có trạng thái "Đã duyệt"
- Công thức chưa được thêm vào danh sách yêu thích của người dùng
- Người dùng chưa đạt giới hạn số lượng công thức yêu thích

**Post-Condition:**
- Công thức được thêm vào danh sách yêu thích của người dùng
- Biểu tượng "Yêu thích" thay đổi trạng thái (đổi màu hoặc icon)
- Thông báo xác nhận được hiển thị
- Số lượng yêu thích của công thức tăng lên 1
- Công thức xuất hiện trong danh sách "Công thức yêu thích" (UC4.3)

**Basic Flow:**
1. Người dùng đã đăng nhập đang xem trang chi tiết công thức (UC2.5) hoặc danh sách công thức
2. Người dùng nhấp vào biểu tượng "Yêu thích" (♥ trống hoặc ☆ trống)
3. Hệ thống kiểm tra các điều kiện:
   - a. Người dùng đã đăng nhập
   - b. Công thức có trạng thái "Đã duyệt"
   - c. Công thức chưa có trong danh sách yêu thích của người dùng
   - d. Người dùng chưa đạt giới hạn yêu thích (tối đa 500 công thức)
4. Hệ thống thêm công thức vào danh sách yêu thích của người dùng:
   - a. Lưu ID công thức và ID người dùng vào bảng yêu thích
   - b. Ghi lại thời gian thêm vào yêu thích
5. Hệ thống cập nhật trạng thái hiển thị:
   - a. Biểu tượng "Yêu thích" đổi sang trạng thái "đã yêu thích" (♥ đỏ hoặc ☆ vàng)
   - b. Thêm hiệu ứng animation (nhỏ dần, đổi màu)
6. Hệ thống tăng số lượng yêu thích của công thức lên 1
7. Hệ thống hiển thị thông báo xác nhận: "Đã thêm '[tên công thức]' vào Yêu thích"
8. Thông báo tự động biến mất sau 3 giây hoặc người dùng có thể đóng thủ công

**Alternative Flow:**
- **Thêm yêu thích từ danh sách:**
  - Người dùng đang xem danh sách công thức (kết quả tìm kiếm, danh mục)
  - Nhấp vào biểu tượng yêu thích trên card công thức
  - Hệ thống thực hiện tương tự như Basic Flow

- **Thêm yêu thích hàng loạt:**
  - Người dùng có thể chọn nhiều công thức và thêm yêu thích cùng lúc
  - Hệ thống hiển thị số lượng công thức sẽ được thêm
  - Xác nhận và thực hiện thêm tất cả

- **Thêm yêu thích từ thông báo:**
  - Người dùng nhận được thông báo về công thức mới hoặc gợi ý
  - Có thể thêm yêu thích trực tiếp từ thông báo

**Exception Flow:**
- **Người dùng chưa đăng nhập:**
  - Nếu người dùng chưa đăng nhập và nhấp vào biểu tượng yêu thích
  - Hệ thống hiển thị popup: "Vui lòng đăng nhập để thêm công thức vào yêu thích"
  - Cung cấp nút "Đăng nhập" để chuyển đến trang đăng nhập
  - Sau khi đăng nhập, tự động quay lại trang hiện tại

- **Công thức đã có trong yêu thích:**
  - Nếu công thức đã được thêm vào yêu thích trước đó
  - Hệ thống hiển thị thông báo: "Công thức này đã có trong danh sách yêu thích của bạn"
  - Biểu tượng vẫn ở trạng thái "đã yêu thích"

- **Đạt giới hạn yêu thích:**
  - Nếu người dùng đã có 500 công thức yêu thích
  - Hệ thống hiển thị: "Bạn đã đạt giới hạn 500 công thức yêu thích. Vui lòng xóa một số công thức cũ để thêm mới."
  - Cung cấp link đến trang "Công thức yêu thích" để quản lý

- **Công thức chưa được duyệt:**
  - Nếu công thức có trạng thái "Chờ duyệt" hoặc "Bị từ chối"
  - Hệ thống hiển thị: "Công thức này chưa được duyệt. Không thể thêm vào yêu thích."
  - Biểu tượng yêu thích bị vô hiệu hóa

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu vào cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể thêm vào yêu thích. Vui lòng thử lại sau."
  - Biểu tượng không thay đổi trạng thái

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình thêm yêu thích
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa hoặc không tồn tại
  - Hệ thống hiển thị: "Công thức không tồn tại hoặc đã bị xóa."
  - Cập nhật trang để loại bỏ công thức không hợp lệ

**Additional Information:**
- **Business Rule:**
  - Mỗi người dùng chỉ có thể thêm tối đa 500 công thức vào yêu thích
  - Mỗi công thức chỉ có thể được thêm vào yêu thích 1 lần cho mỗi người dùng
  - Chỉ có thể thêm công thức có trạng thái "Đã duyệt" vào yêu thích
  - Thời gian thêm vào yêu thích được ghi lại để sắp xếp theo thời gian
  - Số lượng yêu thích của công thức được cập nhật real-time
  - Danh sách yêu thích được sắp xếp theo thời gian thêm (mới nhất trước)

- **Non-Functional Requirement:**
  - Performance: Thời gian thêm yêu thích phải dưới 1 giây
  - Usability: Biểu tượng yêu thích phải rõ ràng, dễ nhận biết trạng thái
  - Security: Chỉ người dùng đã đăng nhập mới có thể thêm yêu thích
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi thêm yêu thích

**Priority:** Low  
**CRUD:** Create


# UCS04-2: Gỡ công thức khỏi Yêu thích [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đã đăng nhập gỡ bỏ một công thức đã được thêm vào danh sách yêu thích, giúp quản lý danh sách yêu thích một cách linh hoạt và cập nhật theo sở thích thay đổi.

**Trigger:** Người dùng đã đăng nhập nhấp vào biểu tượng "Yêu thích" đang ở trạng thái "đã yêu thích" (♥ đỏ hoặc ☆ vàng) trên trang chi tiết công thức hoặc trong danh sách công thức.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Công thức tồn tại trong danh sách yêu thích của người dùng
- Biểu tượng "Yêu thích" đang ở trạng thái "đã yêu thích"

**Post-Condition:**
- Công thức được gỡ khỏi danh sách yêu thích của người dùng
- Biểu tượng "Yêu thích" trở về trạng thái ban đầu (♥ trống hoặc ☆ trống)
- Thông báo xác nhận được hiển thị
- Số lượng yêu thích của công thức giảm xuống 1
- Công thức không còn xuất hiện trong danh sách "Công thức yêu thích" (UC4.3)

**Basic Flow:**
1. Người dùng đã đăng nhập đang xem trang chi tiết công thức (UC2.5) hoặc danh sách công thức
2. Người dùng nhấp vào biểu tượng "Yêu thích" đang ở trạng thái "đã yêu thích" (♥ đỏ hoặc ☆ vàng)
3. Hệ thống kiểm tra các điều kiện:
   - a. Người dùng đã đăng nhập
   - b. Công thức tồn tại trong danh sách yêu thích của người dùng
4. Hệ thống gỡ bỏ công thức khỏi danh sách yêu thích:
   - a. Xóa bản ghi yêu thích từ bảng yêu thích
   - b. Ghi lại thời gian gỡ khỏi yêu thích (để audit)
5. Hệ thống cập nhật trạng thái hiển thị:
   - a. Biểu tượng "Yêu thích" đổi về trạng thái "chưa yêu thích" (♥ trống hoặc ☆ trống)
   - b. Thêm hiệu ứng animation (nhỏ dần, mờ đi)
6. Hệ thống giảm số lượng yêu thích của công thức xuống 1
7. Hệ thống hiển thị thông báo xác nhận: "Đã gỡ '[tên công thức]' khỏi Yêu thích"
8. Thông báo tự động biến mất sau 3 giây hoặc người dùng có thể đóng thủ công

**Alternative Flow:**
- **Gỡ yêu thích từ danh sách:**
  - Người dùng đang xem danh sách công thức yêu thích (UC4.3)
  - Nhấp vào biểu tượng yêu thích trên card công thức
  - Hệ thống thực hiện tương tự như Basic Flow

- **Gỡ yêu thích hàng loạt:**
  - Người dùng có thể chọn nhiều công thức và gỡ yêu thích cùng lúc
  - Hệ thống hiển thị số lượng công thức sẽ được gỡ
  - Xác nhận và thực hiện gỡ tất cả

- **Gỡ yêu thích từ trang quản lý:**
  - Người dùng vào trang "Quản lý yêu thích" với danh sách đầy đủ
  - Có thể gỡ yêu thích nhiều công thức cùng lúc
  - Có tùy chọn gỡ tất cả yêu thích

**Exception Flow:**
- **Người dùng chưa đăng nhập:**
  - Nếu người dùng chưa đăng nhập và nhấp vào biểu tượng yêu thích
  - Hệ thống hiển thị popup: "Vui lòng đăng nhập để quản lý yêu thích"
  - Cung cấp nút "Đăng nhập" để chuyển đến trang đăng nhập

- **Công thức không có trong yêu thích:**
  - Nếu công thức không có trong danh sách yêu thích
  - Hệ thống hiển thị thông báo: "Công thức này không có trong danh sách yêu thích của bạn"
  - Biểu tượng vẫn ở trạng thái "chưa yêu thích"

- **Lỗi xóa cơ sở dữ liệu:**
  - Nếu không thể xóa khỏi cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể gỡ khỏi yêu thích. Vui lòng thử lại sau."
  - Biểu tượng không thay đổi trạng thái

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình gỡ yêu thích
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị thông báo: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa khỏi hệ thống
  - Hệ thống tự động gỡ khỏi danh sách yêu thích
  - Hiển thị thông báo: "Công thức đã bị xóa. Đã tự động gỡ khỏi danh sách yêu thích."

- **Lỗi cập nhật số lượng yêu thích:**
  - Nếu không thể cập nhật số lượng yêu thích của công thức
  - Hệ thống vẫn gỡ khỏi danh sách yêu thích của người dùng
  - Hiển thị cảnh báo: "Đã gỡ khỏi yêu thích nhưng có lỗi khi cập nhật thống kê."

**Additional Information:**
- **Business Rule:**
  - Chỉ có thể gỡ công thức khỏi danh sách yêu thích của chính mình
  - Việc gỡ yêu thích không ảnh hưởng đến công thức gốc
  - Thời gian gỡ yêu thích được ghi lại để audit
  - Số lượng yêu thích của công thức được cập nhật real-time
  - Sau khi gỡ, công thức có thể được thêm lại vào yêu thích bất cứ lúc nào
  - Danh sách yêu thích được cập nhật ngay lập tức

- **Non-Functional Requirement:**
  - Performance: Thời gian gỡ yêu thích phải dưới 1 giây
  - Usability: Biểu tượng yêu thích phải rõ ràng về trạng thái hiện tại
  - Security: Chỉ người dùng đã đăng nhập mới có thể gỡ yêu thích của mình
  - Reliability: Hệ thống phải đảm bảo tính toàn vẹn dữ liệu khi gỡ yêu thích

**Priority:** Low  
**CRUD:** Delete


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


# UCS04-5: Xem đánh giá và bình luận [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng đọc các bình luận và điểm đánh giá từ những người dùng khác về một công thức nấu ăn, giúp đưa ra quyết định có nên thử công thức đó hay không.

**Trigger:** Người dùng cuộn xuống phần "Đánh giá và Bình luận" trên trang chi tiết công thức (UC2.5) hoặc nhấn vào tab "Đánh giá".

**Pre-Condition:**
- Công thức tồn tại và có trạng thái "Đã duyệt"
- Công thức có ít nhất một đánh giá từ người dùng khác
- Hệ thống đang hoạt động bình thường

**Post-Condition:**
- Tất cả đánh giá và bình luận của công thức được hiển thị
- Người dùng có thể đọc và tham khảo ý kiến của cộng đồng
- Thống kê đánh giá được cập nhật (nếu có đánh giá mới)

**Basic Flow:**
1. Người dùng đang xem trang chi tiết công thức (UC2.5)
2. Người dùng cuộn xuống phần "Đánh giá và Bình luận" hoặc nhấn tab "Đánh giá"
3. Hệ thống truy vấn cơ sở dữ liệu để lấy tất cả đánh giá của công thức
4. Hệ thống hiển thị phần đánh giá với layout:

   **Tổng quan đánh giá:**
   - a. Điểm đánh giá trung bình (VD: 4.3/5 sao)
   - b. Tổng số đánh giá (VD: 127 đánh giá)
   - c. Biểu đồ phân phối điểm (số lượng đánh giá cho mỗi mức sao)
   - d. Tỷ lệ phần trăm cho mỗi mức sao

   **Bộ lọc và sắp xếp:**
   - e. Bộ lọc theo số sao (Tất cả, 5 sao, 4 sao, 3 sao, 2 sao, 1 sao)
   - f. Sắp xếp theo (Mới nhất, Cũ nhất, Hữu ích nhất, Điểm cao nhất)
   - g. Tìm kiếm trong bình luận

   **Danh sách đánh giá:**
   - h. Mỗi đánh giá hiển thị:
      - Ảnh đại diện người đánh giá
      - Tên người đánh giá (có thể ẩn danh)
      - Số sao đánh giá
      - Nội dung bình luận
      - Thời gian đánh giá
      - Ảnh món ăn đã nấu (nếu có)
      - Số lượt "Hữu ích" và nút "Hữu ích"
      - Nút "Báo cáo" (nếu nội dung không phù hợp)

5. Hệ thống hiển thị danh sách đánh giá với phân trang (10 đánh giá/trang)
6. Người dùng có thể:
   - a. Đọc các bình luận chi tiết
   - b. Xem ảnh món ăn từ người đánh giá
   - c. Nhấn "Hữu ích" cho các đánh giá hữu ích
   - d. Sử dụng bộ lọc để xem đánh giá theo tiêu chí
   - e. Tìm kiếm từ khóa trong bình luận

**Alternative Flow:**
- **Xem đánh giá từ trang chủ:**
  - Người dùng có thể xem đánh giá tổng quan từ trang chủ
  - Hiển thị điểm trung bình và số lượng đánh giá
  - Có thể click để xem chi tiết

- **Xem đánh giá từ danh sách:**
  - Trong danh sách công thức, hiển thị điểm đánh giá trung bình
  - Có thể preview một vài đánh giá nổi bật

- **Xem đánh giá của người dùng cụ thể:**
  - Click vào tên người đánh giá để xem profile
  - Xem các đánh giá khác của người đó

- **Xuất báo cáo đánh giá:**
  - Admin có thể xuất báo cáo tổng hợp đánh giá
  - Bao gồm thống kê và phân tích xu hướng

**Exception Flow:**
- **Chưa có đánh giá nào:**
  - Nếu công thức chưa có đánh giá từ người dùng
  - Hệ thống hiển thị: "Chưa có đánh giá nào cho công thức này."
  - Hiển thị gợi ý: "Hãy là người đầu tiên đánh giá công thức này!"
  - Cung cấp nút "Viết đánh giá" để chuyển đến form đánh giá

- **Lỗi tải dữ liệu:**
  - Nếu có lỗi khi truy vấn đánh giá từ cơ sở dữ liệu
  - Hệ thống hiển thị: "Không thể tải đánh giá. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới" để thử lại

- **Không tìm thấy kết quả lọc:**
  - Nếu bộ lọc không trả về đánh giá nào
  - Hệ thống hiển thị: "Không có đánh giá nào phù hợp với bộ lọc đã chọn."
  - Cung cấp nút "Xóa bộ lọc" để xem tất cả đánh giá

- **Lỗi hiển thị ảnh:**
  - Nếu ảnh đại diện hoặc ảnh món ăn không tải được
  - Hệ thống hiển thị ảnh placeholder mặc định
  - Không ảnh hưởng đến việc hiển thị nội dung đánh giá

- **Nội dung bị ẩn:**
  - Nếu đánh giá bị ẩn do vi phạm quy định
  - Hệ thống hiển thị: "Đánh giá này đã bị ẩn do vi phạm quy định cộng đồng."
  - Không hiển thị nội dung chi tiết

- **Công thức không tồn tại:**
  - Nếu công thức đã bị xóa hoặc không tồn tại
  - Hệ thống hiển thị: "Công thức không tồn tại hoặc đã bị xóa."
  - Chuyển hướng về trang trước

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị đánh giá của công thức có trạng thái "Đã duyệt"
  - Đánh giá được sắp xếp theo thời gian mới nhất theo mặc định
  - Đánh giá bị ẩn do vi phạm không được hiển thị
  - Tên người đánh giá có thể được ẩn danh nếu họ chọn
  - Điểm đánh giá trung bình được làm tròn đến 1 chữ số thập phân
  - Phân trang tối đa 10 đánh giá/trang để đảm bảo hiệu suất
  - Đánh giá được cache trong 5 phút để tăng tốc độ

- **Non-Functional Requirement:**
  - Performance: Thời gian tải danh sách đánh giá phải dưới 2 giây
  - Usability: Giao diện phải rõ ràng, dễ đọc, có thể lọc và tìm kiếm
  - Security: Nội dung đánh giá phải được sanitize để tránh XSS
  - Reliability: Hệ thống phải hoạt động ổn định ngay cả khi có nhiều đánh giá

**Priority:** Low  
**CRUD:** Read


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


# UCS05-1: Thêm nguyên liệu vào tủ [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng thêm một loại nguyên liệu mới vào danh sách nguyên liệu hiện có trong "Tủ lạnh ảo" của mình để quản lý tồn kho cá nhân.

**Trigger:** Người dùng đã đăng nhập nhấn nút "Thêm nguyên liệu" trên trang "Tủ lạnh ảo".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Hệ thống có danh mục nguyên liệu chuẩn hóa để gợi ý/autocomplete

**Post-Condition:**
- Nguyên liệu mới được thêm vào danh sách trong "Tủ lạnh ảo" của người dùng
- Số lượng và đơn vị được lưu lại chính xác
- Thời gian thêm được ghi lại để audit/sắp xếp
- Danh sách hiển thị được cập nhật real-time

**Basic Flow:**
1. Người dùng truy cập trang "Tủ lạnh ảo" (UC5)
2. Người dùng nhấn nút "Thêm nguyên liệu"
3. Hệ thống hiển thị form thêm nguyên liệu với các trường:
   - a. Tên nguyên liệu (autocomplete từ danh mục chuẩn hóa)
   - b. Số lượng (số thập phân, >= 0)
   - c. Đơn vị (vd: gram, kg, ml, lít, trái, bó)
   - d. Ngày hết hạn (tùy chọn)
   - e. Ghi chú (tùy chọn)
4. Người dùng nhập thông tin và nhấn "Lưu"
5. Hệ thống validate dữ liệu đầu vào (tên hợp lệ, số lượng > 0, đơn vị hợp lệ)
6. Hệ thống thêm bản ghi nguyên liệu vào kho cá nhân của người dùng
7. Hệ thống cập nhật giao diện danh sách nguyên liệu
8. Hệ thống hiển thị thông báo: "Đã thêm nguyên liệu thành công"

**Alternative Flow:**
- **Thêm nhanh từ gợi ý phổ biến:**
  - Hệ thống hiển thị danh sách nguyên liệu phổ biến
  - Người dùng chọn nhanh và nhập số lượng/đơn vị
  - Tiếp tục từ bước 6 của Basic Flow

- **Quét mã vạch (barcode):**
  - Người dùng sử dụng camera/thiết bị quét mã vạch
  - Hệ thống nhận diện sản phẩm và gợi ý nguyên liệu, đơn vị
  - Người dùng xác nhận và nhập số lượng

- **Nhập hàng loạt:**
  - Người dùng mở modal nhập nhiều dòng (CSV đơn giản)
  - Hệ thống parse và hiển thị preview
  - Người dùng xác nhận để thêm hàng loạt

**Exception Flow:**
- **Nguyên liệu không hợp lệ/không tồn tại trong danh mục:**
  - Hệ thống gợi ý nguyên liệu gần đúng hoặc cho phép tạo nguyên liệu custom (đánh dấu)
  - Hiển thị cảnh báo: "Nguyên liệu này chưa được chuẩn hóa"

- **Trùng nguyên liệu:**
  - Nếu nguyên liệu đã tồn tại trong danh sách
  - Hệ thống hỏi: "Gộp số lượng vào mục có sẵn?"
  - Nếu đồng ý, cộng dồn số lượng và cập nhật ngày hết hạn theo quy tắc

- **Giá trị số lượng không hợp lệ:**
  - Nếu số lượng <= 0 hoặc không phải số
  - Hệ thống hiển thị lỗi và yêu cầu nhập lại

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu do lỗi hệ thống
  - Hiển thị: "Không thể thêm nguyên liệu. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Tên nguyên liệu ưu tiên chọn từ danh mục chuẩn hóa để đồng nhất tìm kiếm
  - Hỗ trợ nhiều đơn vị, có quy tắc quy đổi nội bộ (vd: 1000g = 1kg)
  - Cho phép tạo nguyên liệu custom nhưng cần review sau
  - Nếu gộp, ngày hết hạn lấy ngày gần nhất (FEFO) hoặc cho người dùng chọn
  - Lưu lịch sử thay đổi (audit trail) cho từng nguyên liệu

- **Non-Functional Requirement:**
  - Performance: Thêm nguyên liệu < 1 giây
  - Usability: Autocomplete mượt, hỗ trợ nhập nhanh bàn phím
  - Security: Chỉ chủ sở hữu được phép thao tác tủ cá nhân
  - Reliability: Dữ liệu được lưu nhất quán, không trùng lặp

**Priority:** Low  
**CRUD:** Create

# UCS05-2: Xem danh sách nguyên liệu trong tủ [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xem toàn bộ các nguyên liệu hiện có trong "Tủ lạnh ảo" của mình, bao gồm tên, số lượng, đơn vị, ngày hết hạn và ghi chú.

**Trigger:** Người dùng đã đăng nhập truy cập trang "Tủ lạnh ảo" từ menu hoặc trang chủ.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Trong tủ có thể có hoặc chưa có nguyên liệu

**Post-Condition:**
- Danh sách nguyên liệu được hiển thị đầy đủ và cập nhật
- Người dùng có thể lọc, sắp xếp, tìm kiếm nguyên liệu
- Thống kê tổng quan được hiển thị (tổng số mục, sắp hết hạn, đã hết hạn)

**Basic Flow:**
1. Người dùng truy cập trang "Tủ lạnh ảo"
2. Hệ thống truy vấn kho nguyên liệu của người dùng
3. Hệ thống hiển thị danh sách theo bảng hoặc thẻ (cards) với các cột:
   - a. Tên nguyên liệu
   - b. Số lượng
   - c. Đơn vị
   - d. Ngày hết hạn (nếu có)
   - e. Trạng thái (Còn dùng/ Sắp hết hạn/ Đã hết hạn)
   - f. Ghi chú (nếu có)
   - g. Hành động (Sửa, Xóa)
4. Thanh công cụ cung cấp:
   - a. Tìm kiếm theo tên nguyên liệu (autocomplete)
   - b. Lọc theo trạng thái (Tất cả/ Sắp hết hạn/ Đã hết hạn)
   - c. Lọc theo danh mục (Rau củ, Thịt, Gia vị... nếu có)
   - d. Sắp xếp theo tên, ngày hết hạn, ngày thêm, số lượng
5. Hệ thống hiển thị màu cảnh báo cho nguyên liệu sắp hết hạn/đã hết hạn
6. Người dùng có thể nhấn "Thêm nguyên liệu" (UC5.1) hoặc "Cập nhật" (UC5.3) hoặc "Xóa" (UC5.4)

**Alternative Flow:**
- **Xem dạng lưới (grid) hoặc danh sách (list):**
  - Người dùng chuyển đổi view giữa grid và list
  - Hệ thống ghi nhớ lựa chọn cho lần sau

- **Xuất danh sách:**
  - Người dùng xuất danh sách ra PDF/Excel để in hoặc chia sẻ

- **Đồng bộ từ thiết bị khác:**
  - Dữ liệu được đồng bộ real-time nếu người dùng mở trên nhiều thiết bị

**Exception Flow:**
- **Chưa có nguyên liệu:**
  - Hệ thống hiển thị trang trống với thông báo: "Tủ lạnh ảo của bạn đang trống."
  - Gợi ý: "Nhấn 'Thêm nguyên liệu' để bắt đầu" và link đến UC5.1

- **Lỗi tải dữ liệu:**
  - Nếu xảy ra lỗi khi truy vấn dữ liệu
  - Hiển thị: "Không thể tải danh sách. Vui lòng thử lại sau."
  - Cung cấp nút "Làm mới"

- **Session hết hạn:**
  - Hệ thống chuyển hướng đến trang đăng nhập
  - Hiển thị: "Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại."

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị nguyên liệu của chính người dùng
  - Trạng thái hết hạn tính theo ngày hiện tại (FEFO)
  - Lưu lựa chọn sắp xếp/lọc của người dùng cho lần truy cập sau
  - Phân trang 50 mục/trang để tối ưu hiệu suất

- **Non-Functional Requirement:**
  - Performance: Thời gian tải danh sách < 2 giây cho 500 mục
  - Usability: Hỗ trợ bàn phím, truy cập nhanh, hiển thị cảnh báo rõ ràng
  - Security: Dữ liệu tách biệt theo người dùng
  - Reliability: Đồng bộ real-time không xung đột

**Priority:** Low  
**CRUD:** Read

# UCS05-3: Cập nhật số lượng nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng thay đổi số lượng hoặc đơn vị của một nguyên liệu đã có trong "Tủ lạnh ảo", giúp quản lý tồn kho chính xác theo thời gian.

**Trigger:** Người dùng nhấn nút "Sửa" hoặc biểu tượng chỉnh sửa trên một nguyên liệu trong danh sách tủ.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Nguyên liệu cần cập nhật tồn tại trong tủ của người dùng

**Post-Condition:**
- Số lượng/đơn vị/ngày hết hạn/ghi chú được cập nhật
- Lịch sử thay đổi được ghi lại (audit trail)
- Danh sách hiển thị được cập nhật real-time

**Basic Flow:**
1. Người dùng mở trang "Tủ lạnh ảo" (UC5.2)
2. Người dùng nhấn "Sửa" tại nguyên liệu cần cập nhật
3. Hệ thống hiển thị form chỉnh sửa với các trường:
   - a. Số lượng (>= 0)
   - b. Đơn vị (gram, kg, ml, lít, trái, bó...)
   - c. Ngày hết hạn (tùy chọn)
   - d. Ghi chú (tùy chọn)
4. Người dùng thay đổi thông tin và nhấn "Lưu"
5. Hệ thống validate dữ liệu (đảm bảo số lượng hợp lệ, đơn vị hợp lệ)
6. Hệ thống cập nhật bản ghi nguyên liệu trong CSDL
7. Hệ thống ghi lại lịch sử thay đổi (trước và sau)
8. Hệ thống cập nhật giao diện danh sách
9. Hiển thị thông báo: "Cập nhật thành công"

**Alternative Flow:**
- **Cộng trừ nhanh:**
  - Người dùng dùng nút + / - để thay đổi nhanh số lượng
  - Hệ thống cập nhật ngay (inline update)

- **Quy đổi đơn vị:**
  - Khi người dùng đổi đơn vị (vd: g -> kg)
  - Hệ thống tự động quy đổi theo tỉ lệ chuẩn (1000g = 1kg)

- **Cập nhật hàng loạt:**
  - Người dùng chọn nhiều nguyên liệu và cập nhật cùng lúc (vd: thêm 10%)
  - Hệ thống hiển thị preview trước khi xác nhận

**Exception Flow:**
- **Nguyên liệu không tồn tại:**
  - Nếu nguyên liệu đã bị xóa/không còn
  - Hiển thị: "Nguyên liệu không tồn tại"

- **Giá trị không hợp lệ:**
  - Nếu số lượng âm hoặc không phải số
  - Hiển thị lỗi và yêu cầu nhập lại

- **Xung đột cập nhật:**
  - Nếu nguyên liệu được cập nhật từ thiết bị khác cùng lúc
  - Hệ thống hiển thị cảnh báo xung đột và yêu cầu làm mới dữ liệu

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể cập nhật do lỗi hệ thống
  - Hiển thị: "Không thể cập nhật. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Số lượng phải >= 0, nếu = 0 có thể gợi ý xóa (UC5.4)
  - Đơn vị phải trong danh mục cho phép, có quy đổi chuẩn
  - Lưu lại lịch sử thay đổi để audit
  - Cảnh báo khi ngày hết hạn sắp tới (< 3 ngày)

- **Non-Functional Requirement:**
  - Performance: Cập nhật < 500ms
  - Usability: Hỗ trợ thao tác nhanh +/-, keyboard friendly
  - Security: Chỉ chủ tủ được cập nhật nguyên liệu của mình
  - Reliability: Xử lý xung đột cập nhật an toàn

**Priority:** Low  
**CRUD:** Update

# UCS05-4: Xóa nguyên liệu khỏi tủ [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xóa một nguyên liệu không còn sử dụng hoặc đã hết khỏi "Tủ lạnh ảo", giúp danh sách luôn gọn gàng và chính xác.

**Trigger:** Người dùng nhấn nút "Xóa" tại một nguyên liệu trong danh sách tủ hoặc chọn nhiều nguyên liệu để xóa hàng loạt.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Nguyên liệu tồn tại trong tủ của người dùng

**Post-Condition:**
- Nguyên liệu bị xóa khỏi danh sách tủ
- Lịch sử xóa được ghi lại (audit trail)
- Danh sách hiển thị được cập nhật real-time

**Basic Flow:**
1. Người dùng mở trang "Tủ lạnh ảo" (UC5.2)
2. Người dùng nhấn "Xóa" tại nguyên liệu cần loại bỏ
3. Hệ thống hiển thị hộp thoại xác nhận xóa:
   - a. Tên nguyên liệu
   - b. Số lượng và đơn vị
   - c. Ngày hết hạn (nếu có)
4. Người dùng nhấn "Xác nhận xóa"
5. Hệ thống xóa bản ghi nguyên liệu khỏi CSDL
6. Hệ thống ghi lại lịch sử xóa (thời gian, người thực hiện)
7. Hệ thống cập nhật giao diện danh sách
8. Hiển thị thông báo: "Đã xóa nguyên liệu"

**Alternative Flow:**
- **Xóa hàng loạt:**
  - Người dùng chọn nhiều nguyên liệu và nhấn "Xóa đã chọn"
  - Hệ thống hiển thị danh sách các nguyên liệu sẽ xóa
  - Người dùng xác nhận, hệ thống xóa tất cả

- **Xóa tự động khi hết hạn:**
  - Hệ thống có tùy chọn tự động xóa các mục "Đã hết hạn" sau X ngày
  - Người dùng bật/tắt trong cài đặt

- **Chuyển sang danh sách mua sắm:**
  - Trước khi xóa, hệ thống gợi ý thêm vào "Danh sách mua sắm"
  - Nếu đồng ý, thêm vào danh sách mua sắm và sau đó xóa khỏi tủ

**Exception Flow:**
- **Người dùng hủy xóa:**
  - Người dùng nhấn "Hủy" trong hộp thoại xác nhận
  - Không có thay đổi nào được thực hiện

- **Nguyên liệu không tồn tại:**
  - Nếu nguyên liệu đã bị xóa từ trước
  - Hệ thống hiển thị: "Nguyên liệu không tồn tại"
  - Làm mới danh sách

- **Lỗi xóa cơ sở dữ liệu:**
  - Nếu không thể xóa do lỗi hệ thống
  - Hiển thị: "Không thể xóa. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Nếu session hết hạn trong quá trình xóa
  - Hệ thống chuyển hướng đến trang đăng nhập

**Additional Information:**
- **Business Rule:**
  - Chỉ chủ sở hữu được xóa nguyên liệu trong tủ của mình
  - Có thể bật xác nhận xóa để tránh thao tác nhầm
  - Lưu lịch sử xóa để audit
  - Gợi ý chuyển sang danh sách mua sắm trước khi xóa

- **Non-Functional Requirement:**
  - Performance: Xóa < 500ms
  - Usability: Hộp thoại xác nhận rõ ràng, có chi tiết nguyên liệu
  - Security: Đảm bảo chỉ người dùng hợp lệ mới thao tác xóa
  - Reliability: Xóa an toàn, không để lại bản ghi mồ côi

**Priority:** Low  
**CRUD:** Delete

# UCS06-1: Tạo kế hoạch bữa ăn [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng tạo một kế hoạch bữa ăn mới (theo ngày/tuần/tháng), thêm các món ăn vào lịch theo ngày và bữa (sáng, trưa, tối), nhằm quản lý thực đơn khoa học và tiện lợi.

**Trigger:** Người dùng nhấn nút "Tạo kế hoạch" từ module "Kế hoạch bữa ăn".

**Pre-Condition:**
- Người dùng đã đăng nhập thành công vào hệ thống
- Session của người dùng đang hoạt động và hợp lệ
- Hệ thống có dữ liệu công thức để lựa chọn

**Post-Condition:**
- Một kế hoạch bữa ăn mới được lưu với các món đã chọn theo ngày/bữa
- Thời gian tạo kế hoạch được ghi lại
- Người dùng có thể xem/chỉnh sửa/xóa kế hoạch

**Basic Flow:**
1. Người dùng mở module "Kế hoạch bữa ăn"
2. Người dùng nhấn "Tạo kế hoạch"
3. Hệ thống hiển thị form tạo kế hoạch gồm:
   - a. Tên kế hoạch (tùy chọn)
   - b. Khoảng thời gian (từ ngày - đến ngày)
   - c. Chu kỳ hiển thị (Ngày/Tuần/Tháng)
   - d. Số khẩu phần mặc định
4. Người dùng chọn ngày trên lịch và thêm món ăn cho từng bữa:
   - a. Sáng, Trưa, Tối (có thể thêm bữa phụ)
   - b. Tìm và chọn món từ tìm kiếm (UC2.1/UC2.2)
   - c. Tùy chỉnh khẩu phần cho từng món
5. Hệ thống hiển thị preview kế hoạch theo lịch
6. Người dùng nhấn "Lưu kế hoạch"
7. Hệ thống validate dữ liệu (khoảng thời gian hợp lệ, có ít nhất 1 món)
8. Hệ thống lưu kế hoạch, gắn với người dùng
9. Hiển thị thông báo: "Đã tạo kế hoạch bữa ăn thành công"

**Alternative Flow:**
- **Tạo từ template có sẵn:**
  - Người dùng chọn một template thực đơn mẫu (VD: Eat Clean 7 ngày)
  - Hệ thống tự điền các món theo ngày/bữa
  - Người dùng có thể chỉnh sửa trước khi lưu

- **Tạo từ danh sách yêu thích:**
  - Người dùng chọn nhanh các món từ UC4 (Yêu thích)
  - Kéo/thả vào lịch để sắp xếp

- **Tạo bằng AI gợi ý thực đơn:**
  - Người dùng nhập mục tiêu (giảm cân, tăng cơ) và số ngày
  - Hệ thống gọi AI gợi ý thực đơn theo mục tiêu
  - Người dùng xem và chỉnh sửa trước khi lưu

**Exception Flow:**
- **Khoảng thời gian không hợp lệ:**
  - Nếu ngày kết thúc trước ngày bắt đầu
  - Hệ thống hiển thị lỗi và yêu cầu chọn lại

- **Không có món nào trong kế hoạch:**
  - Nếu người dùng chưa thêm món nào
  - Hệ thống yêu cầu thêm ít nhất 1 món

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu không thể lưu kế hoạch
  - Hiển thị: "Không thể lưu kế hoạch. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Nếu session hết hạn
  - Chuyển hướng đăng nhập và giữ bản nháp kế hoạch

**Additional Information:**
- **Business Rule:**
  - Một kế hoạch phải có ít nhất 1 món
  - Hỗ trợ nhiều bữa/ngày, có thể thêm bữa phụ
  - Khẩu phần có thể tùy chỉnh theo bữa hoặc theo kế hoạch
  - Có thể lưu thành template để tái sử dụng

- **Non-Functional Requirement:**
  - Performance: Lưu kế hoạch < 2 giây
  - Usability: Giao diện kéo/thả, xem theo lịch tuần/tháng
  - Security: Kế hoạch riêng tư theo người dùng
  - Reliability: Tự động lưu nháp định kỳ

**Priority:** Medium  
**CRUD:** Create

# UCS06-2: Xem kế hoạch bữa ăn [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xem lại kế hoạch bữa ăn đã tạo theo dạng lịch (ngày/tuần/tháng), với đầy đủ thông tin món ăn theo từng bữa và các thao tác nhanh.

**Trigger:** Người dùng truy cập module "Kế hoạch bữa ăn" và chọn một kế hoạch trong danh sách.

**Pre-Condition:**
- Người dùng đã đăng nhập thành công
- Đã có ít nhất một kế hoạch bữa ăn được tạo (hoặc hiển thị trang trống nếu chưa có)

**Post-Condition:**
- Kế hoạch được hiển thị đầy đủ theo thời gian đã chọn
- Người dùng có thể thực hiện các thao tác nhanh: xem chi tiết món, đổi bữa, xóa món

**Basic Flow:**
1. Người dùng mở module "Kế hoạch bữa ăn"
2. Hệ thống hiển thị danh sách các kế hoạch gần đây
3. Người dùng chọn một kế hoạch để xem
4. Hệ thống hiển thị kế hoạch theo dạng lịch:
   - a. Chọn chế độ xem: Ngày/Tuần/Tháng
   - b. Mỗi ô hiển thị các bữa: Sáng/Trưa/Tối/Bữa phụ
   - c. Mỗi món hiển thị tên, ảnh thumbnail, thời gian nấu, độ khó
5. Người dùng có thể click vào món để xem chi tiết (UC2.5)
6. Người dùng có thể dùng thao tác kéo/thả để đổi vị trí món giữa các bữa (không lưu ngay)
7. Có các thao tác nhanh:
   - a. Đổi bữa (move)
   - b. Xóa món (remove)
   - c. Sao chép món sang ngày khác (copy)
8. Có bộ lọc hiển thị:
   - a. Theo bữa (chỉ hiển thị bữa Trưa, v.v.)
   - b. Theo danh mục món
   - c. Theo độ khó
9. Nút "Chỉnh sửa" để chuyển sang UC6.3

**Alternative Flow:**
- **Xem nhiều kế hoạch:**
  - Người dùng có thể chọn so sánh 2 kế hoạch song song

- **In kế hoạch:**
  - Xuất ra PDF phiên bản tối ưu để in

- **Chia sẻ kế hoạch:**
  - Tạo link chia sẻ để gia đình cùng xem

**Exception Flow:**
- **Chưa có kế hoạch:**
  - Hiển thị: "Bạn chưa có kế hoạch bữa ăn nào"
  - Nút "Tạo kế hoạch" (UC6.1)

- **Lỗi tải dữ liệu:**
  - Hiển thị: "Không thể tải kế hoạch. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Chuyển hướng đăng nhập, giữ ngữ cảnh kế hoạch

**Additional Information:**
- **Business Rule:**
  - Chỉ hiển thị kế hoạch của chính người dùng
  - Dữ liệu món ăn được lấy snapshot tại thời điểm lưu kế hoạch để đảm bảo nhất quán

- **Non-Functional Requirement:**
  - Performance: Tải kế hoạch < 2 giây
  - Usability: Lịch tương tác mượt, hỗ trợ kéo/thả
  - Security: Kế hoạch là dữ liệu riêng tư

**Priority:** Low  
**CRUD:** Read

# UCS06-3: Chỉnh sửa kế hoạch bữa ăn [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng thay đổi, di chuyển hoặc thay thế các món ăn trong kế hoạch bữa ăn đã tạo, áp dụng cho một ngày/bữa cụ thể hoặc cho cả phạm vi ngày.

**Trigger:** Người dùng nhấn nút "Chỉnh sửa" khi đang xem kế hoạch (UC6.2) hoặc từ danh sách kế hoạch.

**Pre-Condition:**
- Người dùng đã đăng nhập
- Kế hoạch bữa ăn tồn tại và thuộc về người dùng

**Post-Condition:**
- Kế hoạch được cập nhật theo các thay đổi
- Lịch sử chỉnh sửa được ghi lại (nếu có)

**Basic Flow:**
1. Người dùng mở kế hoạch bữa ăn (UC6.2) và nhấn "Chỉnh sửa"
2. Hệ thống chuyển sang chế độ chỉnh sửa (edit mode)
3. Người dùng thực hiện các thao tác:
   - a. Kéo/thả món giữa các bữa hoặc ngày
   - b. Thay thế món: tìm món khác và thay
   - c. Thêm món mới vào bữa
   - d. Xóa món khỏi bữa
   - e. Đổi khẩu phần cho món/bữa
4. Hệ thống hiển thị preview thay đổi ngay trên lịch
5. Người dùng nhấn "Lưu thay đổi"
6. Hệ thống validate dữ liệu và cập nhật kế hoạch
7. Hiển thị thông báo: "Cập nhật kế hoạch thành công"

**Alternative Flow:**
- **Chỉnh sửa theo batch:**
  - Áp dụng thay đổi cho nhiều ngày cùng lúc (VD: thay tất cả bữa trưa bằng món X)

- **Hoàn nguyên (Undo/Redo):**
  - Hỗ trợ hoàn nguyên thao tác gần nhất

- **Sao chép kế hoạch:**
  - Tạo một bản sao kế hoạch để chỉnh sửa mà không ảnh hưởng bản gốc

**Exception Flow:**
- **Xung đột dữ liệu:**
  - Nếu kế hoạch được chỉnh sửa đồng thời trên thiết bị khác
  - Hệ thống cảnh báo và yêu cầu tải lại kế hoạch

- **Lỗi lưu cơ sở dữ liệu:**
  - Nếu cập nhật thất bại
  - Hiển thị: "Không thể cập nhật. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Tự động lưu nháp và yêu cầu đăng nhập lại

**Additional Information:**
- **Business Rule:**
  - Sau chỉnh sửa vẫn phải đảm bảo cấu trúc hợp lệ (ngày/bữa)
  - Khẩu phần có thể được kế thừa từ thiết lập kế hoạch

- **Non-Functional Requirement:**
  - Performance: Lưu thay đổi < 1 giây
  - Usability: Kéo/thả mượt, có Undo/Redo
  - Reliability: Tự động lưu nháp định kỳ

**Priority:** Medium  
**CRUD:** Update

# UCS06-4: Xóa kế hoạch bữa ăn [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép người dùng xóa một kế hoạch bữa ăn đã tạo khi không còn sử dụng, giúp quản lý danh sách kế hoạch gọn gàng.

**Trigger:** Người dùng nhấn nút "Xóa" tại một kế hoạch trong danh sách kế hoạch hoặc khi đang xem chi tiết kế hoạch.

**Pre-Condition:**
- Người dùng đã đăng nhập
- Kế hoạch tồn tại và thuộc về người dùng

**Post-Condition:**
- Kế hoạch bị xóa khỏi hệ thống
- Danh sách kế hoạch được cập nhật

**Basic Flow:**
1. Người dùng mở danh sách kế hoạch bữa ăn
2. Nhấn biểu tượng "Xóa" tại kế hoạch cần xóa
3. Hệ thống hiển thị hộp thoại xác nhận xóa với tên kế hoạch và khoảng thời gian
4. Người dùng nhấn "Xác nhận xóa"
5. Hệ thống xóa kế hoạch khỏi CSDL
6. Hiển thị thông báo: "Đã xóa kế hoạch"

**Alternative Flow:**
- **Xóa hàng loạt:**
  - Chọn nhiều kế hoạch và xóa cùng lúc

- **Lưu trữ (Archive) thay vì xóa:**
  - Đưa kế hoạch vào danh sách lưu trữ để tham khảo sau

**Exception Flow:**
- **Người dùng hủy xóa:**
  - Nhấn "Hủy" trong hộp thoại xác nhận

- **Kế hoạch không tồn tại:**
  - Hiển thị: "Kế hoạch không tồn tại hoặc đã bị xóa"

- **Lỗi xóa cơ sở dữ liệu:**
  - Hiển thị: "Không thể xóa. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Chỉ chủ sở hữu được xóa kế hoạch của mình
  - Có thể yêu cầu nhập lại tên kế hoạch nếu cần xác nhận đặc biệt

- **Non-Functional Requirement:**
  - Performance: Xóa < 1 giây
  - Usability: Hộp thoại xác nhận rõ ràng
  - Security: Đảm bảo chỉ người có quyền mới thao tác xóa

**Priority:** Low  
**CRUD:** Delete

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

# UCA01-1: Xem danh sách người dùng [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem danh sách tất cả người dùng trong hệ thống, kèm tìm kiếm, lọc, phân trang để quản trị hiệu quả.

**Trigger:** Admin truy cập mục "Quản lý Người dùng" trong bảng điều khiển quản trị.

**Pre-Condition:**
- Admin đã đăng nhập bằng tài khoản quản trị
- Admin có quyền "User.Read"

**Post-Condition:**
- Danh sách người dùng được hiển thị theo các tiêu chí lọc/sắp xếp
- Admin có thể chuyển sang xem chi tiết một người dùng

**Basic Flow:**
1. Admin mở trang "Quản lý Người dùng"
2. Hệ thống truy vấn danh sách người dùng (theo trang)
3. Hệ thống hiển thị bảng gồm các cột:
   - a. ID
   - b. Ảnh đại diện
   - c. Tên hiển thị
   - d. Email
   - e. Ngày tham gia
   - f. Trạng thái (Hoạt động/Bị khóa)
4. Thanh công cụ gồm:
   - a. Ô tìm kiếm theo tên/email
   - b. Bộ lọc trạng thái (Tất cả/Hoạt động/Bị khóa)
   - c. Bộ lọc thời gian tham gia
   - d. Sắp xếp theo tên/ngày tham gia
5. Hệ thống hiển thị phân trang (mặc định 20 người dùng/trang)
6. Admin nhấp vào hàng người dùng để xem chi tiết (UCA01-2)

**Alternative Flow:**
- **Xuất danh sách:**
  - Admin xuất danh sách ra CSV/Excel

- **Lưu bộ lọc:**
  - Lưu bộ lọc thường dùng cho lần truy cập sau

**Exception Flow:**
- **Không có dữ liệu:**
  - Hiển thị: "Không tìm thấy người dùng nào phù hợp"

- **Lỗi tải dữ liệu:**
  - Hiển thị: "Không thể tải danh sách người dùng. Vui lòng thử lại sau."

- **Session hết hạn:**
  - Chuyển hướng đến trang đăng nhập quản trị

**Additional Information:**
- **Business Rule:**
  - Chỉ tài khoản có quyền thích hợp mới truy cập được trang này
  - Phân trang 20 mục/trang, tối đa tải 1000 mục khi xuất file
  - Tìm kiếm không phân biệt hoa thường, hỗ trợ không dấu

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây cho 10k người dùng (server-side paging)
  - Usability: Bảng có cố định header, cột co giãn
  - Security: Endpoint chỉ dành cho admin; audit truy cập

**Priority:** Low  
**CRUD:** Read

# UCA01-2: Xem chi tiết người dùng [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem thông tin chi tiết của một người dùng, bao gồm hồ sơ tài khoản, thống kê hoạt động và lịch sử gần đây.

**Trigger:** Admin nhấp vào một người dùng trong danh sách (UCA01-1) hoặc dùng chức năng tìm kiếm để mở trang chi tiết.

**Pre-Condition:**
- Admin đã đăng nhập bằng tài khoản quản trị
- Admin có quyền "User.Read"
- Người dùng mục tiêu tồn tại trong hệ thống

**Post-Condition:**
- Thông tin chi tiết người dùng được hiển thị đầy đủ
- Admin có thể chuyển sang các thao tác quản trị khác (khóa/mở khóa)

**Basic Flow:**
1. Admin từ trang danh sách người dùng (UCA01-1) chọn một người dùng
2. Hệ thống truy vấn dữ liệu chi tiết của người dùng gồm:
   - a. Thông tin tài khoản: Ảnh đại diện, Tên hiển thị, Email, ID, Ngày đăng ký, Lần đăng nhập cuối
   - b. Trạng thái tài khoản: Hoạt động/Bị khóa, Lý do khóa (nếu có)
   - c. Thống kê hoạt động: Tổng số công thức đã đăng, đã duyệt, bị từ chối, tổng số bình luận
   - d. Lịch sử hoạt động gần đây: Danh sách công thức đã đăng gần nhất (link đến chi tiết)
3. Hệ thống hiển thị trang chi tiết với bố cục rõ ràng
4. Admin có thể nhấn nút "Khóa/Mở khóa" tài khoản (chuyển sang UCA01-3/UCA01-4)
5. Admin có thể nhấn vào từng mục lịch sử để mở trang chi tiết liên quan

**Alternative Flow:**
- **Mở trực tiếp bằng ID:**
  - Admin nhập ID người dùng vào ô tìm kiếm nhanh
  - Hệ thống mở thẳng trang chi tiết

- **Xuất báo cáo người dùng:**
  - Admin xuất thông tin người dùng ra PDF để lưu trữ

**Exception Flow:**
- **Người dùng không tồn tại:**
  - Hiển thị: "Người dùng không tồn tại hoặc đã bị xóa"

- **Thiếu quyền:**
  - Hiển thị: "Bạn không có quyền xem chi tiết người dùng"

- **Lỗi tải dữ liệu:**
  - Hiển thị: "Không thể tải thông tin người dùng. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Dữ liệu hiển thị phải tuân thủ bảo vệ thông tin cá nhân (PII)
  - Chỉ Admin/Super Admin mới xem được thống kê đầy đủ

- **Non-Functional Requirement:**
  - Performance: Tải trang chi tiết < 2 giây
  - Usability: Bố cục rõ ràng, có breadcrumb quay lại danh sách
  - Security: Audit khi truy cập trang chi tiết

**Priority:** Low  
**CRUD:** Read

# UCA01-3: Vô hiệu hóa tài khoản người dùng [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin khóa tài khoản của người dùng vi phạm để ngăn họ đăng nhập và sử dụng hệ thống, với quy trình xác nhận và lưu lý do.

**Trigger:** Admin nhấn nút "Khóa" tại trang danh sách người dùng (UCA01-1) hoặc trang chi tiết người dùng (UCA01-2).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "User.Disable"
- Tài khoản người dùng mục tiêu tồn tại và đang ở trạng thái Hoạt động

**Post-Condition:**
- Trạng thái tài khoản chuyển sang "Bị khóa"
- Người dùng không thể đăng nhập vào hệ thống
- Lý do khóa được lưu lại, có thể hiển thị cho người dùng khi đăng nhập thất bại

**Basic Flow:**
1. Admin nhấn "Khóa" tại người dùng mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận với các trường:
   - a. Lý do khóa (bắt buộc, tối đa 255 ký tự)
   - b. Thời hạn khóa (tùy chọn: 15 phút, 1 ngày, vĩnh viễn)
3. Admin nhập lý do và chọn thời hạn
4. Admin nhấn "Xác nhận"
5. Hệ thống cập nhật trạng thái tài khoản thành "Bị khóa" và lưu lý do/thời hạn
6. Hệ thống đăng xuất tất cả session hiện tại của tài khoản
7. Hệ thống ghi log audit (ai khóa, khi nào, lý do)
8. Hiển thị thông báo: "Đã khóa tài khoản thành công"

**Alternative Flow:**
- **Khóa tạm thời theo chính sách:**
  - Hệ thống có preset thời hạn (15m, 1h, 24h)

- **Khóa hàng loạt:**
  - Admin chọn nhiều người dùng và thực hiện khóa cùng lúc
  - Hệ thống lưu lý do đồng nhất hoặc riêng cho từng người dùng

- **Thông báo đến người dùng:**
  - Gửi email thông báo tài khoản bị khóa và lý do

**Exception Flow:**
- **Thiếu quyền:**
  - Hiển thị: "Bạn không có quyền khóa tài khoản"

- **Tài khoản đã bị khóa:**
  - Hiển thị: "Tài khoản đã ở trạng thái bị khóa"

- **Lỗi cập nhật CSDL:**
  - Hiển thị: "Không thể cập nhật trạng thái. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Khóa tài khoản phải ghi nhận lý do rõ ràng
  - Khi hết thời hạn khóa tạm thời, hệ thống tự mở khóa
  - Tài khoản bị khóa không thể thực hiện bất kỳ hành động yêu cầu đăng nhập

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ các hành động khóa/mở khóa
  - Reliability: Đảm bảo đăng xuất toàn bộ session ngay lập tức
  - Performance: Cập nhật trạng thái < 1 giây

**Priority:** Medium  
**CRUD:** Update

# UCA01-4: Kích hoạt lại tài khoản người dùng [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin mở khóa (kích hoạt lại) tài khoản người dùng đã bị khóa trước đó để họ có thể đăng nhập và sử dụng hệ thống bình thường.

**Trigger:** Admin nhấn nút "Mở khóa" tại trang danh sách người dùng (UCA01-1) hoặc trang chi tiết người dùng (UCA01-2).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "User.Enable"
- Tài khoản người dùng mục tiêu tồn tại và đang ở trạng thái "Bị khóa"

**Post-Condition:**
- Trạng thái tài khoản chuyển sang "Hoạt động"
- Người dùng có thể đăng nhập lại bình thường
- Lý do mở khóa được ghi log (nếu có)

**Basic Flow:**
1. Admin nhấn "Mở khóa" tại tài khoản mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận mở khóa
3. Admin có thể nhập ghi chú (tùy chọn)
4. Admin nhấn "Xác nhận"
5. Hệ thống cập nhật trạng thái tài khoản sang "Hoạt động"
6. Hệ thống ghi log audit (ai mở, khi nào, ghi chú)
7. Hiển thị thông báo: "Đã mở khóa tài khoản thành công"

**Alternative Flow:**
- **Tự động mở khóa khi hết hạn:**
  - Nếu tài khoản bị khóa tạm thời, hệ thống tự động mở khóa khi đến hạn

- **Mở khóa hàng loạt:**
  - Admin chọn nhiều tài khoản để mở khóa cùng lúc

- **Thông báo đến người dùng:**
  - Gửi email thông báo đã mở khóa tài khoản

**Exception Flow:**
- **Thiếu quyền:**
  - Hiển thị: "Bạn không có quyền mở khóa tài khoản"

- **Tài khoản đang hoạt động:**
  - Hiển thị: "Tài khoản đang ở trạng thái hoạt động"

- **Lỗi cập nhật CSDL:**
  - Hiển thị: "Không thể cập nhật trạng thái. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Mọi thao tác mở khóa phải được audit
  - Nếu tài khoản bị khóa do vi phạm nghiêm trọng, chỉ Super Admin mới được mở khóa

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ, phân quyền rõ ràng
  - Performance: Cập nhật trạng thái < 1 giây
  - Reliability: Trạng thái đồng bộ trên tất cả phiên

**Priority:** Medium  
**CRUD:** Update

# UCA02-1: Xem danh sách công thức hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem, tìm kiếm và lọc danh sách tất cả công thức có trên hệ thống (bao gồm công thức hệ thống và công thức người dùng đã được duyệt), có phân trang và sắp xếp.

**Trigger:** Admin truy cập mục "Quản lý Công thức" trong bảng điều khiển quản trị.

**Pre-Condition:**
- Admin đã đăng nhập bằng tài khoản quản trị
- Admin có quyền "Recipe.Read"

**Post-Condition:**
- Danh sách công thức được hiển thị theo tiêu chí lọc/sắp xếp
- Admin có thể chuyển sang xem/sửa/xóa công thức

**Basic Flow:**
1. Admin mở trang "Quản lý Công thức"
2. Hệ thống truy vấn danh sách công thức (server-side paging)
3. Hệ thống hiển thị bảng gồm các cột:
   - a. ID
   - b. Tên công thức
   - c. Người tạo (Hệ thống/User: Tên)
   - d. Trạng thái (Đã duyệt/Chờ duyệt/Bị từ chối)
   - e. Ngày tạo
   - f. Điểm đánh giá trung bình
4. Thanh công cụ gồm:
   - a. Ô tìm kiếm theo tên
   - b. Bộ lọc theo trạng thái
   - c. Bộ lọc theo người tạo (Hệ thống/User)
   - d. Sắp xếp theo ngày tạo/điểm đánh giá/tên
5. Hệ thống hiển thị phân trang (20 công thức/trang)
6. Admin nhấp vào một công thức để xem chi tiết/sửa/xóa (UCA02-3/UCA02-4)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV/Excel toàn bộ công thức theo bộ lọc hiện tại

- **Lưu bộ lọc thường dùng:** Lưu preset lọc/sắp xếp

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Không tìm thấy công thức nào phù hợp"

- **Lỗi tải dữ liệu:** Hiển thị "Không thể tải danh sách. Vui lòng thử lại sau."

**Additional Information:**
- **Business Rule:**
  - Chỉ Admin/Super Admin có thể truy cập
  - Phân trang 20 mục/trang, xuất tối đa 5000 dòng
  - Tìm kiếm không dấu, không phân biệt hoa thường

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây với 100k công thức (paging)
  - Usability: Bảng có cố định header, cột tuỳ biến
  - Security: Audit truy cập quản trị

**Priority:** Low  
**CRUD:** Read

# UCA02-2: Thêm công thức hệ thống [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin tạo một công thức mới với tư cách là nguồn chính thức của hệ thống. Công thức do Admin tạo mặc định ở trạng thái "Đã duyệt".

**Trigger:** Admin nhấn nút "Thêm công thức mới" trong mục "Quản lý Công thức".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Create"
- Có danh mục/đơn vị/nguyên liệu chuẩn hoá để lựa chọn

**Post-Condition:**
- Công thức mới được lưu vào CSDL với trạng thái "Đã duyệt"
- Công thức xuất hiện trong danh sách công khai

**Basic Flow:**
1. Admin nhấn "Thêm công thức mới"
2. Hệ thống hiển thị form nhập liệu chi tiết:
   - a. Tên món, mô tả ngắn
   - b. Ảnh/video đại diện
   - c. Danh mục món ăn
   - d. Thời gian chuẩn bị/nấu, độ khó, khẩu phần
   - e. Danh sách nguyên liệu (tên, định lượng, đơn vị)
   - f. Các bước thực hiện (có thể kèm ảnh)
3. Admin nhập đầy đủ thông tin và nhấn "Lưu/Xuất bản"
4. Hệ thống validate dữ liệu, upload media nếu có
5. Hệ thống lưu công thức với trạng thái "Đã duyệt" và gắn Admin là người tạo
6. Hiển thị thông báo: "Đã tạo công thức thành công"

**Alternative Flow:**
- **Lưu nháp:** Lưu ở trạng thái "Nháp" để chỉnh sửa sau

- **Nhập từ file:** Import từ file JSON/CSV theo format chuẩn

- **Sao chép từ công thức có sẵn:** Copy và chỉnh sửa nội dung

**Exception Flow:**
- **Thiếu thông tin bắt buộc:** Hiển thị danh sách trường thiếu và yêu cầu bổ sung

- **Ảnh/video không hợp lệ:** Báo lỗi kích thước/định dạng

- **Lỗi lưu CSDL:** Thông báo lỗi và không tạo công thức

**Additional Information:**
- **Business Rule:**
  - Tên món phải duy nhất trong phạm vi công thức hệ thống
  - Phải có tối thiểu 3 nguyên liệu và 3 bước
  - Media phải đạt chuẩn kích thước/định dạng

- **Non-Functional Requirement:**
  - Performance: Lưu < 3 giây (không tính upload lớn)
  - Security: Chỉ Admin/Super Admin được phép
  - Reliability: Lưu nháp tự động định kỳ

**Priority:** Medium  
**CRUD:** Create

# UCA02-3: Sửa công thức hệ thống [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin chỉnh sửa thông tin của một công thức bất kỳ trên hệ thống, bao gồm nội dung, ảnh, nguyên liệu và bước thực hiện.

**Trigger:** Admin chọn một công thức và nhấn nút "Sửa" trong mục "Quản lý Công thức" (UCA02-1).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Update"
- Công thức tồn tại trong hệ thống

**Post-Condition:**
- Các thay đổi của công thức được lưu lại
- Thời gian cập nhật được ghi nhận

**Basic Flow:**
1. Admin mở chi tiết công thức và nhấn "Sửa"
2. Hệ thống hiển thị form với dữ liệu đã điền sẵn
3. Admin chỉnh sửa các thông tin cần thiết:
   - a. Tên món, mô tả, ảnh/video
   - b. Danh mục, thời gian, độ khó, khẩu phần
   - c. Danh sách nguyên liệu (thêm/bớt/sửa)
   - d. Các bước thực hiện (thêm/bớt/sửa/sắp xếp)
4. Admin nhấn "Lưu"
5. Hệ thống validate và cập nhật vào CSDL
6. Hiển thị thông báo: "Cập nhật công thức thành công"

**Alternative Flow:**
- **Lưu nháp:** Lưu thay đổi ở trạng thái nháp

- **Versioning:** Lưu phiên bản cũ để có thể hoàn nguyên

**Exception Flow:**
- **Không có quyền:** Thông báo "Bạn không có quyền sửa công thức"

- **Dữ liệu không hợp lệ:** Hiển thị danh sách lỗi validation

- **Lỗi CSDL:** Thông báo lỗi, không cập nhật thay đổi

**Additional Information:**
- **Business Rule:**
  - Thay đổi phải đảm bảo tối thiểu 3 nguyên liệu và 3 bước
  - Ảnh/video phải đạt tiêu chuẩn

- **Non-Functional Requirement:**
  - Usability: Giữ nguyên layout và validation giống khi tạo mới
  - Reliability: Lưu lịch sử thay đổi để audit

**Priority:** Medium  
**CRUD:** Update

# UCA02-4: Xóa công thức hệ thống [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xóa vĩnh viễn một công thức ra khỏi hệ thống, với xác nhận để tránh xóa nhầm.

**Trigger:** Admin chọn một công thức và nhấn nút "Xóa" trong mục "Quản lý Công thức" (UCA02-1).

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Delete"
- Công thức tồn tại trong hệ thống

**Post-Condition:**
- Công thức bị xóa vĩnh viễn khỏi CSDL
- Không còn xuất hiện trong tìm kiếm công khai

**Basic Flow:**
1. Admin nhấn "Xóa" tại công thức mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận với tên công thức
3. Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa công thức khỏi CSDL và file media liên quan
5. Hiển thị thông báo: "Đã xóa công thức"

**Alternative Flow:**
- **Soft Delete:** Đánh dấu xóa và ẩn khỏi tìm kiếm, có thể khôi phục trong 30 ngày

- **Xóa hàng loạt:** Chọn nhiều công thức và xóa cùng lúc

**Exception Flow:**
- **Thiếu quyền:** Hiển thị "Bạn không có quyền xóa công thức"

- **Công thức không tồn tại:** Thông báo "Công thức không tồn tại hoặc đã bị xóa"

- **Lỗi CSDL/Storage:** Thông báo lỗi, đề nghị thử lại

**Additional Information:**
- **Business Rule:**
  - Xóa phải kèm xác nhận; với công thức có tương tác cao, yêu cầu nhập lại tên để xác nhận

- **Non-Functional Requirement:**
  - Security: Audit hành động xóa
  - Reliability: Xóa an toàn, không để dữ liệu mồ côi

**Priority:** Low  
**CRUD:** Delete

# UCA02-5: Xem danh sách công thức chờ duyệt [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem danh sách các công thức do người dùng gửi lên đang chờ duyệt, kèm tìm kiếm, lọc và phân trang.

**Trigger:** Admin truy cập mục "Kiểm duyệt" hoặc bộ lọc trạng thái = "Chờ duyệt" trong "Quản lý Công thức".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Moderate"

**Post-Condition:**
- Danh sách công thức chờ duyệt được hiển thị
- Admin có thể mở chi tiết để phê duyệt/từ chối

**Basic Flow:**
1. Admin mở trang "Công thức chờ duyệt"
2. Hệ thống truy vấn danh sách công thức có trạng thái "Chờ duyệt"
3. Hệ thống hiển thị các cột:
   - a. ID, Tên công thức
   - b. Người gửi (User)
   - c. Ngày gửi
   - d. Trạng thái hiện tại
4. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên
   - b. Lọc theo người gửi
   - c. Sắp xếp theo ngày gửi
5. Admin nhấp vào một công thức để xem chi tiết và thực hiện phê duyệt (UCA02-6) hoặc từ chối (UCA02-7)

**Alternative Flow:**
- **Phân công kiểm duyệt viên:** Gán công thức cho kiểm duyệt viên cụ thể

- **Xuất danh sách chờ duyệt:** Xuất CSV để tổng hợp

**Exception Flow:**
- **Không có công thức chờ duyệt:** Hiển thị: "Không có công thức nào trong hàng đợi"

- **Lỗi tải dữ liệu:** Thông báo lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Chỉ công thức do người dùng gửi mới có trạng thái "Chờ duyệt"
  - Mỗi công thức chỉ được duyệt bởi 1 admin tại một thời điểm (lock)

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây
  - Security: Chỉ người có quyền kiểm duyệt mới truy cập được

**Priority:** Low  
**CRUD:** Read

# UCA02-6: Phê duyệt công thức [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin duyệt và cho phép công thức do người dùng gửi lên được hiển thị công khai trên hệ thống sau khi kiểm tra chất lượng nội dung.

**Trigger:** Admin mở chi tiết một công thức đang ở trạng thái "Chờ duyệt" và nhấn nút "Phê duyệt".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Moderate"
- Công thức đang ở trạng thái "Chờ duyệt"

**Post-Condition:**
- Trạng thái công thức chuyển sang "Đã duyệt"
- Công thức hiển thị công khai trong hệ thống
- Người dùng gửi công thức nhận thông báo đã được duyệt

**Basic Flow:**
1. Admin mở chi tiết công thức chờ duyệt (UCA02-5)
2. Hệ thống hiển thị đầy đủ nội dung: tên, mô tả, ảnh/video, nguyên liệu, bước thực hiện
3. Admin kiểm tra chất lượng nội dung (độ rõ ràng, phù hợp, không vi phạm)
4. Admin nhấn "Phê duyệt"
5. Hệ thống cập nhật trạng thái công thức thành "Đã duyệt"
6. Hệ thống ghi lại log kiểm duyệt (ai duyệt, thời gian)
7. Hệ thống gửi thông báo cho người dùng gửi công thức
8. Hiển thị thông báo: "Đã phê duyệt công thức"

**Alternative Flow:**
- **Chỉnh sửa nhẹ trước khi duyệt:** Admin chỉnh sửa lỗi chính tả/định dạng nhỏ rồi duyệt

- **Phân công kiểm duyệt:** Gán cho kiểm duyệt viên khác nếu cần chuyên môn

**Exception Flow:**
- **Thiếu quyền:** Thông báo "Bạn không có quyền phê duyệt"

- **Trạng thái không hợp lệ:** Công thức không ở "Chờ duyệt" -> thông báo lỗi

- **Lỗi cập nhật CSDL:** Thông báo lỗi, không thay đổi trạng thái

**Additional Information:**
- **Business Rule:**
  - Nội dung phải tuân thủ quy định cộng đồng và pháp luật
  - Ảnh/video phải phù hợp, không phản cảm

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ các hành động kiểm duyệt
  - Reliability: Trạng thái đồng bộ tức thời

**Priority:** Medium  
**CRUD:** Update

# UCA02-7: Từ chối công thức [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin từ chối công thức do người dùng gửi lên khi nội dung không đạt yêu cầu, đồng thời gửi lý do từ chối cho người gửi.

**Trigger:** Admin mở chi tiết công thức ở trạng thái "Chờ duyệt" và nhấn "Từ chối".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Recipe.Moderate"
- Công thức ở trạng thái "Chờ duyệt"

**Post-Condition:**
- Trạng thái công thức chuyển sang "Bị từ chối"
- Lý do từ chối được lưu và gửi cho người dùng
- Công thức không được công khai

**Basic Flow:**
1. Admin mở chi tiết công thức chờ duyệt (UCA02-5)
2. Admin nhấn nút "Từ chối"
3. Hệ thống hiển thị hộp thoại yêu cầu nhập lý do từ chối
4. Admin nhập lý do (bắt buộc) và nhấn "Xác nhận"
5. Hệ thống cập nhật trạng thái công thức thành "Bị từ chối"
6. Hệ thống lưu lý do từ chối và ghi log kiểm duyệt
7. Hệ thống gửi thông báo cho người dùng kèm lý do từ chối
8. Hiển thị thông báo: "Đã từ chối công thức"

**Alternative Flow:**
- **Mẫu lý do nhanh:** Cung cấp các mẫu lý do phổ biến để chọn nhanh (VD: Hình ảnh mờ, Hướng dẫn không rõ ràng)

- **Gợi ý chỉnh sửa:** Đề xuất gợi ý để người dùng chỉnh sửa và gửi lại

**Exception Flow:**
- **Thiếu quyền:** Hiển thị: "Bạn không có quyền từ chối"

- **Trạng thái không hợp lệ:** Công thức không ở trạng thái "Chờ duyệt"

- **Lỗi cập nhật CSDL:** Thông báo lỗi, không đổi trạng thái

**Additional Information:**
- **Business Rule:**
  - Lý do từ chối phải rõ ràng, tôn trọng
  - Có thể đính kèm gợi ý chỉnh sửa

- **Non-Functional Requirement:**
  - Security: Audit đầy đủ hành động kiểm duyệt
  - Reliability: Thông báo gửi đi ngay lập tức

**Priority:** Medium  
**CRUD:** Update

# UCA03-1: Xem danh sách danh mục [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem tất cả danh mục món ăn hiện có trong hệ thống với khả năng tìm kiếm, lọc và phân trang.

**Trigger:** Admin truy cập mục "Quản lý Danh mục".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Read"

**Post-Condition:**
- Danh sách danh mục hiển thị theo bộ lọc/sắp xếp

**Basic Flow:**
1. Admin mở trang "Quản lý Danh mục"
2. Hệ thống truy vấn và hiển thị bảng danh mục gồm:
   - a. ID danh mục
   - b. Tên danh mục
   - c. Mô tả (nếu có)
   - d. Số công thức trong danh mục
   - e. Ngày tạo/cập nhật
3. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên danh mục
   - b. Sắp xếp theo tên/ngày tạo
   - c. Phân trang (20 mục/trang)
4. Admin có thể mở chi tiết/sửa/xóa danh mục (UCA03-3/UCA03-4)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV/Excel

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Chưa có danh mục nào"

- **Lỗi tải dữ liệu:** Hiển thị lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Tên danh mục là duy nhất

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây
  - Security: Chỉ Admin mới truy cập

**Priority:** Low  
**CRUD:** Read

# UCA03-2: Thêm danh mục mới [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin tạo một danh mục món ăn mới để phân loại công thức.

**Trigger:** Admin nhấn nút "Thêm mới" trong trang "Quản lý Danh mục".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Create"

**Post-Condition:**
- Danh mục mới được lưu và xuất hiện trong danh sách

**Basic Flow:**
1. Admin nhấn "Thêm mới"
2. Hệ thống hiển thị form nhập liệu:
   - a. Tên danh mục (bắt buộc)
   - b. Mô tả (tùy chọn)
3. Admin nhập thông tin và nhấn "Lưu"
4. Hệ thống validate dữ liệu (tên không trùng, độ dài hợp lệ)
5. Hệ thống lưu danh mục và hiển thị thông báo "Đã thêm danh mục mới"

**Alternative Flow:**
- **Nhập hàng loạt:** Import nhiều danh mục từ file CSV

**Exception Flow:**
- **Tên trùng:** Hiển thị "Tên danh mục đã tồn tại"

- **Lỗi lưu CSDL:** Hiển thị lỗi, không tạo danh mục

**Additional Information:**
- **Business Rule:**
  - Tên danh mục duy nhất, không phân biệt hoa thường

- **Non-Functional Requirement:**
  - Performance: Lưu < 1 giây
  - Security: Chỉ Admin được phép

**Priority:** Low  
**CRUD:** Create

# UCA03-3: Sửa tên danh mục [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin chỉnh sửa tên hoặc mô tả của một danh mục món ăn hiện có.

**Trigger:** Admin chọn một danh mục và nhấn "Sửa".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Update"
- Danh mục tồn tại

**Post-Condition:**
- Tên/mô tả danh mục được cập nhật
- Thời gian cập nhật được ghi nhận

**Basic Flow:**
1. Admin nhấn "Sửa" tại danh mục mục tiêu
2. Hệ thống hiển thị form chỉnh sửa với thông tin hiện tại
3. Admin cập nhật tên/mô tả
4. Admin nhấn "Lưu"
5. Hệ thống validate (tên không trùng)
6. Hệ thống cập nhật CSDL
7. Hiển thị: "Cập nhật danh mục thành công"

**Alternative Flow:**
- **Đổi slug tự động:** Cập nhật slug theo tên mới (nếu có)

**Exception Flow:**
- **Tên trùng:** Hiển thị "Tên danh mục đã tồn tại"

- **Lỗi CSDL:** Thông báo lỗi và không cập nhật

**Additional Information:**
- **Business Rule:**
  - Tên danh mục duy nhất

- **Non-Functional Requirement:**
  - Performance: Cập nhật < 1 giây

**Priority:** Low  
**CRUD:** Update

# UCA03-4: Xóa danh mục [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xóa một danh mục khỏi hệ thống. Nếu danh mục đang được sử dụng bởi các công thức, hệ thống cảnh báo và yêu cầu chuyển các công thức đó sang danh mục khác trước khi xóa.

**Trigger:** Admin nhấn "Xóa" tại một danh mục trong trang "Quản lý Danh mục".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Category.Delete"
- Danh mục tồn tại

**Post-Condition:**
- Danh mục bị xóa khỏi hệ thống (nếu không còn được tham chiếu)
- Danh sách danh mục được cập nhật

**Basic Flow:**
1. Admin nhấn "Xóa" tại danh mục mục tiêu
2. Hệ thống kiểm tra ràng buộc tham chiếu (công thức đang gắn danh mục)
3. Nếu không có tham chiếu: Hiển thị xác nhận, Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa danh mục khỏi CSDL
5. Hiển thị thông báo: "Đã xóa danh mục"

**Alternative Flow:**
- **Có công thức đang tham chiếu:**
  - Hệ thống hiển thị danh sách công thức đang dùng danh mục này
  - Yêu cầu chọn danh mục thay thế
  - Sau khi xác nhận chuyển, thực hiện xóa danh mục cũ

- **Soft Delete:** Đánh dấu xóa để ẩn, có thể khôi phục

**Exception Flow:**
- **Không đủ quyền:** Thông báo "Bạn không có quyền xóa danh mục"

- **Lỗi ràng buộc CSDL:** Thông báo lỗi do ràng buộc khóa ngoại

- **Danh mục không tồn tại:** Thông báo "Danh mục không tồn tại"

**Additional Information:**
- **Business Rule:**
  - Không được xóa danh mục nếu còn công thức tham chiếu, trừ khi đã chuyển

- **Non-Functional Requirement:**
  - Security: Audit hành động xóa/chuyển danh mục
  - Reliability: Đảm bảo tính toàn vẹn dữ liệu khi chuyển

**Priority:** Low  
**CRUD:** Delete

# UCA03-5: Xem danh sách nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem danh sách tất cả nguyên liệu đã được chuẩn hóa trong CSDL, phục vụ cho việc tìm kiếm và thêm công thức.

**Trigger:** Admin truy cập mục "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Read"

**Post-Condition:**
- Danh sách nguyên liệu được hiển thị theo bộ lọc/sắp xếp

**Basic Flow:**
1. Admin mở trang "Quản lý Nguyên liệu"
2. Hệ thống hiển thị bảng nguyên liệu gồm:
   - a. ID
   - b. Tên nguyên liệu (chuẩn hóa, không dấu)
   - c. Đơn vị mặc định
   - d. Danh mục (Rau củ, Thịt, Gia vị,...)
   - e. Ngày tạo/cập nhật
3. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên (có gợi ý)
   - b. Lọc theo danh mục/đơn vị
   - c. Phân trang (50 mục/trang)
4. Admin có thể thêm/sửa/xóa nguyên liệu (UCA03-6/7/8)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV để đồng bộ với hệ thống khác

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Chưa có nguyên liệu nào"

- **Lỗi tải dữ liệu:** Hiển thị lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Tên nguyên liệu phải duy nhất (case-insensitive, bỏ dấu)

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây với 10k nguyên liệu (paging)
  - Security: Chỉ Admin mới truy cập

**Priority:** Low  
**CRUD:** Read

# UCA03-6: Thêm nguyên liệu mới [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin thêm một nguyên liệu mới vào CSDL chuẩn hoá để hệ thống nhận diện và người dùng có thể sử dụng khi tìm kiếm hoặc thêm công thức.

**Trigger:** Admin nhấn "Thêm nguyên liệu" trong trang "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Create"

**Post-Condition:**
- Nguyên liệu mới được lưu vào CSDL và xuất hiện trong danh sách

**Basic Flow:**
1. Admin nhấn "Thêm nguyên liệu"
2. Hệ thống hiển thị form nhập liệu:
   - a. Tên nguyên liệu (bắt buộc, chuẩn hoá, không dấu)
   - b. Đơn vị mặc định (vd: gram, ml, cái)
   - c. Danh mục (Rau củ, Thịt, Gia vị,...)
   - d. Tên đồng nghĩa (tùy chọn)
3. Admin nhập thông tin và nhấn "Lưu"
4. Hệ thống validate (tên duy nhất, độ dài hợp lệ)
5. Hệ thống lưu nguyên liệu và hiển thị thông báo "Đã thêm nguyên liệu"

**Alternative Flow:**
- **Nhập hàng loạt:** Import danh sách nguyên liệu từ CSV

- **Gợi ý từ dữ liệu người dùng:** Đề xuất nguyên liệu mới dựa trên tần suất nhập custom

**Exception Flow:**
- **Tên trùng:** Hiển thị "Nguyên liệu đã tồn tại"

- **Lỗi lưu CSDL:** Thông báo lỗi và không tạo mới

**Additional Information:**
- **Business Rule:**
  - Tên nguyên liệu duy nhất (case-insensitive, không dấu)
  - Có thể lưu alias để hỗ trợ tìm kiếm

- **Non-Functional Requirement:**
  - Performance: Lưu < 1 giây
  - Security: Chỉ Admin/Super Admin được phép

**Priority:** Medium  
**CRUD:** Create

# UCA03-7: Sửa thông tin nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin chỉnh sửa tên, đơn vị và thông tin liên quan của một nguyên liệu đã có trong CSDL chuẩn hoá.

**Trigger:** Admin chọn một nguyên liệu và nhấn "Sửa" trong trang "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Update"
- Nguyên liệu tồn tại

**Post-Condition:**
- Thông tin nguyên liệu được cập nhật
- Thời gian cập nhật được ghi nhận

**Basic Flow:**
1. Admin nhấn "Sửa" tại nguyên liệu mục tiêu
2. Hệ thống hiển thị form với dữ liệu hiện tại
3. Admin chỉnh sửa các trường:
   - a. Tên nguyên liệu (chuẩn hoá, không dấu)
   - b. Đơn vị mặc định
   - c. Danh mục
   - d. Alias (đồng nghĩa)
4. Admin nhấn "Lưu"
5. Hệ thống validate (tên duy nhất)
6. Hệ thống cập nhật CSDL
7. Hiển thị: "Cập nhật nguyên liệu thành công"

**Alternative Flow:**
- **Đổi tên có alias:** Tự động thêm tên cũ vào alias

**Exception Flow:**
- **Tên trùng:** Hiển thị "Nguyên liệu đã tồn tại"

- **Lỗi CSDL:** Thông báo lỗi

**Additional Information:**
- **Business Rule:**
  - Tên duy nhất (case-insensitive, không dấu)

- **Non-Functional Requirement:**
  - Performance: Cập nhật < 1 giây

**Priority:** Low  
**CRUD:** Update

# UCA03-8: Xóa nguyên liệu [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xóa một nguyên liệu khỏi CSDL chuẩn hoá. Nếu nguyên liệu đang được tham chiếu bởi công thức, cần xử lý ràng buộc trước khi xóa.

**Trigger:** Admin nhấn "Xóa" tại một nguyên liệu trong trang "Quản lý Nguyên liệu".

**Pre-Condition:**
- Admin đã đăng nhập và có quyền "Ingredient.Delete"
- Nguyên liệu tồn tại

**Post-Condition:**
- Nguyên liệu bị xóa hoặc được đánh dấu xóa/ẩn tuỳ chính sách

**Basic Flow:**
1. Admin nhấn "Xóa" tại nguyên liệu mục tiêu
2. Hệ thống kiểm tra ràng buộc (công thức sử dụng nguyên liệu này)
3. Nếu không có ràng buộc: Hiển thị xác nhận, Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa nguyên liệu khỏi CSDL
5. Hiển thị: "Đã xóa nguyên liệu"

**Alternative Flow:**
- **Có ràng buộc:**
  - Hiển thị danh sách công thức đang sử dụng nguyên liệu
  - Yêu cầu thay thế bằng nguyên liệu khác hoặc huỷ xóa

- **Soft Delete:** Đánh dấu ẩn để không phá vỡ tham chiếu

**Exception Flow:**
- **Không đủ quyền:** Thông báo "Bạn không có quyền xóa nguyên liệu"

- **Lỗi ràng buộc CSDL:** Thông báo lỗi khóa ngoại

- **Nguyên liệu không tồn tại:** Thông báo "Nguyên liệu không tồn tại"

**Additional Information:**
- **Business Rule:**
  - Không được xóa nếu còn tham chiếu, trừ khi đã thay thế/soft delete

- **Non-Functional Requirement:**
  - Security: Audit hành động xóa
  - Reliability: Đảm bảo toàn vẹn dữ liệu

**Priority:** Low  
**CRUD:** Delete

# UCA04-1: Xem danh sách tài khoản Admin [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin xem danh sách các tài khoản quản trị (Admin) khác, với khả năng tìm kiếm, lọc và phân trang.

**Trigger:** Super Admin truy cập mục "Quản lý Tài khoản Admin".

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.Read`

**Post-Condition:**
- Danh sách tài khoản Admin hiển thị theo bộ lọc/sắp xếp

**Basic Flow:**
1. Super Admin mở trang "Quản lý Tài khoản Admin"
2. Hệ thống truy vấn danh sách tài khoản Admin
3. Bảng hiển thị các cột:
   - a. ID
   - b. Ảnh đại diện
   - c. Tên hiển thị
   - d. Email
   - e. Vai trò (Role)
   - f. Ngày tạo
   - g. Trạng thái (Hoạt động/Bị khóa)
4. Công cụ hỗ trợ:
   - a. Tìm kiếm theo tên/email
   - b. Lọc theo vai trò/trạng thái
   - c. Phân trang (20 mục/trang)
5. Super Admin có thể mở chi tiết/phan quyền/xóa (UCA04-3/UCA04-4)

**Alternative Flow:**
- **Xuất danh sách:** Xuất CSV/Excel

**Exception Flow:**
- **Không có dữ liệu:** Hiển thị "Chưa có tài khoản Admin nào"

- **Lỗi tải dữ liệu:** Thông báo lỗi và gợi ý thử lại

**Additional Information:**
- **Business Rule:**
  - Chỉ Super Admin được truy cập màn hình này

- **Non-Functional Requirement:**
  - Performance: Tải trang < 2 giây
  - Security: Audit truy cập

**Priority:** Low  
**CRUD:** Read

# UCA04-2: Tạo tài khoản Admin mới [MEDIUM PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin tạo một tài khoản quản trị mới để phân quyền quản lý hệ thống.

**Trigger:** Super Admin nhấn "Tạo tài khoản Admin" trong mục "Quản lý Tài khoản Admin".

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.Create`

**Post-Condition:**
- Tài khoản Admin mới được tạo và gửi email kích hoạt

**Basic Flow:**
1. Super Admin nhấn "Tạo tài khoản Admin"
2. Hệ thống hiển thị form:
   - a. Họ tên hiển thị
   - b. Email (bắt buộc, duy nhất)
   - c. Vai trò (Role) mặc định hoặc chọn từ danh sách
3. Super Admin nhập thông tin và nhấn "Tạo"
4. Hệ thống validate (email hợp lệ, chưa tồn tại)
5. Hệ thống tạo tài khoản ở trạng thái "Chờ kích hoạt" và gửi email kích hoạt
6. Hiển thị: "Đã tạo tài khoản Admin và gửi email kích hoạt"

**Alternative Flow:**
- **Gán role ngay khi tạo:** Chọn role cụ thể khi tạo mới

- **Nhập hàng loạt:** Import danh sách admin từ CSV, gửi email hàng loạt

**Exception Flow:**
- **Email trùng:** Hiển thị "Email đã tồn tại"

- **Lỗi gửi email:** Tạo tài khoản thành công nhưng hiển thị cảnh báo không gửi được email, cho phép gửi lại

- **Lỗi CSDL:** Thông báo lỗi và không tạo mới

**Additional Information:**
- **Business Rule:**
  - Email là danh tính đăng nhập duy nhất
  - Tài khoản mới phải kích hoạt qua email trước khi sử dụng

- **Non-Functional Requirement:**
  - Security: Chỉ Super Admin được phép tạo
  - Reliability: Hỗ trợ gửi lại email kích hoạt

**Priority:** Medium  
**CRUD:** Create

# UCA04-3: Phân quyền cho tài khoản Admin [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin gán hoặc thay đổi quyền hạn (roles/permissions) cho một tài khoản Admin, đảm bảo nguyên tắc phân quyền tối thiểu (least privilege).

**Trigger:** Super Admin mở chi tiết một tài khoản Admin và nhấn "Phân quyền".

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.ManageRoles`
- Tài khoản Admin mục tiêu tồn tại

**Post-Condition:**
- Quyền hạn/Vai trò của tài khoản Admin được cập nhật
- Hệ thống ghi log thay đổi quyền

**Basic Flow:**
1. Super Admin mở màn hình phân quyền của tài khoản Admin
2. Hệ thống hiển thị danh sách vai trò (Roles) và quyền (Permissions) khả dụng
3. Super Admin chọn/bỏ chọn các vai trò hoặc quyền cụ thể
4. Super Admin nhấn "Lưu phân quyền"
5. Hệ thống xác thực thay đổi (không tự hạ quyền của chính Super Admin nếu là tài khoản duy nhất)
6. Hệ thống cập nhật vai trò/quyền trong CSDL
7. Hệ thống ghi log audit thay đổi quyền (ai, khi nào, trước/sau)
8. Hiển thị: "Cập nhật phân quyền thành công"

**Alternative Flow:**
- **Gán quyền tạm thời:** Thiết lập thời hạn cho quyền tạm thời

- **Áp dụng từ mẫu (Role Template):** Chọn preset role để áp nhanh

- **Xem thử quyền hiệu lực (Effective Permissions):** Xem tổng hợp quyền sau khi áp

**Exception Flow:**
- **Thiếu quyền:** "Bạn không có quyền phân quyền cho Admin"

- **Xung đột vai trò:** Nếu vai trò mâu thuẫn, hệ thống cảnh báo và yêu cầu điều chỉnh

- **Không thể tự hạ quyền duy nhất:** Ngăn việc xoá role Super Admin cuối cùng

**Additional Information:**
- **Business Rule:**
  - Tuân thủ nguyên tắc least privilege
  - Mọi thay đổi quyền phải có audit
  - Không cho phép tình trạng hệ thống không còn Super Admin

- **Non-Functional Requirement:**
  - Security: RBAC/ABAC rõ ràng, audit đầy đủ
  - Reliability: Áp quyền có hiệu lực ngay lập tức

**Priority:** High  
**CRUD:** Update

# UCA04-4: Xóa tài khoản Admin [LOW PRIORITY]

**Use Case Description:** Chức năng này cho phép Super Admin xóa một tài khoản quản trị khỏi hệ thống, có xác nhận để tránh xóa nhầm.

**Trigger:** Super Admin nhấn "Xóa" tại tài khoản Admin trong danh sách quản trị.

**Pre-Condition:**
- Super Admin đã đăng nhập
- Quyền: `AdminAccount.Delete`
- Tài khoản mục tiêu tồn tại

**Post-Condition:**
- Tài khoản Admin bị xóa khỏi hệ thống
- Lịch sử/audit ghi nhận sự kiện

**Basic Flow:**
1. Super Admin nhấn "Xóa" tại tài khoản mục tiêu
2. Hệ thống hiển thị hộp thoại xác nhận với thông tin tài khoản
3. Super Admin nhấn "Xác nhận xóa"
4. Hệ thống xóa tài khoản khỏi CSDL
5. Hiển thị: "Đã xóa tài khoản Admin"

**Alternative Flow:**
- **Soft Delete:** Đánh dấu vô hiệu hóa thay vì xóa vĩnh viễn

- **Xóa hàng loạt:** Chọn nhiều tài khoản và xóa cùng lúc

**Exception Flow:**
- **Không thể xóa Super Admin cuối cùng:** Hệ thống ngăn chặn, hiển thị cảnh báo

- **Thiếu quyền:** "Bạn không có quyền xóa tài khoản Admin"

- **Lỗi CSDL:** Thông báo lỗi, không xóa

**Additional Information:**
- **Business Rule:**
  - Không được xóa khi tài khoản là Super Admin cuối cùng
  - Mọi thao tác xóa phải được audit

- **Non-Functional Requirement:**
  - Security: Kiểm soát truy cập nghiêm ngặt
  - Reliability: Đảm bảo toàn vẹn dữ liệu và quan hệ

**Priority:** Low  
**CRUD:** Delete

# UCA05-1: Xem báo cáo, thống kê [HIGH PRIORITY]

**Use Case Description:** Chức năng này cho phép Admin xem các báo cáo và biểu đồ thống kê về hoạt động của hệ thống (người dùng, công thức, tương tác), hỗ trợ ra quyết định và theo dõi sức khỏe hệ thống.

**Trigger:** Admin truy cập mục "Dashboard & Thống kê" trong bảng điều khiển quản trị.

**Pre-Condition:**
- Admin đã đăng nhập và có quyền truy cập báo cáo (`Report.Read`)
- Hệ thống có dữ liệu thống kê được tổng hợp định kỳ hoặc real-time

**Post-Condition:**
- Các báo cáo, biểu đồ hiển thị theo phạm vi thời gian và bộ lọc đã chọn
- Admin có thể xuất báo cáo hoặc đào sâu (drill-down) vào dữ liệu chi tiết

**Basic Flow:**
1. Admin mở trang "Dashboard & Thống kê"
2. Hệ thống hiển thị tổng quan (KPIs):
   - a. Tổng số người dùng, người dùng mới (theo ngày/tuần/tháng)
   - b. Tổng số công thức, công thức mới
   - c. Tỷ lệ phê duyệt công thức
   - d. Số lượt xem, yêu thích, bình luận
3. Khu vực biểu đồ tương tác:
   - a. Biểu đồ đường: tăng trưởng người dùng theo thời gian
   - b. Biểu đồ cột: số công thức tạo mới theo danh mục
   - c. Biểu đồ tròn: phân bổ trạng thái công thức (Đã duyệt/Chờ duyệt/Bị từ chối)
   - d. Heatmap: hoạt động theo khung giờ/ngày trong tuần
4. Bộ lọc báo cáo:
   - a. Khoảng thời gian (7 ngày, 30 ngày, tùy chọn)
   - b. Theo danh mục, theo vai trò người dùng, theo trạng thái công thức
5. Admin có thể nhấp vào một KPI/biểu đồ để drill-down xem danh sách chi tiết
6. Admin có thể xuất báo cáo (PDF/CSV) theo bộ lọc hiện tại
7. Hệ thống lưu lựa chọn bộ lọc ưa thích

**Alternative Flow:**
- **Báo cáo tùy chỉnh:**
  - Admin tạo report tùy biến với các trường và biểu đồ chọn lọc
  - Lưu template report để dùng lại

- **Lên lịch gửi báo cáo:**
  - Thiết lập lịch gửi email báo cáo định kỳ (hàng tuần/tháng)

- **Realtime dashboard:**
  - Hiển thị số liệu realtime cho một số chỉ số quan trọng

**Exception Flow:**
- **Không có dữ liệu trong phạm vi:**
  - Hiển thị "Không có dữ liệu cho khoảng thời gian đã chọn"

- **Lỗi tải dữ liệu:**
  - Thông báo: "Không thể tải dữ liệu thống kê. Vui lòng thử lại sau."

- **Thiếu quyền:**
  - Chuyển hướng hoặc thông báo quyền truy cập không hợp lệ

**Additional Information:**
- **Business Rule:**
  - Số liệu phải khớp với nguồn dữ liệu chính (consistency)
  - Báo cáo phải có timestamp dữ liệu

- **Non-Functional Requirement:**
  - Performance: Truy vấn và hiển thị dashboard < 3 giây (với dữ liệu đã được tổng hợp)
  - Usability: Biểu đồ tương tác, hỗ trợ hover để xem chi tiết
  - Security: Phân quyền truy cập báo cáo theo vai trò
  - Reliability: Hệ thống dự phòng khi dịch vụ thống kê lỗi

**Priority:** High  
**CRUD:** Read

