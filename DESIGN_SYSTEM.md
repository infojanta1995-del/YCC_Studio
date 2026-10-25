# YCC Studio — Design System and UX Specification

**Status:** Visual direction approved for planning; exact tokens and implementation TBD  
**Last updated:** 2026-10-25

## 1. Design purpose and principles

YCC Studio should feel like a premium AI command center, not a game HUD or movie poster. Futuristic elements should communicate state and relationships while the main content remains readable and actionable.

### Principles
1. **Clarity before spectacle:** scripts, prompts, source facts, and SEO text are primary content.
2. **One consistent command center:** ULTRA CORE is visually central but does not obscure task content.
3. **Progress is explicit:** users can tell what is running, waiting for approval, stale, blocked, or complete.
4. **User remains in control:** edit, cancel, regenerate, and approve actions are clear.
5. **No hidden mutations:** regeneration and settings changes explain which outputs are affected.
6. **Project context stays visible:** active project and input versions are always discoverable.
7. **Motion has meaning:** animation indicates analysis, generation, completion, or warning.
8. **Accessible by design:** keyboard, focus, contrast, labels, and reduced motion are supported.

## 2. Visual direction

### Overall
- Premium futuristic HUD / AI production command center.
- Matte black, graphite, deep navy surfaces.
- Electric cyan and teal as main interaction accents.
- Restrained amber for attention/approval warnings.
- Glassmorphism used sparingly and only where it preserves contrast.
- Fine grid lines, thin connectors, subtle radial geometry, small telemetry indicators.
- Avoid green hacker code rain, dense unreadable microtext, excessive glow, fantasy characters, and decorative panels with no function.

### Proposed color tokens
These are starting proposals and must be contrast-tested in implementation.

| Token | Proposed value | Use |
|---|---|---|
| `color-bg` | `#080C12` | App background |
| `color-surface-1` | `#101722` | Main panels |
| `color-surface-2` | `#151F2B` | Raised cards |
| `color-border` | `#263746` | Panel boundaries |
| `color-text-primary` | `#F2F7FA` | Main text |
| `color-text-secondary` | `#A7B8C7` | Secondary text |
| `color-cyan` | `#42DDF5` | Primary accent/focus |
| `color-teal` | `#36D6B0` | Active/success accent |
| `color-amber` | `#F2B65C` | Warning/approval |
| `color-error` | `#FF6B76` | Errors |
| `color-info` | `#80AFFF` | Informational state |

Do not use color alone to communicate state; pair it with labels/icons/text.

## 3. Typography

- Font family: TBD; choose a readable UI sans-serif with multilingual Hindi/Latin support.
- Display headings: compact, strong, not excessively wide-tracked.
- Body/editor text: prioritize reading comfort over sci-fi styling.
- Code-like telemetry may use a monospaced face, but long prompts and scripts should use a highly readable proportional or carefully selected editor font.
- Establish semantic tokens for display, page title, section title, body, label, helper text, metadata, and code.
- Exact sizes/weights/line heights: TBD after framework and screen testing.
- Hindi text must be tested for glyph coverage, line height, wrapping, and mixed Latin/Hindi content.

## 4. Layout and information architecture

### Desktop shell
- **Top bar:** YCC STUDIO, active project, format, language/voice context, global status, user/menu controls if applicable.
- **Left navigation:** Projects, Source Analysis, Reference Script, Master Script, Video Prompts, Thumbnail Studio, SEO Studio, Project Memory, History.
- **Main workspace:** screen-specific content.
- **Right inspector:** ULTRA CORE directives, validation, approvals, memory sync, workflow status.
- **Bottom command area:** “Command Ultra Core…” and/or workflow timeline; must not cover primary editor controls.

### Responsive behaviour
Desktop-first is the visual concept. Supported breakpoints and mobile layouts are TBD.
- Do not force a three-column layout onto narrow screens.
- Collapse navigation and inspector into accessible drawers/panels.
- Preserve access to primary actions and current project context.
- Avoid horizontal overflow for scripts, prompts, tags, and tables.
- Allow editor panes to stack on smaller viewports.

## 5. Screen specifications

### Screen 1 — Main Command Center
Purpose: orient the user and show current project/workflow status.
- Center: ULTRA CORE ring/orb with state label.
- Left: primary navigation.
- Top: project identity and master settings.
- Surrounding cards: Video Prompt Generator, Thumbnail Generator, SEO Generator, plus current stage.
- Right: Core Directives, Workflow Integrity, Fact Check, Approval Gates, Memory Sync.
- Bottom: command input and stage timeline.
- Main actions: resume project, create project, open pending approval, inspect current task.
- Avoid making the orb so large that it pushes actionable project information below the fold.

### Screen 2 — New Project Setup
Purpose: collect initial project settings.
- Format cards: 9:16 Vertical and 16:9 Landscape.
- Settings: project name, duration options, voice gender, language, optional story/source type and style.
- Format-specific controls appear after selection.
- Clearly explain duration/segment rules.
- Main action: Save Project / Continue.
- Show validation inline; do not silently change selections.

### Screen 3 — Source Intelligence
Purpose: edit sources and inspect saved analysis/memory.
- Left: source editor and source list.
- Center: structured analysis fields.
- Right: Project Memory, freshness, unresolved verification, source versions.
- Clearly distinguish fact, claim, prediction, opinion, and unknown.
- Main actions: Save Source, Analyze Story, Refresh Analysis, View History.
- If analysis is stale, show reason and refresh action.

### Screen 4 — Master Script Studio
Purpose: review and approve the canonical script.
- Left: source facts and Reference Script versions.
- Center: editable part-aware script editor.
- Right: validation, duration, source alignment, language, warnings.
- Top: settings snapshot and version selector.
- Actions: Compare Versions, Edit, Regenerate, Save Draft, Approve.
- Approval state must be unambiguous and tied to a specific version.

