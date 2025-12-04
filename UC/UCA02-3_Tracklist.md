# Tracklist · UCA02-3 “Sửa công thức hệ thống”

> **Scope**: Admin edits existing recipes (system-created or approved user recipes) including basic metadata, ingredients, steps, media. Supports publish/draft toggle, validation parity with create flow, optional versioning and auto-save.

> **Architecture**: Maintain Layered API (`DTO → Validator → Repository → Service → Controller → Routes`) and FE separation. Reuse structures from UCA02-2 where possible.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open detail → click edit → form prefilled → admin edits fields (title, desc, category, times, difficulty, servings, ingredients, steps, media) → submit → backend validates, updates data, logs audit → success message.
- **Alternative**: save draft (status DRAFT), versioning to revert changes.
- **Exception**: no permission, invalid data (min ingredients/steps, invalid media), DB errors, concurrent edits.
- **Rules/NFR**:
  - Maintain ≥3 ingredients & steps.
  - Step numbers must remain sequential.
  - Media files validated (size/type).
  - Keep history for audit (versioning future).

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-BE-DTO-01 | Ensure `UpdateRecipeRequestDto` includes all editable fields (media updates, status) | `src/modules/admin/recipes/dto/request/update-recipe.dto.ts` | ✅ | Already syncs with create DTO via shared interfaces |
| R3-BE-VLD-01 | `updateRecipeValidator` enforces same constraints as create (min ingredients/steps when present) | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ⬜ | Consider custom rule for sequential steps |

### 2.2 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-BE-REP-01 | `AdminRecipesRepository.update` to handle uniqueness & includes | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ⬜ | Already checks unique title; extend if required |
| R3-BE-REP-02 | `updateIngredients` & `updateSteps` maintain order & integrity | same | ✅ | Already implemented |
| R3-BE-REP-03 | Add `upsertImages` method to handle gallery edits (optional) | same | ⬜ | If multi-image supported |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-BE-SVC-01 | `AdminRecipesService.updateRecipe` – validate, slug changes, update data, log | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ✅ | Already performs validation + updates |
| R3-BE-SVC-02 | Support media replace/delete (call storage service) | same + storage layer | ⬜ | Manage uploaded files lifecycle |
| R3-BE-SVC-03 | Versioning hook (optional) | same | ⬜ | Store previous version snapshot |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-BE-CTL-01 | `updateRecipe` endpoint ensures auth & uses validator | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ✅ | Already implemented |
| R3-BE-RT-01 | Route `PUT /admin/recipes/:id` with permission `Recipe.Update` | `src/modules/admin/recipes/routes.ts` | ✅ | Already defined |

### 2.5 Testing & Observability
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-BE-TST-01 | Unit tests for update logic (slug regen, validation) | `src/modules/admin/recipes/services/__tests__/admin-recipes.service.spec.ts` | ⬜ | Mock repo/storage |
| R3-BE-TST-02 | Integration tests `PUT /admin/recipes/:id` | `src/tests/integration/admin/recipes.update.spec.ts` | ⬜ | Success, validation error, permission |
| R3-BE-OBS-01 | Audit log for updates | `admin-audit` service | ⬜ | Action `RECIPE_UPDATE` w/ metadata |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-FE-TYP-01 | `UpdateAdminRecipePayload` mirroring DTO | `fe-web/src/types/admin-recipes.types.ts` | ⬜ |
| R3-FE-VLD-01 | Client validator for edit form | `fe-web/src/validators/admin-recipes.validator.ts` | ⬜ | Enforce min ingredients/steps if changed |
| R3-FE-SVC-01 | `adminRecipesApi.update(id, payload)` | `fe-web/src/services/admin-recipes.service.ts` | ⬜ | Multipart if media included |

### 3.2 Hooks & Controllers
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-FE-HOOK-01 | `useEditRecipeController` handling form state, validation, diffs | `fe-web/src/hooks/admin-recipes/useEditRecipeController.ts` | ⬜ |
| R3-FE-HOOK-02 | `useRecipeVersioning` (optional) | same | ⬜ |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-FE-CMP-01 | `AdminRecipeEditForm` (prefilled form) | `fe-web/src/components/admin/recipes/AdminRecipeEditForm.tsx` | ⬜ | Reuse create form components |
| R3-FE-CMP-02 | `IngredientListEditor` reuse for edit | same | ⬜ |
| R3-FE-CMP-03 | `StepsEditor` support reorder, add/remove | same | ⬜ |
| R3-FE-CMP-04 | `MediaManager` (preview & replace) | same | ⬜ |
| R3-FE-CMP-05 | `VersionHistoryPanel` (optional) | same | ⬜ |

### 3.4 Screen Integration
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-FE-VIEW-01 | Edit screen route `app/admin/(recipes)/[id]/edit.tsx` | ⬜ | Preload recipe detail & render form |
| R3-FE-VIEW-02 | Add “Sửa” CTA from list/detail screens | `app/admin/(recipes)/index.tsx` + detail page | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-FE-TST-01 | Component tests for edit form (init values, validation) | `fe-web/src/components/admin/recipes/__tests__/AdminRecipeEditForm.test.tsx` | ⬜ |
| R3-FE-TST-02 | Hook tests for controller (success/error) | `fe-web/src/hooks/admin-recipes/__tests__/useEditRecipeController.test.ts` | ⬜ |
| R3-FE-TST-03 | E2E: edit recipe, handle validation errors | `fe-web/tests/admin/recipes-edit.spec.ts` | ⬜ |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R3-CC-01 | Media storage cleanup when replacing files | storage service | ⬜ |
| R3-CC-02 | Versioning/ audit doc update | `docs/admin/recipes.md` | ⬜ |
| R3-CC-03 | Copy deck for edit success/error messages | `DOCS/UI/content/admin-recipes.md` | ⬜ |
| R3-CC-04 | Autosave/draft strategy (optional) | README / future story | ⬜ |

---

## 5. QA Checklist
- [ ] API update tests cover slug change, validation errors, unauthorized access.
- [ ] FE ensures ingredient/step lists maintain constraints.
- [ ] Media replacement tested (old file deleted).
- [ ] Audit log records edit action with admin ID.
- [ ] UX parity with create form (layout, validation messages).
- [ ] Performance: update <2s for typical payload.

---

## 6. Assumptions
1. Edit form reuses create form components for consistency.
2. Media uploads handled via same service as create; editing may remove/replace assets.
3. Versioning/autosave not in initial scope but placeholders added to track list.
4. Only Admin/Super Admin can edit system recipes; permission `Recipe.Update`.
5. Concurrent edits resolved last-write-wins (no locking) unless future requirement adds locking.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist from UC spec |


