# Tracklist · UCA02-5 “Xem danh sách công thức chờ duyệt”

> **Scope**: Admin view for pending recipes awaiting moderation (user-submitted). Includes search, filter (by creator), sort, pagination; ability to export list, assign reviewer, lock recipe for moderation.

> **Architecture**: Follow Layered API & FE separation. Reuse admin recipes module.

Legend: ⬜ Pending · 🔄 In Progress · ✅ Done · ❌ Blocked

---

## 1. UC Breakdown
- **Basic**: load pending recipes (`status=PENDING`), show ID/title/author/date/status, filter by search & author, sort by submission date, paginate 20/page. Row click opens detail for approve/reject.
- **Alternative**: assign to reviewer (lock), export queue.
- **Exception**: empty queue, errors, session timeout.
- **Rules/NFR**:
  - Permission `Recipe.Moderate`.
  - Each recipe locked to one moderator at a time (future).
  - Perf <2s.

---

## 2. Backend Tasks

### 2.1 DTO & Validators
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-BE-DTO-01 | Ensure `GetPendingRecipesQueryDto` covers search, author filter, paging, sort | `src/modules/admin/recipes/dto/request/get-pending-recipes.dto.ts` | ✅ | Already exists |
| R5-BE-DTO-02 | DTO for export (pending queue) | new file | ⬜ | `limit<=5000`, same filters |
| R5-BE-VLD-01 | Validator for pending query | `src/modules/admin/recipes/validators/admin-recipes.validator.ts` | ✅ | `getPendingRecipesQueryValidator` |
| R5-BE-VLD-02 | Validator for export query | same | ⬜ | New `exportPendingRecipesValidator` |

### 2.2 Repository
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-BE-REP-01 | `findPendingRecipes` (status filter, search, creator) | `src/modules/admin/recipes/repositories/admin-recipes.repository.ts` | ✅ | Already implemented |
| R5-BE-REP-02 | Export method for pending list | same | ⬜ | Reuse `findPendingRecipes` with limit |
| R5-BE-REP-03 | Moderation lock storage (optional) | same or new repo | ⬜ | Add lock flag timestamp |

### 2.3 Service Layer
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-BE-SVC-01 | `AdminRecipesService.getPendingRecipes` returning data + total | `src/modules/admin/recipes/services/admin-recipes.service.ts` | ✅ | Already implemented |
| R5-BE-SVC-02 | `exportPendingRecipes(query)` | same | ⬜ | Build CSV |
| R5-BE-SVC-03 | Moderation lock/unlock (optional) | same | ⬜ | Reserve recipe for reviewer |

### 2.4 Controller & Routes
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-BE-CTL-01 | `getPendingRecipes` endpoint | `src/modules/admin/recipes/controllers/admin-recipes.controller.ts` | ✅ |
| R5-BE-CTL-02 | `exportPending` endpoint | same | ⬜ | New handler |
| R5-BE-RT-01 | Route `GET /admin/recipes/pending` with validator + permission | `src/modules/admin/recipes/routes.ts` | ✅ |
| R5-BE-RT-02 | Route for export/lock | same | ⬜ |

### 2.5 Testing & Observability
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-BE-TST-01 | Integration test pending list endpoint | `src/tests/integration/admin/recipes.pending.spec.ts` | ⬜ |
| R5-BE-TST-02 | Export pending list test | same | ⬜ |
| R5-BE-OBS-01 | Audit log for accessing pending queue | `admin-audit` | ⬜ |

---

## 3. Frontend Tasks

### 3.1 Types, Validators, Services
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-FE-TYP-01 | DTO types for pending list items & query | `fe-web/src/types/admin-recipes.types.ts` | ⬜ |
| R5-FE-SVC-01 | `adminRecipesApi.listPending(query)` | `fe-web/src/services/admin-recipes.service.ts` | ⬜ |
| R5-FE-SVC-02 | `adminRecipesApi.exportPending(query)` | same | ⬜ |

### 3.2 Hooks & State
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-FE-HOOK-01 | `usePendingRecipesList` | `fe-web/src/hooks/admin-recipes/usePendingRecipesList.ts` | ⬜ | Manage filters, pagination |
| R5-FE-HOOK-02 | `usePendingExport` | same dir | ⬜ |
| R5-FE-HOOK-03 | `useModerationLock` (optional) | same | ⬜ |

### 3.3 Components
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-FE-CMP-01 | `PendingRecipesToolbar` | `fe-web/src/components/admin/recipes/PendingRecipesToolbar.tsx` | ⬜ | Search, filter by author, export button |
| R5-FE-CMP-02 | `PendingRecipesTable` | same namespace | ⬜ | Columns per UC, lock indicator |
| R5-FE-CMP-03 | `PendingEmptyState` / `PendingErrorState` | same | ⬜ |
| R5-FE-CMP-04 | `AssignReviewerDialog` (optional) | same | ⬜ |

### 3.4 Screen
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-FE-VIEW-01 | Pending list screen route `app/admin/(recipes)/pending.tsx` | ⬜ | Compose toolbar + table |
| R5-FE-VIEW-02 | Quick link from main list to pending tab | `app/admin/(recipes)/index.tsx` | ⬜ |

### 3.5 Testing
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-FE-TST-01 | Hook tests for pending list | `fe-web/src/hooks/admin-recipes/__tests__/usePendingRecipesList.test.ts` | ⬜ |
| R5-FE-TST-02 | Component tests (toolbar/table) | `fe-web/src/components/admin/recipes/__tests__/` | ⬜ |
| R5-FE-TST-03 | E2E flow: view pending, filter, export | `fe-web/tests/admin/recipes-pending.spec.ts` | ⬜ |

---

## 4. Cross-cutting
| ID | Task | File | Status | Notes |
|----|------|------|--------|-------|
| R5-CC-01 | Moderation lock (per-recipe) to avoid double review | shared service | ⬜ |
| R5-CC-02 | Audit logging for queue views | backend audit service | ⬜ |
| R5-CC-03 | Copy deck for empty/error states | `DOCS/UI/content/admin-recipes.md` | ⬜ |
| R5-CC-04 | Analytics event for moderation queue usage | FE analytics | ⬜ |

---

## 5. QA Checklist
- [ ] Pending list endpoint returns only PENDING recipes; filters verified.
- [ ] Export limited to 5000 rows, includes filter metadata.
- [ ] UI handles empty queue gracefully.
- [ ] Permissions enforced (`Recipe.Moderate`), unauthorized returns 403.
- [ ] Potential moderation lock prevents double approvals (if implemented).
- [ ] Audit logs capture queue access/export.

---

## 6. Assumptions
1. Pending queue only includes user-submitted recipes (`isSystemRecipe=false`).
2. Moderation lock/assignment optional; initial implementation may skip.
3. Export only available to moderators with permission.
4. Pagination defaults to 20 items/page; can adjust via backend config.
5. Search uses normalized title.

---

## 7. Changelog
| Date | Author | Notes |
|------|--------|-------|
| 2025-11-27 | AI Agent | Initial tracklist from UC spec |


