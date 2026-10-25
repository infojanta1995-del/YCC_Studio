# YCC Studio — Feature Specifications

**Status:** Planned; not implemented  
**Last updated:** 2026-10-25

This file defines feature-level behaviour. Requirement IDs referenced below are defined in PROJECT_REQUIREMENTS.md. Feature status must not be promoted to Implemented or Verified without evidence.

## 1. Feature inventory

| Feature ID | Feature | Priority | Status | Dependencies |
|---|---|---|---|---|
| F-001 | Project Manager | Must | Planned | Persistence |
| F-002 | Project History Manager | Must | Planned | F-001 |
| F-003 | Master Details | Must | Planned | F-001 |
| F-004 | Source Manager | Must | Planned | F-001 |
| F-005 | Source Analysis + Project Memory | Must | Planned | F-004 |
| F-006 | Reference Script integration | Must | Blocked pending ZIP inspection | F-005 |
| F-007 | Master Script Studio | Must | Planned | F-003, F-005, F-006 |
| F-008 | Video Prompt Studio | Must | Planned | Approved F-007 |
| F-009 | Thumbnail Studio | Must | Planned | Approved F-007 |
| F-010 | SEO Studio | Must | Planned | F-004, approved F-007 |
| F-011 | ULTRA CORE command/orchestration | Must | Planned | Workflow services |
| F-012 | Validation + version manager | Must | Planned | All output modules |
| F-013 | HUD design system and motion | Must | Planned | UI stack TBD |
| F-014 | Error recovery and history | Must | Planned | Persistence/workflow |
| F-015 | Export/copy controls | Should | Planned | Output modules |
| F-016 | Authentication/multi-user access | TBD | TBD | Architecture decision |

## 2. Shared feature conventions

### Shared UI states
Every data-driven screen should define:
- Initial/empty state.
- Loading/generating state with progress where available.
- Success state.
- Warning state.
- Blocking error state.
- Disabled state with explanation.
- Stale/outdated state.
- Awaiting approval state.
- Cancelled state when cancellation is supported.

### Shared generation behaviour
- Show which project and input versions will be used.
- Validate required inputs before starting.
- Display progress/status without inventing precise completion estimates.
- Preserve previous successful versions if a generation fails.
- Save a new version for successful regeneration.
- Do not mutate unrelated outputs.
- Display provider/integration limitations honestly.
- Provide retry only when the retry behaviour is safe.
- Store provenance and validation results.

## F-001 — Project Manager

**Related requirements:** FR-001, FR-002, FR-003

### Purpose
Create and maintain isolated workspaces for stories.

### Entry points
- New Project action in command center.
- Project history/sidebar.
- ULTRA CORE command such as “Create a new project” after command interpretation and confirmation of required details.

### Inputs
- Project name: optional or generated default; exact default naming rule TBD.
- Optional source/content type and initial metadata.
- Format/settings can be requested now or in the next Master Details step.

### Workflow
1. User selects New Project.
2. UI opens a short setup view.
3. Validate entered fields.
4. Create project with unique ID and initial workflow state.
5. Open the new project and show next required action.

### Outputs
- Project detail page.
- History entry.
- Creation event.

### Actions
- Open, rename, duplicate, archive, delete.
- Delete requires confirmation and must follow the approved retention/soft-delete policy.

### Edge cases
- Duplicate names: allowed or blocked TBD; IDs must remain unique.
- Persistence failure: show error and avoid misleading success state.
- User double-click/duplicate request: prevent accidental duplicate project creation where possible.
- Empty project: show next-step guidance, not an error.

### Acceptance criteria
- Project survives reload/reopen.
- Project ID is stable.
- New project does not inherit unrelated project memory.
- Duplicate projects become independent after duplication.
- Destructive actions require explicit confirmation.

## F-002 — Project History Manager

**Related requirements:** FR-003, FR-004

### Purpose
Let users find, reopen, and resume previous projects.

