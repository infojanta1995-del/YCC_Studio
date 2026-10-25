# YCC Studio — Project Memory and Continuity

**Last updated:** 2026-10-25  
**Current phase:** Planning / specification  
**Overall status:** Planning package created; implementation not started  
**Last verified working state:** Documentation files generated in this package only. No application code, build, integration, or tests have been verified.

## 1. Current project state

YCC Studio is planned as a project-based YouTube content production workspace with a central ULTRA CORE command/orchestration interface. This project context should be treated independently; do not mix requirements from unrelated projects or chats unless the user explicitly supplies them.

### Confirmed product concept
- Project-based workflow; new source story defaults to its own project.
- Persistent project-specific memory and versioned history.
- Master Details: video format, duration, voice gender, script language, and optional project metadata.
- Source Manager and structured Source Analysis.
- Reference Script based on a user-supplied Script Writer skill/ZIP, which must be inspected before integration decisions.
- Master Script Studio with editing, version comparison, validation, and approval.
- Video Prompt Generator: one standalone prompt per actual script part.
- Thumbnail Generator: exactly two distinct image-generation prompts.
- SEO Generator: complete ten-section YouTube SEO package based on source story plus approved script.
- ULTRA CORE: central command/workflow/validation concept; actual permissions and business rules must be enforced by application services.
- Premium dark HUD design with seven key screens.

## 2. Documentation created

- `README.md` — overview, status, setup placeholders, limitations.
- `PROJECT_REQUIREMENTS.md` — FR IDs, business rules, scope, success criteria, open decisions.
- `ARCHITECTURE.md` — logical components, data flow, entities, security, integrations.
- `FEATURES.md` — feature-level workflows, inputs/outputs, UI states, validation, acceptance criteria.
- `IMPLEMENTATION_PLAN.md` — phased roadmap, tasks, exit criteria, risks, release checklist.
- `DESIGN_SYSTEM.md` — seven screens, design direction, component and motion rules.
- `PROJECT_MEMORY.md` — current state, decisions, blockers, next steps.

## 3. Prompt specifications captured

### Video Prompt Generator
- Analyze the complete script before creating prompts.
- Preserve actual part count/order and exact narration.
- Default prompt assumptions: Google Flow-targeted, 9:16, 8 seconds, ultra-realistic cinematic; authoritative project settings override defaults.
- No captions, subtitles, on-screen text, labels, headlines, logos, or watermarks.
- Include camera, action, lighting, realism, narration, voice, music/SFX, continuity, and factuality constraints.
- One prompt per actual part; independent regeneration and version history.

### Thumbnail Generator
- Analyze full script and produce exactly two copy-ready prompts.
- Option A: high impact; Option B: distinct cinematic/story curiosity.
- Ask for ratio if not supplied; do not assume it equals video format.
- Text preference: Yes / No / Let AI decide.
- Preserve facts; do not fabricate events or imply illustrative reconstruction is authentic footage.
- Independent option editing/regeneration and versioning.

### SEO Generator
- Analyze original source and final approved script together.
- Check source/script alignment and flag serious discrepancies.
- Primary hook title plus up to two useful alternatives.
- Complete copy-ready description and workflow-accurate AI disclaimer.
- Grouped keywords with honest live-data verification labels.
- Comma-separated YouTube tags, <=500 characters, exact count.
- Separate relevant hashtags and final upload checklist.
- Section-level editing/regeneration and versioning.

## 4. Confirmed settings and business rules

- Video formats: 9:16 vertical and 16:9 landscape.
- 9:16 duration choices: 24s, 32s, 40s, 48s, using 8-second segments.
- Voice gender: Male/Female.
- Script language: Hindi/English.
- Master Script approval is required before downstream generation.
- Each generated output links to exact upstream versions.
- Regenerating one output must not overwrite unrelated outputs.
- Source/settings changes mark dependent outputs outdated rather than silently rewriting them.
- Failed refresh/generation preserves the last valid output.
- Search volume/trend claims must not be made unless actual research was performed.
- Do not claim integrations or test success without evidence.

