# Tracklist · Frontend UC-A1 → UC-A5

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

> Kiến trúc UI tuân thủ chuỗi: **Validator → Service Gateway → Hook/Controller → Presentational Component → Page/Route**, kèm permission guard, saved filters và thông báo trạng thái nhất quán.

---

## UC-A1 · Quản lý người dùng

### UCA01-1 · Xem danh sách người dùng
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A101-FE-VLD-01 | Bộ validator bộ lọc (search, status, role, date) | `fe-web/src/validators/admin-users/list.validator.ts` | ⬜ |
| A101-FE-SVC-01 | `adminUsersApi.list(query)` & `export(query)` | `fe-web/src/services/admin-users.service.ts` | ⬜ |
| A101-FE-HOOK-01 | `useAdminUsersTable` (pagination, saved filters, export) | `fe-web/src/hooks/admin-users/useAdminUsersTable.ts` | ⬜ |
| A101-FE-CMP-01 | Toolbar (filters, export button, saved filter dropdown) | `fe-web/src/components/admin/users/AdminUsersToolbar.tsx` | ⬜ |
| A101-FE-CMP-02 | Table + status/role chips + actions | `fe-web/src/components/admin/users/AdminUsersTable.tsx` | ⬜ |
| A101-FE-VIEW-01 | Page `/admin/users` integration + permission guard | `fe-web/app/admin/(users)/page.tsx` | ⬜ |
| A101-FE-STATE-01 | Saved filters storage adaptor reuse (localStorage + API) | `fe-web/src/store/admin-saved-filters.store.ts` | ⬜ |
| A101-FE-TST-01 | Tests (hook + table) | `__tests__/admin-users/list.test.tsx` | ⬜ |

### UCA01-2 · Xem chi tiết người dùng
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A102-FE-SVC-01 | `adminUsersApi.getDetail(id)` + `exportDetail(id, params)` | `admin-users.service.ts` | ⬜ |
| A102-FE-HOOK-01 | `useAdminUserDetail(id)` | `hooks/admin-users/useAdminUserDetail.ts` | ⬜ |
| A102-FE-CMP-01 | Profile overview cards | `components/admin/users/UserProfileCard.tsx` | ⬜ |
| A102-FE-CMP-02 | Activity tabs (recipes, comments, moderation log) | `components/admin/users/UserDetailTabs.tsx` | ⬜ |
| A102-FE-CMP-03 | Export drawer (format, includeLogs) | `components/admin/users/UserExportDrawer.tsx` | ⬜ |
| A102-FE-VIEW-01 | Route `/admin/users/[id]` | `app/admin/(users)/[id]/page.tsx` | ⬜ |
| A102-FE-TST-01 | Snapshot + hook test | `__tests__/admin-users/detail.test.tsx` | ⬜ |

### UCA01-3 · Vô hiệu hóa tài khoản
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A103-FE-VLD-01 | Disable form validator (reason, preset/custom duration, sendEmail) | `validators/admin-users/disable.validator.ts` | ⬜ |
| A103-FE-SVC-01 | API `adminUsersApi.disable(id, payload)` | `admin-users.service.ts` | ⬜ |
| A103-FE-HOOK-01 | `useDisableUser` mutation | `hooks/admin-users/useDisableUser.ts` | ⬜ |
| A103-FE-CMP-01 | Modal/form UI (severity, notes) | `components/admin/users/DisableUserModal.tsx` | ⬜ |
| A103-FE-TST-01 | Validator + modal tests | `__tests__/admin-users/disable.test.tsx` | ⬜ |

### UCA01-4 · Kích hoạt lại tài khoản
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A104-FE-VLD-01 | Enable form validator (note, sendEmail) | `validators/admin-users/enable.validator.ts` | ⬜ |
| A104-FE-SVC-01 | API `adminUsersApi.enable(id, payload)` | `admin-users.service.ts` | ⬜ |
| A104-FE-HOOK-01 | `useEnableUser` mutation | `hooks/admin-users/useEnableUser.ts` | ⬜ |
| A104-FE-CMP-01 | Confirmation modal | `components/admin/users/EnableUserModal.tsx` | ⬜ |
| A104-FE-TST-01 | Tests | `__tests__/admin-users/enable.test.tsx` | ⬜ |

---

## UC-A2 · Quản lý công thức hệ thống

### UCA02-1 · Xem danh sách công thức hệ thống
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A201-FE-VLD-01 | List filter validator (status, creator, range) | `validators/admin-recipes/list.validator.ts` | ⬜ |
| A201-FE-SVC-01 | `adminRecipesApi.list`, `export` | `services/admin-recipes.service.ts` | ⬜ |
| A201-FE-HOOK-01 | `useAdminRecipesTable` | `hooks/admin-recipes/useAdminRecipesTable.ts` | ⬜ |
| A201-FE-CMP-01 | Table + status badges + actions | `components/admin/recipes/AdminRecipesTable.tsx` | ⬜ |
| A201-FE-CMP-02 | Toolbar (filters, export, saved filters) | `components/admin/recipes/AdminRecipesToolbar.tsx` | ⬜ |
| A201-FE-VIEW-01 | `/admin/recipes` page | `app/admin/(recipes)/page.tsx` | ⬜ |

### UCA02-2 · Thêm công thức hệ thống
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A202-FE-VLD-01 | Create recipe form schema (basics, ingredients, steps) | `validators/admin-recipes/create.validator.ts` | ⬜ |
| A202-FE-SVC-01 | API `adminRecipesApi.create(payload)` | `admin-recipes.service.ts` | ⬜ |
| A202-FE-HOOK-01 | `useCreateSystemRecipe` | `hooks/admin-recipes/useCreateSystemRecipe.ts` | ⬜ |
| A202-FE-CMP-01 | Multi-step form (info, ingredients, steps, preview) | `components/admin/recipes/RecipeForm.tsx` | ⬜ |
| A202-FE-CMP-02 | Ingredient picker + inline add | `components/admin/recipes/RecipeIngredientsField.tsx` | ⬜ |
| A202-FE-VIEW-01 | `/admin/recipes/new` route | `app/admin/(recipes)/new/page.tsx` | ⬜ |
| A202-FE-TST-01 | Form validation tests | `__tests__/admin-recipes/create.test.tsx` | ⬜ |

### UCA02-3 · Sửa công thức hệ thống
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A203-FE-SVC-01 | API `adminRecipesApi.update(id, payload)` | `admin-recipes.service.ts` | ⬜ |
| A203-FE-HOOK-01 | `useEditSystemRecipe` w/ optimistic update | `hooks/admin-recipes/useEditSystemRecipe.ts` | ⬜ |
| A203-FE-CMP-01 | Reuse RecipeForm with initial data | `components/admin/recipes/RecipeForm.tsx` | ⬜ |
| A203-FE-VIEW-01 | `/admin/recipes/[id]/edit` | `app/admin/(recipes)/[id]/edit/page.tsx` | ⬜ |

### UCA02-4 · Xóa công thức hệ thống
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A204-FE-SVC-01 | API `adminRecipesApi.delete(id)` | `admin-recipes.service.ts` | ⬜ |
| A204-FE-HOOK-01 | `useDeleteSystemRecipe` | `hooks/admin-recipes/useDeleteSystemRecipe.ts` | ⬜ |
| A204-FE-CMP-01 | Confirm modal + optional soft-delete toggle | `components/admin/recipes/DeleteRecipeModal.tsx` | ⬜ |

### UCA02-5 · Xem danh sách công thức chờ duyệt
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A205-FE-SVC-01 | `adminRecipesApi.listPending(query)` | `admin-recipes.service.ts` | ⬜ |
| A205-FE-HOOK-01 | `usePendingRecipesTable` | `hooks/admin-recipes/usePendingRecipesTable.ts` | ⬜ |
| A205-FE-CMP-01 | Pending table with moderation actions | `components/admin/recipes/PendingRecipesTable.tsx` | ⬜ |
| A205-FE-VIEW-01 | `/admin/recipes/pending` page | `app/admin/(recipes)/pending/page.tsx` | ⬜ |