### UI
- Search field.
- Status/stage filters.
- Project list/cards with name, stage, last updated, status, pending approvals.
- Actions menu.
- Empty state with New Project action.

### Behaviour
- Search should match supported project fields; exact fields TBD.
- Opening a project restores its persisted stage and versions.
- Rename updates display metadata, not project identity.
- Archive hides from default active view but preserves data according to retention rules.
- Delete semantics must be defined before production.
- Duplicate must explicitly define whether it copies sources/settings/outputs; it must not share mutable memory records.

### Acceptance criteria
- Search/filter results are project-scoped.
- Reopening restores exact saved state.
- Archived projects remain retrievable through archive filters if policy permits.

## F-003 — Master Details

**Related requirements:** FR-005–FR-008

### Purpose
Set authoritative production settings for a project.

### Required controls
- Video format: 9:16 Vertical / 16:9 Landscape.
- Voiceover gender: Male / Female.
- Script language: Hindi / English.
- Duration: format-dependent.
- Optional project name, source type, story/content type, script style, source verification status.

### Format rules
**9:16**
- 24 seconds = 3 parts.
- 32 seconds = 4 parts.
- 40 seconds = 5 parts.
- 48 seconds = 6 parts.
- Each target segment is 8 seconds.

**16:9**
- Minimum total duration is 180 seconds.
- Longer durations are allowed.
- Open decision: 180 seconds as 22 × 8 seconds + 4-second final part, or 192 seconds as 24 × 8 seconds. UI must not conceal this unresolved rule.

### Workflow
1. Ask video format when a new project is created.
2. Display relevant settings for selected format.
3. Validate required settings.
4. Save settings as a version/snapshot.
5. Make settings available to analysis/script/generation modules.
6. If settings change, mark dependent outputs outdated when affected.

### Validation
- Reject unsupported duration combinations only after the 16:9 rule is approved.
- Do not silently convert user selections.
- Explain which outputs may become outdated after a change.

### Acceptance criteria
- Settings persist and reopen.
- Downstream generation uses the exact saved settings version.
- Changing a setting never silently edits old output text.

## F-004 — Source Manager

**Related requirements:** FR-009, FR-010

### Supported input classes
- Pasted story/article text.
- Editable source title and text.
- Supported document upload (formats TBD).
- YouTube URL, only if supported retrieval integration exists.

### UI
- Source list.
- Source type badge.
- Title/content editor.
- URL field only for URL-based source.
- Add, save, edit, remove controls.
- Source version/history view.

### Workflow
1. User selects source type or pastes content.
2. UI shows relevant fields.
3. Validate required content/URL.
4. Save a source version.
5. If URL, resolve/show the identified source where feasible and attempt supported transcript retrieval.
6. Show transcript/source retrieval outcome.
7. Allow user to confirm/edit the material before analysis.

### Rules
- Do not run URL-specific retrieval on pasted text.
- Do not fabricate transcripts.
- If retrieval fails, preserve URL and display an actionable message.
- Adding a related source to an existing project requires explicit user choice.

### Acceptance criteria
- Source text can be reopened.
- Changes create a new version or equivalent history.
- Analysis references exact source versions.
- Unsupported uploads/URLs are rejected with explanation.

## F-005 — Source Analysis and Project Memory

**Related requirements:** FR-011, FR-012

### Analysis fields
- Main topic and central story.
- Genre/content type.
- People, organizations, locations, countries/states.
- Dates, numbers, measurements.
- Event chronology.
- Core facts and claims.
- Predictions, allegations, opinions, and unconfirmed information distinguished.
- Audience relevance and likely interest.
- Potential hook/story angle.
- Conflicting claims and verification needs.
- Source freshness concerns.
- Provenance to source version(s).

### Workflow
1. Validate that source material exists.
2. Check for a valid existing analysis.
3. Reuse valid analysis if dependencies are unchanged.
4. Otherwise run analysis.
5. Validate required fields and store result.
6. Update project memory references and status.
7. Show key findings and unresolved questions.

