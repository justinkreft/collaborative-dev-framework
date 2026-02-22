# Project Governance Framework

**Purpose:** Establish clear processes for decision-making, collaboration, and quality standards that work for any software project.

**Last Updated:** February 21, 2026

---

## Core Principles

### 1. **Transparency Over Secrecy**
- All decisions documented with rationale
- Architecture choices explained in DECISIONS.md
- Trade-offs made explicit
- "Why" matters more than "what"

### 2. **Quality Over Speed**
- Code must be tested before merging
- Documentation updated with code changes
- Technical debt tracked, not hidden
- Refactoring is valued work

### 3. **Simplicity Over Cleverness**
- Straightforward solutions preferred
- Avoid premature optimization
- Clear code > clever code
- Delete unused code immediately

### 4. **Collaboration Over Isolation**
- Regular check-ins on progress
- Ask questions early and often
- Share context proactively
- Code reviews are learning opportunities

### 5. **Iteration Over Perfection**
- Ship working code, then improve
- Feedback loops are essential
- Small, frequent changes > large rewrites
- Learn from production usage

---

## Decision-Making Framework

### When to Decide Alone
- Implementation details within agreed architecture
- Code style choices (follow existing patterns)
- Internal refactoring with same external behavior
- Bug fixes with clear root cause

### When to Discuss
- Architecture changes
- Adding new dependencies
- Breaking changes to APIs
- Performance vs. maintainability trade-offs
- Security-sensitive changes

### When to Document
**Always document:**
- Architecture decisions (DECISIONS.md)
- API changes (changelog)
- New features (README + docs)
- Breaking changes (UPGRADE.md if needed)

**Document when helpful:**
- Complex algorithms (code comments + docs)
- Non-obvious bug fixes (commit message + LESSONS.md)
- Performance optimizations (benchmark results)

### Decision Template

```markdown
## Decision: [Short Title]

**Date:** YYYY-MM-DD
**Status:** [Proposed | Accepted | Deprecated | Superseded]

**Context:**
What is the situation and problem we're facing?

**Options Considered:**
1. Option A - pros/cons
2. Option B - pros/cons
3. Option C - pros/cons

**Decision:**
We chose [option] because [rationale].

**Consequences:**
- Positive: ...
- Negative: ...
- Trade-offs: ...

**Follow-up:**
What needs to happen next?
```

---

## Code Quality Standards

### Definition of "Done"

Code is done when:
1. ✅ Feature works as specified
2. ✅ Tests written and passing
3. ✅ Documentation updated
4. ✅ Code reviewed (if applicable)
5. ✅ No new warnings or errors
6. ✅ Performance acceptable
7. ✅ Committed with clear message

### Testing Requirements

**Minimum:**
- Unit tests for business logic
- Edge cases covered
- Error handling tested

**Encouraged:**
- Integration tests for workflows
- Property-based tests for invariants
- Performance benchmarks for critical paths

**Not Required (but valuable):**
- UI/E2E tests (high maintenance)
- 100% code coverage (diminishing returns)

### Code Review Checklist

**Functionality:**
- [ ] Code does what it claims
- [ ] Edge cases handled
- [ ] Error messages helpful
- [ ] No obvious bugs

**Quality:**
- [ ] Tests included
- [ ] Documentation updated
- [ ] No unnecessary complexity
- [ ] Follows existing patterns

**Maintainability:**
- [ ] Variable names clear
- [ ] Functions focused
- [ ] Comments explain "why" not "what"
- [ ] No dead code

**Security:**
- [ ] Input validated
- [ ] No injection vulnerabilities
- [ ] Secrets not hardcoded
- [ ] Dependencies safe

---

## Communication Protocols

### Status Updates

**When to update:**
- When blocked (immediately)
- When approach changes (before implementing)
- When timeline shifts (as soon as known)
- Regular check-ins (daily/weekly as agreed)

**What to include:**
- What's done
- What's in progress
- What's blocked
- What's next

### Asking Questions

