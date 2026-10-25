# YCC Studio — Project Requirements

**Status:** Planning specification; implementation not started  
**Document version:** 0.1  
**Last updated:** 2026-10-25  
**Owner:** TBD  
**Release target:** TBD

## 1. Problem statement

Creators need a repeatable way to turn changing source stories into coherent YouTube production assets. Without project isolation, versioning, structured analysis, approval gates, and source-to-output traceability, the workflow can lose facts, mix unrelated stories, overwrite approved outputs, or generate metadata that does not match the finished video.

YCC Studio should centralize this workflow while keeping the user in control of consequential decisions and approvals.

## 2. Objectives

1. Let the user create and reopen independent story projects.
2. Preserve each project's source, settings, analysis, scripts, generated outputs, versions, approvals, and history.
3. Reuse valid saved analysis rather than needlessly repeating it.
4. Make the approved Master Script the canonical input for downstream production modules.
5. Generate one video prompt per actual script part without silently merging, omitting, reordering, or rewriting narration.
6. Generate exactly two distinct thumbnail prompt concepts per generation request.
7. Generate an SEO package grounded in both the original source and final approved script.
8. Keep all output revisions traceable to the input versions used to create them.
9. Make loading, errors, warnings, approval requirements, and outdated states understandable.
10. Provide a coherent dark HUD interface with a central ULTRA CORE coordination concept.

## 3. Target users and roles

### Primary role: Project operator / creator
- Creates and manages projects.
- Supplies source content and project settings.
- Reviews, edits, regenerates, copies, and approves outputs.
- Decides when a generated artifact is ready to feed the next stage.

### Other roles
- Additional roles such as administrator, collaborator, reviewer, or read-only user: **TBD**.
- Authentication and permission model: **TBD**.
- Until multi-user access is specified, do not assume public sharing or cross-user access.

## 4. Scope

### In scope for the planned product
- Project and project history management.
- Per-project settings and memory.
- Source input and source analysis.
- Reference Script skill integration after inspection.
- Master Script generation and approval.
- Video prompt generation.
- Thumbnail prompt generation.
- SEO package generation.
- ULTRA CORE command/orchestration interface.
- Validation, version history, approval gates, error recovery, and UI feedback.
- Documentation and tests for each implemented feature.

### Out of scope or not yet approved
- Automatically publishing videos to YouTube.
- Automatically booking or uploading assets to external services.
- Guaranteed access to Google Flow internals or APIs.
- Guaranteed search ranking, virality, CTR, or impressions.
- Treating generated reconstructions as verified authentic footage.
- Unattended autonomous approval of content.
- Direct final thumbnail image generation from the app (prompt generation is confirmed; direct image creation remains TBD).
- Any technology/provider not explicitly selected in an architecture decision.

## 5. Functional requirements

### Project lifecycle
**FR-001 — Create project**
- Input: project name (optional if the UI can create a default name), source type (optional), initial settings as available.
- Processing: create a unique project identifier and initialize workflow state.
- Output: project detail view and saved project record.
- Failure: show actionable error; do not create duplicate partial records.
- Acceptance: created project can be retrieved by its ID and appears in project history.

**FR-002 — Independent project isolation**
- Each story defaults to a separate project unless the user explicitly adds a related source to an existing project.
- A project’s memory and outputs must never be mixed with another project.
- Acceptance: tests demonstrate retrieval and mutation scoped to the correct project.

**FR-003 — Project history**
- List, search, filter, open, rename, duplicate, archive, and delete with confirmation.
- Show stage, status, last updated, and pending approvals when available.
- Acceptance: reopening restores saved workflow state; duplicate gets its own ID and independent subsequent edits.

**FR-004 — Project-specific persistent memory**
- Store references to sources, analysis versions, factual entities/claims, unresolved verification items, scripts, output versions, statuses, and relationships.
- Reuse analysis only when its source dependencies remain valid.
- Acceptance: reopening project restores saved state; source changes cause dependent analysis/output to be marked stale or outdated.

### Master Details
**FR-005 — Video format**
- User selects 9:16 vertical Shorts/Reels or 16:9 landscape long-form.
- Format is stored at project level and is available to dependent modules.
- Acceptance: downstream output records reference the settings version used.

