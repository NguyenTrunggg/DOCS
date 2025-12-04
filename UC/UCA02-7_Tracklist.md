# Tracklist · UCA02-7 “Từ chối công thức”

> **Scope**: Moderators reject pending recipes, record reason, optionally give suggestions, notify submitter, and ensure audit logging. Tied to detail view from pending queue (UCA02-5).

> **Architecture**: Same Layered API / FE separation as UCA02-6 (approve). Reuse admin recipes module.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic flow**: open pending recipe detail → click Reject → dialog prompts reason (required) & optional suggestion → backend validates status, updates to REJECTED, logs, notifies user, shows success toast.
- **Alternative**: preset reasons list, suggestions for re-submission, ability to assign reviewer before rejection.
- **Exception**: missing permission, recipe not pending, DB error.
- **Rules/NFR**:
  - Reason must be clear, respectful; stored in DB.
  - Email notification sent immediately.
  - Audit log for moderation actions.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-BE-DTO-01 | `RejectRecipeRequestDto` fields (reason, suggestion?) | `src/modules/admin/recipes/dto/request/reject-recipe.dto.ts` | ✅ | Already includes reason & suggestion |
| R7-BE-VLD-01 | Validator ensures reason length, optional suggestion | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ✅ | `rejectRecipeValidator` present |

### 2.2 Service/Repository
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-BE-SVC-01 | `AdminRecipesService.rejectRecipe` (status check, update, email) | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ✅ | Existing logic updates status + sends email |
| R7-BE-SVC-02 | Support preset reasons / suggestion templates (optional) | same | ⬜ | Extend DTO to allow reason codes |
| R7-BE-SVC-03 | Moderation lock release after rejection | same | ⬜ |
| R7-BE-REP-01 | `updateStatus` handles REJECTED state + rejectionReason | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ✅ |

### 2.3 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-BE-CTL-01 | `POST /admin/recipes/:id/reject` | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ✅ |
| R7-BE-RT-01 | Route with permission `Recipe.Moderate` | `src/modules/admin/recipes/routes.ts` | ✅ |

### 2.4 Notifications & Audit
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-BE-NOT-01 | Email to creator with reason & suggestion | `emailService.sendRecipeRejectedEmail` | ✅ |
| R7-BE-AUD-01 | Audit log entry `RECIPE_REJECT` | `admin-audit` | ⬜ | Add metadata (reason, adminId) |

### 2.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-BE-TST-01 | Unit test service (status guard, email) | `src/modules/admin/recipes/services/__tests__/admin-recipes.service.spec.ts` | ⬜ |
| R7-BE-TST-02 | Integration test endpoint (success, non-pending, unauthorized) | `src/tests/integration/admin/recipes.reject.spec.ts` | ⬜ |

---

## 3. Frontend Tasks

### 3.1 Types, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-FE-TYP-01 | `RejectRecipePayload` type (reason, suggestion?) | `fe-web/src/types/admin-recipes.types.ts` | ⬜ |
| R7-FE-SVC-01 | `adminRecipesApi.reject(id, payload)` | `fe-web/src/services/admin-recipes.service.ts` | ⬜ |

### 3.2 Hooks & Controllers
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-FE-HOOK-01 | `useRejectRecipe` hook (dialog state, API call) | `fe-web/src/hooks/admin-recipes/useRejectRecipe.ts` | ⬜ |
| R7-FE-HOOK-02 | Optional hook for preset reasons | same | ⬜ |

### 3.3 Components / UI
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-FE-CMP-01 | `RejectRecipeDialog` (textarea + dropdown of presets) | `fe-web/src/components/admin/recipes/RejectRecipeDialog.tsx` | ⬜ |
| R7-FE-CMP-02 | `ModerationActions` update to show reject CTA | same namespace | ⬜ |
| R7-FE-CMP-03 | Success/Error toast | shared | ⬜ |

### 3.4 Screen Integration
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-FE-VIEW-01 | Integrate reject action on pending detail view | `app/admin/(recipes)/[id]/pending.tsx` | ⬜ |
| R7-FE-VIEW-02 | Refresh pending list & detail after rejection | pending list hook | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-FE-TST-01 | Component tests for dialog (validation, preset) | `fe-web/src/components/admin/recipes/__tests__/RejectRecipeDialog.test.tsx` | ⬜ |
| R7-FE-TST-02 | Hook tests for `useRejectRecipe` | `fe-web/src/hooks/admin-recipes/__tests__/useRejectRecipe.test.ts` | ⬜ |
| R7-FE-TST-03 | E2E: reject pending recipe, ensure status updated | `fe-web/tests/admin/recipes-reject.spec.ts` | ⬜ |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R7-CC-01 | Moderation lock release | shared service | ⬜ |
| R7-CC-02 | Audit log integration | backend audit service | ⬜ |
| R7-CC-03 | Preset reason config file | `DOCS/UI/content/moderation-reasons.md` | ⬜ |
| R7-CC-04 | Analytics event for rejections | FE analytics | ⬜ |

---

## 5. QA Checklist
- [ ] API ensures recipe status must be PENDING before rejection.
- [ ] Reason recorded & emailed to creator.
- [ ] UI validation prevents empty reason.
- [ ] Pending list updates after rejection.
- [ ] Audit logs capture action.
- [ ] Optional: preset reasons appear correctly.

---

## 6. Assumptions
1. Rejection requires reason; suggestion optional.
2. Preset reasons list defined elsewhere; backend stores final text only.
3. Moderation lock optional; ensures only assigned reviewer can reject.
4. Email template already exists; just ensure invocation.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist drafted |


