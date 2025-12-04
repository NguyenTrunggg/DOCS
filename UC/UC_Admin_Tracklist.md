# UC Admin Tracklist - Quản trị hệ thống

> **Nguyên tắc chung**:
> - Bám kiến trúc Layered API: `DTO → Validator → Repository → Service → Controller → Routes`
> - UI separation: `Validator → Service gateway → Hook/Controller → Presentational component → App view`
> - Tuân thủ SOLID, audit/logging, phân quyền RBAC
> - Ghi assumption khi BE chưa sẵn sàng

---

## Tổng quan tiến độ

| UC Group | Mô tả | Priority | API Status | UI Status |
|----------|-------|----------|------------|-----------|
| UC-A1 | Quản trị người dùng | Medium | ⬜ Pending | ⬜ Pending |
| UC-A2 | Quản trị công thức & kiểm duyệt | Medium | ⬜ Pending | ⬜ Pending |
| UC-A3 | Danh mục & Nguyên liệu | Low | ⬜ Pending | ⬜ Pending |
| UC-A4 | Quản trị tài khoản Admin | High | ⬜ Pending | ⬜ Pending |
| UC-A5 | Dashboard & Báo cáo | High | ⬜ Pending | ⬜ Pending |

**Legend**: ⬜ Pending | 🔄 In Progress | ✅ Done | ❌ Blocked

---

## UC-A1 · Quản trị người dùng

### API Backend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A1-BE-01 | DTO Request: `GetAdminUsersQueryDto` | `src/modules/admin/users/dto/request/` | ⬜ | search, status, role, date range, sort, pagination |
| A1-BE-02 | DTO Request: `DisableUserRequestDto` | `src/modules/admin/users/dto/request/` | ⬜ | reason, duration preset, customDuration, note, severity |
| A1-BE-03 | DTO Request: `EnableUserRequestDto` | `src/modules/admin/users/dto/request/` | ⬜ | note optional |
| A1-BE-04 | DTO Request: `BatchLockRequestDto`, `BatchUnlockRequestDto` | `src/modules/admin/users/dto/request/` | ⬜ | userIds array + common fields |
| A1-BE-05 | DTO Request: `SaveFilterRequestDto` | `src/modules/admin/users/dto/request/` | ⬜ | name, payload object |
| A1-BE-06 | DTO Response: `AdminUserListResponseDto` | `src/modules/admin/users/dto/response/` | ⬜ | profile, status badge, stats summary |
| A1-BE-07 | DTO Response: `AdminUserDetailResponseDto` | `src/modules/admin/users/dto/response/` | ⬜ | full profile + stats + moderation logs |
| A1-BE-08 | Validator: `listAdminUsersQueryValidator` | `src/modules/admin/users/validators/` | ⬜ | Joi schema cho query params |
| A1-BE-09 | Validator: `disableUserValidator` | `src/modules/admin/users/validators/` | ⬜ | reason required, duration enum, conditional custom |
| A1-BE-10 | Validator: `enableUserValidator` | `src/modules/admin/users/validators/` | ⬜ | note optional |
| A1-BE-11 | Validator: `batchLockValidator`, `batchUnlockValidator` | `src/modules/admin/users/validators/` | ⬜ | userIds array validation |
| A1-BE-12 | Repository: `AdminUsersRepository` | `src/modules/admin/users/repositories/` | ⬜ | extends BaseRepository, buildWhere, normalize search |
| A1-BE-13 | Repository: `getUserActivityStats` method | `src/modules/admin/users/repositories/` | ⬜ | groupBy recipe/comment/review counts |
| A1-BE-14 | Repository: `lockUser`, `unlockUser` methods | `src/modules/admin/users/repositories/` | ⬜ | update status, lockedAt, lockExpiresAt, reasons |
| A1-BE-15 | Repository: `createModerationLog`, `getRecentModerationLogs` | `src/modules/admin/users/repositories/` | ⬜ | audit trail |
| A1-BE-16 | Service: `AdminUsersService.getUsers` | `src/modules/admin/users/services/` | ⬜ | pagination, filters, return list + total |
| A1-BE-17 | Service: `AdminUsersService.getUserById` | `src/modules/admin/users/services/` | ⬜ | detail + stats + logs |
| A1-BE-18 | Service: `AdminUsersService.disableUser` | `src/modules/admin/users/services/` | ⬜ | conflict check, calc expiry, invalidate sessions, email |
| A1-BE-19 | Service: `AdminUsersService.enableUser` | `src/modules/admin/users/services/` | ⬜ | conflict check, severity guard (Super Admin only), email |
| A1-BE-20 | Service: `AdminUsersService.batchDisableUsers` | `src/modules/admin/users/services/` | ⬜ | loop disable, return success/fail per user |
| A1-BE-21 | Service: `AdminUsersService.batchEnableUsers` | `src/modules/admin/users/services/` | ⬜ | loop enable, return success/fail per user |
| A1-BE-22 | Service: `AdminUsersService.exportUsers` | `src/modules/admin/users/services/` | ⬜ | CSV generation, max 1000 rows |
| A1-BE-23 | Service: `AdminUsersService.saveFilter`, `listFilters`, `deleteFilter` | `src/modules/admin/users/services/` | ⬜ | saved filter CRUD |
| A1-BE-24 | Service: `autoUnlockExpiredUsers` | `src/modules/admin/users/services/` | ⬜ | cron job hook |
| A1-BE-25 | Controller: `AdminUsersController` | `src/modules/admin/users/controllers/` | ⬜ | asyncHandler, paginatedResponse, successResponse |
| A1-BE-26 | Routes: `/admin/users` endpoints | `src/modules/admin/users/routes.ts` | ⬜ | GET, GET/:id, PATCH/:id/disable, PATCH/:id/enable, batch, export, filters |
| A1-BE-27 | Register routes in `server.ts` | `src/server.ts` | ⬜ | mount admin/users routes |