**FR-006 — Duration rules**
- 9:16 options: 24 seconds (3 × 8-second parts), 32 seconds (4), 40 seconds (5), 48 seconds (6).
- 16:9 minimum total duration: 3 minutes; longer duration allowed.
- Unresolved: whether a 180-second long-form target may use 22 × 8 seconds plus a 4-second final part, or should default to 24 × 8 seconds = 192 seconds. Must be decided before implementing strict segment validation.
- Acceptance: UI and generator use one approved rule; conflicts produce explicit warnings.

**FR-007 — Voice and language**
- Voiceover gender selection: Male/Female.
- Language selection: Hindi/English.
- Selected language controls Master Script language. The actual available voice/accent capabilities depend on the selected provider and must be verified.
- Acceptance: selected values are stored and passed to generators.

**FR-008 — Settings updates**
- Allow authorized setting edits.
- Changes to relevant settings mark dependent outputs as outdated; do not silently mutate previously approved artifacts.
- Acceptance: dependency-based stale state is visible and history is preserved.

### Source and analysis
**FR-009 — Source Manager**
- Accept pasted story/article text, editable source title/content, supported documents, and supported YouTube URL.
- Add, save, edit, and remove sources under the active project.
- URL-specific retrieval must not be applied to plain pasted text.
- Acceptance: saved source text and metadata can be reopened.

**FR-010 — YouTube source retrieval**
- Identify and show the resolved source to the user for confirmation when feasible.
- Attempt transcript retrieval only through a supported/authorized capability.
- Never fabricate a transcript if unavailable; report that retrieval failed or transcript is unavailable.
- Provider and supported formats: TBD.
- Acceptance: unavailable transcript produces a clear state and preserves URL/source record.

**FR-011 — Source analysis**
- Extract topic, core story, entities, locations, dates, events, claims, sequence, facts, audience relevance, hook/angle, contradictions, and verification needs.
- Save versioned analysis in project memory.
- Acceptance: output is saved and linked to the exact source version(s).

**FR-012 — Analysis validity**
- Reuse valid saved analysis.
- If source content changes, mark analysis stale and offer refresh.
- If refresh fails, preserve the last valid analysis and disclose that it is stale.
- Acceptance: failed refresh does not destroy prior analysis.

### Reference Script and Master Script
**FR-013 — Reference Script**
- Use the supplied Script Writer skill/ZIP only after inspecting files, runtime, dependencies, inputs, outputs, and license/operational constraints.
- Display output in a Reference Script tab; permit review and versioning.
- It is a reference, not the canonical downstream script.
- Acceptance: output records link to source/analysis/settings versions and integration errors are visible.

**FR-014 — Master Script**
- Inputs: original source, valid source analysis, Reference Script, confirmed Master Details.
- Preserve relevant facts; follow selected language, target duration, narrative approach, and meaningful part structure.
- Provide edit, regenerate, compare versions, save draft, and approve actions.
- Acceptance: approved version becomes the canonical input for downstream modules.

**FR-015 — Script validation**
- Check source alignment, names/dates/locations/claims, language, part count, duration/narration timing, and unresolved conflicts.
- If narration does not fit target duration, warn and ask for user choice; do not silently shorten or rewrite.
- Acceptance: blocking issues prevent approval only when the rule is explicitly defined as blocking; otherwise display warning and require informed user action.

### Video Prompt Generator
**FR-016 — Analyze full script before generating**
- Inspect complete approved Master Script, identify actual parts, exact narration per part, topic/genre/story progression, visuals, emotion, continuity, and factual constraints.

**FR-017 — One prompt per actual part**
- Generate exactly one standalone video prompt for each actual script part.
- Never merge, omit, reorder, invent, or silently alter parts/narration.
- Default from prompt: Google Flow-targeted prompts, 9:16, 8 seconds, ultra-realistic cinematic style, unless authoritative project settings override.
- Acceptance: prompt count equals script part count; each prompt links to the exact Master Script version and part.

**FR-018 — Video prompt visual/audio specification**
- Include scene/action, shot/timing plan, camera, lighting, depth/composition, realism, exact original-language narration, voice gender/consistency, music, SFX/ambience, transitions, continuity, and negative constraints.
- Do not add captions, subtitles, on-screen text, headlines, labels, logos, or watermarks.
- Real-event reconstructions must not be presented as verified authentic footage.
- If the target video tool cannot reliably generate exact narration, preserve narration for a separate voiceover workflow.

