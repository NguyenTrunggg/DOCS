# Tracklist · UCA02-1 “Xem danh sách công thức hệ thống”

> **Scope**: Admin recipe management list view (system + approved user recipes). Covers search, filters, sorting, pagination, export, saved filters, batch navigation to edit/delete, audit logging.

> **Architecture**: Follow Layered API (`DTO → Validator → Repository → Service → Controller → Routes`) and UI separation (`Validator → Service gateway → Hook → Component → View`). Align with existing admin modules (recipes).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: load paginated recipes, show columns (ID, name, author type/name, status, createdAt, avgRating). Toolbar provides search, status filter, creator filter, sort options (name/date/rating). Pagination default 20.
- **Alternative**: export CSV/Excel based on current filters (max 5000 rows), save frequently used filter set.
- **Exception**: empty dataset message, errors with retry, session timeout redirect.
- **Rules/NFR**:
  - Permission `Recipe.Read` required.
  - Search insensitive (no accent).
  - Response < 2s for 100k recipes (server paging).
  - Audit access to list/filter usage.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-BE-DTO-01 | Define `GetAdminRecipesQueryDto` (search, status, creatorType, categoryId, sort, pagination) | `src/modules/admin/recipes/dto/request/get-recipes.dto.ts` | ✅ | Already existing (page/limit/sort) |
| R1-BE-DTO-02 | Response DTO for list items (`AdminRecipeListItemDto`) | `src/modules/admin/recipes/dto/response/admin-recipe-list-response.dto.ts` | ✅ | Reused existing DTO |
| R1-BE-DTO-03 | Query DTO for export (limit <= 5000) | `dto/request/export-recipes.dto.ts` | ✅ | Added `ExportRecipesQueryDto` with limit cap |
| R1-BE-VLD-01 | Joi validator for list query | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ✅ | Already enforced (page/limit/search) |
| R1-BE-VLD-02 | Joi validator for export query | same | ✅ | Added `exportRecipesQueryValidator` with limit<=5000 |
| R1-BE-VLD-03 | Saved filter payload validator (if reusing from UC-A1) | same or shared | ⬜ | Name + payload object |

### 2.2 Repository
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-BE-REP-01 | Implement `AdminRecipesRepository.findAll(filters)` with joins (author info) | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ⬜ | Use normalized search, filter by status/creatorType |
| R1-BE-REP-02 | `count(filters)` for pagination meta | same | ⬜ | Mirror filters |
| R1-BE-REP-03 | `export(filters, limit)` returning raw list for CSV | same | ⬜ | Hard cap 5000 |
| R1-BE-REP-04 | Optional saved filters repository (if reusing general admin filters) | `src/modules/admin/recipes/repositories/admin-recipes-filters.repository.ts` | ⬜ | CRUD saved filters |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-BE-SVC-01 | `AdminRecipesService.getRecipes(query)` returning data + total | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ✅ | Already implemented |
| R1-BE-SVC-02 | `AdminRecipesService.exportRecipes(query)` | same | ✅ | Added CSV export with escape helper, limit 5000 |
| R1-BE-SVC-03 | `AdminRecipesService.saveFilter/listFilters/deleteFilter` | same | ⬜ | Optional if feature needed |
| R1-BE-SVC-04 | Ensure `AdminAuditService` logs list view & export actions | same or `admin-audit` module | ⬜ | Metadata includes filters |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-BE-CTL-01 | Controller method `getRecipes` using pagination utils + audit logging | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ✅ | Already present (without audit) |
| R1-BE-CTL-02 | `exportRecipes` endpoint streaming CSV | same | ✅ | Added new handler |
| R1-BE-CTL-03 | Filter endpoints (save/list/delete) | same | ⬜ | Optional feature |
| R1-BE-RT-01 | Routes definition (`GET /admin/recipes`, `/export`, `/filters`) with middleware chain | `src/modules/admin/recipes/routes.ts` | ✅ | `/export` added with validator & permission |

