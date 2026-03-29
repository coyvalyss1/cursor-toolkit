# Cursor Toolkit

> The power user setup we use to build AI products with Cursor. Free, open source, built by DoMoreWorld, a Valyss LLC company.

Cursor rules, skills, and best practices for building with AI coding assistants. Stop making the same mistakes twice. Give your AI a memory.

---

## Why Rules and Skills Matter

Most developers use AI coding assistants with the default settings. The AI hallucinates, makes the same mistakes repeatedly, and forces you to babysit every change. This toolkit fixes that.

### 1. Safety and Guardrails

Rules prevent destructive operations that AI agents would otherwise happily execute.

**Without rules:**
- AI agents will force-push to main, skip pre-commit hooks, and commit secrets
- They'll run `git add .` before checking `.gitignore`, leaking sensitive folders
- They'll execute destructive commands without confirmation

**With rules:**
- `git-workflow.mdc` prevents force-pushes, enforces commit discipline, and requires build verification between structural changes
- `public-content-security.mdc` catches Firebase project IDs, API keys, and internal domain names before they leak into open-source repos
- Hard limits prevent >500 lines of uncommitted changes across >5 files

### 2. Reducing Hallucination and Drift

Context rules keep AI on-topic and consistent across sessions.

**Without rules:**
- You'll end sessions with 30 uncommitted files and no idea what changed
- The AI will use an expensive model for a simple rename, or a fast model for complex architecture
- Every session starts from scratch; the AI forgets your patterns
- The AI will fabricate metrics with full confidence ("Build time improved by 40%") without any measurement

**With rules:**
- "Always commit after finishing a logical unit of work" — the AI does it automatically
- Model selection guide prevents Cursor from using the wrong model for the task
- Product context rules route Swift files to iOS-specific conventions, React files to web conventions
- **Provenance requirement**: All metrics must be sourced. The AI cites where numbers came from or explicitly says "not measured"

### 3. Creating Your Clone — Voice, Thinking, Patterns

This is the distinctive angle. Rules and skills aren't just guardrails; they teach the AI to be *you*.

**Your writing voice:**
- We encode our writing voice: conversational, no dashes, "we/our" framing, never condescending
- Every email, social post, and marketing page the AI writes sounds like us, not like corporate AI
- The `examples/writing-voice-template.mdc` gives you a fill-in template for your own voice

**Your thinking patterns:**
- We define how we think about UX decisions, marketing messaging by audience, and code architecture
- The AI doesn't guess our preferences; it already knows them
- When we say "add a feature," the AI follows our end-to-end workflow: model → service → view → tests → docs

**Your code patterns:**
- SwiftUI view composition, navigation patterns, state management
- Error handling, async loading, common mistakes to avoid
- The AI writes code the way you would, not the way it was trained

### 4. Research-Backed Best Practices

Rules can encode domain expertise from real research, so the AI applies it automatically.

**UX Typography:**
- Our typography rule cites WCAG 2.1, iOS HIG, and Material Design 3
- The AI checks font sizes, contrast ratios, and line height against real standards, not vibes
- It knows that 16px minimum for body text, 4.5:1 contrast ratio, and 1.6+ line height are requirements, not suggestions

**UX Design:**
- Nielsen's 10 Usability Heuristics encoded as patterns
- The AI knows to show loading states, provide undo options, and make error messages helpful
- It catches accessibility issues (keyboard navigation, ARIA labels, alt text) automatically

**Marketing Messaging:**
- Our marketing rule encodes HBR and McKinsey research on messaging by generation
- When the AI writes copy for GenX, it knows: direct, authentic, no hype
- When targeting Millennials: values-driven, social proof, personalized
- Research-backed channel strategy: 74% of GenX on Facebook, YouTube #1 platform overall

---

## Quick Start

```bash
# Clone this repo
git clone https://github.com/coyvalyss1/cursor-toolkit.git
cd cursor-toolkit

# Copy rules and skills to your Cursor directory
cp -r rules/* ~/.cursor/rules/
cp -r skills/* ~/.cursor/skills/
cp -r best-practices/* ~/.cursor/best-practices/

# (Optional) Use the setup prompt to scaffold project-specific rules
# See setup/setup-prompt.md for the full guided setup
```

That's it. Cursor will automatically load rules and skills from `~/.cursor/`.

---

## What's Included

### Rules (`.cursor/rules/`)

Drop-in rules that activate based on file type or always apply.

