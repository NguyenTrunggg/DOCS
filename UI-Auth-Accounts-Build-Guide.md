# UI Build Guide: `@auth` & `@accounts`

## 1. Scope & Target readers
- Tài liệu nền tảng cho mọi agent/frontend khi cần dựng UI các domain xác thực và quản trị tài khoản.
- Mục tiêu: mô tả cách đọc backend contract, biến thành UI/Service layer mà không ràng buộc framework cụ thể.
- Dùng được cho mobile (Expo), web hoặc desktop miễn là giữ chuẩn payload/response.

## 2. Khung tư duy chung
- **SOLID trước khi code**: chia rõ module hiển thị (View), xử lý (Controller/Hook) và gateway gọi API.
- **Luôn đọc hợp đồng BE** (DTO + validator + routes) trước khi phát thảo UI hoặc state machine.
- **Tài liệu hóa assumption**: nếu backend chưa hoàn tất (vd `admin/accounts`), ghi rõ mock và vùng chờ để người sau dễ cập nhật.
- **Bám môi trường chuẩn**: mọi service lấy base URL từ config, không hard-code.

## 3. Khai thác nguồn BE

### 3.1 DTO request
- Các DTO trong `src/modules/auth/dto/request` mô tả payload chuẩn. Giữ nguyên tên field, phân biệt chữ hoa chữ thường.
- Khi phát sinh UI mới, clone DTO này sang file type riêng phía client để tránh sai lệch.

```5:12:src/modules/auth/dto/request/register.dto.ts
export interface RegisterRequestDto {
  email: string;
  password: string;
  confirmPassword: string;
  fullName: string;
  phone?: string;
  acceptTerms: boolean;
}
```

### 3.2 DTO response
- `AuthResponseDto` cung cấp token, dữ liệu user và session. Bất kỳ client nào cũng cần flow lưu token bảo mật + quản lý refresh.
- `ProfileResponseDto` và `RegisterResponseDto` giúp xác định thông tin cần hiển thị hoặc điều hướng (ví dụ xem user có cần verify email).

```5:21:src/modules/auth/dto/response/auth-response.dto.ts
export interface AuthResponseDto {
  user: {
    id: string;
    email: string;
    fullName: string;
    role: string;
    avatar?: string | null;
    status: string;
  };
  token: string;
  refreshToken?: string;
  session?: {
    id: string;
    expiresAt: Date;
    rememberMe: boolean;
  };
}
```

### 3.3 Validator
- Tất cả ràng buộc input được định nghĩa ở `src/modules/auth/validators/auth.validator.ts`. Khi tạo form, hãy tái sử dụng cùng constraint (regex, min/max, confirm password, v.v.).
- Chờ backend cập nhật => chỉ cần chỉnh validator FE, không chạm phần service.

```9:58:src/modules/auth/validators/auth.validator.ts
export const registerValidator = Joi.object({
  email: Joi.string().email().required(),
  password: Joi.string().min(8).required(),
  confirmPassword: Joi.string().valid(Joi.ref('password')).required(),
  fullName: Joi.string().min(2).max(120).required(),
  phone: Joi.string().pattern(PHONE_REGEX).optional(),
  acceptTerms: Joi.boolean().valid(true).required(),
});
// ... (login/change/update/forgot/reset/verify/resend)
```

> Quy ước: mọi agent phải cập nhật validator FE ngay khi phát hiện chênh lệch với file trên.

## 4. Quy trình dựng `@auth`

1. **Đọc routes**: `src/modules/auth/routes.ts` liệt kê toàn bộ endpoint, middleware và mục đích.
2. **Xây service gateway**: tạo lớp/đối tượng `authApi` (tên tùy hệ thống) map 1-1 với routes, truyền DTO tương ứng.
3. **Thiết kế flow UI**:
   - Đăng ký → login/verify tùy `requiresVerification`.
   - Đăng nhập → lưu session/token, xử lý `rememberMe`.
   - Quên mật khẩu → gửi identifier → nhập token + mật khẩu mới.
   - Hồ sơ → lấy dữ liệu từ `GET /profile`, hỗ trợ cập nhật avatar theo chuẩn multipart.
