# Đánh Giá Luồng Đăng Nhập Hệ Thống (UCS01-2)

**Ngày đánh giá:** $(date)  
**Use Case:** UCS01-2 - Đăng nhập vào hệ thống

---

## 📋 Tổng Quan

Báo cáo này đánh giá việc triển khai luồng đăng nhập so với tài liệu use case, xác định các điểm khớp và khác biệt.

---

## ✅ Các Điểm Đã Triển Khai Đúng

### 1. **Basic Flow - Luồng Chính**

#### ✅ Bước 1-4: Giao diện và Input
- **Frontend (`LoginForm.tsx`):**
  - ✅ Có input cho email/số điện thoại (`identifier`)
  - ✅ Có input cho mật khẩu với toggle hiển thị
  - ✅ Có checkbox "Ghi nhớ đăng nhập" (`rememberMe`)
  - ✅ Có link "Quên mật khẩu?"
  - ✅ Validation phía client

#### ✅ Bước 5-6: Xác thực và Kiểm tra Tài khoản
- **Backend (`auth.service.ts` - `login()`):**
  - ✅ Tìm user theo email hoặc phone (`findByEmailOrPhone`)
  - ✅ Kiểm tra tài khoản tồn tại
  - ✅ Kiểm tra account unlocked (`ensureAccountUnlocked`)
  - ✅ Kiểm tra tài khoản đã xác thực (`emailVerified` hoặc `phoneVerified`)
  - ✅ Kiểm tra trạng thái tài khoản (`status !== PENDING/INACTIVE`)
  - ✅ So sánh mật khẩu đã mã hóa (`comparePassword`)

#### ✅ Bước 7-8: Tạo Session và Cập nhật
- **Backend:**
  - ✅ Tạo JWT token và refresh token
  - ✅ Tạo session trong database (`SessionService.createUserSession`)
  - ✅ Cập nhật `lastLoginAt` trong database
  - ✅ Xử lý `rememberMe`: Session 24h (mặc định) hoặc 30 ngày (nếu rememberMe)

### 2. **Exception Flow - Xử lý Lỗi**

#### ✅ Thông tin đăng nhập không chính xác
- **Backend:** Trả về `UnauthorizedError('Invalid email/phone or password')`
- **Frontend:** Hiển thị thông báo lỗi phù hợp

#### ✅ Tài khoản chưa được xác thực
- **Backend:** Kiểm tra và trả về `UnauthorizedError('Account is not verified. Please check your email.')`
- **Frontend:** Có thể xử lý thông qua error message

#### ✅ Tài khoản bị khóa
- **Backend:** 
  - Kiểm tra status LOCKED
  - Phân biệt khóa tạm thời và khóa vĩnh viễn (CRITICAL)
  - Trả về thông báo phù hợp

#### ✅ Quá nhiều lần đăng nhập sai
- **Backend (`handleFailedLoginAttempt`):**
  - ✅ Đếm số lần đăng nhập sai (`failedLoginAttempts`)
  - ✅ Tạm khóa sau 5 lần sai (`MAX_LOGIN_ATTEMPTS = 5`)
  - ✅ Thời gian khóa 15 phút (`LOGIN_LOCK_DURATION_MINUTES = 15`)
  - ✅ Tự động mở khóa sau thời hạn (`ensureAccountUnlocked`)
  - ✅ Reset counter khi đăng nhập thành công

#### ✅ Lỗi hệ thống
- **Backend:** Sử dụng `asyncHandler` để bắt lỗi
- **Frontend:** Hiển thị thông báo lỗi chung

### 3. **Business Rules**

#### ✅ Thời hạn Session
- ✅ Session mặc định: 24 giờ (`DEFAULT_SESSION_TTL_MS`)
- ✅ Session với rememberMe: 30 ngày (`REMEMBER_ME_SESSION_TTL_MS`)

#### ✅ Giới hạn Session
- ✅ Mỗi user chỉ có 1 session hoạt động (`deleteByUserId` trước khi tạo session mới)

#### ✅ Bảo mật
- ✅ Mật khẩu được mã hóa (bcrypt)
- ✅ Token được tạo bằng JWT
- ✅ Session lưu trong database

---

## ⚠️ Các Điểm Khác Biệt / Thiếu Sót

### 1. **Cookie vs Token trong Response**

**Tài liệu yêu cầu:**
> "Nếu có 'Ghi nhớ đăng nhập': Hệ thống tạo cookie với thời hạn dài hơn"

**Code thực tế:**
- ❌ Backend không set cookie, chỉ trả về token trong JSON response
- ✅ Frontend lưu token vào `localStorage` thay vì cookie

**Đánh giá:**
- **Vấn đề:** Token trong localStorage dễ bị XSS attack hơn cookie với HttpOnly flag
- **Khuyến nghị:** 
  - Nên sử dụng HttpOnly cookie cho token (bảo mật hơn)
  - Hoặc giữ nguyên localStorage nhưng thêm các biện pháp bảo mật khác (CSP headers, sanitization)

