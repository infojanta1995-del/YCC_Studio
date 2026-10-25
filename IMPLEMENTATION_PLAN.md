# YCC Studio — Implementation Plan

**Status:** Planning only; no implementation tasks are verified complete  
**Last updated:** 2026-10-25

## 1. Development strategy

Build in small, verifiable increments. First inspect the environment and supplied Script Writer ZIP, then approve architecture decisions, establish the project shell and data model, and implement the workflow foundations before generation modules. Keep the seven-screen HUD consistent through shared design tokens/components.

Do not create application code until the user explicitly moves from planning to implementation. Do not assume a technology stack.

## 2. Phase overview

### Phase 0 — Requirements and artifact audit
**Objective:** Close major unknowns and inspect supplied materials.
Tasks:
- P0-01: Confirm that this documentation reflects the agreed YCC plan.
- P0-02: Inspect the Script Writer ZIP: archive contents, runtime, dependencies, entry point, I/O, licence, risks.
- P0-03: Decide the 16:9 duration/segment arithmetic.
- P0-04: Decide whether app generates actual thumbnail images or only prompts.
- P0-05: Decide source URL/transcript support and permitted methods.
- P0-06: Decide live keyword research expectations.
Deliverables: audit report, open-question list, updated docs.
Exit criteria: major unknowns are either decided or explicitly left TBD with impact understood.

### Phase 1 — Architecture and stack decisions
**Objective:** Select an implementable stack based on constraints.
Tasks:
- P1-01: Inspect existing repository if any; do not overwrite or duplicate existing structure.
- P1-02: Select frontend/backend/runtime/database.
- P1-03: Select AI integration strategy and credential handling.
- P1-04: Define authentication and project access model.
- P1-05: Define version/provenance data model and workflow state transitions.
- P1-06: Record ADRs.
Deliverables: approved architecture and verified setup instructions.
Exit criteria: a minimal project can be built and tested using documented commands.

### Phase 2 — Project foundation and design system
**Objective:** Establish the shell, navigation, project persistence, and common UI.
Tasks:
- P2-01: Create app skeleton and verified development/test commands.
- P2-02: Implement design tokens, typography, spacing, cards, buttons, inputs, status indicators.
- P2-03: Implement HUD shell, sidebar, top bar, responsive navigation.
- P2-04: Implement reduced-motion behaviour and keyboard focus.
- P2-05: Implement Project Manager and Project History.
- P2-06: Implement project-scoped data access and tests.
Deliverables: navigable app with persistent project records.
Exit criteria: project create/open/rename/duplicate/archive/delete behaviour passes tests and is documented.

### Phase 3 — Master Details, Source Manager, Project Memory
**Objective:** Persist authoritative project settings and source analysis.
Tasks:
- P3-01: Implement format/duration/voice/language settings.
- P3-02: Implement source text/title editor and source versions.
- P3-03: Add supported document upload only after file constraints are chosen.
- P3-04: Add URL resolution/transcript integration only after provider/method is approved.
- P3-05: Implement source analysis structured output.
- P3-06: Implement analysis validity, stale states, and refresh recovery.
- P3-07: Implement project memory references and provenance.
Deliverables: a project can save sources/settings and restore analysis.
Exit criteria: source changes mark analysis stale; failed refresh preserves old valid data.

### Phase 4 — Reference Script integration
**Objective:** Integrate the inspected Script Writer skill safely.
Tasks:
- P4-01: Build a normalized input/output contract from the skill audit.
- P4-02: Implement a controlled adapter/runner.
- P4-03: Add timeout, dependency, malformed-output, and cancellation handling.
- P4-04: Persist Reference Script versions and provenance.
- P4-05: Add UI review/edit/history.
Deliverables: verified Reference Script generation.
Exit criteria: known test inputs produce valid saved outputs; failure paths preserve prior versions.

### Phase 5 — Master Script Studio and workflow gates
**Objective:** Make the approved Master Script canonical.
Tasks:
- P5-01: Implement generator input assembly from source, analysis, Reference Script, and settings snapshot.
- P5-02: Implement part-aware editor and version history.
- P5-03: Implement source/fact/language/duration validation.
- P5-04: Implement compare, save draft, regenerate, approve.
- P5-05: Implement workflow transition rules and blocking/ warning distinction.
Deliverables: Master Script can be generated, edited, validated, approved, and reopened.
Exit criteria: downstream generation is blocked until a specific Master Script version is approved.

### Phase 6 — Video Prompt Studio
**Objective:** Generate and manage one standalone prompt per script part.
Tasks:
- P6-01: Implement full-script analysis and part extraction.
- P6-02: Implement prompt template/service with exact narration preservation.
- P6-03: Implement duration, ratio, no-text, continuity, factuality validation.
- P6-04: Implement independent part editing/regeneration/versioning.
- P6-05: Implement copy and approval controls.
Deliverables: versioned prompts linked to Master Script version.
Exit criteria: prompt count equals script part count; one-part regeneration does not modify other parts.

### Phase 7 — Thumbnail Studio
**Objective:** Generate exactly two distinct thumbnail prompt concepts.
Tasks:
- P7-01: Implement ratio preference flow and missing-ratio question.
- P7-02: Implement text preference rules.
- P7-03: Generate Option A and B with distinct visual approaches.
- P7-04: Implement factuality and composition validation.
- P7-05: Implement independent option editing/regeneration/history/approval.
- P7-06: If direct image generation is approved, separately evaluate provider and image-output workflow.
Deliverables: two versioned prompts with ratio/text compliance.
Exit criteria: exactly two distinct options and isolated regeneration verified.