### UCA02-6 · Phê duyệt công thức
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A206-FE-VLD-01 | Approve form validator (note) | `validators/admin-recipes/approve.validator.ts` | ⬜ |
| A206-FE-SVC-01 | API `adminRecipesApi.approve(id, payload)` | `admin-recipes.service.ts` | ⬜ |
| A206-FE-HOOK-01 | `useApproveRecipe` | `hooks/admin-recipes/useApproveRecipe.ts` | ⬜ |
| A206-FE-CMP-01 | Approve drawer/modal | `components/admin/recipes/ApproveRecipeModal.tsx` | ⬜ |

### UCA02-7 · Từ chối công thức
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A207-FE-VLD-01 | Reject form validator (reason, note, visibility) | `validators/admin-recipes/reject.validator.ts` | ⬜ |
| A207-FE-SVC-01 | API `adminRecipesApi.reject(id, payload)` | `admin-recipes.service.ts` | ⬜ |
| A207-FE-HOOK-01 | `useRejectRecipe` | `hooks/admin-recipes/useRejectRecipe.ts` | ⬜ |
| A207-FE-CMP-01 | Reject modal | `components/admin/recipes/RejectRecipeModal.tsx` | ⬜ |

---

## UC-A3 · Danh mục & Nguyên liệu

### UCA03-1 · Xem danh sách danh mục
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A301-FE-VLD-01 | Category list filter validator | `validators/admin-categories/list.validator.ts` | ⬜ |
| A301-FE-SVC-01 | `adminCategoriesApi.list(query)` | `services/admin-categories.service.ts` | ⬜ |
| A301-FE-HOOK-01 | `useAdminCategoriesTable` | `hooks/admin-categories/useAdminCategoriesTable.ts` | ⬜ |
| A301-FE-CMP-01 | Category table + search + pagination | `components/admin/categories/AdminCategoriesTable.tsx` | ⬜ |
| A301-FE-VIEW-01 | `/admin/categories` page | `app/admin/(categories)/page.tsx` | ⬜ |

### UCA03-2 · Thêm danh mục mới
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A302-FE-VLD-01 | Create category validator (name/slug/icon) | `validators/admin-categories/create.validator.ts` | ⬜ |
| A302-FE-SVC-01 | API `adminCategoriesApi.create(payload)` | `admin-categories.service.ts` | ⬜ |
| A302-FE-HOOK-01 | `useCreateCategory` | `hooks/admin-categories/useCreateCategory.ts` | ⬜ |
| A302-FE-CMP-01 | Modal/form | `components/admin/categories/CategoryForm.tsx` | ⬜ |

### UCA03-3 · Sửa tên danh mục
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A303-FE-SVC-01 | API `adminCategoriesApi.update(id, payload)` | `admin-categories.service.ts` | ⬜ |
| A303-FE-HOOK-01 | `useUpdateCategory` | `hooks/admin-categories/useUpdateCategory.ts` | ⬜ |
| A303-FE-CMP-01 | Inline edit / modal reuse | `components/admin/categories/CategoryForm.tsx` | ⬜ |

### UCA03-4 · Xóa danh mục
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A304-FE-SVC-01 | API `adminCategoriesApi.delete(id)` | `admin-categories.service.ts` | ⬜ |
| A304-FE-HOOK-01 | `useDeleteCategory` | `hooks/admin-categories/useDeleteCategory.ts` | ⬜ |
| A304-FE-CMP-01 | Confirm dialog (transfer child recipes?) | `components/admin/categories/DeleteCategoryModal.tsx` | ⬜ |

### UCA03-5 · Xem danh sách nguyên liệu
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A305-FE-VLD-01 | Ingredient list validator (search, unit) | `validators/admin-ingredients/list.validator.ts` | ⬜ |
| A305-FE-SVC-01 | `adminIngredientsApi.list(query)` | `services/admin-ingredients.service.ts` | ⬜ |
| A305-FE-HOOK-01 | `useAdminIngredientsTable` | `hooks/admin-ingredients/useAdminIngredientsTable.ts` | ⬜ |
| A305-FE-CMP-01 | Table + nutritional tags | `components/admin/ingredients/AdminIngredientsTable.tsx` | ⬜ |
| A305-FE-VIEW-01 | `/admin/ingredients` page | `app/admin/(ingredients)/page.tsx` | ⬜ |

