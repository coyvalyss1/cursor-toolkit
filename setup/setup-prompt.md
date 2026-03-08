# How to Stop AI Coding Assistants from Making the Same Dumb Mistakes

Here's the thing about AI coding tools: they're incredibly powerful, and they will absolutely break your app in the same way twice if you let them. I've been building with Cursor for a while now, and the single biggest unlock wasn't learning better prompts or switching models. It was teaching the AI my patterns so it stops making mistakes I've already solved.

Cursor has a rules and skills system that most people never set up. Once you do, it's like giving the AI a memory. It stops silently swallowing errors. It stops reformatting things you already fixed. It commits incrementally instead of accumulating 40 files of uncommitted changes and then panicking.

This isn't a magic fix. The AI will still surprise you. But it will surprise you with new problems instead of the same old ones, and that's actual progress.

Below is the architecture I use, followed by a prompt you can paste directly into Cursor to scaffold your own version. The prompt asks you the right questions first so it builds something tailored to your stack, not a generic template.

---

## What This System Does

Cursor supports two powerful customization layers:

1. **Rules** (`.mdc` files in `.cursor/rules/`) — persistent context injected into every AI conversation. Rules can always apply, or only activate when specific file types are open.
2. **Skills** (`SKILL.md` files in `.cursor/skills/`) — reusable, multi-step workflows the AI agent can follow when it recognizes a matching task.

Together, they give the AI a persistent memory of your conventions, your past mistakes, and your team's patterns.

---

## Architecture Overview

```
~/.cursorrules                          # Global root rules (project routing, safety, preferences)
~/.cursor/
  rules/                                # Global rules (apply across all projects)
    git-workflow.mdc                    #   Always-apply: commit discipline
    structural-change-safety.mdc        #   Always-apply: safe multi-file refactors
    swiftui-patterns.mdc                #   Glob: **/*.swift — view composition patterns
    ios-firebase.mdc                    #   Glob: **/*.swift — Firestore/Auth patterns
    close-button-styling.mdc            #   Glob: **/*.jsx — UI component standards
    avatar-icon-consistency.mdc         #   Glob: **/*Modal*.jsx — specific component rules
  skills/                               # Personal skills (available in all projects)
    ios-new-feature/SKILL.md            #   End-to-end iOS feature workflow
    firebase-functions-v2-export/SKILL.md  # Firebase Functions export pattern

your-project/
  .cursorrules                          # Project-specific root rules
  .cursor/
    rules/                              # Project-specific rules
      api-patterns.mdc
      deployment-workflow.mdc
    skills/                             # Project-specific skills (shared via git)
      data-migration/SKILL.md
```

### Key Concepts

- **`~/.cursorrules`** (home directory): Global rules that apply to every project. Good for personal preferences, git safety, writing voice, and routing rules that point the AI to the right project-specific context.
- **`~/.cursor/rules/*.mdc`**: Global rules with frontmatter that controls when they activate.
- **`your-project/.cursorrules`**: Project-specific rules. Contains project structure, architecture decisions, current phase, key file paths.
- **`your-project/.cursor/rules/*.mdc`**: Project-specific rules scoped to file patterns.
- **`~/.cursor/skills/`**: Personal skills available everywhere.
- **`your-project/.cursor/skills/`**: Project skills, shared with collaborators via version control.
- **Never put anything in `~/.cursor/skills-cursor/`** — that's Cursor's internal directory.

---

## The Prompt

Copy everything below this line and paste it into a Cursor Agent chat. Replace the placeholder content with your own conventions.

---

### Prompt Start

I want you to help me set up a Cursor rules and skills system. Here's what I need you to create. Ask me questions where I've left placeholders, then create all the files.

#### 1. Global Root Rules (`~/.cursorrules`)

Create a `~/.cursorrules` file that includes:

- **Project routing**: If I have multiple projects in my workspace, route Swift files to one project's rules and React/JS files to another. (Tell me what projects you have and I'll set this up.)
- **Git safety**: Before starting work, check `git status` and `git log`. Commit after each logical unit. Never accumulate >500 lines of uncommitted changes across >5 files. Write commit messages explaining "why" not "what."
- **Secrets safety**: Never hardcode API keys. Never `cat .env`. Always use environment variables.
- **Documentation**: Fixes go in a session log (append at top, never delete). Substantial features get their own docs. Never create standalone fix docs.
- **User preferences**: [Add your own — e.g., "concise responses", "production-ready code", "proactive suggestions welcome", "mobile-first design"]
- **Writing voice** (if applicable): [Describe your writing style for any content generation — tone, structure, things to avoid]

