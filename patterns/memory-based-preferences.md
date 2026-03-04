# Memory-Based Preferences

## Overview

A pattern for making AI assistants remember team/individual preferences across sessions, enabling consistent behavior without repeated instructions.

## The Problem

**Without persistent memory:**
- Users repeat the same instructions every session
- Inconsistent behavior across conversations
- Team members get different experiences
- Hard to maintain standards across projects

**Example friction:**
```
Session 1: "Always cite sources with URLs"
Session 2: "Remember to cite sources with URLs"
Session 3: "Like I said before, cite sources..."
```

## The Solution

Use a persistent memory file that:
1. Loads automatically at session start
2. Contains working preferences and standards
3. Updates as patterns are discovered
4. Stays under size limits for consistent loading

---

## Implementation

### Location

**Per-project memory:**
```
~/.ai-assistant/projects/<project-name>/memory/MEMORY.md
```

**Global memory:**
```
~/.ai-assistant/memory/MEMORY.md
```

### Structure

```markdown
# Memory - Working Preferences

## [Category Name]

[Preference or pattern description]

**Example:**
[Concrete example of the preference in action]

**Related files:**
[Links to detailed documentation]
```

### Size Limits

**Critical:** Keep under 200 lines (or tool-specific limit)
- Exceeding limit → memory gets truncated
- Truncation → preferences lost
- Keep main file concise
- Link to detailed docs for complex patterns

---

## What to Save

### ✅ Save These

**Stable patterns confirmed across multiple interactions:**
```markdown
## Citation Standards

When providing information from knowledge bases:
- Include inline citations [1], [2] throughout
- Add Sources section at end with:
  - Quote or context
  - Actual discussion URL
  - Document title

**Example:**
The policy changed in June 2025 [1]

### Sources
[1] "Policy update announcement" - https://internal.example.com/...
```

**Key architectural decisions:**
```markdown
## Project Architecture

- Database: PostgreSQL (decided 2026-03-01)
- API: REST with FastAPI
- Frontend: React + TypeScript
- Deployment: Railway

See: docs/ARCHITECTURE.md for details
```

**User preferences for workflow:**
```markdown
## Working Style

- Response depth: Default to Balanced (can specify Quick/Deep)
- Commit messages: Use conventional commits format
- Code style: Follow existing patterns, run formatter before commit
```

**Solutions to recurring problems:**
```markdown
## Common Issues

### Database Connection Timeouts
- Symptom: "connection pool exhausted"
- Fix: Increase pool size in config.py
- See: docs/troubleshooting.md#db-timeouts
```

### ❌ Don't Save These

**Session-specific context:**
- Current task details
- In-progress work
- Temporary state

**Incomplete information:**
- Unverified conclusions
- Speculative approaches
- "I think maybe..." statements

**Duplicates of existing docs:**
- Don't copy entire ARCHITECTURE.md into memory
- Link to it instead

**Anything that contradicts project docs:**
- CLAUDE.md or README.md take precedence
- Memory supplements, not replaces

---

## Maintenance Patterns

### When to Update

**Add immediately (explicit user request):**
```
User: "Always use bun instead of npm"
AI: [Updates memory with this preference]
```

**Add after confirmation (pattern discovered):**
```
Session 1: User prefers X
Session 2: User confirms X again
AI: [Updates memory - pattern confirmed]
```

**Remove when wrong:**
```
User: "Actually, stop doing X"
AI: [Removes X from memory]
```

### Organization

**Semantic, not chronological:**

❌ **Bad (chronological):**
```markdown
## March 2026 Decisions
- Use PostgreSQL
- Prefer TypeScript
- ...
```

✅ **Good (semantic):**
```markdown
## Technology Stack
- Database: PostgreSQL
- Languages: TypeScript, Python

## Code Standards
- Prefer functional patterns
- Always type annotate
```

### Linking Strategy

**Main memory stays small, links to details:**

```markdown
## Testing Strategy

We use pytest with coverage >80%

**Details:** See testing-guide.md for:
- How to write tests
- Coverage exceptions
- CI/CD integration
```

---

## Example Memory File

```markdown
# Memory - Project X Preferences

## Response Patterns

### Information Retrieval
- Default depth: Balanced (user can specify Quick/Deep)
- Always cite sources with URLs
- Use numbered citations [1], [2] throughout

### Code Changes
- Read existing code before suggesting changes
- Follow existing patterns over "best practices"
- Run tests before committing

## Technology Decisions

### Stack
- Backend: FastAPI + PostgreSQL
- Frontend: React + TypeScript
- See: ARCHITECTURE.md for rationale

### Deployment
- Platform: Railway
- Environment vars in .env (never commit)
- See: docs/deployment.md

## Working Style

### Communication
- Be direct, skip pleasantries
- Present options with trade-offs
- Challenge assumptions when warranted

### Session Management
- Create PICKUP.md for sessions >2 hours
- Update LESSONS.md when bugs found
- Commit with Co-Authored-By line

## Common Patterns

### Database Migrations
Always backup before migration
See: docs/migrations.md

### Error Handling
Log errors, don't silence them
See: src/utils/errors.py for patterns

---

**File size:** Keep under 200 lines. Link to detailed docs.
**Last updated:** 2026-03-04
```

---

## Benefits

### For Individuals
- Don't repeat yourself
- Consistent experience
- Assistant "knows" your preferences
- Builds on previous learnings

### For Teams
- Shared standards enforced automatically
- New team members get same guidance
- Patterns persist beyond individuals
- Living documentation

### For AI Assistants
- Clear behavioral expectations
- No ambiguity about preferences
- Can reference past decisions
- Continuous improvement loop

---

## Anti-Patterns

❌ **Memory bloat:**
- Copying entire docs into memory
- Saving everything "just in case"
- Not pruning outdated info

❌ **Contradictory preferences:**
- Memory says X, docs say Y
- No clear source of truth
- User has to resolve conflicts

❌ **Session details in memory:**
- "Currently working on feature X"
- "Bug in progress: Y"
- These belong in session notes, not memory

❌ **No maintenance:**
- Preferences change, memory doesn't
- Outdated patterns stick around
- File grows until truncated

---

## Integration with Other Patterns

**Tiered Response System:**
```markdown
## Response Preferences
- Default depth: Balanced
- For documentation questions: Deep
- For quick lookups: Quick
```

**Source Attribution:**
```markdown
## Citation Requirements
- Always include Sources section
- Format: [1] "Quote" - URL
- Link to internal discussions when available
```

**Session Management:**
```markdown
## Session Patterns
- Create PICKUP.md before multi-day breaks
- Update LESSONS.md when fixing bugs
- Run tests before committing
```

---

## Success Metrics

- **Reduced repetition:** User stops re-stating preferences
- **Consistent behavior:** Same experience across sessions
- **Faster onboarding:** New team members get instant context
- **Living knowledge:** Memory evolves with project

**Red flags:**
- User frequently corrects behavior (memory wrong or incomplete)
- Memory file exceeds size limit (too much detail)
- Conflicts between memory and project docs

---

## Related Patterns

- Tiered Response System - Store user's default depth preference
- Source Attribution Standards - Store citation format requirements
- Session Management - When to update memory vs session notes

---

**Remember:** Memory should be stable patterns, not session state. If it changes every day, it doesn't belong in memory.