### UI Frontend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A1-FE-01 | Validator: `admin-users.validator.ts` | `src/validators/` | ⬜ | mirror BE Joi constraints |
| A1-FE-02 | Service: `admin-users.service.ts` | `src/services/` | ⬜ | wrap endpoints with HttpClient |
| A1-FE-03 | Hook: `useAdminUsersList` | `src/hooks/admin-users/` | ⬜ | debounce search, pagination, refresh |
| A1-FE-04 | Hook: `useAdminUserDetail` | `src/hooks/admin-users/` | ⬜ | fetch detail + stats + logs |
| A1-FE-05 | Hook: `useAdminUserActions` | `src/hooks/admin-users/` | ⬜ | disable/enable with cache invalidation |
| A1-FE-06 | Component: `UsersToolbar` | `components/admin/users/` | ⬜ | search, filters, export button |
| A1-FE-07 | Component: `UsersTable` | `components/admin/users/` | ⬜ | avatar, badge, sticky header |
| A1-FE-08 | Component: `UserProfileCard` | `components/admin/users/` | ⬜ | detail view header |
| A1-FE-09 | Component: `UserStatsGrid` | `components/admin/users/` | ⬜ | recipe/comment/review counts |
| A1-FE-10 | Component: `DisableUserDialog` | `components/admin/users/` | ⬜ | reason textarea, duration presets, email checkbox |
| A1-FE-11 | Component: `EnableUserDialog` | `components/admin/users/` | ⬜ | note input, show last lock log |
| A1-FE-12 | Component: `SavedFiltersPanel` | `components/admin/users/` | ⬜ | list/apply/delete presets |
| A1-FE-13 | View: User List | `app/admin/(users)/index.tsx` | ⬜ | combine toolbar + table + pagination |
| A1-FE-14 | View: User Detail | `app/admin/(users)/[userId]/index.tsx` | ⬜ | profile card + stats + logs + CTAs |
| A1-FE-15 | View: Disable Flow | `app/admin/(users)/[userId]/disable.tsx` | ⬜ | dialog integration |
| A1-FE-16 | View: Enable Flow | `app/admin/(users)/[userId]/enable.tsx` | ⬜ | dialog integration |