#### 2. Global Always-Apply Rules (`~/.cursor/rules/`)

Create these `.mdc` files with `alwaysApply: true`:

**`git-workflow.mdc`** — Git discipline:
- Check state at session start (`git status`, `git branch`, `git log --oneline -3`)
- Alert if >10 files have uncommitted changes
- Commit in logical groups for multi-file changes
- Build-verify between commits
- Never force-push to main

**`structural-change-safety.mdc`** — Safe multi-file refactors:
- Define what counts as structural (renaming tabs, changing data models, reorganizing files, changing navigation)
- Require a phased process: Prepare → Create (commit) → Wire (commit) → Clean up (commit)
- Hard limits: Never >500 lines uncommitted across >5 files. Never skip build verification. Never combine create + wire + remove in one batch.

#### 3. File-Specific Rules (`~/.cursor/rules/`)

Create `.mdc` files with `alwaysApply: false` and appropriate `globs`:

For each rule, use this format:
```yaml
---
description: What this rule enforces
globs: "**/*.ext"
alwaysApply: false
---
```

Examples of rules I find useful (adapt to your stack):

- **Language/framework patterns** (glob: `**/*.swift` or `**/*.tsx`): Standard view structure, state management patterns, navigation patterns, async loading patterns, common mistakes to avoid.
- **Backend patterns** (glob: `**/*.js` in your functions dir): API patterns, error handling, export conventions.
- **UI component standards** (glob: `**/*.jsx` or `**/*.css`): Consistent styling for common components like close buttons, avatars, modals.

Keep each rule under 50 lines. One concern per rule. Include concrete code examples showing the right AND wrong way.

#### 4. Skills (`~/.cursor/skills/`)

Create skill directories with `SKILL.md` files for workflows you repeat. Each skill needs:

```yaml
---
name: skill-name-here
description: What this skill does and WHEN to use it. Be specific with trigger words.
---
```

Example skills I find useful:

- **New feature workflow**: Step-by-step checklist for adding a feature end-to-end (model → service → viewmodel → view → navigation → tests → docs). Include code templates for each step.
- **Deployment patterns**: Correct patterns for your deployment target (e.g., Firebase Functions export pattern, AWS Lambda handler pattern). Especially useful for patterns that silently fail when done wrong.
- **Code review checklist**: Standards for reviewing PRs.

Skills should be under 500 lines. Put detailed reference material in separate files within the skill directory and link to them from SKILL.md.

#### 5. Project-Specific Rules (per project)

For each project, create:

- **`project/.cursorrules`**: Project structure, architecture, key file paths, current development phase, tech stack specifics.
- **`project/.cursor/rules/*.mdc`**: File-scoped rules specific to that project (API patterns, CSS conventions, permission rules, etc.).

---

### What to Ask Me

Before you start creating files, ask me:

1. What projects do I have in my workspace? (For routing in `~/.cursorrules`)
2. What's my tech stack for each project?
3. What are my biggest recurring mistakes or pain points that rules should prevent?
4. What multi-step workflows do I repeat that should become skills?
5. Do I have a preferred writing voice or communication style?
6. Any specific UI/component standards I want enforced?

Then create all the files with real content based on my answers.

### Prompt End

---

## Tips for Maintaining Your Rules

Here's what I keep coming back to after months of iterating on this:

- **Rules are living documents.** When you hit a recurring bug or pattern, create a rule for it right then. Don't wait for a "rules cleanup day" that never comes.
- **Keep rules concise.** If a rule exceeds 50 lines, split it into two rules. The AI's context window is shared with your conversation, so every token in a rule competes with your actual work.
- **Use the "wrong vs right" pattern.** Showing a concrete code example of what NOT to do is often more useful than just showing the correct way. The AI learns from contrast.
- **Skills save the most time for fragile workflows.** If a process silently fails when you skip a step or get the order wrong (looking at you, Firebase Functions v2), that's a perfect skill candidate.
- **Review periodically.** Every few weeks, scan your rules for anything outdated or redundant. Kill rules that aren't earning their token cost.

The whole point is that you're training a system, not writing documentation. Every rule should exist because you got burned by its absence.