**FR-019 — Video prompt controls**
- Edit, regenerate one part, copy, approve, view version history.
- Regenerating one prompt must not replace unrelated prompts.
- Acceptance: isolated regeneration creates a new version and preserves prior version.

### Thumbnail Generator
**FR-020 — Generate two concepts**
- Analyze the complete approved script and generate exactly two distinct image-generation prompts for the same story.
- Option A: high-impact, immediate-attention concept.
- Option B: distinct story-driven/cinematic curiosity concept.
- Do not force mystery or sensationalism when story does not support it.

**FR-021 — Thumbnail ratio**
- If an explicit thumbnail ratio is supplied, use it.
- Otherwise ask one direct question: 16:9 YouTube video or 9:16 Shorts/Reels.
- Do not assume thumbnail ratio equals video ratio unless established by user/project rules.
- Vertical design must be redesigned for central focus and UI-safe zones, not simply cropped from landscape.

**FR-022 — Thumbnail text preference**
- Support Yes / No / Let AI decide.
- No: explicitly prohibit text, letters, numbers, captions, logos, watermarks.
- Yes: include the exact approved headline in the prompt, in the requested/original language.
- Let AI decide: make a story-appropriate decision and disclose it.
- Acceptance: output obeys selected preference and ratio.

**FR-023 — Thumbnail factuality and controls**
- Do not invent casualties, destruction, official announcements, people, locations, statistics, or outcomes.
- Permit clearly illustrative reconstructions without implying authentic documentary footage.
- Edit, regenerate A or B independently, regenerate both, copy, approve, version history.
- Acceptance: regenerating one option does not overwrite the other.

### SEO Generator
**FR-024 — Analyze source and final script**
- Read both original source and final approved script.
- Metadata must reflect what the finished video actually communicates.
- Identify core facts, entities, dates, viewer intent, opening hook, key information, language, format, and story angle.

**FR-025 — Source/script accuracy check**
- Compare facts and claims; distinguish facts, forecasts, allegations, opinions, and speculation.
- Flag serious contradictions and potentially outdated time-sensitive claims before presenting metadata.
- Use current reliable sources only when web research is available and appropriate.
- Never claim Google Trends/search-volume data was checked unless it was.

**FR-026 — SEO title**
- One primary hook title based on script opening; up to two optional alternatives only if meaningfully different.
- Use script language unless user specifies otherwise.
- Avoid unsupported claims, keyword stuffing, unnecessary caps, and misleading clickbait.

**FR-027 — SEO description**
- Ready-to-paste description: opening lines, summary, useful key information, natural keywords, supported source context, appropriate production disclaimer, and small relevant hashtags.
- Do not invent source names, links, dates, or quotations.
- AI disclaimer must match actual production workflow.

**FR-028 — Keyword research**
- Groups: primary, secondary, long-tail, entity/location, language variations, timely terms when accurate.
- Clearly label inferred suggestions versus live verified search-interest results.
- Never claim volume/trending status without reliable evidence.

**FR-029 — YouTube tags**
- Comma-separated copy-ready block.
- Maximum 500 characters including commas and spaces.
- Calculate exact character count and display it.
- Avoid unrelated trending terms and redundant variations.

**FR-030 — Hashtags and upload checklist**
- Provide recommended hashtags separately and keep description hashtag selection small.
- Checklist verifies title/script match, description accuracy, keyword naturalness, tag limit, hashtags, time-sensitive fact checking, and disclaimer accuracy.

**FR-031 — SEO editing and versions**
- Allow editing/regeneration/copy/version history and approval.
- Regenerating one SEO section must not silently replace unrelated sections.
- Full-package regeneration is a separate explicit action.
- Acceptance: all outputs link to exact source and Master Script versions.

### ULTRA CORE and UX
**FR-032 — Central command/orchestration concept**
- Provide a command input, project/workflow context, current task status, and suggested next valid action.
- Route commands to explicit application services and permission-checked actions.
- Do not imply unlimited autonomous intelligence or unsupported capabilities.
- Acceptance: commands are validated, scoped to active project, and return understandable success/error results.

**FR-033 — Approval gates**
- Master Script approval gates video prompts, thumbnail prompts, and SEO generation.
- Additional per-module approvals gate the next workflow stage as specified in the workflow configuration.
- User may edit/regenerate before approval.
- Acceptance: unauthorized/out-of-order generation is blocked or clearly handled.

