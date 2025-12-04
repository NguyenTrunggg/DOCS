# Tracklist · UCA03-7 “Sửa thông tin nguyên liệu”

> **Scope**: Admin updates ingredient properties (name, default unit, category, aliases, active state). Enforces uniqueness, maintains normalized names, handles alias serialization, optional addition of previous name to alias list.

> **Architecture**: Admin ingredients module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: admin opens ingredient detail → edit form prefilled → change fields → submit → backend validates uniqueness, updates record, returns updated data.
- **Alternative**: when name changes, add old name to alias automatically; reorder or change category; toggle active state.
- **Exception**: duplicate name, invalid input, DB failure.
- **Rules/NFR**:
  - Name unique (normalized).
  - Update <1s.

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C7-BE-DTO-01 | `UpdateIngredientRequestDto` | `dto/request/update-ingredient.dto.ts` | ✅ |
| C7-BE-VLD-01 | `updateIngredientValidator` | `validators/admin-ingredients.validator.ts` | ✅ |
| C7-BE-SVC-01 | `AdminIngredientsService.updateIngredient` (name normalization, alias handling) | `services/admin-ingredients.service.ts` | ✅ |
| C7-BE-REP-01 | Repo helper `findByNormalizedName`, `update` | `repositories/admin-ingredients.repository.ts` | ✅ |
| C7-BE-CTL-01 | Controller route `PUT /admin/ingredients/:id` | `controllers/admin-ingredients.controller.ts` + `routes.ts` | ✅ |
| C7-BE-CC-01 | Audit log `INGREDIENT_UPDATE` | admin audit service | ⬜ |
| C7-BE-OPT-01 | Auto-add old name to aliases when name changes | service | ⬜ |

Testing:
| Task | Status |
|------|--------|
| Unit tests for update service (duplicate, alias serialization) | ⬜ |
| Integration tests for endpoint | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C7-FE-TYP-01 | `UpdateIngredientPayload` type | `fe-web/src/types/admin-ingredients.types.ts` | ⬜ |
| C7-FE-VLD-01 | Client validator | `fe-web/src/validators/admin-ingredients.validator.ts` | ⬜ |
| C7-FE-SVC-01 | `adminIngredientsApi.update(id, payload)` | `fe-web/src/services/admin-ingredients.service.ts` | ⬜ |
| C7-FE-HOOK-01 | `useEditIngredientController` | `fe-web/src/hooks/admin-ingredients/useEditIngredientController.ts` | ⬜ |
| C7-FE-CMP-01 | `IngredientEditForm` (prefilled, alias chips) | `fe-web/src/components/admin/ingredients/IngredientEditForm.tsx` | ⬜ |
| C7-FE-CMP-02 | Inline edit options | `IngredientsTable` | ⬜ |
| C7-FE-TST-01 | Component/hook tests | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit logging, copy deck, analytics | ⬜ | Similar to creation flow |
| Optional alias auto-add config | ⬜ |

---

## 5. QA Checklist
- [ ] Duplicate name check works.
- [ ] Aliases persisted correctly.
- [ ] Category/unit updates reflected in list.
- [ ] FE validation + success toast.
- [ ] Audit log recorded.

---

## 6. Assumptions
1. Changing name does not rename references; only metadata.
2. Alias list stored as JSON string.
3. Auto-add old name to alias optional feature.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


