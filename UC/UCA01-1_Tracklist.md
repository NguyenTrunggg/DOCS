# Tracklist · UCA01-1 “Xem danh sách người dùng”

> **Scope**: Admin user list page (UC-A1-1) including search, filter, sort, pagination, export, saved filters, empty/error/session timeout handling.  
> **Standards**: Follow Layered Architecture (`DTO → Validator → Repository → Service → Controller → Routes`) and UI pipeline (`Validator → Service gateway → Hook → Component → View`) per `UI-Auth-Accounts-Build-Guide.md` & `API_DEVELOPMENT_GUIDE.md`.  
> **Permissions**: Require `User.Read`. Audit every access. Server-side paging (20 items/page), export max 1000 rows, diacritic-insensitive search.

---

## 1. UC Breakdown
- **Basic**: fetch paginated list, display table columns (ID, avatar, name, email, joinedAt, status), toolbar search/filter/sort, pagination.
- **Alternative**:
  - Export CSV/Excel using current filters.
  - Save & reapply frequently used filters.
- **Exception**:
  - Empty state message.
  - Data load error fallback with retry.
  - Session timeout → redirect login + toast.
- **NFR/Rules**:
  - Response <2s for 10k users (server pagination, DB indexes).
  - Search insensitive (lowercase + remove accents).
  - Audit log per request, guard by RBAC middleware.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 2. Backend Tasks

### 2.1 DTO & Types
| ID | Task | File/Module | Status | Notes |
|----|------|-------------|--------|-------|
| A1-BE-DTO-01 | Define `GetAdminUsersQueryDto` | `src/modules/admin/users/dto/request/get-admin-users.dto.ts` | ⬜ | `search?, status?, role?, startDate?, endDate?, sortBy?, sortOrder?, page?, limit?` |
| A1-BE-DTO-02 | Define `SavedFilterDto` & `SaveFilterRequestDto` | `src/modules/admin/users/dto/request/save-filter.dto.ts` | ⬜ | For alternative “Lưu bộ lọc” |
| A1-BE-DTO-03 | Define `AdminUserListItemDto` | `src/modules/admin/users/dto/response/admin-user-list-item.dto.ts` | ⬜ | Include avatar url, badges, normalized status |
| A1-BE-DTO-04 | Define `AdminUserListResponseDto` (data + meta) | `src/modules/admin/users/dto/response/admin-user-list-response.dto.ts` | ⬜ | Contains `items: AdminUserListItemDto[]`, `meta` (page, limit, total, totalPages) |

### 2.2 Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-BE-VLD-01 | Joi schema `listAdminUsersQueryValidator` | `src/modules/admin/users/validators/admin-users.validator.ts` | ⬜ | Validate query; default `page=1`, `limit<=50`, enforce ISO dates |
| A1-BE-VLD-02 | Joi schema `saveFilterValidator` | same file | ⬜ | `name`, `payload` (object) with allowed keys; used by POST filters |
| A1-BE-VLD-03 | Joi schema `exportUsersQueryValidator` | same file | ✅ | Added `exportAdminUsersQueryValidator` to reuse base query with `limit<=1000` |

### 2.3 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-BE-REP-01 | Extend `AdminUsersRepository` with `buildListWhere(filters)` | `src/modules/admin/users/repositories/admin-users.repository.ts` | ⬜ | Normalize search using `normalizeText()` & indexes |
| A1-BE-REP-02 | Implement `findManyWithPagination(filters)` | same | ⬜ | Accept skip/take, sort mapping, include status + joinedAt |
| A1-BE-REP-03 | Implement `countUsers(filters)` | same | ⬜ | For meta total |
| A1-BE-REP-04 | Implement `exportUsers(filters, limit)` | same | ⬜ | Return raw rows for CSV, enforce hard cap 1000 |
| A1-BE-REP-05 | Implement saved filter CRUD (`listFilters`, `saveFilter`, `deleteFilter`) | same or dedicated `admin-users.filters.repository.ts` | ⬜ | Backed by `admin_saved_filters` table |
| A1-BE-REP-06 | Add `logUserListAccess(adminId, payload)` | same or `admin-audit.repository.ts` | ⬜ | For audit trail |