### 2. **Chuyển hướng đến Trang Yêu Cầu Trước Đó**

**Tài liệu yêu cầu:**
> "Hệ thống chuyển hướng người dùng đến trang chủ hoặc trang được yêu cầu trước đó"

**Code thực tế:**
- ✅ Frontend chuyển hướng đến `/` (trang chủ)
- ❌ Không có logic lưu và chuyển hướng đến trang yêu cầu trước đó

**Khuyến nghị:**
- Lưu `returnUrl` trong query params hoặc session storage
- Sau khi đăng nhập thành công, kiểm tra và chuyển hướng đến `returnUrl` nếu có

### 3. **Thông báo Lỗi Chi Tiết**

**Tài liệu yêu cầu:**
- Tài khoản chưa xác thực: Cung cấp tùy chọn "Gửi lại email/SMS xác thực"
- Tài khoản bị khóa: Cung cấp thông tin liên hệ hỗ trợ

**Code thực tế:**
- ✅ Có thông báo lỗi
- ❌ Chưa có link/button "Gửi lại email xác thực" trong form đăng nhập
- ❌ Chưa có thông tin liên hệ hỗ trợ khi tài khoản bị khóa

**Khuyến nghị:**
- Thêm link "Gửi lại email xác thực" khi lỗi account not verified
- Hiển thị thông tin liên hệ hỗ trợ khi account bị khóa

### 4. **Alternative Flow - Đăng nhập bằng Google/Facebook**

**Tài liệu yêu cầu:**
> Có luồng đăng nhập bằng Google/Facebook

**Code thực tế:**
- ❌ Chưa có implementation cho OAuth login

**Đánh giá:**
- Đây là tính năng tùy chọn, có thể triển khai sau

### 5. **Security Log**

**Tài liệu không đề cập:**
- Code có `SecurityLogService` nhưng chưa được sử dụng trong `login()`

**Khuyến nghị:**
- Có thể thêm logging cho các sự kiện đăng nhập (thành công/thất bại) để audit

---

## 📊 Bảng So Sánh Chi Tiết

| Yêu cầu | Tài liệu | Code Thực Tế | Trạng thái |
|---------|----------|--------------|------------|
| Input email/phone | ✅ | ✅ | ✅ Đúng |
| Input password | ✅ | ✅ | ✅ Đúng |
| Checkbox rememberMe | ✅ | ✅ | ✅ Đúng |
| Validation input | ✅ | ✅ | ✅ Đúng |
| Kiểm tra user tồn tại | ✅ | ✅ | ✅ Đúng |
| Kiểm tra password | ✅ | ✅ | ✅ Đúng |
| Kiểm tra account verified | ✅ | ✅ | ✅ Đúng |
| Kiểm tra account status | ✅ | ✅ | ✅ Đúng |
| Tạo session | ✅ | ✅ | ✅ Đúng |
| Cập nhật lastLoginAt | ✅ | ✅ | ✅ Đúng |
| Session 24h/30 ngày | ✅ | ✅ | ✅ Đúng |
| Set cookie (rememberMe) | ✅ | ❌ | ⚠️ Khác biệt |
| Chuyển hướng returnUrl | ✅ | ❌ | ⚠️ Thiếu |
| Xử lý failed attempts | ✅ | ✅ | ✅ Đúng |
| Tạm khóa sau 5 lần sai | ✅ | ✅ | ✅ Đúng |
| Tự động mở khóa sau 15 phút | ✅ | ✅ | ✅ Đúng |
| Thông báo lỗi chi tiết | ✅ | ⚠️ | ⚠️ Thiếu một phần |
| Link "Gửi lại email xác thực" | ✅ | ❌ | ⚠️ Thiếu |
| OAuth login (Google/FB) | ✅ | ❌ | ⚠️ Chưa triển khai |

---

## 🔍 Phân Tích Code Chi Tiết

### Backend Flow