### Stale-state behaviour
- Source change marks analysis stale.
- Refresh success creates new analysis version.
- Refresh failure preserves prior valid analysis and shows that it is stale.
- Never represent an unverified claim as verified fact.

### Acceptance criteria
- Analysis includes source version references.
- Reopen/reload restores analysis.
- Repeated requests with unchanged source can reuse valid analysis according to cache policy.
- Failure does not delete prior successful analysis.

## F-006 — Reference Script Integration

**Related requirements:** FR-013

### Status
Blocked pending inspection of the user's Script Writer ZIP/code.

### Required inspection before implementation
- Archive file list and entry points.
- Language/runtime and dependency manifest.
- License/usage restrictions if included.
- Input format and expected output.
- Prompt/template files.
- External network/API needs.
- File-system assumptions.
- Security concerns and execution boundaries.
- Error and timeout behaviour.
- Whether direct execution or a wrapper/adapter is required.

### Workflow after approval
1. Prepare source, valid analysis, and settings.
2. Invoke the inspected skill through a controlled adapter.
3. Validate output shape.
4. Save Reference Script version and provenance.
5. Display in Reference Script tab.
6. User reviews/edits and may regenerate.
7. Master Script Generator uses the selected Reference Script version.

### Failure states
- Skill archive invalid.
- Dependency missing.
- Runtime unsupported.
- Timeout.
- Output malformed/empty.
- Provider/API unavailable.
- User cancellation.

### Acceptance criteria
- No uninspected unknown code is executed with unnecessary privileges.
- Failures do not erase previous Reference Script versions.
- Integration claims match actual test results.

## F-007 — Master Script Studio

**Related requirements:** FR-014, FR-015

### Inputs
- Original source version(s).
- Valid Source Analysis.
- Selected Reference Script version.
- Master Details/settings version.
- Project memory relevant to the active project.

### Output
- Editable Master Script draft.
- Clearly numbered actual parts.
- Exact narration text per part.
- Version metadata.
- Validation warnings and approval status.

### Generation rules
- Preserve relevant factual information.
- Use the selected script language.
- Follow approved style and target duration.
- Maintain meaningful part structure.
- Do not use the Reference Script as a replacement for source truth.
- Flag unresolved source/script contradictions.
- If narration exceeds target duration, warn; never silently shorten, rewrite, translate, or omit.

### UI layout
- Left: source facts, analysis, Reference Script versions.
- Center: main script editor with part navigation.
- Right: ULTRA CORE validation, timing, source alignment, status.
- Top: project/settings/version context.
- Bottom or toolbar: compare, edit, regenerate, save draft, approve.

### States
- Draft.
- Validation warning.
- Awaiting approval.
- Approved.
- Outdated.
- Generation failed.

### Acceptance criteria
- Draft and approved versions are distinguishable.
- Approval applies to a specific version.
- Downstream modules use only the selected approved version.
- Compare/version history is accessible.

## F-008 — Video Prompt Studio

**Related requirements:** FR-016–FR-019

### Purpose
Turn every actual part of the approved Master Script into a standalone, copy-ready video-generation prompt.

### Default prompt rules from supplied specification
- Target: Google Flow prompt style.
- Aspect ratio: 9:16 unless authoritative project setting/instruction says otherwise.
- Segment duration: 8 seconds by default.
- Ultra-realistic cinematic visuals.
- Detect narration language from script; preserve original language.
- Use selected Male/Female voice.
- No captions, subtitles, on-screen text, headlines, labels, lower thirds, logos, watermarks, or displayed dialogue.

Project settings are authoritative where they conflict with template defaults.

### Full-script analysis before generation
Identify topic/genre, actual part count, exact narration per part, action/subject, environment, emotional tone, strongest visual moment, viewer understanding, sound links, continuity, and final resolution.