4. **Quản lý trạng thái**: cần lớp session store để tự động refresh token và cleanup khi logout/logout-all.
5. **Thông báo lỗi**: chuẩn hóa parser cho lỗi Joi từ backend để hiển thị đúng field hoặc toast chung.

```20:83:src/modules/auth/routes.ts
router.post('/register', validate({ body: registerValidator }), authController.register);
// ... (login, verify, resend, forgot, reset)
router.get('/profile', authenticate, authController.getProfile);
router.patch('/profile', authenticate, uploadSingleImage('avatar'), validate({ body: updateProfileValidator }), authController.updateProfile);
router.post('/logout', authenticate, authController.logout);
router.post('/logout/all', authenticate, authController.logoutAll);
router.post('/change-password', authenticate, validate({ body: changePasswordValidator }), authController.changePassword);
```

## 5. Quy trình dựng `@accounts`

- Backend module `admin/accounts` đang ở mức scaffold (controller/service chưa có logic). Điều này nghĩa là mọi agent FE cần:
  1. Tra cứu thêm yêu cầu nghiệp vụ trong `DOCS/UC/UC-A4`.
  2. Thiết kế UI/luồng dựa trên use case (danh sách admin, tạo mới, phân quyền, vô hiệu hóa).
  3. Mock service layer theo hợp đồng tạm thời và ghi lại assumptions ở cuối tài liệu hoặc README của feature.
  4. Khi backend cập nhật router + DTO chính thức, quay lại bước 3 (Khai thác nguồn BE) để đồng bộ.
- Luôn tách riêng component trình bày (table/form) và logic bất đồng bộ (service/store) để agent khác có thể thay thế backend mà không đập UI.

## 6. Service layer & tích hợp UI

- Service nên là lớp mỏng, nhận DTO chuẩn và trả dữ liệu typed (đã map sang interface FE). Ví dụ:

```ts
// Pseudocode
authApi.register(payload: RegisterRequestDto) => Promise<AuthResponseDto>;
authApi.updateProfile(formData: FormData) => Promise<ProfileResponseDto>;
```

- Các màn hình/flow (register, login, profile...) chỉ biết tới service, không quan tâm HTTP client bên dưới.
- Khi dùng router (Expo, Next, React Router, v.v.), luôn có AuthGuard để redirect dựa trên session store.

## 7. Checklist dùng chung
1. Đồng bộ type từ BE (copy DTO hoặc generate tự động).
2. Định nghĩa validator FE bám chuẩn Joi.
3. Map đầy đủ routes vào service layer.
4. Thiết kế UI/flow dựa trên response (`requiresVerification`, `stats`, `session`...).
5. Viết test cho hook/service quan trọng.
6. Ghi chú assumption/missing API để người sau xử lý tiếp.

## 8. Theo dõi & cập nhật
- Khi backend đổi router, DTO hoặc validator, cập nhật ngay mục 3 và 4 để tránh thông tin lỗi thời.
- Luôn bổ sung changelog (gợi ý: `DOCS/CHANGELOG.md`) cho mỗi lần sửa tài liệu này để agent khác nắm lịch sử.


## 9. Tracklist dựng view cho UCA01 (`@accounts`)

- **Nguyên tắc chung**  
  - Không chạm component/service dùng chung trừ khi được chủ module phê duyệt.  
  - Mọi file UC-A1 đặt trong namespace riêng:  
    - Validators/service/hook: `src/**`  
    - UI component: `components/admin/users/**`  
    - Screen: `app/admin/(users)/**`  
  - Ghi assumption (mock data, BE chưa có flag) ngay trong README feature hoặc comment đầu file.

