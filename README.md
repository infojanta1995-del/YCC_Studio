# YCC Studio — Project Documentation

**Document status:** Planning specification  
**Project status:** Requirements and UX planning; implementation not started  
**Document date:** 2026-10-25  
**Technology stack:** TBD — do not infer or silently select a stack from this document.

## 1. Project identity

- **Official name:** YCC Studio
- **Core concept:** A project-based YouTube content production workspace coordinated by a central intelligence/orchestration layer called **ULTRA CORE**.
- **Primary outputs:** Reference Script, approved Master Script, per-part video-generation prompts, two thumbnail-generation prompts, and a YouTube SEO package.
- **Current release target:** TBD.
- **Intended users:** A creator/operator who wants to turn source stories into reviewed, versioned YouTube production assets.
- **Current status:** Planning only. No feature should be represented as implemented until code and tests verify it.

## 2. Overview

YCC Studio manages the end-to-end workflow from a source story to production-ready assets. Each new story normally starts as an independent project. A project stores its source material, settings, analysis, generated outputs, versions, approvals, workflow state, and project-specific memory.

ULTRA CORE is the user-facing command and coordination concept. In the actual system it should be implemented as explicit application services, workflow rules, validation, and a command interface—not as an unexplained autonomous agent. It must respect approval gates, preserve project isolation, explain failures, and avoid silently changing approved content.

## 3. Primary workflow

1. Create a project.
2. Select video format and project settings.
3. Add source story text/documents and, where supported, a YouTube URL.
4. Analyze the source and save structured project memory.
5. Generate or integrate a Reference Script using the supplied Script Writer skill after that ZIP/code has been inspected.
6. Review and edit the Reference Script.
7. Generate, revise, and approve the Master Script.
8. Generate video prompts for each actual Master Script part.
9. Review and approve video prompts.
10. Generate two distinct thumbnail prompts.
11. Review and approve thumbnail concepts.
12. Generate the SEO package from both source story and approved Master Script.
13. Review and approve SEO outputs.
14. Preserve versions, status, source relationships, and history throughout.

A project may pause and resume at any stage. Required approvals gate dependent stages.

## 4. Features

### Planned
- Project creation, listing, search, filtering, rename, duplicate, archive, and confirmed delete.
- Project History Manager and project-isolated persistent memory.
- Master Details settings for format, duration, voice, language, and optional project metadata.
- Source Manager and shared Source Analysis service.
- Reference Script generation/integration, pending inspection of the supplied skill ZIP.
- Master Script generation, editing, comparison, versioning, validation, and approval.
- Video Prompt Generator.
- Thumbnail Generator.
- SEO Generator.
- ULTRA CORE command/orchestration and workflow status interface.
- Animated HUD design system and seven-screen storyboard.
- Error handling, recovery, validation, and audit/history records.

### Implemented
- None confirmed in this planning package.

## 5. Technology stack

All items remain **TBD** pending a codebase audit and explicit architecture decision:
- Language:
- Frontend framework:
- Backend/runtime:
- Database:
- Authentication:
- AI model/provider:
- Image generation provider:
- Video prompt target / Google Flow integration approach:
- Search/trend data provider:
- File/object storage:
- Test framework:
- Build/deployment platform:

Do not claim direct Google Flow API access, live Google Trends access, image-generation API access, or execution of a supplied skill unless verified.

## 6. Prerequisites and setup

This is a documentation-only planning package. It does not include runnable application code. Therefore no app install, dev-server, database, or test commands are yet verified.

Once the stack is selected, update this section with:
- Minimum runtime and package-manager versions.
- Dependency installation command.
- Environment setup.
- Database initialization and migrations.
- Development command.
- Build and test commands.
- Expected local URL or CLI output.

## 7. Environment configuration

Required environment variables are TBD. When decided, document variable **names and purposes only** in committed documentation. Put actual secrets in a local ignored environment file or secret manager. Never commit API keys, passwords, access tokens, or private source credentials.

Likely categories—not approved variable names—may include AI provider credentials, database connection settings, source retrieval credentials, and optional search-provider credentials.

## 8. Suggested documentation and code structure

```text
your-project/
├── README.md
├── PROJECT_REQUIREMENTS.md
├── ARCHITECTURE.md
├── FEATURES.md
├── IMPLEMENTATION_PLAN.md
├── DESIGN_SYSTEM.md
├── PROJECT_MEMORY.md
├── src/                    # Actual application code; structure TBD
├── tests/                  # Automated tests; framework TBD
├── assets/                 # Approved static assets; formats TBD
├── docs/                   # Optional additional ADRs and diagrams
└── .env.example            # Names/placeholders only; never real secrets
```

This ZIP contains the seven Markdown documentation files at its root. It does not create `src/`, `tests/`, or `assets/` because the implementation stack and code structure have not been selected.

## 9. Known limitations and unresolved items

- The Script Writer skill ZIP/code has not yet been inspected.
- No codebase, package configuration, database schema, API contract, or test results have been verified.
- Exact AI providers and integration capabilities are TBD.
- For 16:9 duration, the existing requirement says minimum 3 minutes, but the segment arithmetic decision is unresolved: 180 seconds can be represented as 22 × 8 seconds plus a final 4-second part, or as 24 × 8 seconds = 192 seconds. The product must explicitly decide how to represent this.
- The thumbnail prompt spec asks for two image prompts; direct generation of final thumbnail images is not yet confirmed as an app requirement.
- Live keyword research must not be claimed unless an actual reliable data source is integrated and queried.
- “ULTRA CORE” is a product/UX concept; its specific autonomy and command permissions must be constrained by approved workflow rules.

## 10. Maintenance rules

- Keep documents aligned with actual code and verified behaviour.
- Mark unresolved decisions `TBD` and distinguish suggestions from approved decisions.
- Do not mark work `Implemented` or `Verified` without evidence.
- Update PROJECT_MEMORY.md after meaningful changes and test runs.
- Use feature IDs, requirement IDs, task IDs, and test references to maintain traceability.
- Do not silently change approved scripts or downstream outputs when one output is regenerated.
