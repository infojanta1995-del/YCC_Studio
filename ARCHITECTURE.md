# YCC Studio — Architecture

**Status:** Conceptual architecture; stack and implementation TBD  
**Last updated:** 2026-10-25

## 1. Architecture overview

YCC Studio should be designed as a project-scoped workflow application with explicit services and versioned records. The UI presents a central ULTRA CORE command center, but orchestration must be implemented through deterministic workflow/state logic and bounded service calls.

### Logical layers
1. **Presentation layer:** HUD shell, project screens, editors, cards, status panels, command console.
2. **Application/workflow layer:** project lifecycle, state transitions, approval gates, command routing, generation orchestration.
3. **Domain layer:** projects, settings, sources, analyses, scripts, prompts, SEO artifacts, versions, validation results.
4. **Integration layer:** AI model provider(s), source/transcript retrieval, optional live search-interest provider, optional file storage.
5. **Persistence layer:** database and file/object storage, chosen later.
6. **Observability layer:** structured errors, audit/workflow events, metrics, safe logs.

This is a logical design, not a decision that all layers must be separate deployed services. Deployment topology is TBD.

## 2. Technology decisions

No programming language, framework, database, runtime, provider, or hosting platform has been approved. Do not infer these from the UI concept.

For every future choice, record an ADR with context, alternatives, chosen option, rationale, consequences, and verification needs.

### Decision criteria
- Project isolation and transactional consistency.
- Reliable versioning and relationship queries.
- Secure handling of provider credentials.
- Support for asynchronous long-running generation and retry.
- Testability of workflow and validation rules.
- Accessible, responsive, animated UI.
- Cost, rate limits, provider availability, and exportability.
- Ability to inspect and integrate the supplied Script Writer skill after its contents are known.

## 3. Logical components

### 3.1 Project Manager
- **Responsibility:** Create, list, search, filter, open, rename, duplicate, archive, delete.
- **Inputs:** project metadata and requested action.
- **Outputs:** project record and workflow state.
- **Dependencies:** persistence, authorization if applicable.
- **Errors:** validation, conflict, not found, permission denied, storage failure.
- **Rule:** duplicate creates a new project ID and independent subsequent history; define exactly which source/settings/artifacts are copied before implementation.

### 3.2 Project History Manager
- Maintains stage, timestamps, current status, pending approvals, recent actions, and navigation back to saved work.
- Must not treat a UI cache as the authoritative project history.
- Delete and destructive archive operations require explicit confirmation according to approved retention rules.

### 3.3 Project Memory Manager
- Stores project-scoped factual notes and references to exact versions of source, analysis, scripts, and outputs.
- Provides relevant context to generation modules only from the active project.
- Must distinguish facts, claims, predictions, opinions, and unresolved verification questions.
- Must not silently mix content from unrelated projects.
- Failed refresh preserves the last valid memory/analysis and marks it stale if appropriate.

### 3.4 Master Details / Settings Service
- Stores format, duration, voice, language, and optional project-specific settings.
- Tracks settings versions or snapshots for reproducibility.
- Changes invalidate dependent outputs according to explicit dependency rules.

### 3.5 Source Manager
- Saves pasted text, title, source type, URL, document metadata, and source versions.
- Applies URL-specific resolution/transcript logic only to URL sources.
- Does not fabricate missing transcripts.
- File format, size, and supported URL policies are TBD.

### 3.6 Shared Source Analysis Service
- Produces structured story analysis and verification needs.
- Saves analysis with source-version references and freshness status.
- Supports reuse when dependencies are unchanged.
- External fact checking is a distinct capability; do not claim current verification unless performed.

### 3.7 Reference Script Adapter / Skill Runner
- Exists only after the supplied ZIP/code has been inspected.
- Responsibilities: verify archive contents, entry points, dependencies, runtime, licensing, input/output contract, security risks, and deterministic execution requirements.
- Adapter normalizes the skill’s output into a Reference Script record.
- Must report missing dependencies, unsupported runtime, malformed output, and timeouts.
- Do not execute unknown code in a privileged environment before inspection.

### 3.8 Master Script Generator
- Inputs: source version(s), valid analysis version, Reference Script version, settings snapshot.
- Output: versioned draft with parts, exact narration per part, validation results, and provenance.
- Approval makes a specific version canonical; later drafts do not replace it until approved.