```86:166:D:\DATN\BE\src\modules\auth\services\auth.service.ts
  async login(
    data: LoginRequestDto,
    context?: {
      ipAddress?: string;
      userAgent?: string;
    }
  ): Promise<AuthResponseDto> {
    const identifier = data.identifier.toLowerCase();
    let user = await this.repository.findByEmailOrPhone(identifier);

    if (!user) {
      throw new UnauthorizedError('Invalid email/phone or password');
    }

    user = await this.ensureAccountUnlocked(user);

    if (!user.emailVerified && !user.phoneVerified) {
      throw new UnauthorizedError('Account is not verified. Please check your email.');
    }

    if (user.status === USER_STATUS.PENDING || user.status === USER_STATUS.INACTIVE) {
      throw new UnauthorizedError('Account is inactive. Please contact support.');
    }

    const isPasswordValid = await comparePassword(data.password, user.password);
    if (!isPasswordValid) {
      const accountLocked = await this.handleFailedLoginAttempt(user.id);
      if (accountLocked) {
        throw new UnauthorizedError(
          'Account has been temporarily locked due to multiple failed login attempts. Please try again later.'
        );
      }

      throw new UnauthorizedError('Invalid email/phone or password');
    }

    await this.repository.resetFailedLoginAttempts(user.id);

    const token = generateToken({
      id: user.id,
      email: user.email,
      role: user.role,
    });

    const refreshToken = generateRefreshToken({
      id: user.id,
      email: user.email,
      role: user.role,
    });

    const session = await this.sessionService.createUserSession({
      userId: user.id,
      token,
      refreshToken,
      rememberMe: data.rememberMe,
      deviceInfo: context?.userAgent,
      ipAddress: context?.ipAddress,
    });

    await this.repository.update(user.id, { lastLoginAt: new Date() });

    logger.info('User logged in successfully', { userId: user.id, email: user.email });

    return {
      user: {
        id: user.id,
        email: user.email,
        fullName: user.fullName,
        role: user.role,
        avatar: user.avatar,
        status: user.status,
      },
      token,
      refreshToken,
      session: {
        id: session.id,
        expiresAt: session.expiresAt,
        rememberMe: Boolean(data.rememberMe),
      },
    };
  }
```

**Điểm mạnh:**
- ✅ Logic rõ ràng, tuân thủ SOLID
- ✅ Xử lý đầy đủ các trường hợp lỗi
- ✅ Có logging và tracking

**Điểm cần cải thiện:**
- ⚠️ Không set cookie, chỉ trả về token trong response
- ⚠️ Có thể thêm security log cho audit trail

### Frontend Flow

```42:83:D:\DATN\fe-web\src\components\auth\LoginForm.tsx
  const handleSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    setErrors({});

    if (!validate()) {
      return;
    }

    setIsLoading(true);

    try {
      const response = await authService.login({
        identifier: formData.identifier.trim(),
        password: formData.password,
        rememberMe: formData.rememberMe,
      });

      // Đăng nhập thành công, chuyển hướng
      router.push('/');
      router.refresh();
    } catch (error: any) {
      setIsLoading(false);
      
      if (error.response?.status === 401) {
        setErrors({
          general: 'Email/số điện thoại hoặc mật khẩu không chính xác. Vui lòng kiểm tra lại.',
        });
      } else if (error.response?.status === 403) {
        setErrors({
          general: 'Tài khoản đã bị khóa. Vui lòng liên hệ quản trị viên để được hỗ trợ.',
        });
      } else if (error.response?.data?.message) {
        setErrors({
          general: error.response.data.message,
        });
      } else {
        setErrors({
          general: 'Đã xảy ra lỗi hệ thống. Vui lòng thử lại sau.',
        });
      }
    }
  };
```

**Điểm mạnh:**
- ✅ Validation phía client
- ✅ Xử lý lỗi đầy đủ
- ✅ UX tốt (loading state, error messages)

**Điểm cần cải thiện:**
- ⚠️ Không xử lý returnUrl
- ⚠️ Có thể cải thiện thông báo lỗi chi tiết hơn (phân biệt các loại lỗi)

---

## 🎯 Khuyến Nghị

### Ưu tiên Cao

1. **Thêm xử lý returnUrl**
   - Lưu `returnUrl` từ query params khi redirect đến login
   - Sau khi đăng nhập thành công, chuyển hướng đến `returnUrl` nếu có

2. **Cải thiện thông báo lỗi**
   - Thêm link "Gửi lại email xác thực" khi account chưa verified
   - Hiển thị thông tin liên hệ hỗ trợ khi account bị khóa

### Ưu tiên Trung bình

3. **Xem xét sử dụng HttpOnly Cookie**
   - Chuyển từ localStorage sang HttpOnly cookie cho token (bảo mật hơn)
   - Hoặc giữ localStorage nhưng thêm các biện pháp bảo mật bổ sung

4. **Thêm Security Logging**
   - Log các sự kiện đăng nhập (thành công/thất bại) vào SecurityLogService
   - Hỗ trợ audit trail

### Ưu tiên Thấp

5. **OAuth Login (Google/Facebook)**
   - Triển khai khi có yêu cầu

---

## ✅ Kết Luận

**Tổng thể:** Luồng đăng nhập đã được triển khai **tốt** và **đầy đủ** so với tài liệu use case.

**Điểm mạnh:**
- ✅ Logic xác thực chặt chẽ và bảo mật
- ✅ Xử lý đầy đủ các trường hợp lỗi
- ✅ Tuân thủ business rules
- ✅ Code clean, dễ maintain

**Điểm cần cải thiện:**
- ⚠️ Thiếu xử lý returnUrl
- ⚠️ Thông báo lỗi có thể chi tiết hơn
- ⚠️ Có thể cải thiện bảo mật bằng HttpOnly cookie

**Đánh giá:** ⭐⭐⭐⭐ (4/5) - Triển khai tốt, chỉ cần một số cải thiện nhỏ.

