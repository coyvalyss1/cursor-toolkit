# Active Work Tracker

Last synced: [DATE]

## In Progress

- [ ] **[Task Title]** | Product: [Product Name] | Plan: `[plan_id]` | Agent: [agent_name]
  - Status: [Current status summary and what remains to be done]

## Up Next (Prioritized)

- [ ] **[Task Title]** | Product: [Product Name] | Plan: `[plan_id]` | [Brief description of the task]
- [ ] **[Task Title]** | Product: [Product Name] | Plan: `[plan_id]` | [Brief description of the task]
- [ ] **[Task Title]** | Product: [Product Name] | Plan: `[plan_id]` | [Brief description of the task]

## Recently Completed

- [x] **[Task Title]** | Product: [Product Name] | Completed: YYYY-MM-DD | [Brief description of what was accomplished]
- [x] **[Task Title]** | Product: [Product Name] | Completed: YYYY-MM-DD | [Brief description of what was accomplished]
- [x] **[Task Title]** | Product: [Product Name] | Completed: YYYY-MM-DD | [Brief description of what was accomplished]

---

## Usage Notes

### Structure
- **In Progress**: Currently active work (ideally 1-2 items max to maintain focus)
- **Up Next**: Prioritized backlog (ordered by importance/impact)
- **Recently Completed**: Last 10-15 completed items for context

### Task Format
```markdown
- [ ] **[Title]** | Product: [X] | Plan: `[plan_id]` | [Description/Context]
```

**Components:**
- **Title**: Brief, action-oriented (e.g., "Fix Gallery Upload", "Migrate to Gemini")
- **Product**: Which product/area this affects (ProjectA, Business, All, etc.)
- **Plan**: Reference to plan file ID if applicable (use backticks for monospace)
- **Description**: One-line context or current status

### Best Practices
- Move completed items to "Recently Completed" with completion date
- Keep "In Progress" minimal (1-3 items) to avoid context switching
- Archive old completed items after 2-3 weeks
- Update "Last synced" date when making changes
- Use consistent product abbreviations throughout

### Product Tags (Customize to Your Projects)
- **All**: Cross-project or infrastructure
- **ProjectA**: [Your first product/project]
- **ProjectB**: [Your second product/project]
- **Business**: Business/marketing initiatives

### When to Create New Entries
- **Do create**: For substantial features, integrations, or multi-session work
- **Don't create**: For quick fixes, typos, or sub-1-hour tasks (log in SESSION_LOG.md instead)

### Integration with Plans
- Link to plan files using the plan ID in backticks: `plan_id_here`
- Plan IDs typically found in `.cursor/plans/` directory
- Not all tasks require a plan (some are straightforward implementations)

### Integration with Session Log
- This tracker = **what** you're working on (high-level roadmap)
- SESSION_LOG.md = **how** you did it (technical details, decisions, files modified)
- Feature docs = **why** and architectural patterns (for substantial new features)