### 3.9 Video Prompt Generator
- Input: approved Master Script version and relevant settings/analysis.
- Output: exactly one prompt per actual script part.
- Validates part count, narration preservation, duration, aspect ratio, visual relevance, no-on-screen-text rule, factuality, and continuity.
- Supports independent per-part editing/regeneration/versioning.

### 3.10 Thumbnail Generator
- Input: approved Master Script and ratio/text preferences.
- Output: Option A and Option B prompt records.
- Ensures different visual strategies while preserving story facts.
- Ratio is asked when not supplied; do not silently infer it from video format.
- Direct image generation is an optional unresolved integration.

### 3.11 SEO Generator
- Inputs: original source version(s), approved Master Script version, settings, optional verified research results, production workflow facts.
- Output: source analysis, script analysis, accuracy check, title(s), description, grouped keywords, tags and count, hashtags, checklist.
- Must label live search-interest data honestly and avoid unsupported performance promises.

### 3.12 Validation Service
- Reusable validation rules for required inputs, source/script alignment, script part count, narration text, durations, ratio, tags character count, required sections, approval status, and stale dependencies.
- Return structured findings with severity, field/path, explanation, and possible next action.
- Separate blocking errors from warnings.

### 3.13 Workflow State Manager
- Enforces allowed transitions and prerequisites.
- Example states: Draft, Ready for Analysis, Analyzed, Reference Script Ready, Master Script Draft, Awaiting Master Approval, Master Approved, Video Prompts Draft, Awaiting Video Approval, Thumbnail Draft, Awaiting Thumbnail Approval, SEO Draft, Awaiting SEO Approval, Complete, Blocked, Failed, Outdated.
- Exact state model is TBD and should be implemented as explicit transitions, not scattered UI conditions.

### 3.14 ULTRA CORE Command Router
- Interprets a user command into a bounded intent/action, verifies active project, permissions, required inputs, and workflow preconditions, then calls a known application service.
- Should show planned action before destructive or expensive operations.
- Must ask for missing essential information; never guess critical settings.
- Must not bypass approvals, fabricate capabilities, or silently modify approved content.
- Logs a safe workflow event with action, project, outcome, and references; do not log secrets.

### 3.15 Output Version Manager
- Stores immutable output versions or equivalent change history.
- Links each version to source, analysis, settings, and upstream output versions.
- Supports compare, restore-as-new-version, and stale status.
- Exact storage strategy is TBD.

## 4. Data flow

### Main pipeline
1. User creates a project.
2. Project Manager saves project and default workflow state.
3. User confirms Master Details.
4. Source Manager saves source versions.
5. Source Analysis Service creates an analysis linked to exact source versions.
6. Reference Script adapter produces a version after skill inspection/integration.
7. Master Script Generator creates a draft linked to source, analysis, reference script, and settings.
8. Validation runs; user reviews and approves a specific Master Script version.
9. Video Prompt, Thumbnail, and SEO services generate drafts from the approved script (SEO also reads source).
10. Validators check outputs; UI displays warnings/errors.
11. User edits and approves output versions.
12. Version Manager stores provenance; Workflow Manager advances only when required approvals exist.

### Regeneration
- User selects one output/section/part and requests regeneration.
- The system records the input versions and scope.
- Only the selected output is regenerated.
- Prior versions remain available.
- Related outputs are marked outdated only when their actual dependencies changed.

### Source/settings change
- Save a new source or settings version.
- Compute affected downstream dependencies.
- Mark dependent analyses/outputs Outdated.
- Keep old outputs readable and traceable.
- Offer explicit refresh/regeneration; do not silently replace approved versions.

## 5. Suggested data architecture

These are logical entities, not a final schema.