---

## UC-A2 · Quản trị công thức & kiểm duyệt

### API Backend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A2-BE-01 | DTO Request: `GetAdminRecipesQueryDto` | `src/modules/admin/recipes/dto/request/` | ⬜ | status, ownerType, category, rating sort, pagination |
| A2-BE-02 | DTO Request: `CreateSystemRecipeDto` | `src/modules/admin/recipes/dto/request/` | ⬜ | title, desc, media, category, ingredients, steps |
| A2-BE-03 | DTO Request: `UpdateRecipeDto` | `src/modules/admin/recipes/dto/request/` | ⬜ | partial update fields |
| A2-BE-04 | DTO Request: `ApproveRecipeDto` | `src/modules/admin/recipes/dto/request/` | ⬜ | optional minor edits |
| A2-BE-05 | DTO Request: `RejectRecipeDto` | `src/modules/admin/recipes/dto/request/` | ⬜ | reason required, suggestion optional |
| A2-BE-06 | DTO Response: `AdminRecipeListResponseDto` | `src/modules/admin/recipes/dto/response/` | ⬜ | list item with owner, status, rating |
| A2-BE-07 | DTO Response: `AdminRecipeDetailResponseDto` | `src/modules/admin/recipes/dto/response/` | ⬜ | full recipe + moderation history |
| A2-BE-08 | Validator: `listAdminRecipesQueryValidator` | `src/modules/admin/recipes/validators/` | ⬜ | |
| A2-BE-09 | Validator: `createSystemRecipeValidator` | `src/modules/admin/recipes/validators/` | ⬜ | title unique, >=3 ingredients/steps |
| A2-BE-10 | Validator: `updateRecipeValidator` | `src/modules/admin/recipes/validators/` | ⬜ | |
| A2-BE-11 | Validator: `approveRecipeValidator`, `rejectRecipeValidator` | `src/modules/admin/recipes/validators/` | ⬜ | |
| A2-BE-12 | Repository: `AdminRecipesRepository` | `src/modules/admin/recipes/repositories/` | ⬜ | extends BaseRepository, system/user flag filter |
| A2-BE-13 | Repository: moderation queue lock helpers | `src/modules/admin/recipes/repositories/` | ⬜ | prevent concurrent moderation |
| A2-BE-14 | Service: `AdminRecipesService.listRecipes` | `src/modules/admin/recipes/services/` | ⬜ | |
| A2-BE-15 | Service: `AdminRecipesService.getRecipe` | `src/modules/admin/recipes/services/` | ⬜ | |
| A2-BE-16 | Service: `AdminRecipesService.createSystemRecipe` | `src/modules/admin/recipes/services/` | ⬜ | auto-approved, media upload |
| A2-BE-17 | Service: `AdminRecipesService.updateRecipe` | `src/modules/admin/recipes/services/` | ⬜ | versioning snapshot |
| A2-BE-18 | Service: `AdminRecipesService.deleteRecipe` | `src/modules/admin/recipes/services/` | ⬜ | soft/hard delete toggle |
| A2-BE-19 | Service: `AdminRecipesService.listPending` | `src/modules/admin/recipes/services/` | ⬜ | moderation queue |
| A2-BE-20 | Service: `AdminRecipesService.approveRecipe` | `src/modules/admin/recipes/services/` | ⬜ | status guard, notify user |
| A2-BE-21 | Service: `AdminRecipesService.rejectRecipe` | `src/modules/admin/recipes/services/` | ⬜ | status guard, reason templates, notify user |
| A2-BE-22 | Service: `AdminRecipesService.exportRecipes` | `src/modules/admin/recipes/services/` | ⬜ | CSV, max 5000 rows |
| A2-BE-23 | Controller: `AdminRecipesController` | `src/modules/admin/recipes/controllers/` | ⬜ | |
| A2-BE-24 | Routes: `/admin/recipes` endpoints | `src/modules/admin/recipes/routes.ts` | ⬜ | CRUD + moderation endpoints |
| A2-BE-25 | Register routes in `server.ts` | `src/server.ts` | ⬜ | |

### UI Frontend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A2-FE-01 | Validator: `admin-recipes.validator.ts` | `src/validators/` | ⬜ | |
| A2-FE-02 | Service: `admin-recipes.service.ts` | `src/services/` | ⬜ | |
| A2-FE-03 | Hook: `useAdminRecipesList` | `src/hooks/admin-recipes/` | ⬜ | |
| A2-FE-04 | Hook: `useAdminRecipeDetail` | `src/hooks/admin-recipes/` | ⬜ | |
| A2-FE-05 | Hook: `useAdminRecipeForm` | `src/hooks/admin-recipes/` | ⬜ | create/edit logic |
| A2-FE-06 | Hook: `useModerationQueue` | `src/hooks/admin-recipes/` | ⬜ | pending list + actions |
| A2-FE-07 | Component: `RecipesToolbar` | `components/admin/recipes/` | ⬜ | |
| A2-FE-08 | Component: `RecipesTable` | `components/admin/recipes/` | ⬜ | |
| A2-FE-09 | Component: `RecipeForm` | `components/admin/recipes/` | ⬜ | step/ingredient builders |
| A2-FE-10 | Component: `ModerationPanel` | `components/admin/recipes/` | ⬜ | approve/reject CTAs |
| A2-FE-11 | Component: `RejectReasonDialog` | `components/admin/recipes/` | ⬜ | templates + custom |
| A2-FE-12 | View: Recipe List | `app/admin/(recipes)/index.tsx` | ⬜ | |
| A2-FE-13 | View: Create Recipe | `app/admin/(recipes)/create.tsx` | ⬜ | |
| A2-FE-14 | View: Edit Recipe | `app/admin/(recipes)/[id]/edit.tsx` | ⬜ | |
| A2-FE-15 | View: Pending Queue | `app/admin/(recipes)/pending.tsx` | ⬜ | |

---

## UC-A3 · Danh mục & Nguyên liệu

### API Backend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A3-BE-01 | DTO Request: Category CRUD | `src/modules/admin/catalog/dto/request/` | ⬜ | name, description |
| A3-BE-02 | DTO Request: Ingredient CRUD | `src/modules/admin/catalog/dto/request/` | ⬜ | name, unit, category, aliases |
| A3-BE-03 | DTO Request: `ReassignCategoryDto` | `src/modules/admin/catalog/dto/request/` | ⬜ | targetCategoryId for delete flow |
| A3-BE-04 | DTO Response: Category/Ingredient list & detail | `src/modules/admin/catalog/dto/response/` | ⬜ | include recipe counts |
| A3-BE-05 | Validator: category validators | `src/modules/admin/catalog/validators/` | ⬜ | unique name |
| A3-BE-06 | Validator: ingredient validators | `src/modules/admin/catalog/validators/` | ⬜ | normalized name, alias array |
| A3-BE-07 | Repository: `CategoryRepository` | `src/modules/admin/catalog/repositories/` | ⬜ | uniqueness, recipe count |
| A3-BE-08 | Repository: `IngredientRepository` | `src/modules/admin/catalog/repositories/` | ⬜ | alias search, reference check |
| A3-BE-09 | Service: `CategoryService` CRUD | `src/modules/admin/catalog/services/` | ⬜ | reassignment flow on delete |
| A3-BE-10 | Service: `IngredientService` CRUD | `src/modules/admin/catalog/services/` | ⬜ | substitution suggestions |
| A3-BE-11 | Service: bulk import methods | `src/modules/admin/catalog/services/` | ⬜ | CSV import |
| A3-BE-12 | Controller: `CatalogController` | `src/modules/admin/catalog/controllers/` | ⬜ | |
| A3-BE-13 | Routes: `/admin/catalog/categories`, `/admin/catalog/ingredients` | `src/modules/admin/catalog/routes.ts` | ⬜ | |
| A3-BE-14 | Register routes in `server.ts` | `src/server.ts` | ⬜ | |