**FR-034 — Animation and states**
- Idle: subtle ring motion/pulse.
- Analyzing: radial scan/nodes active.
- Generating: progress trail and stage status.
- Needs approval: amber warning and clear action.
- Success: teal completion state.
- Error: visible issue, recovery action, and no lost saved data.
- Respect reduced-motion preferences.

## 6. Non-functional requirements

- **Security:** secrets are not exposed in client code, logs, or documentation; authorization rules are enforced server-side if a server exists.
- **Data isolation:** every project-owned record must be scoped by project ID and access policy.
- **Reliability:** failures must preserve last valid saved outputs; retries should not duplicate unintended records.
- **Traceability:** generated outputs retain source version, analysis version, settings version, and upstream output version references where relevant.
- **Usability:** loading, empty, success, warning, error, disabled, stale, and approval-required states must be designed.
- **Accessibility:** keyboard access, visible focus, adequate contrast, labels, and reduced motion support.
- **Performance targets:** TBD until workload and architecture are chosen.
- **Browser/platform support:** TBD.
- **Backup/retention:** TBD.
- **Logging/monitoring:** TBD; never log secrets or unnecessary sensitive source content.
- **Localization:** initial UI language and multilingual support beyond Hindi/English: TBD.

## 7. Business rules

1. New story defaults to a new independent project.
2. Cross-project memory reuse is prohibited unless the user explicitly requests a supported copy/reference operation.
3. Approved Master Script is the canonical source for downstream generation.
4. All generated outputs are versioned and tied to exact upstream versions.
5. Changing an input marks dependent outputs outdated; it does not silently rewrite them.
6. Regenerating one output must not modify unrelated outputs.
7. Failed refresh must preserve the previous valid record.
8. Missing source evidence must not be replaced with invented facts.
9. User approval is required at specified workflow gates.
10. `Implemented` and `Verified` are different statuses.
11. Actual API/tool capabilities must be verified before being presented as available.
12. Do not promise SEO rankings, virality, or guaranteed model output.

## 8. Constraints, assumptions, and open questions

### Confirmed from planning
- Product name: YCC Studio.
- Central command/intelligence concept: ULTRA CORE.
- Format options: 9:16 and 16:9.
- 9:16 duration options: 24, 32, 40, 48 seconds, using 8-second segments.
- Voiceover choices: Male/Female.
- Script language choices: Hindi/English.
- Three generation modules: Video Prompt, Thumbnail, SEO.
- Video prompts: one per actual script part.
- Thumbnail prompts: exactly two distinct concepts.
- SEO tags: maximum 500 characters with count.
- Approval/versioning/project isolation are core requirements.

### Open / TBD
- Technology stack and deployment.
- Authentication and multi-user access.
- AI providers, model selection, budgets, and retry policy.
- Whether Google Flow is accessed manually via copied prompts or via an authorized integration.
- How the supplied Script Writer ZIP is executed.
- Source/document file formats and maximum sizes.
- Transcript retrieval provider and policy.
- Live search-interest provider and geographic settings.
- Whether direct image generation is included in app scope.
- 16:9 segment/duration arithmetic rule.
- Exact data retention, backup, export, and delete semantics.
- UI language and responsive/mobile requirements.
- Accessibility conformance target.
- Quantitative performance and reliability targets.

## 9. Success criteria

The release is not successful merely because screens render. At minimum:
- A project can be created, saved, reopened, duplicated, and isolated.
- Sources and settings can be saved and versioned.
- Analysis is reused when valid and invalidated when inputs change.
- Approved Master Script is required before downstream generation.
- Video prompt count and narration match actual script parts.
- Thumbnail generation returns exactly two distinct concepts.
- SEO output contains all required sections and an exact tag count under 500 characters.
- Errors preserve prior valid versions.
- Users can edit, regenerate, copy, approve, and inspect versions.
- Critical flows pass automated or documented manual tests.
- No integration capability is marked verified without an actual verification result.

## 10. Traceability

Use the IDs in this document as requirement identifiers. Each feature in FEATURES.md should reference relevant FR IDs; each implementation task in IMPLEMENTATION_PLAN.md should reference feature/FR IDs; tests should reference the same IDs. Maintain a traceability table when implementation begins.
