# Tracklist · UCA03-5 “Xem danh sách nguyên liệu”

> **Scope**: Admin ingredient list with search, category/unit filter, status filter, sorting, pagination (50/page), optional export & saved filters.

> **Architecture**: Admin ingredients module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: list ingredients showing ID, name, default unit, category, created/updated, active state; search normalized name; filter by category/unit/status; paginate 50/page.
- **Alternative**: export CSV, saved filters, suggestion search.
- **Exception**: empty list, errors, permission issues.
- **Rules/NFR**:
  - Permission `Ingredient.Read`.
  - Performance target: <2s for 10k ingredients (server paging).

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C5-BE-DTO-01 | Query DTO `GetAdminIngredientsQueryDto` | `dto/request/get-ingredients.dto.ts` | ✅ |
| C5-BE-DTO-02 | Response DTO (`IngredientListItemDto`) | `dto/response/ingredient-response.dto.ts` | ✅ |
| C5-BE-VLD-01 | Validator `getAdminIngredientsQueryValidator` | `validators/admin-ingredients.validator.ts` | ✅ |
| C5-BE-REP-01 | Repo methods `findAll`/`count` with filters | `repositories/admin-ingredients.repository.ts` | ✅ |
| C5-BE-SVC-01 | Service `getIngredients` returning data + total | `services/admin-ingredients.service.ts` | ✅ |
| C5-BE-REP-02 | Export helper (limit 5000) | repo/service | ⬜ |
| C5-BE-SVC-02 | `exportIngredients(query)` CSV builder | service | ⬜ |
| C5-BE-CTL-01 | Controller route `GET /admin/ingredients` (pagination utils) | `controllers/admin-ingredients.controller.ts` | ✅ |
| C5-BE-RT-01 | Route definition with permission | `routes.ts` | ✅ |
| C5-BE-CC-01 | Audit log for list/export | admin audit service | ⬜ |

Testing:
| Task | Status |
|------|--------|
| Unit tests for service filter mapping | ⬜ |
| Integration tests for list/export endpoints | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status |
|----|------|------|--------|
| C5-FE-TYP-01 | Ingredient list/query types | `fe-web/src/types/admin-ingredients.types.ts` | ⬜ |
| C5-FE-SVC-01 | `adminIngredientsApi.list/export` | `fe-web/src/services/admin-ingredients.service.ts` | ⬜ |
| C5-FE-HOOK-01 | `useAdminIngredientsList` (filters, pagination) | `fe-web/src/hooks/admin-ingredients/useAdminIngredientsList.ts` | ⬜ |
| C5-FE-HOOK-02 | `useIngredientsExport` | same | ⬜ |
| C5-FE-CMP-01 | `IngredientsToolbar` (search, filters, export) | `fe-web/src/components/admin/ingredients/IngredientsToolbar.tsx` | ⬜ |
| C5-FE-CMP-02 | `IngredientsTable` | same | ⬜ |
| C5-FE-CMP-03 | Pagination, empty/error state, skeleton | same | ⬜ |
| C5-FE-VIEW-01 | Ingredient list page | `fe-web/app/admin/(catalog)/ingredients/page.tsx` | ⬜ |
| C5-FE-TST-01 | Hook/component tests | ⬜ |
| C5-FE-TST-02 | E2E filters + export | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit logging integration | ⬜ |
| Copy deck for empty/error | ⬜ |
| Analytics event | ⬜ |

---

## 5. QA Checklist
- [ ] API filters/sorts behave correctly.
- [ ] Export limited to 5000 rows.
- [ ] FE handles empty/error gracefully.
- [ ] Permission `Ingredient.Read` enforced.
- [ ] Audit logs recorded.

---

## 6. Assumptions
1. Search uses normalized names + aliases.
2. Export optional; default to CSV.
3. Saved filters future feature.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


