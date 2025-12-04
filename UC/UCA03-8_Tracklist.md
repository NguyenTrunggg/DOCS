# Tracklist · UCA03-8 “Xóa nguyên liệu”

> **Scope**: Admin deletes ingredients while maintaining referential integrity. Supports replacement flow (transfer references), soft delete option, batch delete, and audit logging.

> **Architecture**: Admin ingredients module (Layered API + FE separation).

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: admin selects ingredient → system checks references (recipes/pantry/shopping) → if none, confirm and delete → list refresh.
- **Alternative**: if references exist, require selecting another ingredient to transfer references; soft delete; batch delete.
- **Exception**: lacking permission, ingredient not found, references added during deletion, DB constraint failure.
- **Rules/NFR**:
  - Audit every delete action.
  - Ensure data integrity (update references before delete).

---

## 2. Backend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C8-BE-DTO-01 | `DeleteIngredientRequestDto` | `dto/request/delete-ingredient.dto.ts` | ✅ | Includes optional `transferToIngredientId` |
| C8-BE-VLD-01 | `deleteIngredientValidator` | `validators/admin-ingredients.validator.ts` | ✅ |
| C8-BE-SVC-01 | `AdminIngredientsService.deleteIngredient` handles reference transfer + validation | `services/admin-ingredients.service.ts` | ✅ |
| C8-BE-REP-01 | Repo helper `countUsage` & delete | `repositories/admin-ingredients.repository.ts` | ✅ |
| C8-BE-CTL-01 | Controller route `DELETE /admin/ingredients/:id` | `controllers/admin-ingredients.controller.ts` + `routes.ts` | ✅ |
| C8-BE-CC-01 | Audit log `INGREDIENT_DELETE` (metadata) | admin audit service | ⬜ |
| C8-BE-OPT-01 | Batch delete endpoint | new | ⬜ |
| C8-BE-OPT-02 | Soft delete flag | service/repo | ⬜ |

Testing:
| Task | Status |
|------|--------|
| Unit tests for service (transfer vs block) | ⬜ |
| Integration tests for delete endpoint | ⬜ |

---

## 3. Frontend Tasks

| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| C8-FE-TYP-01 | `DeleteIngredientPayload` type | `fe-web/src/types/admin-ingredients.types.ts` | ⬜ |
| C8-FE-VLD-01 | Client validator (transfer selection) | `fe-web/src/validators/admin-ingredients.validator.ts` | ⬜ |
| C8-FE-SVC-01 | `adminIngredientsApi.delete(id, payload)` | `fe-web/src/services/admin-ingredients.service.ts` | ⬜ |
| C8-FE-HOOK-01 | `useDeleteIngredient` hook | `fe-web/src/hooks/admin-ingredients/useDeleteIngredient.ts` | ⬜ |
| C8-FE-CMP-01 | `DeleteIngredientDialog` (transfer selection + usage warning) | `fe-web/src/components/admin/ingredients/DeleteIngredientDialog.tsx` | ⬜ |
| C8-FE-CMP-02 | Usage summary component | same | ⬜ |
| C8-FE-CMP-03 | Batch delete UI (optional) | same | ⬜ |
| C8-FE-TST-01 | Component/hook tests | ⬜ |

---

## 4. Cross-cutting
| Task | Status | Notes |
|------|--------|-------|
| Audit logging integration | ⬜ |
| Copy deck for confirm dialog | ⬜ |
| Analytics event | ⬜ |

---

## 5. QA Checklist
- [ ] API blocks deletion without transfer when references exist.
- [ ] Transfer moves references correctly.
- [ ] FE dialog enforces replacement selection when needed.
- [ ] Permissions enforced (`Ingredient.Delete`).
- [ ] Audit logs recorded.
- [ ] Optional soft delete/batch delete tested if implemented.

---

## 6. Assumptions
1. Hard delete is current behavior; soft delete optional.
2. References tracked in recipes, pantry, shopping list.
3. Batch delete will reuse same transfer logic per item.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist |


