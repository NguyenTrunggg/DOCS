# Tracklist · UCA02-2 “Thêm công thức hệ thống”

> **Scope**: Admin creates official (system) recipes with full metadata (basic info, ingredients, steps, media). Includes optional draft mode, validations, media uploads, slug uniqueness, audit/logging, optional import/copy flows (future).

> **Architecture**: Follow Layered API (`DTO → Validator → Repository → Service → Controller → Routes`) + FE separation (`Validator → Service gateway → Hook/Controller → Component/View`). All files stay within admin recipes namespace.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic flow**: open “Add recipe” form → fill fields (title, description, category, times, difficulty, servings, ingredients >=3, steps >=3, media) → submit → backend validates, uploads media, creates recipe with status `APPROVED` (if publish) or `DRAFT` → success message & redirect to detail/list.
- **Alternative**: save as draft (publish=false), import from file (JSON/CSV), duplicate existing recipe.
- **Exception**: missing mandatory info, invalid media, DB errors (unique title, slug), permission issues.
- **Business rules/NFR**:
  - Title must be unique among system recipes.
  - Ingredients/steps require minimum count and sequential step numbers.
  - Media must meet size/format constraints (JPG/PNG/...).
  - Save operation <3s excluding uploads; auto-save drafts (future).

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-BE-DTO-01 | Review `CreateRecipeRequestDto` for all fields (publish flag, media list support) | `src/modules/admin/recipes/dto/request/create-recipe.dto.ts` | ⬜ | Add optional `images` array, `videos?`, `tags?` if needed |
| R2-BE-DTO-02 | Define DTO for draft import (if import feature prioritized) | new file | ⬜ | Optional |
| R2-BE-VLD-01 | Ensure `createRecipeValidator` enforces min ingredients/steps, media constraints, slug-safe title | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ⬜ | Add optional media schema (size/mime validated elsewhere) |

### 2.2 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-BE-REP-01 | `AdminRecipesRepository.create` already handles ingredients/steps; extend to store images/videos if needed | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ⬜ | Add support for gallery records |
| R2-BE-REP-02 | Unique title check for system recipes | same | ✅ | Already exists before create |
| R2-BE-REP-03 | Upsert for draft auto-save (optional) | same | ⬜ | For auto-save feature |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-BE-SVC-01 | `AdminRecipesService.createRecipe` (validate, slugify, create, log) | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ✅ | Already implemented; confirm image upload + publish flag behavior |
| R2-BE-SVC-02 | Media upload handling (image/video) | same or `@infrastructure/storage` | ⬜ | Use existing storage utilities; ensure cleanup on failure |
| R2-BE-SVC-03 | Auto-save drafts service (optional) | same | ⬜ | If later needed |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-BE-CTL-01 | Controller `createRecipe` ensures auth + uses validator | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ✅ | Already present |
| R2-BE-RT-01 | Route `POST /admin/recipes` with `Recipe.Create` permission | `src/modules/admin/recipes/routes.ts` | ✅ | Already defined |
| R2-BE-CTL-02 | Endpoints for import/copy (optional) | controller/routes | ⬜ | If import feature prioritized |

### 2.5 Testing & Observability
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-BE-TST-01 | Unit test for `createRecipe` (validations, slug uniqueness) | `src/modules/admin/recipes/services/__tests__/admin-recipes.service.spec.ts` | ⬜ | Mock repo + storage |
| R2-BE-TST-02 | Integration test `POST /admin/recipes` | `src/tests/integration/admin/recipes.create.spec.ts` | ⬜ | Cover success, validation errors, permission |
| R2-BE-OBS-01 | Audit logging when admin creates recipe | `admin-audit` module | ⬜ | Action `RECIPE_CREATE` w/ metadata |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-FE-TYP-01 | Define `CreateAdminRecipePayload` | `fe-web/src/types/admin-recipes.types.ts` | ⬜ | Mirror BE fields including publish |
| R2-FE-VLD-01 | Client validator for form (title unique hint, ingredient/step count, media) | `fe-web/src/validators/admin-recipes.validator.ts` | ⬜ | Align with Joi constraints |
| R2-FE-SVC-01 | `adminRecipesApi.create(payload)` using multipart/form-data | `fe-web/src/services/admin-recipes.service.ts` | ⬜ | handles uploads |
| R2-FE-SVC-02 | Support import/copy endpoints if available | same | ⬜ |

### 3.2 Hooks & Controllers
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-FE-HOOK-01 | `useCreateRecipeController` (form state, validator, preview) | `fe-web/src/hooks/admin-recipes/useCreateRecipeController.ts` | ⬜ | Manage auto-save placeholder |
| R2-FE-HOOK-02 | `useRecipeMediaUploader` (image/video uploads) | same namespace | ⬜ | Reuse storage utils |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-FE-CMP-01 | `AdminRecipeForm` (inputs for all fields) | `fe-web/src/components/admin/recipes/AdminRecipeForm.tsx` | ⬜ | Use modular sections |
| R2-FE-CMP-02 | `IngredientListEditor` (add/remove, reorder) | same namespace | ⬜ | Validate >=3 |
| R2-FE-CMP-03 | `StepsEditor` with reorder + media per step | same | ⬜ |
| R2-FE-CMP-04 | `RecipeMediaUploader` (image/video) | shared component | ⬜ | Limit size & types |
| R2-FE-CMP-05 | `PublishToggle` (publish vs draft) | same | ⬜ |
| R2-FE-CMP-06 | `AutoSaveIndicator` | same | ⬜ | Optional future |

### 3.4 Screen & Navigation
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-FE-VIEW-01 | Create screen route (e.g., `app/admin/(recipes)/create.tsx`) | `fe-web/app/admin/(recipes)/create.tsx` | ⬜ | Compose form + controller |
| R2-FE-VIEW-02 | Connect list CTA (from UC-A2-1 toolbar) to create screen | `app/admin/(recipes)/index.tsx` | ⬜ |
| R2-FE-VIEW-03 | Optional import/copy modals | same | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-FE-TST-01 | Form component tests (validation, adding ingredients/steps) | `fe-web/src/components/admin/recipes/__tests__/AdminRecipeForm.test.tsx` | ⬜ |
| R2-FE-TST-02 | Hook tests for controller (success/error/draft) | `fe-web/src/hooks/admin-recipes/__tests__/useCreateRecipeController.test.ts` | ⬜ |
| R2-FE-TST-03 | E2E flow: create recipe success, validation errors | `fe-web/tests/admin/recipes-create.spec.ts` | ⬜ |

---

## 4. Cross-cutting / Infrastructure
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R2-CC-01 | Media upload config (max size, allowed mime) | `config/file-upload.ts` & FE uploader | ⬜ | Align BE & FE |
| R2-CC-02 | Auto-save drafts (cron or local storage) documentation | `docs/admin/recipes.md` | ⬜ |
| R2-CC-03 | Audit logging hook for recipe creation | `admin-audit` service | ⬜ | Save metadata (title, status, isSystemRecipe) |
| R2-CC-04 | Copy deck for success/error messages | `DOCS/UI/content/admin-recipes.md` | ⬜ | VN translations |

---

## 5. QA Checklist
- [ ] API tests for creation (success, validation, duplicate title).
- [ ] Media upload tested with valid/invalid files.
- [ ] Audit log entry verified when admin creates recipe.
- [ ] UI ensures min 3 ingredients/steps and sequential step numbers.
- [ ] Draft vs publish flows validated (status set correctly).
- [ ] Performance: create API latency <3s (excluding large uploads).

---

## 6. Assumptions
1. Default behavior publishes recipe (`publish !== false`). Draft toggle sets status to `DRAFT`.
2. Photo/video storage reuse existing storage service; no CDN setup change required.
3. Import/copy flows are optional stretch goals; not blocking base UC.
4. Auto-save drafts handled later; currently manual save only.
5. Slug uniqueness enforced server-side; FE may provide pre-check but not required.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist from UC spec |


