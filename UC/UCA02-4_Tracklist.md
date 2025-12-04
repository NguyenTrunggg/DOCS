# Tracklist · UCA02-4 “Xóa công thức hệ thống”

> **Scope**: Admin deletes system recipes (soft delete by default) with confirmation dialog, optional name re-entry, batch delete, and media cleanup. Ensures audit logging and consistent search exclusion.

> **Architecture**: Continue Layered API approach (DTO → Validator → Repository → Service → Controller → Routes) and FE separation. Reuse existing admin recipes module.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: admin clicks delete → confirm modal with recipe name → backend deletes recipe (soft delete) and related media → success message.
- **Alternative**: soft delete with 30-day restore window; batch delete multiple recipes at once.
- **Exception**: missing permission, recipe not found, storage failure.
- **Business/NFR**:
  - Confirmation required; high-engagement recipes may require typing recipe name.
  - Audit log each delete (who, when, reason).
  - Remove recipe from search/list immediately.
  - Media cleanup ensures no orphaned files.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-BE-DTO-01 | Optional: define `DeleteRecipeRequestDto` if reason/confirm text required | `src/modules/admin/recipes/dto/request/delete-recipe.dto.ts` (new) | ⬜ | Fields: `confirmName`, `reason?` |
| R4-BE-DTO-02 | DTO for batch delete payload | new file | ⬜ | `recipeIds[]`, `confirmName`, `reason?` |
| R4-BE-VLD-01 | Validator for delete/batch delete body | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ⬜ | Ensure confirm text matches, proper arrays |

### 2.2 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-BE-REP-01 | `AdminRecipesRepository.delete(id)` soft delete (already) | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ✅ | Sets `deletedAt` |
| R4-BE-REP-02 | Add `hardDelete` (optional restore window) | same | ⬜ | Remove record & relations |
| R4-BE-REP-03 | `deleteMany(ids)` for batch | same | ⬜ |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-BE-SVC-01 | `AdminRecipesService.deleteRecipe(id, adminId, payload?)` handle confirm & audit | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ⬜ | Need confirm logic & media cleanup |
| R4-BE-SVC-02 | `batchDeleteRecipes(ids, adminId, payload?)` | same | ⬜ | Return per-recipe result |
| R4-BE-SVC-03 | `restoreRecipe(id)` (if soft delete restore) | same | ⬜ | Optional feature |
| R4-BE-SVC-04 | Media cleanup utility (delete images/videos) | service + storage | ⬜ | Remove from storage when deleting |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-BE-CTL-01 | `deleteRecipe` endpoint accept optional body | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ⬜ | Validate confirm text & reason |
| R4-BE-CTL-02 | Add `POST /admin/recipes/batch/delete` for bulk deletes | same | ⬜ | Permission `Recipe.Delete` |
| R4-BE-RT-01 | Update routes to use new validators | `src/modules/admin/recipes/routes.ts` | ⬜ |

### 2.5 Testing & Observability
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-BE-TST-01 | Unit tests `deleteRecipe` (success, not found, confirm mismatch) | `src/modules/admin/recipes/services/__tests__/admin-recipes.service.spec.ts` | ⬜ |
| R4-BE-TST-02 | Integration tests for delete/batch delete endpoints | `src/tests/integration/admin/recipes.delete.spec.ts` | ⬜ |
| R4-BE-OBS-01 | Audit log entries for delete | `admin-audit` service | ⬜ | Action `RECIPE_DELETE` |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-FE-TYP-01 | Define `DeleteRecipePayload` & `BatchDeletePayload` | `fe-web/src/types/admin-recipes.types.ts` | ⬜ |
| R4-FE-VLD-01 | Client validator for confirm dialog (name matching) | `fe-web/src/validators/admin-recipes.validator.ts` | ⬜ |
| R4-FE-SVC-01 | `adminRecipesApi.delete(id, payload)` | `fe-web/src/services/admin-recipes.service.ts` | ⬜ |
| R4-FE-SVC-02 | `adminRecipesApi.batchDelete(payload)` | same | ⬜ |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-FE-HOOK-01 | `useDeleteRecipe` (single) | `fe-web/src/hooks/admin-recipes/useDeleteRecipe.ts` | ⬜ | Manage confirm + API call |
| R4-FE-HOOK-02 | `useBatchDeleteRecipes` | same dir | ⬜ | Works with list selection |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-FE-CMP-01 | `DeleteRecipeDialog` | `fe-web/src/components/admin/recipes/DeleteRecipeDialog.tsx` | ⬜ | Confirm text input, reason optional |
| R4-FE-CMP-02 | `BatchDeleteSheet` | same namespace | ⬜ | Show selected count |
| R4-FE-CMP-03 | List action buttons (trash icon, menu) | `RecipesTable` | ⬜ | Trigger dialog |
| R4-FE-CMP-04 | Success/Error toast components | shared | ⬜ |

### 3.4 Screens
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-FE-VIEW-01 | Integrate delete dialog into recipe list | `app/admin/(recipes)/index.tsx` | ⬜ |
| R4-FE-VIEW-02 | Add delete CTA on recipe detail page | `app/admin/(recipes)/[id]/index.tsx` | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-FE-TST-01 | Component tests for delete dialog (confirm & mismatch) | `fe-web/src/components/admin/recipes/__tests__/DeleteRecipeDialog.test.tsx` | ⬜ |
| R4-FE-TST-02 | Hook tests for delete/batch delete | `fe-web/src/hooks/admin-recipes/__tests__/` | ⬜ |
| R4-FE-TST-03 | E2E scenario: delete recipe, batch delete, permission error | `fe-web/tests/admin/recipes-delete.spec.ts` | ⬜ |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R4-CC-01 | Media cleanup service (images/videos) on delete | storage service | ⬜ |
| R4-CC-02 | Restore flow documentation (if soft delete) | docs | ⬜ |
| R4-CC-03 | Confirmation copy deck (VN) | `DOCS/UI/content/admin-recipes.md` | ⬜ |
| R4-CC-04 | Analytics event for deletions | FE analytics | ⬜ |

---

## 5. QA Checklist
- [ ] API tests cover success, not found, confirm mismatch, batch delete.
- [ ] Storage cleanup verified (no orphaned files).
- [ ] Audit logs capture delete action.
- [ ] FE dialog enforces confirm text and displays warnings.
- [ ] Batch delete handles partial failures gracefully.
- [ ] Performance: delete <1s, even with media cleanup.

---

## 6. Assumptions
1. Soft delete default; hard delete optional & requires elevated permission.
2. Confirm text (typing recipe name) only for high-engagement recipes flag.
3. Batch delete uses same confirmation for all selected recipes; advanced per-item confirm later.
4. Restore endpoint may be future scope; for now, soft delete hides from UI/search immediately.
5. Media stored in object storage; cleanup performed asynchronously if needed.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist from UC spec |