### UI Frontend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A3-FE-01 | Validator: `catalog.validator.ts` | `src/validators/` | ⬜ | |
| A3-FE-02 | Service: `catalog.service.ts` | `src/services/` | ⬜ | |
| A3-FE-03 | Hook: `useCategoriesList`, `useCategoryForm` | `src/hooks/catalog/` | ⬜ | |
| A3-FE-04 | Hook: `useIngredientsList`, `useIngredientForm` | `src/hooks/catalog/` | ⬜ | |
| A3-FE-05 | Component: `CategoriesTable` | `components/admin/catalog/` | ⬜ | |
| A3-FE-06 | Component: `CategoryFormModal` | `components/admin/catalog/` | ⬜ | |
| A3-FE-07 | Component: `IngredientsTable` | `components/admin/catalog/` | ⬜ | |
| A3-FE-08 | Component: `IngredientFormModal` | `components/admin/catalog/` | ⬜ | alias chips |
| A3-FE-09 | Component: `ReassignCategoryDialog` | `components/admin/catalog/` | ⬜ | for delete with references |
| A3-FE-10 | Component: `BulkImportDrawer` | `components/admin/catalog/` | ⬜ | CSV upload |
| A3-FE-11 | View: Categories | `app/admin/(catalog)/categories.tsx` | ⬜ | |
| A3-FE-12 | View: Ingredients | `app/admin/(catalog)/ingredients.tsx` | ⬜ | |

---

## UC-A4 · Quản trị tài khoản Admin

### API Backend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A4-BE-01 | DTO Request: `GetAdminAccountsQueryDto` | `src/modules/admin/accounts/dto/request/` | ⬜ | search, role, status filter |
| A4-BE-02 | DTO Request: `CreateAdminAccountDto` | `src/modules/admin/accounts/dto/request/` | ⬜ | name, email, initial role |
| A4-BE-03 | DTO Request: `AssignRolesDto` | `src/modules/admin/accounts/dto/request/` | ⬜ | roles array, optional expiry |
| A4-BE-04 | DTO Response: Admin account list & detail | `src/modules/admin/accounts/dto/response/` | ⬜ | include effective permissions |
| A4-BE-05 | Validator: admin account validators | `src/modules/admin/accounts/validators/` | ⬜ | email unique, role enum |
| A4-BE-06 | Repository: `AdminAccountsRepository` | `src/modules/admin/accounts/repositories/` | ⬜ | CRUD + role membership |
| A4-BE-07 | Repository: Super Admin invariant check | `src/modules/admin/accounts/repositories/` | ⬜ | ensure >=1 Super Admin |
| A4-BE-08 | Service: `AdminAccountsService.listAccounts` | `src/modules/admin/accounts/services/` | ⬜ | |
| A4-BE-09 | Service: `AdminAccountsService.createAccount` | `src/modules/admin/accounts/services/` | ⬜ | generate invite token, send email |
| A4-BE-10 | Service: `AdminAccountsService.assignRoles` | `src/modules/admin/accounts/services/` | ⬜ | audit diff, optional expiry |
| A4-BE-11 | Service: `AdminAccountsService.deleteAccount` | `src/modules/admin/accounts/services/` | ⬜ | Super Admin guard |
| A4-BE-12 | Service: `AdminAccountsService.resendInvite` | `src/modules/admin/accounts/services/` | ⬜ | |
| A4-BE-13 | Controller: `AdminAccountsController` | `src/modules/admin/accounts/controllers/` | ⬜ | |
| A4-BE-14 | Routes: `/admin/accounts` endpoints | `src/modules/admin/accounts/routes.ts` | ⬜ | Super Admin only |
| A4-BE-15 | Register routes in `server.ts` | `src/server.ts` | ⬜ | |