**Good questions include:**
- Context (what you're trying to do)
- What you've tried
- Specific confusion point
- Impact if unknown

**Example:**
> "I'm implementing the user auth flow and trying to decide between JWT and session cookies. I've read that JWT is stateless (good for scaling) but session cookies are more secure (can revoke immediately). Our app will have ~1000 concurrent users. Which should I use?"

**Not helpful:**
> "How do I do auth?"

### Giving Feedback

**Framework: Observation + Impact + Suggestion**

✅ Good:
> "I noticed the function is 200 lines long, which makes it hard to test individual pieces. Could we extract the validation logic into a separate function?"

❌ Not helpful:
> "This function is too long."

---

## Project Lifecycle

### Starting a New Project

1. **Define the problem** (README.md)
   - What problem does this solve?
   - Who is it for?
   - What are non-goals?

2. **Set up governance** (copy this file)
   - Adapt principles to project
   - Define decision-making process
   - Establish quality standards

3. **Create initial architecture** (DECISIONS.md)
   - Document major technology choices
   - Explain trade-offs
   - Plan for evolution

4. **Establish development workflow** (DEVELOPMENT.md)
   - How to set up locally
   - How to run tests
   - How to deploy
   - How to contribute

### During Development

1. **Regular evaluation** (monthly/quarterly)
   - Review DECISIONS.md - still valid?
   - Review LESSONS.md - what did we learn?
   - Review technical debt - what needs fixing?
   - Review processes - what's working?

2. **Continuous documentation**
   - Update docs with code
   - Capture decisions as made
   - Record lessons learned
   - Keep README current

3. **Quality checkpoints**
   - Before major releases
   - After significant refactoring
   - When bringing on new contributors
   - When technical debt accumulates

### Winding Down / Handoff

1. **Document current state**
   - Architecture overview
   - Known issues
   - Future roadmap
   - Dependencies and versions

2. **Knowledge transfer**
   - Key decisions and why
   - Tricky parts of codebase
   - Operational runbooks
   - Common issues and fixes

3. **Archive artifacts**
   - Code repository
   - Documentation
   - Design documents
   - Postmortem/retrospective

---


---

## Commit Message Standards

### Format

**Single-line commits (simple changes):**
```
Fix typo in README

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Multi-paragraph commits (complex changes):**
```
Short summary of change (50 chars or less)

Detailed explanation of what changed and why:
- Bullet point 1
- Bullet point 2
- Bullet point 3

Additional context:
- Why this approach was chosen
- What alternatives were considered
- What impact this has

Specific changes to files:
- file1.py: What changed
- file2.jsx: What changed

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

### Components

**1. Summary Line (Required)**
- 50 characters or less
- Imperative mood ("Add feature" not "Added feature")
- Capitalize first word
- No period at end

**2. Body (For non-trivial changes)**
- Separate from summary with blank line
- Explain WHAT and WHY, not HOW (code shows how)
- Use bullet points for lists
- Wrap at 72 characters

**3. Details (For complex changes)**
- Explain reasoning
- Note alternatives considered
- List file-by-file changes if many files
- Link to issues, docs, or decisions

**4. Co-Authorship (For AI collaboration)**
```
Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

### Examples from Real Projects

**Example 1: Feature Addition**
```
Match web UI output to CLI/TUI exactly

Backend changes:
- Add pre_explosion field to RollResult (solo and multiplayer)
- Add threshold field to all results
- Add all CLI metrics: total_sum, max_success_bound, overfill, verified_optimal, remainder_count
- Calculate pre-explosion stats and store in response when exploding
- Fix WebSocket accept() placement (moved to endpoint, removed from ConnectionManager)

Frontend changes:
- Rewrite RollResult.jsx to match CLI output exactly
- Rewrite SoloRoller.jsx to match CLI output exactly
- Add explosion indicator (💥 EXPLOSIONS: ON / ⦿ EXPLOSIONS: OFF)
- Add solver mode messages matching CLI
- Add dice legend (original, 10, exploded)
- Add totals section with threshold and all CLI fields
- Add explosion rolls section (post-explosion only)
- Add remainder dice count display
- Format success groups as #01, #02 with commas and (sum=X)
- Show detailed status messages (optimal variants)
- Add PRE-EXPLOSION / POST-EXPLOSION comparison view
- Add CSS for all new sections

Result: Solo and multiplayer modes now identical, matching CLI/TUI completely.

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Example 2: Documentation**
```
Add session documentation and ignore database files

- DEPLOYMENT_PLAN.md: Railway deployment guide
- QUICK_MULTIPLAYER_TEST.md: 10-minute localhost test procedure
- RETRO_CLI_PARITY.md: Retrospective on CLI matching work
- .gitignore: Add *.db to ignore SQLite databases

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Example 3: Framework Update**
```
Add lessons from nexus_roller web UI CLI parity session

New lessons added:
1. Read the Reference Implementation First
   - Replication workflow checklist
   - Pattern: Read source code before building
   - Example: Should have read formatters.py first

2. "Match Exactly" Means EVERYTHING
   - Surface vs deep understanding
   - Details that matter (labels, formatting, order)
   - User perspective on intentional design

3. Effective User Feedback Patterns
   - What worked: Direct communication
   - Escalation patterns
   - "Read first, build second" pattern

Context: 2026-02-21 session implementing web UI to match CLI/TUI
Source: nexus_roller_web/RETRO_CLI_PARITY.md

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

### When to Use Which Format

**Single-line (no body):**
- Typo fixes
- Simple formatting changes
- Small refactors
- Documentation tweaks
- One-file, obvious changes

**Multi-paragraph (with body):**
- Feature additions
- Bug fixes that need explanation
- Architecture changes
- Multiple files changed
- Non-obvious decisions
- Anything future-you will wonder about

**Detailed (with context):**
- Major features
- Breaking changes
- Performance improvements
- Security fixes
- Framework updates
- Anything that needs "why" explained

### Anti-Patterns

❌ **Vague messages:**
```
Fix bug
Update code
WIP
```

❌ **Too technical (only "how"):**
```
Change variable x to y and refactor function z
```

❌ **Missing context:**
```
Add feature
```
(What feature? Why? What does it do?)

❌ **Missing co-authorship:**
(When AI did significant work, credit it)

### Guidelines

**Think about future readers:**
- 6 months from now, will you remember what this was about?
- Can someone understand the change without reading the code?
- Is it clear why this approach was chosen?

**Be specific:**
- "Fix login bug" → "Fix 401 error when user password contains special characters"
- "Update README" → "Add installation instructions for M1 Macs"
- "Refactor code" → "Extract validation logic from 200-line function for testability"

**Explain reasoning:**
- Not just what changed
- Why it changed
- What alternatives were considered
- What impact it has



---

## Quality Gates

### Before Committing
- [ ] Code compiles/runs
- [ ] Tests pass
- [ ] No debug code left in
- [ ] Commit message clear

### Before Merging to Main
- [ ] All tests pass
- [ ] Documentation updated
- [ ] Breaking changes noted
- [ ] Code reviewed (if team project)

### Before Releasing
- [ ] All features tested
- [ ] Performance acceptable
- [ ] Security reviewed
- [ ] Migration path documented
- [ ] Rollback plan exists
- [ ] Changelog updated

---

## Conflict Resolution

### When Disagreeing on Approach

1. **Understand both positions**
   - State the other person's view back to them
   - Identify shared goals
   - Clarify constraints

2. **Evaluate objectively**
   - What does data/evidence suggest?
   - What are the long-term implications?
   - What's reversible vs. irreversible?

3. **Decide and commit**
   - If no consensus, person closest to the work decides
   - If high stakes, escalate or seek outside input
   - Once decided, everyone commits
   - Document decision and rationale

4. **Review and learn**
   - Set a review date
   - Evaluate outcome
   - Update understanding
   - Document in LESSONS.md

### When Something Goes Wrong

1. **Fix the immediate problem**
2. **Understand the root cause**
3. **Document what happened** (LESSONS.md)
4. **Implement safeguards**
5. **No blame, just learning**

---

## Adapting This Framework

This governance framework is a starting point. Adapt it to your project's needs:

**For solo projects:**
- Simplify decision documentation
- Focus on clarity for future you
- Keep what adds value, drop what doesn't

**For small teams (2-5):**
- Informal code reviews
- Quick sync meetings
- Shared decision-making
- Light documentation

**For larger teams (5+):**
- Formal code review process
- Defined roles and responsibilities
- Structured decision-making
- Comprehensive documentation

**For open source:**
- Public decision-making
- Contributor guidelines
- Code of conduct
- Governance model (BDFL, committee, etc.)

---

## Success Metrics

**Process metrics (how we work):**
- Time from idea to production
- Code review turnaround time
- Deployment frequency
- Documentation freshness

**Quality metrics (what we produce):**
- Test coverage (not a goal, but a signal)
- Bug rate in production
- Performance benchmarks
- Security vulnerabilities

**Team metrics (how we feel):**
- Developer satisfaction
- Onboarding time for new contributors
- Knowledge silos (bus factor)
- Burnout indicators

**Pick 3-5 metrics that matter for your project. Don't measure everything.**

---

## Templates

### DECISIONS.md Structure
```markdown
# Architecture Decisions

## Active Decisions
- Decision 001: [Title] - [Date]
- Decision 002: [Title] - [Date]

## Deprecated Decisions
- Decision 000: [Title] - [Date] - Superseded by Decision 001
```

### LESSONS.md Structure
```markdown
# Lessons Learned

## [Category: Architecture | Testing | Process | etc.]

### Lesson: [Short Title]
**Date:** YYYY-MM-DD
**Context:** What was the situation?
**What Happened:** What went wrong or right?
**Lesson:** What did we learn?
**Action:** What changed as a result?
```

### DEVELOPMENT.md Structure
```markdown
# Development Guide

## Setup
[How to get started]

## Development Workflow
[How to make changes]

## Testing
[How to run tests]

## Deployment
[How to deploy]

## Troubleshooting
[Common issues]
```

---

## Maintenance

**Review this document:**
- When starting a new project (adapt as needed)
- Quarterly during project (update what's not working)
- When onboarding new contributors (ensure it's current)
- When project wraps up (capture final lessons)

**This document should evolve with your understanding of effective collaboration.**
