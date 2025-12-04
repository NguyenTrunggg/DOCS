# Tracklist · UCA03-6 “Thêm nguyên liệu mới”

> **Scope**: Admin adds standardized ingredients with unique name (normalized), default unit, optional category & aliases. Includes validation, slug/normalized handling, audit, optional bulk import.

> **Architecture**: Admin ingredients module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open “Add ingredient” form → input name (normalized, unique), default unit, category, aliases → submit → backend validates, ensures unique normalized name, stores record → success toast.
- **Alternative**: bulk import from CSV; suggestions from user-submitted data.
- **Exception**: duplicate name, invalid input, DB failure.
- **Rules/NFR**:
  - Name unique (case-insensitive, diacritics removed).
  - Aliases optional; stored sanitized.
  - Performance <1s; only Admin/Super Admin.

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C6-BE-DTO-01 | `CreateIngredientRequestDto` fields | `dto/request/create-ingredient.dto.ts` | ✅ |
| C6-BE-VLD-01 | `createIngredientValidator` | `validators/admin-ingredients.validator.ts` | ✅ |
| C6-BE-SVC-01 | `AdminIngredientsService.createIngredient` (normalize, uniqueness, ensure category) | `services/admin-ingredients.service.ts` | ✅ |
| C6-BE-REP-01 | Repo helper `findByNormalizedName`, `create` | `repositories/admin-ingredients.repository.ts` | ✅ |
| C6-BE-CTL-01 | Controller route `POST /admin/ingredients` | `controllers/admin-ingredients.controller.ts` + `routes.ts` | ✅ |
| C6-BE-CC-01 | Audit log `INGREDIENT_CREATE` | admin audit service | ⬜ |
| C6-BE-OPT-01 | Bulk import endpoint | new | ⬜ |

Testing:
| Task | Status | Notes |
|------|--------|-------|
| Unit tests (duplicate name, success) | ⬜ | `services/__tests__` |
| Integration test `POST /admin/ingredients` | ⬜ | `tests/integration/admin/ingredients.create.spec.ts` |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C6-FE-TYP-01 | `CreateIngredientPayload` type | `fe-web/src/types/admin-ingredients.types.ts` | ⬜ |
| C6-FE-VLD-01 | Client validator (name min length, alias dedupe) | `fe-web/src/validators/admin-ingredients.validator.ts` | ⬜ |
| C6-FE-SVC-01 | `adminIngredientsApi.create(payload)` | `fe-web/src/services/admin-ingredients.service.ts` | ⬜ |
| C6-FE-HOOK-01 | `useCreateIngredientController` | `fe-web/src/hooks/admin-ingredients/useCreateIngredientController.ts` | ⬜ |
| C6-FE-CMP-01 | `IngredientForm` (name, default unit, category select, alias chips, active toggle) | `fe-web/src/components/admin/ingredients/IngredientForm.tsx` | ⬜ |
| C6-FE-CMP-02 | Create modal/page | `fe-web/app/admin/(catalog)/ingredients/new.tsx` | ⬜ |
| C6-FE-TST-01 | Component/hook tests | ⬜ |
| C6-FE-OPT-01 | Bulk import UI | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit logging integration | ⬜ | log ingredientId, name |
| Copy deck for success/error | ⬜ |
| Analytics event for new ingredient | ⬜ |

---

## 5. QA Checklist
- [ ] Duplicate name blocked with clear message.
- [ ] Aliases stored unique & trimmed.
- [ ] Category validation ensures existence.
- [ ] FE form shows validation errors & success toast.
- [ ] Optional bulk import validates each row.

---

## 6. Assumptions
1. Default `isActive=true` unless specified.
2. Aliases stored as JSON array string.
3. Bulk import out of scope unless specified.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