### UI Frontend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A4-FE-01 | Validator: `admin-accounts.validator.ts` | `src/validators/` | ⬜ | |
| A4-FE-02 | Service: `admin-accounts.service.ts` | `src/services/` | ⬜ | |
| A4-FE-03 | Hook: `useAdminAccountsList` | `src/hooks/admin-accounts/` | ⬜ | |
| A4-FE-04 | Hook: `useAdminAccountForm` | `src/hooks/admin-accounts/` | ⬜ | create + role assignment |
| A4-FE-05 | Component: `AdminAccountsTable` | `components/admin/accounts/` | ⬜ | |
| A4-FE-06 | Component: `CreateAdminModal` | `components/admin/accounts/` | ⬜ | |
| A4-FE-07 | Component: `RoleAssignmentPanel` | `components/admin/accounts/` | ⬜ | matrix + effective preview |
| A4-FE-08 | Component: `DeleteAdminDialog` | `components/admin/accounts/` | ⬜ | Super Admin warning |
| A4-FE-09 | View: Admin Accounts List | `app/admin/(accounts)/index.tsx` | ⬜ | |
| A4-FE-10 | View: Admin Account Detail | `app/admin/(accounts)/[id].tsx` | ⬜ | role management + audit log |

---

## UC-A5 · Dashboard & Báo cáo

### API Backend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A5-BE-01 | DTO Request: `DashboardFiltersDto` | `src/modules/admin/reports/dto/request/` | ⬜ | date range, category, role, status |
| A5-BE-02 | DTO Request: `SaveReportTemplateDto` | `src/modules/admin/reports/dto/request/` | ⬜ | name, config object |
| A5-BE-03 | DTO Request: `ScheduleReportDto` | `src/modules/admin/reports/dto/request/` | ⬜ | cron expression, recipients |
| A5-BE-04 | DTO Response: `DashboardKPIsDto` | `src/modules/admin/reports/dto/response/` | ⬜ | totals, growth rates |
| A5-BE-05 | DTO Response: `ChartDatasetDto` | `src/modules/admin/reports/dto/response/` | ⬜ | line/bar/pie/heatmap data |
| A5-BE-06 | DTO Response: `DrillDownTableDto` | `src/modules/admin/reports/dto/response/` | ⬜ | paginated detail rows |
| A5-BE-07 | Validator: dashboard/report validators | `src/modules/admin/reports/validators/` | ⬜ | |
| A5-BE-08 | Repository/Data Layer: Aggregation queries | `src/modules/admin/reports/repositories/` | ⬜ | user/recipe/activity stats |
| A5-BE-09 | Service: `ReportsService.getDashboardKPIs` | `src/modules/admin/reports/services/` | ⬜ | |
| A5-BE-10 | Service: `ReportsService.getChartData` | `src/modules/admin/reports/services/` | ⬜ | multiple chart types |
| A5-BE-11 | Service: `ReportsService.getDrillDown` | `src/modules/admin/reports/services/` | ⬜ | |
| A5-BE-12 | Service: `ReportsService.exportReport` | `src/modules/admin/reports/services/` | ⬜ | PDF/CSV |
| A5-BE-13 | Service: `ReportsService.saveTemplate`, `listTemplates`, `deleteTemplate` | `src/modules/admin/reports/services/` | ⬜ | |
| A5-BE-14 | Service: `ReportsService.scheduleReport` | `src/modules/admin/reports/services/` | ⬜ | email job scheduling |
| A5-BE-15 | Controller: `ReportsController` | `src/modules/admin/reports/controllers/` | ⬜ | |
| A5-BE-16 | Routes: `/admin/reports` endpoints | `src/modules/admin/reports/routes.ts` | ⬜ | dashboard, export, templates, schedule |
| A5-BE-17 | Register routes in `server.ts` | `src/server.ts` | ⬜ | |