- **Quy trình Valid → Service → Component → View**  
  1. **Validator (`src/validators/admin-users.validator.ts`)**: copy constraint từ BE (Joi) và chuẩn hóa thông báo lỗi tiếng Việt.  
  2. **Service (`src/services/admin-users.service.ts`)**: dùng `HttpClient` + `apiConfig` để wrap endpoint `/admin/users`, `/admin/users/:id`, `/disable`, `/enable`. Không fetch trực tiếp ở view.  
  3. **Hook/controller (`src/hooks/admin-users/**`)**: `useAdminUsersList`, `useAdminUserDetail`, `useAdminUserActions` quản lý state, cache invalidation, error message.  
  4. **Component (`components/admin/users/**`)**: thuần presentational (`UsersToolbar`, `UsersTable`, `UserProfileCard`, `UserStatsGrid`, dialogs).  
  5. **App view (`app/admin/(users)/**`)**: kết hợp hook + component, định nghĩa route qua Expo Router.

- **UCA01-1 · User List View (`app/admin/(users)/index.tsx`)**  
  - Component chính: `UsersToolbar`, `UsersTable`, (tương lai `UsersPagination`).  
  - Hook: `useAdminUsersList` (debounce search, phân trang, refresh).  
  - Endpoint: `GET /admin/users` (params `search`, `status`, `role`, `startDate`, `endDate`, `sortBy`, `sortOrder`, `page`, `limit`).  
  - UI checklist: sticky header, avatar fallback, badge trạng thái, empty/error state card, export button (disable nếu API chưa sẵn).  
  - Tech debt: local store preset filter, add pagination control khi backend finalize metadata.

- **UCA01-2 · User Detail View (`app/admin/(users)/[userId]/index.tsx`)**  
  - Component: `UserProfileCard`, `UserStatsGrid`, `RecentActivitiesList` (stub).  
  - Hook: `useAdminUserDetail(userId)` fetch detail + stats + moderation logs.  
  - Endpoint: `GET /admin/users/:id` (stats/logs trả về chung DTO).  
  - UX: breadcrumb quay lại list, CTA `Khóa/Mở khóa`, hiển thị log mới nhất + nút refresh.  
  - Security: khi backend expose flag role, ẩn thống kê nhạy cảm nếu không phải Super Admin.

- **UCA01-3 · Disable Modal/Flow (`app/admin/(users)/[userId]/disable.tsx`)**  
  - UI: `DisableUserDialog` (textarea lý do, preset thời hạn `lockDurationLabels`, ghi chú tùy chọn).  
  - Hook: `useAdminUserActions().disable` gọi `PATCH /admin/users/:id/disable`, invalidate list/detail.  
  - Extra: checkbox "Gửi email thông báo" + preview (mock) khi backend bổ sung.  
  - Error UX: banner riêng khi BE trả `ConflictError` (user đã khóa) hoặc `Unauthorized`.

- **UCA01-4 · Enable Modal/Flow (`app/admin/(users)/[userId]/enable.tsx`)**  
  - UI: `EnableUserDialog` với ô note, hiển thị log khóa gần nhất (kéo từ detail hook).  
  - Hook: `useAdminUserActions().enable` gọi `PATCH /admin/users/:id/enable`, cập nhật state optimistic.  
  - Batch mode: chuẩn bị `EnableUsersBulkSheet` sử dụng cùng hook khi endpoint `/batch/unlock` sẵn sàng.  
  - Audit reminder: copywriter note “mọi thao tác sẽ được ghi log”.

- **Song song cần chuẩn bị**  
  - Mock service (MSW/axios adapter) để test offline; toggle qua ENV `USE_MOCK_ADMIN_USERS`.  
  - Unit test hook/controller (loading/success/error) + component snapshot.  
  - README `app/admin/(users)/README.md`: ghi rõ contract BE hiện tại, TODO sau khi server đổi DTO/route.


## 10. Tracklist dựng view cho UC1 (`@auth`)
- **Nguyên tắc chung**  
  - Tách namespace `apps/auth` hoặc tương đương; không can thiệp component chung (modal, button, input) trừ khi chủ repo duyệt.  
  - Mọi form tuân thủ validator FE sao chép từ Joi backend, khai báo tại `apps/auth/validators/*`.  
  - Service gateway dùng `authApi` bọc toàn bộ routes; controller/hook chỉ gọi gateway, không gọi HTTP client trực tiếp.  
  - Ghi rõ assumption (vd: chưa có social login) trong `apps/auth/README.md` để agent khác tiếp tục.