### Prompt content
- Self-contained scene and action.
- Shot-by-shot timeline appropriate to narration and duration.
- Camera angle/movement, composition, focal hierarchy, foreground/midground/background.
- Realistic light, materials, weather, anatomy, motion, and physics.
- Exact narration, no rewrite/paraphrase/translation.
- Voice identity consistency and delivery guidance.
- Continuous music identity and part-specific ambience/SFX.
- Cross-part continuity details repeated in each prompt.
- Factual constraints and negative prompts.
- Aspect ratio and duration.
- No unsupported spectacle or false claims.

### UI
- Left: segment list, preview placeholder, part status.
- Center: editable prompt text.
- Right: ratio, duration, voice, continuity, narration and factuality checks.
- Actions: Generate All, Edit, Regenerate This Part, Copy, Approve, Version History.

### Validation and errors
- Actual prompt count must equal actual script part count.
- Exact narration must match the source part.
- Long narration triggers warning/action; no silent rewrite.
- Unsupported direct voice generation must be disclosed.
- Real-event illustrative reconstructions must not be labeled authentic footage.
- Failed regeneration preserves prior prompt.

### Acceptance criteria
- Each prompt is independently usable and contains necessary continuity facts.
- One-part regeneration leaves other parts unchanged.
- Output records link to exact approved Master Script version.

## F-009 — Thumbnail Studio

**Related requirements:** FR-020–FR-023

### Inputs
- Approved Master Script.
- Optional explicit thumbnail ratio.
- Text preference: Yes / No / Let AI decide.
- Optional visual style/restrictions.

### Required output
Exactly two distinct prompts:
- **Option A — High Impact:** immediate visual attention, dominant subject, clear hierarchy, truthful hook.
- **Option B — Cinematic Curiosity:** distinct composition, lighting/mood/story angle; no forced mystery.

Both concepts must match the same story and facts.

### Ratio behaviour
- Explicit ratio supplied: use it.
- Missing ratio: ask exactly one direct question, “Thumbnail 16:9 YouTube video ke liye chahiye ya 9:16 Shorts/Reels ke liye?”
- Vertical design must be composed for vertical layout, central focus, and UI-safe zones; not just cropped.

### Text behaviour
- Yes: include exact requested headline and language.
- No: explicitly prohibit readable text, letters, numbers, logos, captions, watermarks.
- Let AI decide: choose based on story and disclose the choice.
- Thumbnail text is distinct from video captions.

### Factuality
- Source/script are the source of truth.
- No invented casualties, destruction, announcements, people, locations, stats, or outcomes.
- Do not depict a prediction as a completed event.
- Avoid fake authentic-news-photo presentation.
- Supported illustrative reconstructions are acceptable when not misleading.

### UI
- Two side-by-side concept cards.
- Prompt editor for each.
- Ratio and text settings.
- Concept distinction explanation.
- Fact check and output status.
- Actions: Edit, Regenerate A, Regenerate B, Regenerate Both, Copy, Approve, History.

### Acceptance criteria
- Exactly two concepts returned.
- Meaningful visual strategy difference.
- Each option independently versioned.
- Regenerating one option does not alter the other.
- Ratio/text rules pass validation.

## F-010 — SEO Studio

**Related requirements:** FR-024–FR-031

### Inputs
- Original source story/version(s).
- Final approved Master Script/version.
- Video format if supplied.
- Script language.
- Target country/audience if supplied.
- Source links if supplied and user wants them included.
- Actual AI production workflow (script/visuals/voice/mixed).
- Preferred title style and additional requirements.
- Optional current research results from a real, available provider.

### Required output order
1. Source Story Analysis.
2. Final Script Analysis.
3. Source–Script Accuracy Check.
4. Primary Hook Title.
5. Optional Alternative Titles (up to two).
6. Complete YouTube Description.
7. Keyword Research.
8. YouTube Tags — Copy and Paste.
9. Hashtags.
10. Final Upload Checklist.