| Rule | What It Does | Always Apply? |
|------|--------------|---------------|
| `git-workflow.mdc` | Commit discipline, structural change guardrails | ✅ Yes |
| `public-content-security.mdc` | Sanitization checklist for open-source work | ✅ Yes |
| `model-selection-guide.mdc` | When to use Haiku/Sonnet/Opus/Gemini | ✅ Yes |
| `documentation-workflow.mdc` | Prevent doc sprawl, enforce provenance for metrics | ✅ Yes |
| `swiftui-patterns.mdc` | SwiftUI composition, navigation, state | 📂 `**/*.swift` |
| `ios-firebase.mdc` | Swift/Firestore patterns | 📂 `**/*.swift` |

### Skills (`.cursor/skills/`)

Reusable, multi-step workflows the AI agent follows automatically.

| Skill | What It Does |
|-------|--------------|
| `create-rule/` | How to create `.mdc` rules |
| `create-skill/` | How to author Agent Skills |
| `update-cursor-settings/` | Modify settings.json via agent |
| `create-subagent/` | Create custom subagents |
| `migrate-to-skills/` | Migrate rules to skills format |
| `session-documentation/` | Where to document (SESSION_LOG vs feature docs), anti-sprawl patterns |
| `meeting-notes/` | Structured meeting prep templates for customer, partnership, and mentorship meetings |
| `firebase-functions-v2-export/` | Correct export pattern for Firebase Functions v2 (prevents silent 404s) |

### Best Practices (`.cursor/best-practices/`)

Research-backed domain rules the AI applies automatically.

| Best Practice | What It Does | Research Sources |
|---------------|--------------|------------------|
| `ux-typography.md` | Font sizes, contrast, line height | WCAG 2.1, iOS HIG, Material Design 3 |
| `ux-design-principles.md` | Nielsen's 10 heuristics, accessibility | Nielsen Norman Group |

### Setup (`setup/`)

| File | What It Does |
|------|--------------|
| `setup-prompt.md` | Guided prompt to scaffold a rules/skills system for your project |

### Examples (`examples/`)

Templates for creating your own rules.

| Template | What It Does |
|----------|--------------|
| `product-context.mdc` | Template for multi-product routing |
| `cursorrules-template` | Template for root `.cursorrules` with routing |
| `writing-voice-template.mdc` | Template for encoding YOUR writing voice |
| `todo-tracker-template.md` | Template for active work tracking (In Progress, Up Next, Recently Completed) |

---

## Philosophy

We follow three principles:

1. **Lean rules**: If a rule exceeds 50 lines, split it. Every token in a rule competes with your actual work.
2. **Deep skills**: Skills save the most time for fragile workflows that silently fail when you skip a step.
3. **Project-specific routing**: Global rules for safety and patterns, project-specific rules for architecture and current phase.

The whole point is that you're training a system, not writing documentation. Every rule should exist because you got burned by its absence.

---

## How to Use This

### For Beginners

1. Copy `rules/` and `best-practices/` to `~/.cursor/`
2. Read `setup/setup-prompt.md` and paste it into Cursor Agent to scaffold project-specific rules
3. Start coding; the AI will automatically apply rules

### For Power Users

1. Copy everything to `~/.cursor/`
2. Customize the templates in `examples/` for your project structure
3. Create project-specific `.cursorrules` files that route to the right rules for each file type
4. Build skills for your most fragile workflows (deployment patterns, multi-step refactors)

### For Teams

1. Fork this repo and add your team's patterns
2. Share `project/.cursor/rules/` via git (project-specific rules)
3. Keep `~/.cursor/rules/` for personal preferences (global rules)
4. Document team conventions in rules, not in Notion docs that go stale

---

## Tips for Maintaining Your Rules

- **Rules are living documents.** When you hit a recurring bug, create a rule for it right then.
- **Keep rules concise.** If a rule exceeds 50 lines, split it into two.
- **Use the "wrong vs right" pattern.** Show concrete code examples of what NOT to do. The AI learns from contrast.
- **Skills save the most time for fragile workflows.** If a process silently fails when you skip a step, that's a perfect skill candidate.
- **Review periodically.** Every few weeks, scan your rules for anything outdated or redundant.

---

## Built By

**DoMoreWorld** — AI-powered purpose and productivity platform  
[domoreworld.com](https://domoreworld.com)

A Valyss LLC company  
[valyss.com](https://valyss.com)

Also check out: [Model Matchmaker](https://github.com/coyvalyss1/model-matchmaker) — LLM model selector

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Found a useful rule or skill? Open a PR.  
Have a question? Open an issue.

---

## License

MIT License — see [LICENSE](LICENSE) for details.
