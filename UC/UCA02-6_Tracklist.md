# Tracklist · UCA02-6 “Phê duyệt công thức”

> **Scope**: Moderators approve pending user recipes after review. Covers detail view CTAs, validation checks, status transition, audit logging, notification, optional minor edits before approval.

> **Architecture**: Same Layered API & FE separation as other admin recipe flows.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: open pending recipe detail → review content → click Approve → backend validates status, updates to APPROVED, logs action, notifies creator → UI shows success & updates lists.
- **Alternative**: allow minor edits before approval; assign to specific reviewer; auto-lock during review.
- **Exception**: lacking permission, recipe not pending, DB errors, validation failure.
- **Rules/NFR**:
  - Only moderators can approve.
  - Audit log & timestamp (approvedBy/approvedAt).
  - Notification email after approval.
  - Consistent status sync across caches.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-BE-DTO-01 | Ensure approve request DTO (if any) handles optional edits | `src/modules/admin/recipes/dto/request/approve-recipe.dto.ts` | ⬜ | Currently empty; extend if edits allowed |
| R6-BE-VLD-01 | Validator for approve payload (maybe empty) | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ✅ | `approveRecipeValidator` exists |

### 2.2 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-BE-SVC-01 | `AdminRecipesService.approveRecipe` ensures status check, update, email | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ✅ | Already implemented (status check + email) |
| R6-BE-SVC-02 | Allow minor edits before approval (optional) | same | ⬜ | Accept partial update before status change |
| R6-BE-SVC-03 | Moderation lock release upon approval | same or separate lock service | ⬜ |

### 2.3 Repository Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-BE-REP-01 | `updateStatus` handles APPROVED status and timestamps | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ✅ |
| R6-BE-REP-02 | Lock table (optional) | repo or new table | ⬜ |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-BE-CTL-01 | `POST /admin/recipes/:id/approve` | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ✅ |
| R6-BE-RT-01 | Route with permission `Recipe.Moderate` | `src/modules/admin/recipes/routes.ts` | ✅ |

### 2.5 Notifications & Audit
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-BE-NOT-01 | Email to creator upon approval | `emailService.sendRecipeApprovedEmail` | ✅ |
| R6-BE-AUD-01 | Audit log entry for approval | `admin-audit` | ⬜ | Add action `RECIPE_APPROVE` with recipeId |

### 2.6 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-BE-TST-01 | Unit test service (status check, email) | `src/modules/admin/recipes/services/__tests__/admin-recipes.service.spec.ts` | ⬜ |
| R6-BE-TST-02 | Integration test endpoint (success, not pending, unauthorized) | `src/tests/integration/admin/recipes.approve.spec.ts` | ⬜ |

---

## 3. Frontend Tasks

### 3.1 Types, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-FE-TYP-01 | Define payload type for approval (optional edits) | `fe-web/src/types/admin-recipes.types.ts` | ⬜ |
| R6-FE-SVC-01 | `adminRecipesApi.approve(id, payload?)` | `fe-web/src/services/admin-recipes.service.ts` | ⬜ |

### 3.2 Hooks & Controllers
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-FE-HOOK-01 | `useApproveRecipe` hook (handles loading, error, success) | `fe-web/src/hooks/admin-recipes/useApproveRecipe.ts` | ⬜ |
| R6-FE-HOOK-02 | `useModerationLock` integration (optional) | same | ⬜ |

### 3.3 Components / UI
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-FE-CMP-01 | Approve button/CTA with confirmation | `fe-web/src/components/admin/recipes/ModerationActions.tsx` | ⬜ |
| R6-FE-CMP-02 | Success toast/badge update | shared | ⬜ |
| R6-FE-CMP-03 | Optionally pre-approve edit modal | same | ⬜ |

### 3.4 Screens
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-FE-VIEW-01 | Integrate approve action into pending detail view | `app/admin/(recipes)/[id]/pending.tsx` (or detail route) | ⬜ |
| R6-FE-VIEW-02 | Ensure list refresh after approval | pending list hook | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-FE-TST-01 | Hook/component tests (approve success/error) | `fe-web/src/hooks/admin-recipes/__tests__/useApproveRecipe.test.ts` | ⬜ |
| R6-FE-TST-02 | E2E flow: open pending recipe, approve, verify list updates | `fe-web/tests/admin/recipes-approve.spec.ts` | ⬜ |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R6-CC-01 | Moderation lock release/assignment | shared service | ⬜ |
| R6-CC-02 | Audit log integration | backend audit service | ⬜ |
| R6-CC-03 | Notification templates & copy | docs/UI content | ⬜ |
| R6-CC-04 | Analytics event for approvals | FE analytics | ⬜ |

---

## 5. QA Checklist
- [ ] API rejects approval when status != PENDING.
- [ ] Email notification verified.
- [ ] UI updates list detail after approval.
- [ ] Permissions enforced (403 without Recipe.Moderate).
- [ ] Audit log entry recorded.
- [ ] Optionally confirm moderation locks released.

---

## 6. Assumptions
1. Approval does not require additional payload unless minor edits supported later.
2. Auto-lock per recipe handled elsewhere; approval simply finalizes it.
3. Email content already implemented; only need to ensure invocation.
4. FE interacts with same detail view used for pending review.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist based on UC spec |