### SEO analysis rules
- Analyze complete source and complete final script.
- Metadata represents the finished video, not unmentioned source details.
- Preserve names, dates, locations, factual meaning.
- Distinguish confirmed facts from predictions, allegations, opinions, and speculation.
- Flag serious contradictions or stale/time-sensitive claims.
- Never claim a trend or search-volume check without actual evidence.

### Title
- One recommended title based primarily on the script opening hook.
- Preserve the hook's meaning.
- Natural main keyword.
- Use script language unless overridden.
- Avoid unsupported claims, stuffing, excessive capitals, and false urgency.

### Description
- First two lines: core story/hook and natural search phrase.
- Concise summary and key facts/questions where useful.
- Natural keyword variations.
- Source context only when supported.
- Include source links only when supplied and desired.
- AI disclaimer must match actual workflow.
- Sensitive topics may need a concise relevant caution.
- End with a small selection of relevant hashtags.
- Return one copy-ready block.

### Keywords
Groups: primary, secondary, long-tail, entity/location, language variations, timely terms when accurate.
- Label suggested phrases separately from verified live search-interest results.
- If live data unavailable, state: “Suggested Search Keywords — not verified by live search-volume data.”
- Do not invent Google Trends results.

### Tags
- Comma-separated tags.
- Maximum 500 characters including commas/spaces.
- Compute and display exact count.
- Prefer relevant entities, locations, story terms, and useful spelling variants.
- Do not add unrelated viral terms.

### Hashtags
- Small, accurate, readable set.
- Separate recommended hashtags from optional extras if needed.
- No unsupported locations or entities.

### UI
- Left or top navigation for the ten sections.
- Main title editor.
- Description editor.
- Grouped keyword panels.
- Tags block and live character count.
- Hashtag editor.
- Right ULTRA CORE validation panel: source/script alignment, research verification status, tag count, disclaimer accuracy.
- Actions: Generate Package, Regenerate Section, Copy Section, Edit, Save Version, Approve.

### Acceptance criteria
- All ten sections are present in required order.
- Tag count is exact and <=500 characters.
- No false claim of verified live search data.
- Disclaimer reflects actual workflow.
- Each section/version traces to source and Master Script versions.
- Regenerating one section does not silently overwrite others.

## F-011 — ULTRA CORE command and orchestration

**Related requirements:** FR-032–FR-034

### Product role
A central interface for commands, workflow status, project context, validation, and navigation. The visual core is not itself the business logic; backend/application services enforce rules.

### Main UI elements
- Central ULTRA CORE ring/orb with state indicator.
- Command input (“Command Ultra Core…”).
- Active project and settings context.
- Workflow timeline.
- Module status cards.
- Current task and next valid action.
- Core directives / validation / approval inspector.
- Safe activity history.

### Command processing
1. Accept typed command.
2. Resolve intended action and active project.
3. Determine required fields and workflow preconditions.
4. If essential information is missing, ask a direct question.
5. If action is destructive, expensive, or affects approvals, show a confirmation where appropriate.
6. Call a known permission-checked application service.
7. Display result and update workflow status.
8. Record safe event/provenance.
9. Offer next valid action.

### Example supported command intents (not a promise of natural-language model capability)
- Create/open project.
- Analyze current source.
- Generate Reference Script / Master Script.
- Generate all video prompts or a selected part.
- Generate thumbnail prompts.
- Generate SEO package or one section.
- Show pending approvals.
- Explain why a step is blocked.
- Open project memory/history.

### Safety rules
- Scope every command to active project unless user explicitly selects another.
- Never bypass approval gates.
- Never silently rewrite approved output.
- Never claim a service ran when it did not.
- Never execute arbitrary code or shell commands from a natural-language instruction.
- Require confirmation for delete/archive or potentially expensive full regeneration as defined by product policy.
- Show unknown/unavailable integrations honestly.

### Animation states
- Idle: subtle ring pulse/rotation.
- Analyzing: scan line and active nodes.
- Generating: progress and active module connection.
- Awaiting approval: amber status.
- Success: teal completion.
- Error: focused warning and recovery action.
- Reduced motion: static or low-motion equivalent.