- `projects`: project identity, name, status, created/updated timestamps.
- `project_settings`: versioned format, duration, voice, language, optional settings.
- `source_documents`: project ID, source type, title, URL/text/file metadata, source version.
- `source_analyses`: project ID, source version references, structured analysis, validity status.
- `reference_scripts`: project ID, source/analysis/settings references, content, version, skill provenance.
- `master_scripts`: project ID, source/analysis/reference/settings references, parts, narration, version, approval state.
- `project_memory`: project-scoped facts/claims/entities/questions and references to supporting versions.
- `video_prompts`: project ID, master script version, part ID/index, prompt text, version, status.
- `thumbnail_outputs`: project ID, master script version, option A/B, ratio/text settings, prompt text, version, status.
- `seo_outputs`: project ID, source and master script versions, section outputs, tag character count, verification state, version, status.
- `workflow_events`: action, stage, status, timestamps, safe metadata, referenced versions.
- `validation_results`: object/version, rule, severity, outcome, message, timestamp.
- `integration_runs`: provider/skill identifier, input references, status, timing, safe error details; do not store secrets.

### Data rules
- Every project-owned record has a project scope.
- Use stable IDs and explicit version relationships.
- Store generated output provenance.
- Prefer structured fields for part indices, approval states, tag counts, and validation results over parsing display text.
- Define deletion, retention, backups, migrations, and export semantics before production.
- Avoid duplicating large source text into every downstream record unless needed for an explicit snapshot/audit requirement.

## 6. API and integration design

Endpoint names, request schemas, authentication, and response contracts are TBD until the stack is chosen. Before implementation, define contracts for:
- Project CRUD and history.
- Settings read/update.
- Source create/update/delete and analysis.
- Reference Script run/status.
- Master Script generate/edit/approve/version compare.
- Video prompts generate/update/approve/regenerate per part.
- Thumbnail prompts generate/update/approve/regenerate per option.
- SEO package generate/update/approve/regenerate per section.
- ULTRA CORE command submission and command status.
- Workflow events and validation results.

Every generation request should include or resolve:
- `project_id`
- exact upstream version IDs
- settings snapshot/version
- requested operation scope
- idempotency/retry strategy where appropriate
- cancellation/timeout behaviour
- structured result or structured error

Do not design a generic endpoint that allows arbitrary project IDs without authorization checks.

## 7. Security and privacy

- Store provider credentials only in server-side secret storage/environment configuration.
- Enforce authorization and project scope in the service/data layer, not only the UI.
- Validate uploads and external URLs; guard against unsafe file types, excessive size, malicious content, and server-side request forgery if URL fetching exists.
- Treat retrieved source text and model-generated text as untrusted data, not executable instructions.
- Restrict skill execution; inspect archive and dependencies before running.
- Do not log secrets or unnecessary personal information.
- Define retention and permanent deletion semantics before production.
- Use rate limits and cost controls for expensive generation.
- Confirm external provider data handling and licensing before sending user content.

## 8. Error handling and observability

Error categories:
- Validation failure.
- Missing prerequisite/approval.
- Stale upstream version.
- Provider timeout/rate limit/unavailable.
- Malformed model output.
- Source retrieval/transcript unavailable.
- Permission failure.
- Persistence failure.
- Skill runtime/dependency failure.
- Cancellation.

For each failure, display:
- What failed.
- Which step/output was affected.
- Whether saved data was preserved.
- Whether retry is safe.
- Available action: edit input, retry, refresh, view details, or cancel.

Store safe structured events and correlation IDs. Redact credentials and avoid logging full sensitive content by default.

## 9. Deployment

Development, staging, production, hosting, build, CI, rollback, and backup designs are TBD. Before release, define:
- Configuration separation.
- Secret management.
- Database migration and rollback.
- Background job handling for long-running generation.
- Provider timeout/rate-limit policy.
- Health checks and logs.
- Backup/restore and data export.
- Versioned deployment and rollback procedure.

## 10. Architecture decision records

Create `docs/adr/ADR-NNN-title.md` when making a major decision. Record context, decision, alternatives, rationale, trade-offs, date, status, and affected requirements.

Initial ADRs to write:
- ADR-001: Technology stack.
- ADR-002: Database and versioning strategy.
- ADR-003: AI provider and generation orchestration.
- ADR-004: Script Writer skill execution model.
- ADR-005: Source/transcript retrieval.
- ADR-006: Live keyword research provider.
- ADR-007: Direct thumbnail image generation inclusion.
- ADR-008: 16:9 duration/segment arithmetic.
- ADR-009: Authentication and project access model.
