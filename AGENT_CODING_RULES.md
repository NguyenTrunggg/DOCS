# AGENT CODING RULES & GUIDELINES

Tài liệu này quy định các quy tắc, chuẩn mực và quy trình mà AI Agent phải tuân thủ khi tham gia viết code cho dự án **DATN**.

## 1. Nguyên Tắc Chung (General Principles)

- **SOLID:** Tuân thủ tuyệt đối 5 nguyên lý SOLID.
  - *S (Single Responsibility):* Một class/function chỉ làm một việc.
  - *O (Open/Closed):* Mở để mở rộng, đóng để sửa đổi (sử dụng kế thừa, interface).
  - *L (Liskov Substitution):* Class con thay thế được class cha.
  - *I (Interface Segregation):* Chia nhỏ interface, không dùng interface khổng lồ.
  - *D (Dependency Inversion):* Phụ thuộc vào abstraction, không phụ thuộc vào implementation cụ thể.
- **Type Safety:** Luôn sử dụng TypeScript với strict types. Hạn chế tối đa `any`.
- **DRY (Don't Repeat Yourself):** Tách logic lặp lại thành helper/utility/hook.
- **Clean Code:** Đặt tên biến/hàm rõ nghĩa (Tiếng Anh), comment giải thích logic phức tạp.

---

## 2. Backend Rules (BE - Node.js/Express)

### 2.1. Cấu Trúc Thư Mục & Modules
Code được tổ chức theo **Layered Architecture** trong `src/modules/{module_name}/`:
- **`controllers/`**: Nhận request, validate input (gọi validator), gọi service, trả response. KHÔNG chứa business logic.
- **`services/`**: Chứa Business Logic. Gọi Repository để lấy data.
- **`repositories/`**: Data Access Layer (DAL). Trực tiếp làm việc với Database (Prisma).
- **`dto/`**: Data Transfer Objects.
  - `request/`: Định nghĩa input (`.dto.ts`).
  - `response/`: Định nghĩa output (`-response.dto.ts`).
- **`validators/`**: Định nghĩa Joi Schema để validate request.
- **`routes.ts`**: Định nghĩa endpoint và gắn middleware.

### 2.2. Coding Conventions
- **Controller:**
  - Dùng `asyncHandler` để bao bọc async function.
  - Trả về kết quả dùng `successResponse` hoặc `paginatedResponse`.
  - Ví dụ: `adminUsersController.getUsers`
- **Service:**
  - Kế thừa `BaseService` (nếu có).
  - Throw custom errors: `NotFoundError`, `ValidationError`, `ConflictError`, `ForbiddenError` (từ `@common/errors`).
  - KHÔNG return response object (res), chỉ return data hoặc throw error.
- **Repository:**
  - Kế thừa `BaseRepository`.
  - Chỉ thực hiện query DB (Prisma).
- **Middleware:**
  - Thứ tự: `authenticate` -> `authorize` -> `validate` -> Controller.

### 2.3. Quy Trình Implement BE
1. **Đọc Use Case:** Hiểu input/output và logic.
2. **Tạo DTO & Validator:** Định nghĩa interface request/response và Joi schema.
3. **Implement Repository:** Viết hàm query DB cần thiết.
4. **Implement Service:** Viết logic nghiệp vụ, gọi Repository.
5. **Implement Controller:** Gọi Service, handle request.
6. **Đăng ký Route:** Thêm vào `routes.ts` và `server.ts` (nếu module mới).

---

## 3. Frontend Rules (FE - Next.js/React)

### 3.1. Cấu Trúc Thư Mục
- **`app/`**: Next.js App Router. Mỗi folder là một route. `page.tsx` là entry point.
- **`src/components/`**:
  - `common/`: UI components tái sử dụng (Button, Input, Modal...).
  - `{module}/{feature}/`: Components đặc thù cho tính năng (ví dụ: `admin/users/UserListTable.tsx`).
- **`src/services/`**: Wrapper cho API calls (Axios).
  - Cấu trúc: `services/{module}/{feature}.service.ts`.
- **`src/hooks/`**: Custom hooks (Logic tách biệt khỏi UI).
  - Ví dụ: `useAdminUsersTable`, `useAdminUserDetail`.
- **`src/types/`**: TypeScript definitions (đồng bộ với BE DTO).

### 3.2. Coding Conventions
- **Components:**
  - Functional Components.
  - Props interface rõ ràng.
  - Tách logic phức tạp ra Custom Hook.
  - Xử lý loading/error state đầy đủ.
- **State Management:**
  - Dùng React Query (nếu có) hoặc `useEffect` + `useState` trong custom hooks.
  - Form handling: React Hook Form + Zod (hoặc Joi resolver).
- **API Integration:**
  - KHÔNG gọi `fetch`/`axios` trực tiếp trong Component.
  - Luôn gọi qua `src/services/`.
  - Handle error (try-catch) và hiển thị Toast notification.

### 3.3. Quy Trình Implement FE
1. **Đọc Use Case & BE DTO:** Hiểu dữ liệu cần hiển thị và gửi đi.
2. **Update Types:** Định nghĩa types trong `src/types/` khớp với API Response/Request.
3. **Create Service:** Viết hàm gọi API trong `src/services/`.
4. **Build Components:** Tạo các component nhỏ (Table, Form, Modal).
5. **Create Hooks:** (Optional) Viết hook để fetch data và handle logic.
6. **Assemble Page:** Ghép các component vào `app/.../page.tsx`.

---

## 4. Workflow Dành Cho Agent

Khi nhận một task (ví dụ: "Implement UC Quản lý User"), Agent cần thực hiện theo các bước:

1.  **Analysis (Phân Tích):**
    - Đọc file Use Case trong `DOCS/UC/...`.
    - Đọc file `API_DEVELOPMENT_GUIDE.md` (BE) hoặc `UI_DEVELOPMENT_GUIDE.md` (FE) để nắm context.
    - Kiểm tra code hiện tại để tìm các pattern tương tự (để copy style).

2.  **Backend Implementation (Nếu có):**
    - Tạo/Update DTO -> Validator -> Repository -> Service -> Controller -> Route.
    - *Check:* Đảm bảo đã import đúng các shared utils và errors.

3.  **Frontend Implementation (Nếu có):**
    - Tạo/Update Types -> Service -> Components -> Page.
    - *Check:* Đảm bảo UI match với Wireframe/Mô tả trong UC.

4.  **Review:**
    - Kiểm tra lại import paths (dùng alias `@/...`).
    - Kiểm tra Type safety.
    - Kiểm tra Linter (nếu có thể).

---

**Lưu ý quan trọng:**
- Tuyệt đối không sửa code của các common libraries/base classes trừ khi được yêu cầu cụ thể.
- Luôn sử dụng lại các components/functions đã có (Reusability).
- Nếu gặp lỗi logic phức tạp, hãy chia nhỏ vấn đề và giải quyết từng phần.