### 2.4 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-BE-SVC-01 | `getUsers(query: GetAdminUsersQueryDto)` | `src/modules/admin/users/services/admin-users.service.ts` | ⬜ | Compose filters, call repo, map to DTO, return meta |
| A1-BE-SVC-02 | `exportUsers(query)` | same | ⬜ | Validate limit, call repo export, format CSV buffer |
| A1-BE-SVC-03 | `saveFilter(adminId, dto)` | same | ⬜ | Upsert by name, limit 20 filters/admin |
| A1-BE-SVC-04 | `listFilters(adminId)` | same | ⬜ | Return saved presets |
| A1-BE-SVC-05 | `deleteFilter(adminId, filterId)` | same | ⬜ | Guard ownership |
| A1-BE-SVC-06 | Invoke `AdminAuditService.logAction` inside above | `src/modules/admin/audit/services/admin-audit.service.ts` | ⬜ | Action codes: `USER_LIST_VIEW`, `USER_EXPORT`, `USER_FILTER_SAVE` |

### 2.5 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-BE-CTL-01 | `AdminUsersController.getUsers` | `src/modules/admin/users/controllers/admin-users.controller.ts` | ⬜ | Use `asyncHandler`, `paginatedResponse`, call service |
| A1-BE-CTL-02 | `AdminUsersController.exportUsers` | same | ⬜ | Stream CSV, set headers |
| A1-BE-CTL-03 | `AdminUsersController.saveFilter/listFilters/deleteFilter` | same | ⬜ | Manage presets |
| A1-BE-RT-01 | Define routes in `src/modules/admin/users/routes.ts` | | ✅ | `/export` now validated; other endpoints already guarded |
| A1-BE-RT-02 | Apply middleware chain | | ⬜ | `authenticate` → `requirePermissions(UserPermissions.USER_READ)` → `validate(...)` |
| A1-BE-RT-03 | Register module in `src/server.ts` | | ⬜ | Ensure route mounted & behind version prefix |

### 2.6 Testing & Observability
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-BE-TST-01 | Unit tests for service filtering/export | `src/modules/admin/users/services/__tests__/admin-users.service.spec.ts` | ⬜ | Cover search normalization, sort, pagination |
| A1-BE-TST-02 | Integration test for `/api/v1/admin/users` | `src/tests/integration/admin/users.list.spec.ts` | ⬜ | Validate RBAC, query params, empty state |
| A1-BE-TST-03 | Integration test for export endpoint | same suite | ⬜ | Ensure CSV limit + headers |
| A1-BE-TEL-01 | Metrics/logging | `src/common/middleware/performance-monitor.ts` config | ⬜ | Add label `admin_users_list` |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-FE-TYP-01 | Mirror DTO as FE types | `fe-web/src/types/admin-users.types.ts` | ⬜ | Generated from BE schema |
| A1-FE-VLD-01 | `admin-users.validator.ts` (search/filter form) | `fe-web/src/validators/` | ⬜ | Reuse Joi constraints |
| A1-FE-SVC-01 | `adminUsersApi.list(query)` | `fe-web/src/services/admin-users.service.ts` | ⬜ | Use HttpClient, map responses |
| A1-FE-SVC-02 | `adminUsersApi.export(query)` | same | ⬜ | Handle blob download, disable button if BE disabled |
| A1-FE-SVC-03 | `adminUsersApi.saveFilter/listFilters/deleteFilter` | same | ⬜ | Manage presets |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-FE-HOOK-01 | `useAdminUsersList` | `fe-web/src/hooks/admin-users/useAdminUsersList.ts` | ⬜ | Debounce search, sync URL params, handle pagination meta |
| A1-FE-HOOK-02 | `useSavedFilters` | same dir | ⬜ | CRUD presets, optimistic updates |
| A1-FE-HOOK-03 | `useExportUsers` | same dir | ⬜ | Manage loading, error toast, progress |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-FE-CMP-01 | `UsersToolbar` | `fe-web/src/components/admin/users/UsersToolbar.tsx` | ⬜ | Search input, status filter, date range, sort, export btn, saved filter dropdown |
| A1-FE-CMP-02 | `UsersTable` | `fe-web/src/components/admin/users/UsersTable.tsx` | ⬜ | Sticky header, avatar fallback, click row → detail |
| A1-FE-CMP-03 | `UsersPagination` | `fe-web/src/components/admin/users/UsersPagination.tsx` | ⬜ | 20 items default, support limit change when BE ready |
| A1-FE-CMP-04 | `SavedFiltersPanel` | `fe-web/src/components/admin/users/SavedFiltersPanel.tsx` | ⬜ | List/apply/delete presets |
| A1-FE-CMP-05 | `ListEmptyState` | `fe-web/src/components/admin/users/UsersEmptyState.tsx` | ⬜ | Message + clear filters CTA |
| A1-FE-CMP-06 | `ListErrorState` | `fe-web/src/components/admin/users/UsersErrorState.tsx` | ⬜ | Retry button, go to login when 401 |
| A1-FE-CMP-07 | Loading skeletons | `fe-web/src/components/admin/users/UsersSkeleton.tsx` | ⬜ | Column placeholders |

