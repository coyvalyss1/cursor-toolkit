---
name: session-documentation
description: Documentation workflow for development sessions. Use when logging work, updating session log, completing a feature, fixing a bug, archiving sessions, or when the user says "log this", "update session log", "document", or after significant implementation work.
---

# Session Documentation

## Where to Document

```
What type of change?
│
├─ Bug fix, UI fix, patch, refactor
│  └─ SESSION_LOG.md (always)
│
├─ Substantial new feature
│  └─ docs/FEATURE_IMPLEMENTATION.md + SESSION_LOG.md
│
├─ Business strategy, revenue, marketing
│  └─ private-docs/ folder (NOT docs/, add to .gitignore)
│
└─ Unsure?
   └─ Default to SESSION_LOG.md
```

## SESSION_LOG.md Entry Format

Location: `[PROJECT_ROOT]/SESSION_LOG.md`

Add new sessions at the TOP (newest first). Never delete old sessions.

```markdown
---

## Session: [Brief Title]

**Date:** YYYY-MM-DD
**Topic:** One-line description
**Status:** COMPLETED | IN PROGRESS | BLOCKED | DECISION

### Context
[Brief background/problem]

### Implementation/Decisions
[What was built/decided]

### Files Modified
- `path/to/file.js` - Description of change

### Testing/Deployment
[Commands used, results]

---
```

## What to Log

**Log these:**
- Design decisions
- Feature implementations
- Bug fixes with root cause
- Configuration changes
- Deployment results

**Skip these:**
- Minor typo fixes
- Simple Q&A without code changes
- Documentation-only edits

## Never Create Standalone Fix Docs

Do NOT create files like:
- `HAMBURGER_MENU_FIX.md`
- `FEATURE_COMPLETE.md`
- `*_SUMMARY.md`
- `*_FIX_FINAL.md`

All fixes go in SESSION_LOG.md. Period.

## Feature Documentation

For substantial new features only:

1. Create `docs/FEATURE_IMPLEMENTATION.md`
2. Add summary to SESSION_LOG.md
3. Update MASTER TODO if applicable

Qualifies as "substantial": new user-facing features, new integrations, new AI capabilities, new revenue streams, migration guides.

Does NOT qualify: bug fixes, UI improvements, CSS fixes, refactoring, performance optimizations.

## Archive Workflow (Every 2 Weeks)

1. Move sessions older than 2 weeks:
   ```bash
   # Create archive file
   docs/archive/SESSION_LOG_[MONTH]_[YEAR].md
   ```

2. Move verified fixes to knowledge base:
   ```bash
   docs/knowledge-base/[category]/[FIX_NAME]_[DATE].md
   ```

   Example categories: `authentication/`, `deployment/`, `react-patterns/`, `ui-fixes/`, `api-integration/`

## Document Organization

| Contains | Location |
|----------|----------|
| Business strategy, revenue, pricing | `private-docs/` (.gitignore) |
| Technical implementation | `docs/` (committed to git) |
| Active work (< 2 weeks) | `SESSION_LOG.md` |
| Verified fixes (> 2 weeks) | `docs/knowledge-base/` |
| Completed milestones | `docs/archive/` |

**Decision test**: "Would I share this on GitHub?" YES = `docs/`, NO = `private-docs/`