### UI Frontend

| # | Task | File/Location | Status | Notes |
|---|------|---------------|--------|-------|
| A5-FE-01 | Validator: `reports.validator.ts` | `src/validators/` | ⬜ | |
| A5-FE-02 | Service: `reports.service.ts` | `src/services/` | ⬜ | |
| A5-FE-03 | Hook: `useDashboardMetrics` | `src/hooks/reports/` | ⬜ | KPIs + charts |
| A5-FE-04 | Hook: `useReportFilters` | `src/hooks/reports/` | ⬜ | filter state management |
| A5-FE-05 | Hook: `useReportTemplates` | `src/hooks/reports/` | ⬜ | save/load presets |
| A5-FE-06 | Hook: `useReportSchedule` | `src/hooks/reports/` | ⬜ | scheduling UI |
| A5-FE-07 | Component: `KPICard` | `components/admin/reports/` | ⬜ | value + trend indicator |
| A5-FE-08 | Component: `LineChart`, `BarChart`, `PieChart`, `Heatmap` | `components/admin/reports/` | ⬜ | interactive charts |
| A5-FE-09 | Component: `ReportFilterPanel` | `components/admin/reports/` | ⬜ | date picker, dropdowns |
| A5-FE-10 | Component: `DrillDownModal` | `components/admin/reports/` | ⬜ | detail table |
| A5-FE-11 | Component: `ExportButton` | `components/admin/reports/` | ⬜ | PDF/CSV options |
| A5-FE-12 | Component: `TemplateManagerDrawer` | `components/admin/reports/` | ⬜ | save/load/delete |
| A5-FE-13 | Component: `ScheduleReportDialog` | `components/admin/reports/` | ⬜ | cron UI |
| A5-FE-14 | View: Dashboard | `app/admin/(reports)/dashboard.tsx` | ⬜ | KPIs + charts grid |

---

## Cross-cutting Tasks

| # | Task | Location | Status | Notes |
|---|------|----------|--------|-------|
| CC-01 | Middleware: `requirePermissions` | `src/common/middleware/` | ⬜ | RBAC check per endpoint |
| CC-02 | Audit logging service | `src/common/services/` | ⬜ | log admin actions |
| CC-03 | Email service integration | `src/common/services/` | ⬜ | account locked/unlocked, invite, moderation notifications |
| CC-04 | Cron job: auto unlock expired users | `src/jobs/` | ⬜ | scheduled task |
| CC-05 | Cron job: scheduled reports | `src/jobs/` | ⬜ | email reports |
| CC-06 | Admin layout & navigation | `app/admin/layout.tsx` | ⬜ | sidebar, header, guards |
| CC-07 | Error boundary & empty states | `components/admin/common/` | ⬜ | consistent UX |
| CC-08 | Loading skeletons | `components/admin/common/` | ⬜ | per view type |

---

## Notes & Assumptions

1. **BE chưa sẵn sàng**: Khi implement FE trước BE, dùng mock service (MSW) và toggle qua ENV `USE_MOCK_ADMIN_*`.
2. **Severity lock**: Tài khoản bị khóa với `severity = CRITICAL` chỉ Super Admin mới unlock được.
3. **Moderation lock**: Một công thức chỉ được 1 admin moderate tại một thời điểm để tránh conflict.
4. **Export limits**: Users max 1000 rows, Recipes max 5000 rows.
5. **Versioning**: Recipe edits tạo snapshot để có thể rollback.

---

## Changelog

| Date | Author | Changes |
|------|--------|---------|
| 2025-11-27 | AI | Initial tracklist created from UC-A1..UC-A5 |

