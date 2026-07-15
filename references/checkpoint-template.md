# checkpoint.md Template

## Overview

`docs/checkpoint.md` is a living document that tracks project progress. It **MUST be updated before every git commit** to maintain an accurate record of development status.

---

## Template

```markdown
# checkpoint.md - [Project Name]

> **⚠️ UPDATE THIS FILE BEFORE EVERY COMMIT**

---

## Current Status

| Metric | Value |
|--------|-------|
| **Overall Progress** | [X]% |
| **Current Phase** | [Milestone Name] |
| **On Track?** | ✅ Yes / ⚠️ At Risk / ❌ Behind |
| **Last Updated** | [YYYY-MM-DD HH:MM] |
| **Last Commit** | [commit hash or "pending"] |

### Tasks Status

See `docs/tasks.md` for the live task list. Summary:

| Metric | Value |
|--------|-------|
| **Active Milestone** | [name] |
| **Tasks Complete** | X / Y |
| **Tasks In Progress** | [count] |
| **Tasks Blocked** | [count] |
| **Active Plan** | `docs/plans/YYYY-MM-DD-<feature>.md` (if any) |

### Progress by Milestone

| Milestone | Status | Progress | Notes |
|-----------|--------|----------|-------|
| 1. Foundation | ✅ Complete | 100% | |
| 2. Core Features | 🔄 In Progress | 65% | [Brief note] |
| 3. Analytics | ⏳ Not Started | 0% | |
| 4. Dashboard | ⏳ Not Started | 0% | |
| 5. Testing | ⏳ Not Started | 0% | |
| 6. Launch | ⏳ Not Started | 0% | |

---

## Completed Items ✅

### This Session
- [x] [Task completed in current work session]
- [x] [Task completed in current work session]

### Previous Sessions
<details>
<summary>Click to expand completed items</summary>

#### [Date]
- [x] [Completed task]
- [x] [Completed task]

#### [Earlier Date]
- [x] [Completed task]

</details>

---

## In Progress 🔄

| Task | Started | Est. Remaining | Assignee |
|------|---------|---------------|----------|
| [Task name] | [Date] | [X hours] | [Name] |
| [Task name] | [Date] | [X hours] | [Name] |

### Current Focus
> [1-2 sentences describing what's actively being worked on RIGHT NOW]

---

## Blockers 🚫

| Blocker | Impact | Owner | Status | Resolution |
|---------|--------|-------|--------|------------|
| [Blocker description] | [High/Med/Low] | [Name] | 🔴 Active | [Action needed] |
| [Blocker description] | [High/Med/Low] | [Name] | 🟡 In Progress | [Current action] |
| [Blocker description] | [High/Med/Low] | [Name] | 🟢 Resolved | [How resolved] |

---

## Next Actions 📋

### Immediate (Next Commit)
1. [ ] [Specific task to do next]
2. [ ] [Specific task to do next]

### Short-term (This Week)
- [ ] [Task]
- [ ] [Task]
- [ ] [Task]

### Upcoming (Next Week)
- [ ] [Task]
- [ ] [Task]

---

## Time Tracking ⏱️

### This Session
| Task | Estimated | Actual | Variance |
|------|-----------|--------|----------|
| [Task] | [X hrs] | [X hrs] | [+/- X hrs] |

### Cumulative
| Milestone | Estimated | Actual | Variance |
|-----------|-----------|--------|----------|
| Foundation | [X hrs] | [X hrs] | [+/- X hrs] |
| Core Features | [X hrs] | [X hrs] | [+/- X hrs] |
| **Total** | **[X hrs]** | **[X hrs]** | **[+/- X hrs]** |

---

## Scope Changes 📝

| Date | Change | Reason | Impact |
|------|--------|--------|--------|
| [Date] | [What changed] | [Why] | [+/- X hours] |

---

## Technical Decisions Log 🔧

| Date | Decision | Rationale | Alternatives Considered |
|------|----------|-----------|------------------------|
| [Date] | [Decision made] | [Why] | [What else was considered] |

---

## Questions & Clarifications ❓

### Open Questions
- [ ] [Question needing answer]
- [ ] [Question needing answer]

### Resolved
- [x] [Question] → [Answer/Resolution]

---

## Change Log 📜

| Date | Commit | Summary |
|------|--------|---------|
| [YYYY-MM-DD] | [hash] | [Brief description of changes] |
| [YYYY-MM-DD] | [hash] | [Brief description of changes] |
| [YYYY-MM-DD] | [hash] | [Brief description of changes] |

---

## Notes 📌

### Session Notes
[Free-form notes about current session, learnings, observations]

### Handoff Notes
[Notes for next person picking up work, or future self]

---

*Last verified: [Date/Time]*  
*Next checkpoint: [Planned date/time or "Before next commit"]*
```

---

## Update Protocol

### Before EVERY Commit

1. **Update "Last Updated"** timestamp
2. **Update "Last Commit"** with new commit hash (after committing, amend or note as pending)
3. **Move completed tasks** from "In Progress" to "Completed Items"
4. **Update progress percentages** for current milestone
5. **Update "Current Focus"** if it changed
6. **Add to Change Log** with commit summary
7. **Update time tracking** if tracking hours

### Quick Update Checklist

```markdown
Pre-commit checklist:
- [ ] Updated "Last Updated" timestamp
- [ ] Moved completed tasks to ✅ section
- [ ] Updated progress percentage
- [ ] Updated "Current Focus" 
- [ ] Added blockers (if any)
- [ ] Updated "Next Actions"
- [ ] Added entry to Change Log
```

### Commit Message Convention

Reference `docs/checkpoint.md` updates in commit messages:

```
feat: implement user authentication

- Added login/logout endpoints
- Integrated JWT tokens
- Updated docs/checkpoint.md (Foundation 85% → 100%)
```

---

## Status Indicators

### Overall Project Status

| Status | Meaning | Action |
|--------|---------|--------|
| ✅ On Track | Progress matches plan | Continue as planned |
| ⚠️ At Risk | Minor delays or issues | Monitor closely, consider mitigation |
| ❌ Behind | Significant delays | Escalate, re-scope, or get help |

### Task/Milestone Status

| Icon | Meaning |
|------|---------|
| ⏳ | Not started |
| 🔄 | In progress |
| ✅ | Complete |
| 🚫 | Blocked |
| ⏸️ | Paused/On hold |

### Blocker Severity

| Icon | Severity | Response Time |
|------|----------|---------------|
| 🔴 | Active/Critical | Immediate |
| 🟡 | In Progress | Within 24 hours |
| 🟢 | Resolved | Document resolution |

---

## Best Practices

### DO:
- Update before every commit (non-negotiable)
- Be specific about what's done vs in progress
- Track actual time vs estimated
- Note blockers immediately when discovered
- Keep the Change Log accurate

### DON'T:
- Skip updates "just this once"
- Leave progress percentages stale
- Forget to move tasks between sections
- Ignore time tracking variance
- Let blockers go undocumented

### Tips for Effective Checkpoints

1. **Time-box updates:** Should take <5 minutes
2. **Be honest:** Optimistic progress reports hurt everyone
3. **Link to specifics:** Reference files, PRs, or line numbers
4. **Note learnings:** Future you will thank present you
5. **Flag early:** Better to raise concerns early than late

---

## Integration with Other Docs

| Document | How `docs/checkpoint.md` Relates |
|----------|--------------------------|
| **`docs/tasks.md`** | `checkpoint.md` summarizes; `tasks.md` is canonical |
| **`docs/planning.md`** | `checkpoint.md` notes any architectural decisions |
| **`docs/prd.md`** | `checkpoint.md` tracks feature completion against the PRD |
| **`docs/plans/...`** | `checkpoint.md` links the active superpowers plan being executed |

### Cross-Reference Pattern

When completing a task from `docs/tasks.md`:
```markdown
## Completed Items ✅
- [x] Implement user authentication (`docs/tasks.md`: Milestone 1, Item 3)
```

When making a technical decision:
```markdown
## Technical Decisions Log
| Date | Decision | Rationale |
|------|----------|-----------|
| 2024-01-15 | Use Prisma ORM | See `docs/planning.md` Section 3 |
```