### 3.4 Screens & Routing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-FE-VIEW-01 | Screen `app/admin/(users)/index.tsx` | `fe-web/app/(admin)/(users)/index.tsx` | ⬜ | Compose toolbar, table, pagination, states |
| A1-FE-VIEW-02 | Route guard integration | `fe-web/app/(admin)/layout.tsx` | ⬜ | Ensure `User.Read` permission required |
| A1-FE-VIEW-03 | Prefetch detail on row hover (optional) | `UsersTable` + router prefetch | ⬜ | Improve UX for step 6 |

### 3.5 UI Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-FE-TST-01 | Hook tests (`useAdminUsersList`) | `fe-web/src/hooks/admin-users/__tests__/useAdminUsersList.test.ts` | ⬜ | Loading, success, error, empty |
| A1-FE-TST-02 | Component tests (`UsersToolbar`, `UsersTable`) | `fe-web/src/components/admin/users/__tests__/` | ⬜ | Filter interactions, row click |
| A1-FE-TST-03 | Playwright/Cypress e2e scenario | `fe-web/tests/admin/users-list.spec.ts` | ⬜ | Search, filter, export, saved filter |

---

## 4. Cross-cutting / Infrastructure
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| A1-CC-01 | Permission constants `UserPermissions.USER_READ` | `src/config/constants.ts` & FE config | ⬜ | Shared enum for gateway guard |
| A1-CC-02 | `requirePermissions` middleware usage | `src/common/middleware/auth.ts` | ⬜ | Ensure error message localized |
| A1-CC-03 | Audit logging service update | `src/modules/admin/audit/services/` | ⬜ | Add actions & serialization for filters |
| A1-CC-04 | Saved filters DB migration verification | `prisma/migrations/**/add_admin_saved_filters` | ⬜ | Confirm columns (name, payload jsonb, ownerId) |
| A1-CC-05 | Export utility (CSV builder) | `src/common/utils/exporter.ts` (new) | ⬜ | Reusable for other modules |
| A1-CC-06 | Feature flag `USE_MOCK_ADMIN_USERS` | `fe-web/src/config/featureFlags.ts` | ⬜ | Allows MSW mocks when BE unavailable |

---

## 5. QA Checklist
- [ ] API unit/integration tests passing (filters, export limit, permissions).
- [ ] UI unit/e2e tests cover search, filter, pagination, export, saved filter CRUD.
- [ ] Performance check: DB query explain with indexes on `normalizedFullName`, `normalizedEmail`, `createdAt`.
- [ ] Security: verify unauthorized users get 403 + FE redirect; audit logs recorded.
- [ ] Accessibility: toolbar inputs labeled, table headers with `scope="col"`, pagination controls keyboard accessible.

---

## 6. Assumptions & Open Questions
1. Export format defaults to CSV; Excel (XLSX) can reuse same dataset later.
2. Saved filters limited to 20 per admin; older entries deleted FIFO when exceeding.
3. Date range filter uses user’s timezone but server stores UTC; conversion handled in FE hook.
4. When search + filters produce zero results, `UsersEmptyState` provides “Xóa bộ lọc” action to reset query.
5. Session timeout handled centrally by `authApi` interceptor; list view just shows error state & triggers redirect.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist drafted from UC spec & architecture guides |