### 2.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-BE-TST-01 | Unit tests for service filter mapping | `src/modules/admin/recipes/services/__tests__/admin-recipes.service.spec.ts` | ⬜ | Search, filter, sorting |
| R1-BE-TST-02 | Integration tests for `/admin/recipes` list | `src/tests/integration/admin/recipes.list.spec.ts` | ⬜ | 200, empty state, unauthorized |
| R1-BE-TST-03 | Integration test for `/admin/recipes/export` | same suite | ⬜ | CSV response, limit enforcement |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-FE-TYP-01 | Define `AdminRecipeListItem` & query types | `fe-web/src/types/admin-recipes.types.ts` | ⬜ | Mirror BE DTO |
| R1-FE-VLD-01 | Client validator for filters (search, status, creatorType, date range) | `fe-web/src/validators/admin-recipes.validator.ts` | ⬜ | Keep in sync with BE |
| R1-FE-SVC-01 | `adminRecipesApi.list(query)` | `fe-web/src/services/admin-recipes.service.ts` | ⬜ | GET `/admin/recipes` |
| R1-FE-SVC-02 | `adminRecipesApi.export(query)` | same | ⬜ | Handle blob download |
| R1-FE-SVC-03 | Saved filters API methods | same | ⬜ | Optional |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-FE-HOOK-01 | `useAdminRecipesList` (debounce search, manage pagination) | `fe-web/src/hooks/admin-recipes/useAdminRecipesList.ts` | ⬜ | Similar to admin users list hook |
| R1-FE-HOOK-02 | `useRecipeFilters` (persist, load saved filters) | same dir | ⬜ | Manage local/saved filters |
| R1-FE-HOOK-03 | `useRecipeExport` | same dir | ⬜ | Manage export state |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-FE-CMP-01 | `RecipesToolbar` (search, filters, export, save filter button) | `fe-web/src/components/admin/recipes/RecipesToolbar.tsx` | ⬜ | Date picker, status, creator filter |
| R1-FE-CMP-02 | `RecipesTable` (columns as per UC) | `fe-web/src/components/admin/recipes/RecipesTable.tsx` | ⬜ | Row click → detail |
| R1-FE-CMP-03 | `RecipesPagination` | same namespace | ⬜ | 20 items default |
| R1-FE-CMP-04 | `RecipesEmptyState` | same | ⬜ | Show message + reset filters |
| R1-FE-CMP-05 | `RecipesErrorState` | same | ⬜ | Retry action |
| R1-FE-CMP-06 | `SavedFiltersPanel` (optional) | same | ⬜ | Manage presets |
| R1-FE-CMP-07 | Loading skeleton for list | same | ⬜ | Placeholder rows |

### 3.4 Screens
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-FE-VIEW-01 | Admin recipe list screen | `fe-web/app/admin/(recipes)/index.tsx` | ⬜ | Compose toolbar + table + pagination |
| R1-FE-VIEW-02 | Hook up row actions (view/edit) linking to other UC screens | same | ⬜ | Route push |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-FE-TST-01 | Hook tests (`useAdminRecipesList`) | `fe-web/src/hooks/admin-recipes/__tests__/useAdminRecipesList.test.ts` | ⬜ | Loading/success/error states |
| R1-FE-TST-02 | Component tests for toolbar/table | `fe-web/src/components/admin/recipes/__tests__/` | ⬜ | Filtering interactions |
| R1-FE-TST-03 | E2E flow: search/filter/export list | `fe-web/tests/admin/recipes-list.spec.ts` | ⬜ | Validate UI + API integration |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R1-CC-01 | Audit logging for recipe list & export | `src/modules/admin/recipes/services/admin-recipes.service.ts` or `admin-audit.service.ts` | ⬜ | Include filters metadata |
| R1-CC-02 | Feature flag for mock data (`USE_MOCK_ADMIN_RECIPES`) | FE config | ⬜ | Support offline |
| R1-CC-03 | Shared CSV exporter utility (if not reusing existing) | `src/common/utils/exporter.ts` | ⬜ | Generic CSV builder |
| R1-CC-04 | Localization/copy for empty/error/export messages | `DOCS/UI/content/admin-recipes.md` | ⬜ | Keep consistent |

---

## 5. QA Checklist
- [ ] API integration tests cover pagination, filters, export.
- [ ] Performance: verify DB indexes (normalized title, createdAt).
- [ ] Security: ensure 403 for accounts lacking `Recipe.Read`.
- [ ] FE validations align with BE (limit, search length).
- [ ] Export limited to 5000 rows, includes filters in filename.
- [ ] Audit log entries visible in admin audit module.

---

## 6. Assumptions
1. Export currently CSV; Excel optional future enhancement.
2. Saved filters feature reused from UC-A1 pattern (if prioritized).
3. Sorting fields limited to `title`, `createdAt`, `averageRating`.
4. Search applies to normalized title & description.
5. Pagination metadata matches existing helper (page, limit, total, totalPages).

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist created from UC specs |