### Screen 5 — Video Prompt Studio
Purpose: manage one prompt per actual script part.
- Left: segment list with status and duration.
- Center: copy/edit prompt editor.
- Right: aspect, voice, continuity, exact narration, no-text and factuality checks.
- Actions: Generate All, Regenerate This Part, Copy, Approve, History.
- Do not hide a narration/duration warning behind a decorative indicator.

### Screen 6 — Thumbnail Studio
Purpose: compare and refine two different visual concepts.
- Two clear cards: Option A and Option B.
- Each card: concept, prompt editor, ratio/text settings, status, actions.
- Right inspector: fact accuracy, distinctness, ratio and text compliance.
- Ratio question appears when no ratio is supplied.
- Direct image previews are placeholders unless direct image generation is approved and implemented.

### Screen 7 — SEO Studio
Purpose: edit and approve a complete SEO package.
- Section navigation for the ten required output sections.
- Title editor, description editor, grouped keywords, tag block/count, hashtags.
- Right inspector: accuracy, research-data status, tag limit, disclaimer.
- Actions: Generate Package, Regenerate Section, Copy Section, Save Version, Approve.
- The tag counter must show the exact count calculated by code, not a model-estimated number.

## 6. Shared components

### Buttons
Variants: primary, secondary, quiet/tertiary, danger, icon-only.
- Include hover, focus, pressed, disabled, and loading states.
- Icon-only actions require accessible names/tooltips.
- Destructive actions require confirmation.

### Inputs
- Label, optional/required marker, helper text, validation state.
- Errors are associated with the input and written in plain language.
- Preserve user input when validation fails.

### Cards and panels
- Use consistent padding, borders, radii, and elevation.
- Avoid unnecessary nested cards.
- Selected, stale, warning, and disabled states need labels and clear contrast.

### Status chips
Suggested states: Draft, Generating, Awaiting Approval, Approved, Outdated, Blocked, Failed, Complete.
- Text label is required; color is supplemental.

### Editors
- Support long text, line breaks, selection, copy, and safe save.
- Unsaved-change state is visible.
- Leaving a dirty editor should prompt to save/discard when appropriate.
- Large text must wrap or offer horizontal scrolling without breaking the layout.

### Modal/dialog
Use for destructive confirmation, essential missing choices, and high-impact decisions. Avoid using modals for routine informational messages.

### Notifications
- Confirm success with concise feedback.
- Errors must explain the next step.
- Do not show success for an operation that only queued or partially completed unless clearly labeled.

### Version comparison
- Show which version is current/approved.
- Highlight changes without implying a new draft is approved.
- Restoring an older version should create a new version or follow an explicit restore policy.

## 7. ULTRA CORE visual and interaction states

| State | Visual treatment | Required text/behaviour |
|---|---|---|
| Idle | Subtle ring pulse, low-motion nodes | “Ready” or current available action |
| Analyzing | Radial scan / active nodes | Current analysis stage |
| Generating | Progress trail to active module | Task name and status; no invented ETA |
| Awaiting approval | Amber indicator | Which version awaits user decision |
| Success | Teal completion ring | Actual completed operation and next action |
| Warning | Amber marker | Explanation and whether action can continue |
| Error | Focused error state | Failure scope, saved-data status, recovery action |
| Outdated | Muted/stale badge plus reason | Which upstream dependency changed |
| Blocked | Clear blocker indicator | Missing prerequisite or approval |

### Motion
- Use subtle, purposeful animation.
- Do not animate every panel simultaneously.
- Avoid strobing, rapid flashing, or motion that competes with reading.
- Respect `prefers-reduced-motion` or the equivalent chosen by the stack.
- Motion must never be the only way to detect status.

## 8. Interaction rules

- Maintain active project context through navigation.
- Confirm before delete/archive and expensive full regeneration according to product policy.
- Do not silently navigate away from unsaved edits.
- Disable impossible actions and explain why.
- Preserve scroll/editor position where feasible after saving.
- Display stale dependencies and offer explicit refresh.
- Do not auto-approve generated content.
- Copy operations must not alter content.
- Keyboard navigation and visible focus are mandatory.

## 9. Accessibility and quality

- Target conformance level: TBD; choose a formal accessibility target before release.
- Verify text and control contrast.
- All fields have labels; status is not conveyed by color alone.
- Keyboard users can reach all actions in a logical order.
- Focus is visible and not obscured by panels.
- Errors are understandable and programmatically associated where supported.
- Reduced-motion mode is available.
- Test Hindi and English text, long titles, long URLs, large prompts, and screen resizing.
- Do not rely on tiny low-contrast HUD labels for critical information.

## 10. Assets and motion implementation

- Icon library: TBD.
- Logo/wordmark: simple YCC Studio wordmark; final asset TBD.
- Use SVG/vector or CSS effects for interface geometry where practical.
- Keep decorative assets lightweight.
- Store approved static assets under `assets/` after the app structure is chosen.
- No generated mockup image should be treated as production code or an interactive component.

## 11. Design acceptance checklist

- [ ] All seven main screens use consistent shell and tokens.
- [ ] Primary actions are obvious.
- [ ] Project and version context are visible.
- [ ] Loading, empty, error, warning, stale, and approval states exist.
- [ ] Animation supports rather than replaces status text.
- [ ] Reduced motion is respected.
- [ ] Hindi/English content is legible.
- [ ] Long content does not break layout.
- [ ] Keyboard focus and labels are present.
- [ ] Screens work at supported viewport sizes once defined.