### Acceptance criteria
- Invalid/out-of-order commands produce a useful explanation.
- Commands cannot mutate unrelated project records.
- User sees the actual operation/result.
- Motion respects reduced-motion settings.

## F-012 — Validation and Version Manager

**Related requirements:** FR-008, FR-015, FR-019, FR-023, FR-031

### Responsibilities
- Validate required inputs and output structure.
- Track version lineage and immutable prior outputs.
- Store approval state per version.
- Mark stale outputs when dependencies change.
- Support compare/history and restore-as-new-version.
- Distinguish blocking error, warning, and informational notice.

### Common validation result fields
- Object ID/version.
- Rule ID.
- Severity: blocking / warning / info.
- Pass/fail.
- Human-readable message.
- Suggested next action.
- Timestamp.

### Acceptance criteria
- Old versions remain accessible under policy.
- A failed generation does not replace the last valid version.
- A stale approved output remains visible but cannot be mistaken for current.
- Approval is attached to a specific version, not just a mutable record.

## F-013 — HUD Design System and Motion

**Related requirements:** FR-034

### Core design direction
- Premium futuristic HUD / AI command center.
- Matte black, graphite, deep navy.
- Electric cyan/teal as primary interaction accents.
- Amber for attention/approval warnings; semantic error/success colours defined in DESIGN_SYSTEM.md.
- Glassmorphism used sparingly.
- Readable content and clear hierarchy; not a movie-poster interface.
- Desktop-first, responsive behaviour TBD.

### Key screens
1. Main Command Center.
2. New Project Setup.
3. Source Intelligence / Project Memory.
4. Master Script Studio.
5. Video Prompt Studio.
6. Thumbnail Studio.
7. SEO Studio.

### Acceptance criteria
- Consistent shared tokens/components.
- High legibility and visible focus.
- Every animated state has a static/reduced-motion alternative.
- Main workflow remains usable if animation is disabled.

## F-014 — Error recovery and history

### Rules
- Preserve prior valid data on failed refresh/generation.
- Show failure scope and retry safety.
- Do not mark a failed task complete.
- Maintain safe workflow event history.
- Provide recovery paths without forcing a full project restart.

### Acceptance criteria
- Failure can be diagnosed from displayed state and safe logs.
- Retry does not accidentally create duplicate outputs.
- User can reopen last valid version.

## F-015 — Copy/export

### Scope
- Copy title, description, keyword group, tags, hashtag set, or individual prompt.
- Copy full SEO package if supported.
- Export files is a possible extension; format and scope TBD.

### Acceptance criteria
- Copy action provides clear success feedback.
- Copying does not modify saved content.
- Exported output preserves intended plain text and line breaks.

## 3. Shared dependency and stale-output matrix

| Changed input | Outputs to re-evaluate |
|---|---|
| Source text/version | Analysis, Reference Script, Master Script, video prompts, thumbnails, SEO |
| Source Analysis version | Reference Script/Master Script if dependent; downstream outputs when approved script changes |
| Master Details/settings | Relevant script and output modules according to setting dependency |
| Reference Script version | Master Script draft/validity; downstream only if new Master Script is approved |
| Master Script draft not approved | No downstream canonical change |
| New approved Master Script version | Video prompts, thumbnails, SEO marked outdated |
| Video prompt edited | Only that prompt version and its approval status |
| Thumbnail Option A edited | Option A only |
| SEO title edited | Title section only unless explicit full regeneration |
| SEO source research refreshed | Sections that use that research should be revalidated |

## 4. Feature completion definition

A feature is complete only when:
- Requirements and acceptance criteria are implemented.
- UI states and error paths are covered.
- Data is persisted as intended.
- Security/project isolation is tested where applicable.
- Relevant tests pass.
- Documentation is updated.
- Verification evidence is recorded in PROJECT_MEMORY.md.