### UCA03-6 · Thêm nguyên liệu mới
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A306-FE-VLD-01 | Create ingredient validator (name, unit, nutrition) | `validators/admin-ingredients/create.validator.ts` | ⬜ |
| A306-FE-SVC-01 | API `adminIngredientsApi.create(payload)` | `admin-ingredients.service.ts` | ⬜ |
| A306-FE-HOOK-01 | `useCreateIngredient` | `hooks/admin-ingredients/useCreateIngredient.ts` | ⬜ |
| A306-FE-CMP-01 | Form component | `components/admin/ingredients/IngredientForm.tsx` | ⬜ |

### UCA03-7 · Sửa thông tin nguyên liệu
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A307-FE-SVC-01 | API `adminIngredientsApi.update(id, payload)` | `admin-ingredients.service.ts` | ⬜ |
| A307-FE-HOOK-01 | `useUpdateIngredient` | `hooks/admin-ingredients/useUpdateIngredient.ts` | ⬜ |
| A307-FE-CMP-01 | Form reuse with edit state | `components/admin/ingredients/IngredientForm.tsx` | ⬜ |

### UCA03-8 · Xóa nguyên liệu
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A308-FE-SVC-01 | API `adminIngredientsApi.delete(id)` | `admin-ingredients.service.ts` | ⬜ |
| A308-FE-HOOK-01 | `useDeleteIngredient` | `hooks/admin-ingredients/useDeleteIngredient.ts` | ⬜ |
| A308-FE-CMP-01 | Confirm modal + transfer option | `components/admin/ingredients/DeleteIngredientModal.tsx` | ⬜ |

---

## UC-A4 · Quản lý tài khoản Admin

### UCA04-1 · Xem danh sách tài khoản Admin
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A401-FE-VLD-01 | Admin account list filters | `validators/admin-accounts/list.validator.ts` | ⬜ |
| A401-FE-SVC-01 | `adminAccountsApi.list`, `export` | `services/admin-accounts.service.ts` | ⬜ |
| A401-FE-HOOK-01 | `useAdminAccountsTable` | `hooks/admin-accounts/useAdminAccountsTable.ts` | ⬜ |
| A401-FE-CMP-01 | Toolbar & table | `components/admin/accounts/AdminAccountsTable.tsx` | ⬜ |
| A401-FE-VIEW-01 | `/admin/accounts` page | `app/admin/(accounts)/page.tsx` | ⬜ |

### UCA04-2 · Tạo tài khoản Admin mới
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A402-FE-VLD-01 | Create admin validator (fullName, email, role) | `validators/admin-accounts/create.validator.ts` | ⬜ |
| A402-FE-SVC-01 | `adminAccountsApi.create(payload)` | `admin-accounts.service.ts` | ⬜ |
| A402-FE-HOOK-01 | `useCreateAdminAccount` | `hooks/admin-accounts/useCreateAdminAccount.ts` | ⬜ |
| A402-FE-CMP-01 | Form/modal + activation email status banner | `components/admin/accounts/AdminAccountForm.tsx` | ⬜ |
| A402-FE-VIEW-01 | `/admin/accounts/new` | `app/admin/(accounts)/new/page.tsx` | ⬜ |

### UCA04-3 · Phân quyền tài khoản Admin
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A403-FE-VLD-01 | Role assignment validator (roleIds/permissionIds, expiresAt) | `validators/admin-accounts/roles.validator.ts` | ⬜ |
| A403-FE-SVC-01 | API `adminAccountsApi.updateRoles(id, payload)` | `admin-accounts.service.ts` | ⬜ |
| A403-FE-HOOK-01 | `useAdminRoleAssignment` | `hooks/admin-accounts/useAdminRoleAssignment.ts` | ⬜ |
| A403-FE-CMP-01 | Drawer UI (role list, permission chips, expiry picker) | `components/admin/accounts/AdminRoleDrawer.tsx` | ⬜ |
| A403-FE-CMP-02 | Effective permission preview component | `components/admin/accounts/EffectivePermissionPanel.tsx` | ⬜ |

