# Tracklist · UCA03-1 “Xem danh sách danh mục”

> **Scope**: Admin category list view (read-only) with search, sort, pagination, optional export and saved filters. Tied to Category CRUD module.

> **Architecture**: Use existing admin categories module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: load categories, display ID, name, description, recipe count, created/updated timestamps; search by name; sort by name/date; paginate (20/page).
- **Alternative**: export CSV, saved filter set.
- **Exception**: empty list, errors, permission issues.
- **Rules/NFR**:
  - Permission `Category.Read`.
  - Unique name enforced.
  - Perf <2s.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-BE-DTO-01 | Query DTO for categories list (search, sort, paging) | `src/modules/admin/categories/dto/request/get-categories.dto.ts` | ✅ | `GetAdminCategoriesQueryDto` already defined |
| C1-BE-DTO-02 | Response DTO for list items (id, name, description, counts) | `src/modules/admin/categories/dto/response/category-response.dto.ts` | ✅ | `CategoryListItemDto` available |
| C1-BE-VLD-01 | Validator for query | `src/modules/admin/categories/validators/admin-categories.validator.ts` | ✅ | `getAdminCategoriesQueryValidator` handles filters |

### 2.2 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-BE-REP-01 | `AdminCategoriesRepository.findAll(filters)` | `src/modules/admin/categories/repositories/admin-categories.repository.ts` | ✅ | Already supports search/sort + recipe counts helper |
| C1-BE-REP-02 | `count(filters)` | same | ✅ |
| C1-BE-REP-03 | Export helper (limit 5000) | same | ⬜ |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-BE-SVC-01 | `AdminCategoriesService.getCategories(query)` returning data + total | `src/modules/admin/categories/services/admin-categories.service.ts` | ✅ | Already implemented |
| C1-BE-SVC-02 | `exportCategories(query)` | same | ⬜ | CSV builder |
| C1-BE-SVC-03 | Saved filters service (optional) | same | ⬜ |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-BE-CTL-01 | Controller `getCategories` uses pagination utils | `src/modules/admin/categories/controllers/admin-categories.controller.ts` | ✅ |
| C1-BE-CTL-02 | Export endpoint | same | ⬜ |
| C1-BE-RT-01 | Routes `GET /admin/categories`, `/export` with permissions | `src/modules/admin/categories/routes.ts` | ✅ | List route with `Category.Read` already defined |

### 2.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-BE-TST-01 | Unit tests for service filters | `src/modules/admin/categories/services/__tests__/admin-categories.service.spec.ts` | ⬜ |
| C1-BE-TST-02 | Integration tests for list/export endpoints | `src/tests/integration/admin/categories.list.spec.ts` | ⬜ |

---

## 3. Frontend Tasks

### 3.1 Types, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-FE-TYP-01 | Category list item & query types | `fe-web/src/types/admin-categories.types.ts` | ⬜ |
| C1-FE-SVC-01 | `adminCategoriesApi.list(query)` & `.export(query)` | `fe-web/src/services/admin-categories.service.ts` | ⬜ |

### 3.2 Hooks
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-FE-HOOK-01 | `useAdminCategoriesList` (search, pagination) | `fe-web/src/hooks/admin-categories/useAdminCategoriesList.ts` | ⬜ |
| C1-FE-HOOK-02 | `useCategoriesExport` | same | ⬜ |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-FE-CMP-01 | `CategoriesToolbar` (search, sort, export) | `fe-web/src/components/admin/categories/CategoriesToolbar.tsx` | ⬜ |
| C1-FE-CMP-02 | `CategoriesTable` (columns per UC) | same | ⬜ |
| C1-FE-CMP-03 | `CategoriesPagination` | same | ⬜ |
| C1-FE-CMP-04 | Empty/Error states & skeletons | same | ⬜ |

### 3.4 Screen
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-FE-VIEW-01 | Categories list page | `fe-web/app/admin/(catalog)/categories/page.tsx` (or similar) | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-FE-TST-01 | Hook/component tests | `fe-web/src/hooks/admin-categories/__tests__/...` | ⬜ |
| C1-FE-TST-02 | E2E: list filters + export | `fe-web/tests/admin/categories-list.spec.ts` | ⬜ |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C1-CC-01 | Audit log for category list access/export | backend audit service | ⬜ |
| C1-CC-02 | Copy/text localization | `DOCS/UI/content/admin-categories.md` | ⬜ |
| C1-CC-03 | Analytics event | FE analytics | ⬜ |

---

## 5. QA Checklist
- [ ] API filters & pagination behave correctly.
- [ ] Export respects limit.
- [ ] FE handles empty/error states gracefully.
- [ ] Permissions enforced (`Category.Read` required).
- [ ] Audit logs recorded.

---

## 6. Assumptions
1. Recipe count per category fetched via aggregate column.
2. Export limited to 5000 rows.
3. Saved filters optional; future story.
4. Search uses normalized name.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist drafted |


