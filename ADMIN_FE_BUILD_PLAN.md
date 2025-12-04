# 🚀 KẾ HOẠCH XÂY DỰNG FRONTEND ADMIN

**Ngày lập kế hoạch:** 2024  
**Phạm vi:** UC-A2, UC-A3, UC-A4, UC-A5  
**Trạng thái Backend:** ✅ Hoàn thiện 100%

---

## 📋 MỤC LỤC

1. [Tổng quan](#tổng-quan)
2. [Cấu trúc dự án](#cấu-trúc-dự-án)
3. [Kế hoạch theo module](#kế-hoạch-theo-module)
4. [Timeline và ưu tiên](#timeline-và-ưu-tiên)
5. [Chi tiết implementation](#chi-tiết-implementation)

---

## 🎯 TỔNG QUAN

### Mục tiêu
Xây dựng Frontend cho 4 module admin còn lại:
- ✅ UC-A1: Quản lý người dùng (Đã hoàn thành - làm mẫu)
- ❌ UC-A2: Quản lý công thức hệ thống
- ❌ UC-A3: Quản lý danh mục & nguyên liệu
- ❌ UC-A4: Quản lý tài khoản admin
- ❌ UC-A5: Dashboard & Thống kê

### Nguyên tắc thiết kế
1. **Tuân thủ SOLID principles**
2. **Tái sử dụng pattern từ UC-A1** (đã làm mẫu)
3. **Consistent UI/UX** across all modules
4. **Type-safe** với TypeScript
5. **Reusable components** cho các module tương tự

---

## 📁 CẤU TRÚC DỰ ÁN

### Cấu trúc tổng thể (theo pattern UC-A1)

```
FE/
├── app/
│   └── admin/
│       ├── (users)/              ✅ Đã có
│       │   ├── _layout.tsx
│       │   ├── index.tsx
│       │   └── [userId]/
│       │       ├── index.tsx
│       │       ├── disable.tsx
│       │       └── enable.tsx
│       │
│       ├── (recipes)/            ❌ Cần xây dựng
│       │   ├── _layout.tsx
│       │   ├── index.tsx
│       │   ├── pending.tsx       # Danh sách chờ duyệt
│       │   ├── create/
│       │   │   └── index.tsx
│       │   └── [recipeId]/
│       │       ├── index.tsx
│       │       ├── edit.tsx
│       │       ├── approve.tsx
│       │       └── reject.tsx
│       │
│       ├── (categories)/         ❌ Cần xây dựng
│       │   ├── _layout.tsx
│       │   ├── index.tsx
│       │   ├── create.tsx
│       │   └── [categoryId]/
│       │       ├── index.tsx
│       │       └── edit.tsx
│       │
│       ├── (ingredients)/        ❌ Cần xây dựng
│       │   ├── _layout.tsx
│       │   ├── index.tsx
│       │   ├── create.tsx
│       │   └── [ingredientId]/
│       │       ├── index.tsx
│       │       └── edit.tsx
│       │
│       ├── (accounts)/           ❌ Cần xây dựng
│       │   ├── _layout.tsx
│       │   ├── index.tsx
│       │   ├── create.tsx
│       │   └── [adminId]/
│       │       ├── index.tsx
│       │       └── roles.tsx     # Phân quyền
│       │
│       └── (analytics)/          ❌ Cần xây dựng
│           ├── _layout.tsx
│           ├── index.tsx         # Dashboard
│           └── exports/
│               └── [fileName]/
│
├── components/
│   └── admin/
│       ├── users/                ✅ Đã có
│       ├── recipes/              ❌ Cần xây dựng
│       ├── categories/           ❌ Cần xây dựng
│       ├── ingredients/          ❌ Cần xây dựng
│       ├── accounts/             ❌ Cần xây dựng
│       ├── analytics/            ❌ Cần xây dựng
│       └── shared/               ❌ Mới - Components dùng chung
│           ├── DataTable.tsx     # Bảng dữ liệu generic
│           ├── SearchBar.tsx     # Thanh tìm kiếm
│           ├── FilterPanel.tsx   # Panel lọc
│           ├── Pagination.tsx    # Phân trang
│           ├── StatusBadge.tsx   # Badge trạng thái
│           └── ConfirmDialog.tsx # Dialog xác nhận
│
├── src/
│   ├── hooks/
│   │   ├── admin-users/          ✅ Đã có
│   │   ├── admin-recipes/        ❌ Cần xây dựng
│   │   ├── admin-categories/     ❌ Cần xây dựng
│   │   ├── admin-ingredients/    ❌ Cần xây dựng
│   │   ├── admin-accounts/       ❌ Cần xây dựng
│   │   └── admin-analytics/      ❌ Cần xây dựng
│   │
│   ├── services/
│   │   ├── admin-users.service.ts       ✅ Đã có
│   │   ├── admin-recipes.service.ts     ❌ Cần xây dựng
│   │   ├── admin-categories.service.ts  ❌ Cần xây dựng
│   │   ├── admin-ingredients.service.ts ❌ Cần xây dựng
│   │   ├── admin-accounts.service.ts    ❌ Cần xây dựng
│   │   └── admin-analytics.service.ts   ❌ Cần xây dựng
│   │
│   ├── types/
│   │   ├── admin-users.ts        ✅ Đã có
│   │   ├── admin-recipes.ts      ❌ Cần xây dựng
│   │   ├── admin-categories.ts   ❌ Cần xây dựng
│   │   ├── admin-ingredients.ts  ❌ Cần xây dựng
│   │   ├── admin-accounts.ts     ❌ Cần xây dựng
│   │   └── admin-analytics.ts    ❌ Cần xây dựng
│   │
│   └── validators/
│       ├── admin-users.validator.ts      ✅ Đã có
│       ├── admin-recipes.validator.ts    ❌ Cần xây dựng
│       ├── admin-categories.validator.ts ❌ Cần xây dựng
│       ├── admin-ingredients.validator.ts ❌ Cần xây dựng
│       ├── admin-accounts.validator.ts   ❌ Cần xây dựng
│       └── admin-analytics.validator.ts  ❌ Cần xây dựng
│
└── constants/
    ├── admin-users.ts            ✅ Đã có
    ├── admin-recipes.ts          ❌ Cần xây dựng
    ├── admin-categories.ts       ❌ Cần xây dựng
    ├── admin-ingredients.ts      ❌ Cần xây dựng
    ├── admin-accounts.ts         ❌ Cần xây dựng
    └── admin-analytics.ts        ❌ Cần xây dựng
```

---

## 🎯 KẾ HOẠCH THEO MODULE

### 📊 UC-A5: Dashboard & Analytics [HIGH PRIORITY]

**Mục tiêu:** Xây dựng dashboard với KPIs, biểu đồ và báo cáo

#### 1. Service Layer
- [ ] `src/services/admin-analytics.service.ts`
  - `getDashboard(params)` - Lấy dữ liệu dashboard
  - `getDrillDown(params)` - Lấy dữ liệu drill-down
  - `exportReport(params)` - Xuất báo cáo
  - `downloadExport(fileName)` - Tải file đã xuất

#### 2. Types
- [ ] `src/types/admin-analytics.ts`
  - `DashboardKPIs`, `DashboardCharts`, `DrillDownParams`, `ExportReportParams`
  - Response types cho dashboard, drill-down, export

#### 3. Validators
- [ ] `src/validators/admin-analytics.validator.ts`
  - Validator cho dashboard query
  - Validator cho drill-down query
  - Validator cho export report

#### 4. Constants
- [ ] `constants/admin-analytics.ts`
  - Time ranges (7 days, 30 days, custom)
  - Chart types
  - Export formats (PDF, CSV)

#### 5. Hooks
- [ ] `src/hooks/admin-analytics/use-dashboard.ts`
- [ ] `src/hooks/admin-analytics/use-drill-down.ts`
- [ ] `src/hooks/admin-analytics/use-export-report.ts`

#### 6. Components
- [ ] `components/admin/analytics/DashboardKPIs.tsx` - Hiển thị KPIs cards
- [ ] `components/admin/analytics/ChartContainer.tsx` - Container cho biểu đồ
- [ ] `components/admin/analytics/UserGrowthChart.tsx` - Biểu đồ tăng trưởng user
- [ ] `components/admin/analytics/RecipeStatsChart.tsx` - Biểu đồ thống kê công thức
- [ ] `components/admin/analytics/StatusDistributionChart.tsx` - Biểu đồ phân bổ trạng thái
- [ ] `components/admin/analytics/ActivityHeatmap.tsx` - Heatmap hoạt động
- [ ] `components/admin/analytics/FilterPanel.tsx` - Panel lọc thời gian
- [ ] `components/admin/analytics/ExportDialog.tsx` - Dialog xuất báo cáo

#### 7. Pages
- [ ] `app/admin/(analytics)/_layout.tsx` - Layout với header
- [ ] `app/admin/(analytics)/index.tsx` - Dashboard chính

#### 8. Thư viện cần thiết
- [ ] `react-native-chart-kit` hoặc `victory-native` - Biểu đồ
- [ ] `date-fns` - Xử lý ngày tháng

---

### 📝 UC-A2: Quản lý Công thức [MEDIUM PRIORITY]

**Mục tiêu:** Quản lý công thức hệ thống và phê duyệt công thức người dùng

#### 1. Service Layer
- [ ] `src/services/admin-recipes.service.ts`
  - `getRecipes(params)` - Danh sách công thức
  - `getPendingRecipes(params)` - Danh sách chờ duyệt
  - `getRecipeById(id)` - Chi tiết công thức
  - `createRecipe(data)` - Tạo công thức mới
  - `updateRecipe(id, data)` - Cập nhật công thức
  - `deleteRecipe(id)` - Xóa công thức
  - `approveRecipe(id, data)` - Phê duyệt công thức
  - `rejectRecipe(id, data)` - Từ chối công thức

#### 2. Types
- [ ] `src/types/admin-recipes.ts`
  - `AdminRecipe`, `AdminRecipeListQuery`, `CreateRecipePayload`, `UpdateRecipePayload`
  - `ApproveRecipePayload`, `RejectRecipePayload`

#### 3. Validators
- [ ] `src/validators/admin-recipes.validator.ts`
  - Validator cho list query, create, update, approve, reject

#### 4. Constants
- [ ] `constants/admin-recipes.ts`
  - Recipe statuses, difficulties, time presets

#### 5. Hooks
- [ ] `src/hooks/admin-recipes/use-recipes-list.ts`
- [ ] `src/hooks/admin-recipes/use-pending-recipes.ts`
- [ ] `src/hooks/admin-recipes/use-recipe-detail.ts`
- [ ] `src/hooks/admin-recipes/use-create-recipe.ts`
- [ ] `src/hooks/admin-recipes/use-update-recipe.ts`
- [ ] `src/hooks/admin-recipes/use-delete-recipe.ts`
- [ ] `src/hooks/admin-recipes/use-approve-recipe.ts`
- [ ] `src/hooks/admin-recipes/use-reject-recipe.ts`

#### 6. Components
- [ ] `components/admin/recipes/RecipesTable.tsx` - Bảng danh sách công thức
- [ ] `components/admin/recipes/RecipesToolbar.tsx` - Thanh công cụ (search, filter)
- [ ] `components/admin/recipes/RecipeCard.tsx` - Card hiển thị công thức
- [ ] `components/admin/recipes/RecipeDetailView.tsx` - Chi tiết công thức
- [ ] `components/admin/recipes/RecipeForm.tsx` - Form tạo/sửa công thức
- [ ] `components/admin/recipes/IngredientInput.tsx` - Input nguyên liệu
- [ ] `components/admin/recipes/StepInput.tsx` - Input bước thực hiện
- [ ] `components/admin/recipes/ApproveDialog.tsx` - Dialog phê duyệt
- [ ] `components/admin/recipes/RejectDialog.tsx` - Dialog từ chối
- [ ] `components/admin/recipes/DeleteConfirmDialog.tsx` - Dialog xác nhận xóa

#### 7. Pages
- [ ] `app/admin/(recipes)/_layout.tsx`
- [ ] `app/admin/(recipes)/index.tsx` - Danh sách công thức hệ thống
- [ ] `app/admin/(recipes)/pending.tsx` - Danh sách chờ duyệt
- [ ] `app/admin/(recipes)/create/index.tsx` - Tạo công thức mới
- [ ] `app/admin/(recipes)/[recipeId]/index.tsx` - Chi tiết công thức
- [ ] `app/admin/(recipes)/[recipeId]/edit.tsx` - Sửa công thức
- [ ] `app/admin/(recipes)/[recipeId]/approve.tsx` - Phê duyệt công thức
- [ ] `app/admin/(recipes)/[recipeId]/reject.tsx` - Từ chối công thức

#### 8. Tái sử dụng từ recipe-management
- [ ] Có thể tái sử dụng form components từ `recipe-management` module

---

### 📁 UC-A3: Quản lý Danh mục [LOW PRIORITY]

**Mục tiêu:** Quản lý danh mục món ăn

#### 1. Service Layer
- [ ] `src/services/admin-categories.service.ts`
  - `getCategories(params)` - Danh sách danh mục
  - `getCategoryById(id)` - Chi tiết danh mục
  - `createCategory(data)` - Tạo danh mục mới
  - `updateCategory(id, data)` - Cập nhật danh mục
  - `deleteCategory(id)` - Xóa danh mục

#### 2. Types
- [ ] `src/types/admin-categories.ts`
  - `AdminCategory`, `AdminCategoryListQuery`, `CreateCategoryPayload`, `UpdateCategoryPayload`

#### 3. Validators
- [ ] `src/validators/admin-categories.validator.ts`

#### 4. Constants
- [ ] `constants/admin-categories.ts`

#### 5. Hooks
- [ ] `src/hooks/admin-categories/use-categories-list.ts`
- [ ] `src/hooks/admin-categories/use-category-detail.ts`
- [ ] `src/hooks/admin-categories/use-create-category.ts`
- [ ] `src/hooks/admin-categories/use-update-category.ts`
- [ ] `src/hooks/admin-categories/use-delete-category.ts`

#### 6. Components
- [ ] `components/admin/categories/CategoriesTable.tsx`
- [ ] `components/admin/categories/CategoriesToolbar.tsx`
- [ ] `components/admin/categories/CategoryCard.tsx`
- [ ] `components/admin/categories/CategoryForm.tsx` - Form tạo/sửa

#### 7. Pages
- [ ] `app/admin/(categories)/_layout.tsx`
- [ ] `app/admin/(categories)/index.tsx` - Danh sách danh mục
- [ ] `app/admin/(categories)/create.tsx` - Tạo danh mục mới
- [ ] `app/admin/(categories)/[categoryId]/index.tsx` - Chi tiết
- [ ] `app/admin/(categories)/[categoryId]/edit.tsx` - Sửa danh mục

---

### 🥗 UC-A3: Quản lý Nguyên liệu [LOW PRIORITY]

**Mục tiêu:** Quản lý nguyên liệu và danh mục nguyên liệu

#### 1. Service Layer
- [ ] `src/services/admin-ingredients.service.ts`
  - `getIngredientCategories()` - Danh sách danh mục nguyên liệu
  - `getIngredients(params)` - Danh sách nguyên liệu
  - `getIngredientById(id)` - Chi tiết nguyên liệu
  - `createIngredient(data)` - Tạo nguyên liệu mới
  - `updateIngredient(id, data)` - Cập nhật nguyên liệu
  - `deleteIngredient(id)` - Xóa nguyên liệu

#### 2. Types
- [ ] `src/types/admin-ingredients.ts`

#### 3. Validators
- [ ] `src/validators/admin-ingredients.validator.ts`

#### 4. Constants
- [ ] `constants/admin-ingredients.ts`

#### 5. Hooks
- [ ] `src/hooks/admin-ingredients/use-ingredients-list.ts`
- [ ] `src/hooks/admin-ingredients/use-ingredient-detail.ts`
- [ ] `src/hooks/admin-ingredients/use-create-ingredient.ts`
- [ ] `src/hooks/admin-ingredients/use-update-ingredient.ts`
- [ ] `src/hooks/admin-ingredients/use-delete-ingredient.ts`

#### 6. Components
- [ ] `components/admin/ingredients/IngredientsTable.tsx`
- [ ] `components/admin/ingredients/IngredientsToolbar.tsx`
- [ ] `components/admin/ingredients/IngredientCard.tsx`
- [ ] `components/admin/ingredients/IngredientForm.tsx`

#### 7. Pages
- [ ] `app/admin/(ingredients)/_layout.tsx`
- [ ] `app/admin/(ingredients)/index.tsx` - Danh sách nguyên liệu
- [ ] `app/admin/(ingredients)/create.tsx` - Tạo nguyên liệu mới
- [ ] `app/admin/(ingredients)/[ingredientId]/index.tsx` - Chi tiết
- [ ] `app/admin/(ingredients)/[ingredientId]/edit.tsx` - Sửa nguyên liệu

---

### 👥 UC-A4: Quản lý Tài khoản Admin [MEDIUM PRIORITY]

**Mục tiêu:** Quản lý tài khoản admin và phân quyền

#### 1. Service Layer
- [ ] `src/services/admin-accounts.service.ts`
  - `getAdminAccounts(params)` - Danh sách admin
  - `getAllRoles()` - Danh sách roles
  - `getAllPermissions()` - Danh sách permissions
  - `getAdminAccountById(id)` - Chi tiết admin
  - `createAdminAccount(data)` - Tạo admin mới
  - `updateAdminRoles(id, data)` - Cập nhật phân quyền
  - `deleteAdminAccount(id)` - Xóa admin

#### 2. Types
- [ ] `src/types/admin-accounts.ts`
  - `AdminAccount`, `Role`, `Permission`, `UpdateRolesPayload`

#### 3. Validators
- [ ] `src/validators/admin-accounts.validator.ts`

#### 4. Constants
- [ ] `constants/admin-accounts.ts`
  - Role names, Permission names

#### 5. Hooks
- [ ] `src/hooks/admin-accounts/use-admin-accounts-list.ts`
- [ ] `src/hooks/admin-accounts/use-admin-account-detail.ts`
- [ ] `src/hooks/admin-accounts/use-create-admin-account.ts`
- [ ] `src/hooks/admin-accounts/use-update-roles.ts`
- [ ] `src/hooks/admin-accounts/use-delete-admin-account.ts`

#### 6. Components
- [ ] `components/admin/accounts/AdminAccountsTable.tsx`
- [ ] `components/admin/accounts/AdminAccountsToolbar.tsx`
- [ ] `components/admin/accounts/AdminAccountCard.tsx`
- [ ] `components/admin/accounts/AdminAccountForm.tsx` - Form tạo admin
- [ ] `components/admin/accounts/RoleSelector.tsx` - Chọn roles
- [ ] `components/admin/accounts/PermissionMatrix.tsx` - Ma trận phân quyền
- [ ] `components/admin/accounts/RolePermissionsEditor.tsx` - Editor phân quyền

#### 7. Pages
- [ ] `app/admin/(accounts)/_layout.tsx`
- [ ] `app/admin/(accounts)/index.tsx` - Danh sách admin
- [ ] `app/admin/(accounts)/create.tsx` - Tạo admin mới
- [ ] `app/admin/(accounts)/[adminId]/index.tsx` - Chi tiết admin
- [ ] `app/admin/(accounts)/[adminId]/roles.tsx` - Phân quyền

---

## 📅 TIMELINE VÀ ƯU TIÊN

### Phase 1: Foundation & Shared Components [Tuần 1]
**Mục tiêu:** Xây dựng components dùng chung và setup cấu trúc

- [ ] Tạo shared components (`components/admin/shared/`)
  - DataTable, SearchBar, FilterPanel, Pagination, StatusBadge, ConfirmDialog
- [ ] Setup routing structure (`app/_layout.tsx`)
- [ ] Tạo constants và utilities chung
- [ ] Setup thư viện biểu đồ cho UC-A5

### Phase 2: UC-A5 Dashboard & Analytics [Tuần 2-3] 🔴 HIGH PRIORITY
**Mục tiêu:** Dashboard với KPIs và biểu đồ

- [ ] Service layer (1 ngày)
- [ ] Types & Validators (0.5 ngày)
- [ ] Hooks (1 ngày)
- [ ] Components (2 ngày)
  - KPI cards, Charts, Filter panel, Export dialog
- [ ] Pages (0.5 ngày)
- [ ] Testing & Polish (1 ngày)

**Tổng:** ~6 ngày

### Phase 3: UC-A2 Quản lý Công thức [Tuần 4-5] 🟡 MEDIUM PRIORITY
**Mục tiêu:** Quản lý công thức và phê duyệt

- [ ] Service layer (1 ngày)
- [ ] Types & Validators (1 ngày)
- [ ] Hooks (1.5 ngày)
- [ ] Components (3 ngày)
  - Table, Form, Detail view, Approve/Reject dialogs
- [ ] Pages (1.5 ngày)
- [ ] Testing & Polish (1 ngày)

**Tổng:** ~9 ngày

### Phase 4: UC-A4 Quản lý Tài khoản Admin [Tuần 6] 🟡 MEDIUM PRIORITY
**Mục tiêu:** Quản lý admin và phân quyền

- [ ] Service layer (0.5 ngày)
- [ ] Types & Validators (0.5 ngày)
- [ ] Hooks (1 ngày)
- [ ] Components (2 ngày)
  - Table, Form, Role selector, Permission matrix
- [ ] Pages (1 ngày)
- [ ] Testing & Polish (0.5 ngày)

**Tổng:** ~5.5 ngày

### Phase 5: UC-A3 Quản lý Danh mục & Nguyên liệu [Tuần 7] 🟢 LOW PRIORITY
**Mục tiêu:** Quản lý dữ liệu cơ bản

#### 5a. Categories (2.5 ngày)
- [ ] Service, Types, Validators (0.5 ngày)
- [ ] Hooks (0.5 ngày)
- [ ] Components (1 ngày)
- [ ] Pages (0.5 ngày)

#### 5b. Ingredients (2.5 ngày)
- [ ] Service, Types, Validators (0.5 ngày)
- [ ] Hooks (0.5 ngày)
- [ ] Components (1 ngày)
- [ ] Pages (0.5 ngày)

**Tổng:** ~5 ngày

### Phase 6: Integration & Polish [Tuần 8]
- [ ] Integration testing
- [ ] UI/UX consistency check
- [ ] Performance optimization
- [ ] Documentation
- [ ] Bug fixes

---

## 📝 CHI TIẾT IMPLEMENTATION

### Pattern chuẩn (theo UC-A1)

#### 1. Service Pattern
```typescript
// src/services/admin-{module}.service.ts
export const admin{Module}Service = {
  async list(query: ListQuery): Promise<PaginatedResponse> {
    // Validate query
    // Call API
    // Return data
  },
  async getDetail(id: string): Promise<Detail> {
    // ...
  },
  // ... other methods
};
```

#### 2. Hook Pattern
```typescript
// src/hooks/admin-{module}/use-{module}-list.ts
export function use{Module}List() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [query, setQuery] = useState<Query>({});

  const fetch = async () => {
    // Fetch logic
  };

  return { data, loading, error, query, setQuery, refresh: fetch };
}
```

#### 3. Component Pattern
```typescript
// components/admin/{module}/{Module}Table.tsx
export function {Module}Table({ data, loading, onPress }) {
  // Table rendering logic
}

// components/admin/{module}/{Module}Toolbar.tsx
export function {Module}Toolbar({ query, onChange, onRefresh }) {
  // Toolbar with search, filters
}
```

#### 4. Page Pattern
```typescript
// app/admin/({module})/index.tsx
export default function Admin{Module}ListScreen() {
  const router = useRouter();
  const { data, loading, error, query, setQuery, refresh } = use{Module}List();

  return (
    <ScrollView>
      <{Module}Toolbar {...} />
      <{Module}Table {...} />
    </ScrollView>
  );
}
```

---

## 🎨 UI/UX GUIDELINES

### Consistent Design
1. **Colors:** Sử dụng theme colors từ `constants/theme.ts`
2. **Spacing:** Consistent padding/margin (16px, 8px)
3. **Typography:** Sử dụng ThemedText component
4. **Buttons:** Consistent button styles
5. **Forms:** Consistent form input styles
6. **Tables:** Sử dụng shared DataTable component

### Responsive Design
- Mobile-first approach
- ScrollView cho danh sách dài
- Modal cho forms phức tạp

### Loading States
- Skeleton loaders cho tables
- Loading indicators cho buttons
- Empty states với messages rõ ràng

### Error Handling
- Error messages user-friendly
- Retry mechanisms
- Offline handling (nếu cần)

---

## ✅ CHECKLIST TỔNG QUAN

### Phase 1: Foundation
- [ ] Shared components
- [ ] Routing setup
- [ ] Constants & utilities
- [ ] Chart library setup

### Phase 2: UC-A5 (Analytics)
- [ ] Service layer
- [ ] Types & validators
- [ ] Hooks
- [ ] Components (KPIs, Charts, Filters)
- [ ] Pages
- [ ] Testing

### Phase 3: UC-A2 (Recipes)
- [ ] Service layer
- [ ] Types & validators
- [ ] Hooks
- [ ] Components (Table, Form, Dialogs)
- [ ] Pages (List, Detail, Create, Edit, Approve, Reject)
- [ ] Testing

### Phase 4: UC-A4 (Admin Accounts)
- [ ] Service layer
- [ ] Types & validators
- [ ] Hooks
- [ ] Components (Table, Form, Role/Permission editors)
- [ ] Pages (List, Detail, Create, Roles)
- [ ] Testing

### Phase 5: UC-A3 (Categories & Ingredients)
- [ ] Categories: Service, Types, Hooks, Components, Pages
- [ ] Ingredients: Service, Types, Hooks, Components, Pages
- [ ] Testing

### Phase 6: Final
- [ ] Integration testing
- [ ] UI/UX polish
- [ ] Performance optimization
- [ ] Documentation
- [ ] Bug fixes

---

## 📚 TÀI LIỆU THAM KHẢO

### Backend API Docs
- UC-A2: `/api/v1/admin/recipes`
- UC-A3: `/api/v1/admin/categories`, `/api/v1/admin/ingredients`
- UC-A4: `/api/v1/admin/accounts`
- UC-A5: `/api/v1/admin/analytics`

### Frontend Reference
- UC-A1 Implementation: `app/admin/(users)/` (làm mẫu)
- Backend Structure: `d:\DATN\BE\docs\BACKEND_STRUCTURE.md`
- Use Cases: `d:\DATN\DOCS\UC\UC-A2` đến `UC-A5`

### Libraries
- React Native: Expo Router
- State Management: React Query hoặc Zustand (tùy chọn)
- Charts: `react-native-chart-kit` hoặc `victory-native`
- Date handling: `date-fns`
- Forms: React Native components

---

## 🎯 KẾT LUẬN

**Tổng thời gian ước tính:** ~8 tuần (40 ngày làm việc)

**Ưu tiên:**
1. 🔴 **HIGH:** UC-A5 (Dashboard) - 6 ngày
2. 🟡 **MEDIUM:** UC-A2 (Recipes) - 9 ngày, UC-A4 (Accounts) - 5.5 ngày
3. 🟢 **LOW:** UC-A3 (Categories & Ingredients) - 5 ngày
4. **Final:** Integration & Polish - 5 ngày

**Yêu cầu:**
- Tuân thủ SOLID principles
- Consistent với UC-A1 pattern
- Type-safe TypeScript
- Reusable components
- Good UX/UI

**Lưu ý:**
- Bắt đầu với shared components để tái sử dụng
- Test từng module trước khi chuyển sang module tiếp theo
- Code review sau mỗi phase
- Document các components phức tạp