- **UC1-1 · Register View**  
  - Component: `RegisterForm`, `AuthCard`, `TermsCheckbox`, optional `SocialButtons`.  
  - Controller/hook: `useRegisterController` quản lý validator, toggle email/phone mode, và logic chuyển hướng khi `requiresVerification`.  
  - API: `POST /auth/register`; chuẩn bị slot cho `verify/resend` nếu response gắn cờ cần xác thực.  
  - UX checklist: password strength meter, error parser cho từng field, toast chung khi lỗi server, link sang login.

- **UC1-2 · Login View**  
  - Component: `LoginForm`, `RememberMeToggle`, `ForgotPasswordLink`, `SocialButtons` (ẩn nếu backend chưa bật).  
  - Controller: `useLoginController` xử lý session store, lưu cookie/secure storage khi `rememberMe = true`, redirect theo `returnTo`.  
  - API: `POST /auth/login`; chuẩn bị parser cho lỗi tài khoản khóa/chưa verify.  
  - Extra: state hiển thị countdown sau khi bị khóa 15 phút, focus-first-input, submit bằng Enter.

- **UC1-3 · Logout Menu & Guard**  
  - Component: `UserMenu` + `LogoutConfirmDialog` (desktop) và `MobileLogoutSheet`.  
  - Controller: `useLogoutAction` gọi `POST /auth/logout` hoặc `/auth/logout/all`, đồng thời flush session store & cache.  
  - UX: confirm dialog, toast thành công, auto-redirect về `/auth/login`; fallback offline mode (thông báo sẽ logout khi có mạng).  
  - Technical note: guard middleware kiểm tra session và điều hướng về login khi token hết hạn.

- **UC1-4 · Profile View**  
  - Component: `ProfileHeader`, `ProfileDetailsGrid`, `ActivityStats`, `ProfileSkeleton`.  
  - Controller: `useProfileController` fetch `GET /auth/profile`, hydrate store, xử lý avatar fallback.  
  - UI: hiển thị trạng thái xác thực, lần đăng nhập cuối, thống kê bài viết/yêu thích/bình luận; cung cấp CTA "Cập nhật thông tin" và "Đổi mật khẩu".  
  - Error states: card thông báo + nút `Retry`; redirect login khi 401.

- **UC1-5 · Profile Edit Drawer**  
  - Component: `EditProfileDrawer`, `AvatarUploader`, `DisplayNameField`, `PreviewPane`.  
  - Controller: `useUpdateProfileController` chuẩn hóa FormData upload, xử lý debounce preview ảnh, rollback khi hủy.  
  - API: `PATCH /auth/profile` (multipart); đảm bảo resize client-side trước khi upload nếu có.  
  - Validation: tên >=2 ký tự, chặn ký tự đặc biệt; ảnh JPG/PNG <5 MB; hiển thị thông báo cụ thể.

- **UC1-6 · Change Password View**  
  - Component: `ChangePasswordForm`, `PasswordCriteriaList`, `SessionInvalidateNotice`.  
  - Controller: `useChangePasswordController` xác thực mật khẩu cũ, disable submit khi không đạt criteria, hiển thị tiến trình.  
  - API: `POST /auth/change-password`; sau thành công, gọi `authStore.logoutAll()` và chuyển về login.  
  - Security UX: nhắc người dùng rằng mọi phiên khác sẽ bị đăng xuất, yêu cầu đăng nhập lại.

- **UC1-7 · Forgot/Reset Password Flow**  
  - Screen A (Request): `ForgotPasswordForm` nhập email/SĐT, controller `useForgotPasswordController` gọi `POST /auth/forgot-password`, hiển thị timer resend và giới hạn 3 req/giờ.  
  - Screen B (Reset): `ResetPasswordForm` với trường token/OTP + mật khẩu mới, controller `useResetPasswordController` gọi `POST /auth/reset-password`.  
  - UI: state xác nhận hướng dẫn đã gửi, link quay lại login, hiển thị note token hết hạn 24h.  
  - Edge cases: banner khi token hết hạn → CTA “Gửi lại”, xử lý OTP vs link bằng tab hoặc toggle.