### UCA04-4 · Xóa tài khoản Admin
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A404-FE-SVC-01 | API `adminAccountsApi.delete(id)` | `admin-accounts.service.ts` | ⬜ |
| A404-FE-HOOK-01 | `useDeleteAdminAccount` | `hooks/admin-accounts/useDeleteAdminAccount.ts` | ⬜ |
| A404-FE-CMP-01 | Confirm modal checking last Super Admin | `components/admin/accounts/DeleteAdminModal.tsx` | ⬜ |

---

## UC-A5 · Dashboard & Thống kê

### UCA05-1 · Xem báo cáo, thống kê
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A501-FE-TYP-01 | Dashboard type definitions | `types/analytics-dashboard.types.ts` | ⬜ |
| A501-FE-VLD-01 | Filter validator (date range, category, role, status) | `validators/analytics-dashboard.validator.ts` | ⬜ |
| A501-FE-SVC-01 | `analyticsApi.getDashboard`, `getDrillDown`, `export` | `services/analytics-dashboard.service.ts` | ⬜ |
| A501-FE-HOOK-01 | `useDashboardData` (filters, caching, error states) | `hooks/analytics/useDashboardData.ts` | ⬜ |
| A501-FE-CMP-01 | Dashboard layout container | `components/analytics/DashboardLayout.tsx` | ⬜ |
| A501-FE-CMP-02 | KPI cards | `components/analytics/KpiCards.tsx` | ⬜ |
| A501-FE-CMP-03 | Chart components (line/bar/pie/heatmap w/ lazy load) | `components/analytics/charts/*` | ⬜ |
| A501-FE-CMP-04 | Filter toolbar + saved filters + quick ranges | `components/analytics/DashboardFilters.tsx` | ⬜ |
| A501-FE-CMP-05 | Drill-down modal/table (metric aware) | `components/analytics/DrillDownModal.tsx` | ⬜ |
| A501-FE-CMP-06 | Export dropdown + success toast w/ download link | `components/analytics/DashboardExportMenu.tsx` | ⬜ |
| A501-FE-CMP-07 | Empty/error/loading states | `components/analytics/DashboardState.tsx` | ⬜ |
| A501-FE-VIEW-01 | `/admin/dashboard` page wiring + permission guard | `app/admin/dashboard/page.tsx` | ⬜ |
| A501-FE-TST-01 | Hook + critical component tests | `__tests__/analytics/dashboard.test.tsx` | ⬜ |

---

## Cross-cutting FE Tasks
| ID | Task | Scope | Status | Notes |
|----|------|-------|--------|-------|
| AC-FE-AUTH-01 | RBAC guard HOC for admin routes | All admin pages | ⬜ |
| AC-FE-LAYOUT-01 | Admin shell layout (breadcrumbs, tabs) | Global | ⬜ |
| AC-FE-STATE-01 | Query caching strategy (React Query config) | Global | ⬜ |
| AC-FE-NOTIFY-01 | Toast + inline alert patterns | Global | ⬜ |
| AC-FE-INTL-01 | Copy deck + i18n keys for admin module | Global | ⬜ |
| AC-FE-TEST-01 | E2E smoke tests covering CRUD flows (Playwright) | Critical UCs | ⬜ |
| AC-FE-ACCESS-01 | Accessibility review for tables/forms | Global | ⬜ |

---

## QA Notes
- Confirm UI validators mirror Joi schemas.
- Ensure loading/empty/error states implemented for every data grid.
- Track saved filter synchronization between local cache và API (nếu áp dụng).
- Export flows phải hiển thị trạng thái xử lý và link tải.
- Permission guard required trên mọi route admin theo PERMISSIONS map.

---

## Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial consolidated FE tracklist |