## 5. Design direction

### Visual
- Premium futuristic HUD / AI command center.
- Matte black, graphite, deep navy.
- Cyan and teal highlights, restrained amber for warnings/approval.
- Fine grid lines, glass panels used sparingly, readable typography.
- Avoid clutter, excessive neon, tiny illegible telemetry, or decorative sci-fi elements that interfere with work.

### Seven planned screens
1. Main Command Center.
2. New Project Setup.
3. Source Intelligence / Project Memory.
4. Master Script Studio.
5. Video Prompt Studio.
6. Thumbnail Studio.
7. SEO Studio.

### ULTRA CORE states
- Idle: subtle pulse.
- Analyzing: radial scan.
- Generating: progress connection to active module.
- Awaiting approval: amber.
- Success: teal.
- Error: clear failure/recovery action.
- Outdated/blocked: explicit reason.
- Reduced-motion mode required.

## 6. Important open decisions / blockers

1. **Script Writer ZIP inspection:** not performed in this planning package. Inspect archive contents, runtime, dependencies, entry point, inputs/outputs, licence, external services, security, and error handling before proposing integration.
2. **Technology stack:** language/framework/runtime/database/deployment all TBD.
3. **16:9 duration rule:** 180 seconds does not divide evenly into 8-second parts. Decide between 22×8s + 4s final part, or 24×8s = 192s (3m12s), or another explicitly approved policy.
4. **Direct thumbnail image generation:** prompt generation is specified; in-app image generation remains unconfirmed.
5. **Google Flow:** prompt formatting is specified, but direct API integration is not confirmed.
6. **Live keyword research:** provider and availability are TBD; unverified suggestions must be labeled honestly.
7. **Source URL/transcript retrieval:** supported sources, methods, permissions, and provider TBD.
8. **Authentication/multi-user:** TBD.
9. **Data retention/delete/export:** TBD.
10. **Responsive targets/accessibility conformance/performance targets:** TBD.

## 7. Work completed vs not completed

### Completed in this package
- Written planning specifications for the seven documentation files.
- Captured the three supplied generation prompt specifications at functional level.
- Recorded HUD direction and seven-screen storyboard.
- Documented key workflow rules, output schemas, validation expectations, and open decisions.

### Not completed / not verified
- No application code created.
- No UI built.
- No database schema migrated.
- No API/provider integrated.
- No Script Writer ZIP inspected or executed.
- No tests, build, deployment, or runtime checks performed.
- No direct Google Flow, Trends, transcript, or image generation integration verified.

## 8. Next steps in priority order

1. Review this documentation and correct any misunderstood requirement.
2. Provide the Script Writer ZIP/code for inspection if it is not already available in the active project context.
3. Inspect existing repository/files before selecting a stack or creating code.
4. Resolve the 16:9 segment/duration rule.
5. Decide whether direct thumbnail image generation and live keyword research are required for the first release.
6. Select the technology stack and record ADRs.
7. Define final data schema and workflow state transitions.
8. Build the design-system shell and Project Manager first.
9. Implement Source Manager, analysis, and project memory.
10. Integrate Reference Script, then Master Script approval.
11. Implement Video Prompt, Thumbnail, and SEO modules.
12. Implement ULTRA CORE command routing and status view.
13. Run unit/integration/end-to-end/accessibility/security tests.
14. Update this file with actual files changed, commands run, test outcomes, and the next exact task.

## 9. Memory update rules

- Update after each meaningful implementation milestone.
- Record exact files changed and verified commands/results.
- Keep failures and blockers visible.
- Distinguish assumptions from decisions.
- Never state “implemented” or “verified” without evidence.
- Keep the next task specific and actionable.
- Do not store credentials or secrets here.