### Phase 8 — SEO Studio
**Objective:** Generate complete source-aware YouTube metadata.
Tasks:
- P8-01: Implement source + approved script analysis.
- P8-02: Implement source/script accuracy check and discrepancy warnings.
- P8-03: Implement primary and alternative titles.
- P8-04: Implement description and workflow-accurate AI disclaimer.
- P8-05: Implement grouped keywords and honest live-data labeling.
- P8-06: Implement tags generation and exact <=500-character validation.
- P8-07: Implement hashtags and final upload checklist.
- P8-08: Implement section-level edit/regenerate/version/approve.
Deliverables: complete ten-section SEO package.
Exit criteria: required section order and exact tag count pass automated tests.

### Phase 9 — ULTRA CORE command orchestration
**Objective:** Provide a consistent central command and workflow view.
Tasks:
- P9-01: Define supported command intents and typed action schema.
- P9-02: Implement active-project context and prerequisite checks.
- P9-03: Implement command routing to known services only.
- P9-04: Add confirmation for destructive/expensive operations.
- P9-05: Add status timeline, core inspector, activity history, error explanations.
- P9-06: Add idle/analyzing/generating/approval/success/error motion states.
Deliverables: command center that coordinates existing services without bypassing rules.
Exit criteria: invalid commands are safely rejected; actions are scoped, traceable, and displayed accurately.

### Phase 10 — Integration, quality, and release readiness
**Objective:** Verify complete end-to-end workflow.
Tasks:
- P10-01: Run unit, integration, UI, and end-to-end tests as appropriate.
- P10-02: Test project isolation and version provenance.
- P10-03: Test all generation failure/retry/recovery paths.
- P10-04: Test keyboard, contrast, reduced motion, and responsive layout.
- P10-05: Review security, upload/URL risks, and secret handling.
- P10-06: Verify documentation against actual commands/code.
- P10-07: Prepare release checklist, backup/rollback, and known limitations.
Deliverables: test evidence and release readiness report.
Exit criteria: critical acceptance criteria pass and unresolved issues are documented.

## 3. Task tracking format

For every task, track:
- Task ID.
- Description.
- Related FR/Feature IDs.
- Files/modules affected.
- Dependencies.
- Status: Planned / In Progress / Implemented / Testing / Verified / Blocked / Deferred.
- Verification command or procedure.
- Result/evidence.
- Notes/blockers.

## 4. Execution rules

1. Inspect existing code and configuration before modifying files.
2. Work on one clearly scoped task at a time.
3. Do not perform broad rewrites when a targeted change is enough.
4. Preserve working functionality.
5. Use tests and actual runtime checks; do not rely on visual appearance alone.
6. If a test fails, record the actual failure and diagnose it before marking complete.
7. Do not claim API or external integration success without a real test.
8. Keep secrets out of code, documentation, logs, and screenshots.
9. Update related docs after meaningful architecture or behaviour changes.
10. Stop and ask when a product decision materially affects architecture or user-visible behaviour.

## 5. Testing strategy

- **Unit:** pure business rules, duration/part calculation, tag character count, stale dependency logic, validators, state transitions.
- **Integration:** persistence, version lineage, project scoping, provider adapters, retry behaviour.
- **UI/component:** field validation, loading/empty/error/approval states, edit/regenerate/copy actions.
- **End-to-end:** create project → source → analysis → Reference Script → Master Script approval → video/thumbnail/SEO generation and approvals.
- **Regression:** ensure regenerating one output does not modify unrelated outputs.
- **Security:** project authorization, upload validation, safe URL retrieval, secret handling, untrusted content handling.
- **Accessibility:** keyboard flow, visible focus, contrast, labels, reduced motion.
- **Performance:** quantitative targets TBD until workload is defined.

## 6. Risks and mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Script Writer skill incompatible with chosen runtime | Reference Script blocked | Inspect ZIP first; build adapter or document blocker |
| Provider lacks exact narration/voice control | Audio mismatch | Preserve narration as separate editable voiceover asset; disclose limitation |
| Source claims conflict or are stale | Misleading outputs | Flag claims and require verification/review |
| Unclear 16:9 part arithmetic | Inconsistent duration | Resolve before strict validation |
| API rate limits/cost | Slow or failed generation | Bounded retries, status, quotas, budget policy TBD |
| Project memory leaks across projects | Severe correctness/privacy issue | Project-scoped queries and isolation tests |
| Regeneration overwrites approved output | Lost work | Immutable versions and explicit approval |
| Live search data unavailable | False SEO claims | Label suggestions as unverified |
| Excessive animation hurts usability | Reduced readability/accessibility | Subtle motion, reduced-motion mode, prioritize task clarity |

## 7. Release checklist

- [ ] Build succeeds with documented command.
- [ ] Critical tests pass.
- [ ] Project isolation verified.
- [ ] Version/provenance links verified.
- [ ] Approval gates verified.
- [ ] Error recovery preserves last valid outputs.
- [ ] Tag character count verified.
- [ ] Source/claim limitations visible.
- [ ] Accessibility and reduced-motion checks completed.
- [ ] Secrets and provider configuration checked.
- [ ] Known limitations documented.
- [ ] Backup/restore and rollback procedures documented where applicable.
- [ ] PROJECT_MEMORY.md updated with verification evidence.
