# Effective Collaboration Patterns

**Purpose:** Document proven patterns for productive human-AI collaboration on software projects, applicable to any project type.

**Last Updated:** February 21, 2026

---

## Core Collaboration Model

### Roles and Responsibilities

**Human (Project Owner):**
- Sets vision and priorities
- Makes final decisions
- Provides domain expertise
- Defines "done"
- Reviews and approves work

**AI Assistant (Implementation Partner):**
- Implements solutions
- Suggests approaches
- Identifies issues proactively
- Documents decisions
- Executes technical tasks

**Shared Responsibilities:**
- Quality standards
- Testing strategy
- Documentation
- Learning from mistakes

---

## Effective Communication Patterns

### Starting a Work Session

**❌ Ineffective:**
> "Fix the app"

**✅ Effective:**
> "The user authentication is broken - users can't log in. I'm seeing a 401 error in the console. Let's debug this and fix it. Start by checking the backend auth endpoint."

**Why:** Provides context, specific issue, and starting point.

### Requesting Features

**❌ Ineffective:**
> "Add a search feature"

**✅ Effective:**
> "Add a search feature to the user list page. It should filter by username and email, update results as you type, and handle at least 10,000 users without performance issues. Use the existing table component styling."

**Why:** Specific requirements, constraints, and integration points.

### Asking for Advice

**❌ Ineffective:**
> "Which database should I use?"

**✅ Effective:**
> "I'm building a dice rolling app with ~1000 concurrent users, storing roll history and room state. Data is relational (users, rooms, rolls). Should I use SQLite for MVP then migrate to PostgreSQL, or start with PostgreSQL? I want fast prototyping but also production-ready code."

**Why:** Context, scale, constraints, priorities stated clearly.

### Giving Feedback

**❌ Ineffective:**
> "This code is bad"

**✅ Effective:**
> "This 200-line function is hard to test and understand. Can we extract the validation logic into a separate function? That would make it easier to test each piece independently."

**Why:** Specific observation, impact, and constructive suggestion.

---

## Work Session Patterns

### Pattern 1: Exploration Then Plan

**When to use:** Complex feature, unclear requirements

**Process:**
1. **Explore:** "Let's look at how authentication currently works"
2. **Understand:** Review code, identify patterns
3. **Discuss:** Talk through approaches
4. **Plan:** Document the approach
5. **Execute:** Implement with plan as guide

**Example conversation:**
```
Human: "I want to add multiplayer to my dice app"
AI: "Let me explore the current architecture first..."
[Exploration happens]
AI: "I see you're using a REST API. For multiplayer you'll need
     WebSockets. Here's a plan..." [Creates detailed plan]
Human: "Looks good, let's do it"
AI: [Executes plan]
```

### Pattern 2: Direct Implementation

**When to use:** Clear requirements, well-defined task

**Process:**
1. **Request:** "Add a logout button to the header"
2. **Clarify if needed:** "Should it redirect to login page?"
3. **Execute:** Implement directly
4. **Verify:** Test and confirm

**Example conversation:**
```
Human: "Add input validation - dice count must be 1-100"
AI: "Adding validation to the input handler..." [Implements]
AI: "Done. Validation added with error messages for invalid input."
```

### Pattern 3: Iterative Refinement

**When to use:** Evolving requirements, unclear final state

**Process:**
1. **Start simple:** "Let's add basic search first"
2. **Review:** "This works but performance is slow"
3. **Improve:** "Adding debouncing and caching"
4. **Review:** "Better, but can we highlight matches?"
5. **Enhance:** "Adding match highlighting"

**Example conversation:**
```
Human: "Make the UI look better"
AI: "That's broad - let me show you what I can improve..." [Lists options]
Human: "Start with the spacing and colors"
AI: [Implements spacing improvements]
Human: "Good! Now the buttons look weird"
AI: [Refines buttons]
```

### Pattern 4: Problem-Solution Discovery

**When to use:** Bug fixing, performance issues, unclear root cause

**Process:**
1. **Describe symptom:** "App is slow with 1000 users"
2. **Investigate:** "Let me check database queries..."
3. **Diagnose:** "Found it: N+1 query problem"
4. **Propose solution:** "We can add eager loading"
5. **Implement:** Fix with verification

**Example conversation:**
```
Human: "The app crashes when I click Roll Dice"
AI: "Let me check the console errors and stack trace..."
AI: "Found it: WebSocket being accepted twice. Here's the fix..."
Human: "Test it first"
AI: [Writes test, implements fix, runs test]
AI: "Fixed and tested ✓"
```

---

## Decision-Making Patterns

### Quick Decisions (< 5 minutes)

**Good for:**
- Code style choices
- Variable naming
- Error message wording
- Test organization

**Pattern:**
```
AI: "Should I name this validateInput or checkInput?"
Human: "validateInput - matches our existing pattern"
AI: "Got it" [proceeds]
```

### Collaborative Decisions (5-30 minutes)

**Good for:**
- Architecture choices
- Technology selection
- API design
- Performance trade-offs

**Pattern:**
```
AI: "For real-time updates, here are three options:
     1. WebSockets - true real-time, more complex
     2. Server-sent events - one-way, simpler
     3. Polling - simplest, higher latency

     For your use case (dice rolling game), I recommend
     WebSockets because..."
Human: "What about scaling? Will WebSockets work with 10k users?"
AI: "Good question. WebSockets can handle that but you'll need..."
     [Discussion continues until decision made]
```

### Research-Heavy Decisions (30+ minutes)

**Good for:**
- Major architectural changes
- Security-critical features
- Data model design
- Production deployment strategy

**Pattern:**
```
Human: "I need to plan the authentication system"
AI: "This is complex. Let me research current best practices,
     review your requirements, and create a comprehensive plan..."
     [Creates detailed plan document]
Human: [Reviews plan, asks questions]
AI: [Refines based on feedback]
Human: "Approved, let's implement"
```

---

## Quality Assurance Patterns

### Pattern: Test-Driven Development

**When to use:** Critical business logic, complex algorithms

```
Human: "Add a dice rolling optimizer that maximizes successes"
AI: "This is complex logic. Let me write tests first..."
AI: [Writes tests defining behavior]
Human: [Reviews tests] "Add test for edge case: all 10s"
AI: [Adds test, implements algorithm, verifies all tests pass]
```

### Pattern: Code Review Before Merge

**When to use:** Significant changes, new features

```
AI: "I've implemented the multiplayer feature. Here's a summary
     of changes: [lists changes]. Ready for review."
Human: "Show me the WebSocket handling code"
AI: [Shows code]
Human: "What happens if user disconnects during a roll?"
AI: "Good catch! Let me add cleanup logic..."
```

### Pattern: Regression Testing

**When to use:** Bug fixes, refactoring

```
Human: "Fix the login bug but don't break anything else"
AI: "I'll run all existing tests first to establish baseline..."
AI: [Runs tests, fixes bug, runs tests again]
AI: "Fix applied. All 47 existing tests still pass ✓"
```

---

## Problem-Solving Patterns

### Pattern: Systematic Debugging

**Process:**
1. Reproduce the issue
2. Isolate the cause
3. Verify the fix
4. Prevent recurrence

**Example:**
```
Human: "Users report rolls not saving"
AI: "Let me check the database writes..." [Investigates]
AI: "Found it: transaction not committing. Here's the fix..."
AI: [Implements fix]
AI: "Also adding a test so this doesn't happen again"
```

### Pattern: Performance Investigation

**Process:**
1. Measure current performance
2. Identify bottlenecks
3. Propose optimizations
4. Measure improvement

**Example:**
```
Human: "App is slow with 200 dice"
AI: "Let me benchmark the current performance..."
AI: "Exact solver taking 5 seconds. Options:
     1. Add timeout and fallback (quick)
     2. Optimize algorithm (complex)
     3. Use heuristic for large pools (pragmatic)"
Human: "Option 3 sounds good"
AI: [Implements, benchmarks] "Now 50ms instead of 5s ✓"
```

### Pattern: Edge Case Discovery

**Process:**
1. Identify happy path
2. List potential edge cases
3. Test each case
4. Handle or document

**Example:**
```
AI: "I've implemented user login. Let me think about edge cases:
     - What if username is empty?
     - What if password is wrong?
     - What if user is already logged in?
     - What if database is down?

     Let me add handling for each..."
```

---

## Documentation Patterns

### Pattern: Code Changes → Doc Updates

**Rule:** Documentation changes with code, not after

```
AI: "I've added the search feature. Updating README with:
     - How to use search
     - Search limitations
     - Configuration options"
```

### Pattern: Decision Documentation

**When:** Architecture changes, technology choices, trade-offs

```
AI: "We chose SQLite for the MVP. Let me document this in
     DECISIONS.md with:
     - Why SQLite now
     - When to migrate to PostgreSQL
     - Migration path"
```

### Pattern: Lessons Captured

**When:** Bugs fixed, mistakes made, insights gained

```
Human: "That bug cost us 2 hours"
AI: "Let me document this in LESSONS.md:
     - What happened
     - Why it happened
     - How we fixed it
     - How to prevent it"
```

---

## Feedback Loops

### Pattern: Continuous Validation

**Approach:** Regular check-ins during long tasks

```
AI: "Starting multiplayer implementation. I'll check in after:
     1. Backend WebSocket setup
     2. Frontend connection logic
     3. Basic message passing"

[After step 1]
AI: "WebSocket server running. Test it before I continue?"
Human: [Tests] "Works! Continue"
```

### Pattern: Show, Don't Tell

**Approach:** Demo working code frequently

```
AI: "The TUI is ready. Run `python3 nexus_roller_tui.py` to see it"
Human: [Tries it] "Nice! But the colors are hard to read"
AI: "Let me adjust the color scheme..."
```

### Pattern: Measure and Compare

**Approach:** Use metrics to guide decisions

```
AI: "I refactored the solver. Before: 100ms, After: 15ms.
     Should I keep the refactoring?"
Human: "Yes, that's a big improvement"
```

---

## Responsiveness and Interrupts

### Pattern: Status Updates During Work

**Rule:** Never work in silence for more than 30 seconds

**Why:** User needs to know system is working, not frozen

**Implementation:**
```
AI: "Reading file..." [reads]
AI: "Making changes to SoloRoller.jsx..." [edits]
AI: "Done. Explosion button moved to bottom."
```

**Timing Guidelines:**
- **Simple operations (<10s):** No status needed, just complete
- **Medium operations (10-30s):** One status update
- **Long operations (>30s):** Progress updates every 20-30s
- **Very long operations (>1 min):** Consider if something is wrong

**Example - Good:**
```
Human: "Refactor the database schema"
AI: "Starting refactor..."
AI: "Analyzing current schema..." [20s]
AI: "Creating migration file..." [15s]
AI: "Testing migration on sample data..." [30s]
AI: "Done. Schema refactored with zero downtime migration."
```

**Example - Bad:**
```
Human: "Refactor the database schema"
[3 minutes of silence]
AI: "Done"
```

**Why this matters:**
- Silence = uncertainty ("Is it working? Is it frozen?")
- Status updates = confidence ("System is working, progress happening")
- User can interrupt if something seems wrong
- Especially critical when background tasks are running

### Pattern: The Stop Protocol

**Rule:** "Stop" means stop immediately, not "finish first"

**When user says "stop":**
1. **Immediately acknowledge:** "Stopping."
2. **Ask for clarification:** "Cancel current work or pause to discuss?"
3. **Wait for instruction:** Don't continue working
4. **Clean up if needed:** Stop background processes, save state

**Example - Correct:**
```
Human: "Add search feature to user list"
AI: "Reading user list component..."
Human: "stop"
AI: "Stopping. Do you want to:
     1. Cancel this work completely
     2. Pause so we can discuss the approach
     3. Continue but with different requirements"
Human: "Option 2 - I want to change the requirements"
AI: "Got it. What should the search do differently?"
```

**Example - Wrong:**
```
Human: "Add search feature"
AI: "Reading files..."
Human: "stop"
[AI continues working for 3 minutes]
AI: "Done! Added search feature."
Human: [frustrated - I said stop!]
```

**Why this matters:**
- Respects user's time and authority
- User might have realized something is wrong
- Requirements might have changed
- User might need to do something else urgently
- "Finish first" wastes time if approach is wrong

**Background tasks during stop:**
- Stop dev servers if they're running unnecessarily
- Don't start new operations
- Clean shutdown is better than hung processes

### Pattern: Responsiveness Contract

**Agreement between human and AI:**

**AI commits to:**
- Acknowledge requests within 5 seconds
- Provide status updates for operations >30s
- Stop immediately when requested
- Never work in silence for >1 minute
- Report if something seems stuck or slow

**Human can expect:**
- No mystery hangs or freezes
- Clear communication during work
- Ability to interrupt at any time
- Transparency about what's happening

**If responsiveness fails:**
- Document what happened (timestamp, operation, duration)
- Add to LESSONS.md
- Investigate root cause
- Improve for next time

**Example incident:**
```markdown
## 2026-02-21: System Hang - 3+ Minute Unresponsiveness

**What happened:** User requested simple UI change, system hung
for 3+ minutes with no status updates

**Root cause:** Multiple tool calls queued, no progress reporting

**Fix:** Added status update pattern to collaboration framework

**Prevention:** Always provide status for operations >10 seconds
```

---

## Learning Patterns

### Pattern: Post-Implementation Review

**When:** After completing a significant feature

**Questions:**
- What went well?
- What was harder than expected?
- What would we do differently?
- What did we learn?

**Example:**
```
Human: "We finished the multiplayer feature. What did we learn?"
AI: "Key learnings:
     1. WebSocket state is tricky - needed careful cleanup
     2. Race conditions appear with 2+ users - locks essential
     3. Database indexes crucial for performance
     4. User testing found issues unit tests missed

     Documented in LESSONS.md for next time."
```

### Pattern: Retrospective After Issues

**When:** Major bug, outage, or problem

**Process:**
1. What happened? (facts, no blame)
2. What was the impact?
3. What was the root cause?
4. What are we changing?

**Example:**
```
Human: "The database migration broke production"
AI: "Let's document this:
     - What: Migration deleted data
     - Impact: Lost 100 user records
     - Root cause: No backup, no dry-run
     - Changes: Always backup first, test migrations

     Added to LESSONS.md under 'Deployment'"
```

---

## Anti-Patterns (What NOT to Do)

### ❌ Vague Requests

**Bad:** "Make it better"
**Why:** "Better" is subjective and undefined
**Instead:** "Improve the spacing between buttons and add hover effects"

### ❌ Scope Creep

**Bad:** "Add login, and also add password reset, and also..."
**Why:** Mixes multiple concerns, hard to test
**Instead:** "Add login. We'll do password reset next."

### ❌ Premature Optimization

**Bad:** "Make it super fast before we even test it works"
**Why:** Optimize based on actual bottlenecks
**Instead:** "Make it work, measure it, then optimize what's slow"

### ❌ No Testing

**Bad:** "Just ship it, we'll test in production"
**Why:** Bugs are expensive to fix later
**Instead:** "Write tests for the core logic, then ship"

### ❌ Undocumented Decisions

**Bad:** Making architectural changes without documentation
**Why:** Future maintainers won't understand why
**Instead:** "Document in DECISIONS.md before implementing"

### ❌ Ignoring Feedback

**Bad:** "I don't like your suggestion" [doesn't explain]
**Why:** Missed opportunity to align
**Instead:** "I prefer approach B because of [reason]. Thoughts?"

### ❌ Context Switching

**Bad:** "Fix this bug, wait actually add this feature, wait..."
**Why:** Nothing gets finished
**Instead:** "Fix the bug first, then we'll add the feature"

---

## Session Management

### Starting Strong

**Good session start:**
```
Human: "Let's work on the user authentication system today.
        Goals:
        1. Fix the login bug (users can't log in)
        2. Add password reset functionality
        3. Update docs

        Start with #1, it's blocking users."
```

**Why:** Clear goals, prioritized, focused start

### Mid-Session Checks

**Pattern:**
```
AI: "Login bug fixed and tested ✓. Move to password reset?"
Human: "Yes, but keep it simple - just email a reset link"
AI: "Got it - simple email-based reset"
```

**Why:** Confirms completion, aligns on next step

### Ending Well

**Good session end:**
```
AI: "Session summary:
     ✓ Fixed login bug (users can now log in)
     ✓ Added password reset (email-based)
     ✓ Updated README with new features
     ✓ All 52 tests passing

     Committed as: 'Add password reset functionality'

     Next session: Add email verification?"
Human: "Perfect. Yes, email verification next time."
```

**Why:** Clear closure, documents progress, plans ahead

---

## Adapting These Patterns

### For Different Project Types

**Solo hobby project:**
- Lighter documentation
- Faster iteration
- Less formal reviews
- Focus on learning

**Professional project:**
- Comprehensive documentation
- Code reviews required
- Quality gates enforced
- Production considerations

**Open source:**
- Public collaboration
- Contributor guidelines
- Community involvement
- Transparent decisions

**Client work:**
- Regular demos
- Stakeholder communication
- Clear deliverables
- Budget awareness

### For Different Collaboration Styles

**Hands-on (pair programming):**
- Frequent check-ins
- Live feedback
- Quick iterations
- Shared exploration

**Delegation:**
- Clear requirements upfront
- Autonomous execution
- Milestone reviews
- Summary reports

**Hybrid (recommended):**
- Clear goals set together
- Autonomous implementation
- Regular check-ins
- Collaborative problem-solving

---

## Success Indicators

**You know collaboration is working when:**

✅ Both parties understand goals clearly
✅ Progress is steady and visible
✅ Blockers are identified early
✅ Decisions are documented
✅ Code quality is consistently high
✅ Learning happens both directions
✅ Surprises are rare
✅ Work is enjoyable

**Warning signs:**

⚠️ Frequent misunderstandings
⚠️ Work needs to be redone often
⚠️ Decisions are forgotten
⚠️ Quality is inconsistent
⚠️ Blockers discovered late
⚠️ Frustration increasing
⚠️ Technical debt accumulating

---

## Templates

### Session Planning Template
```markdown
## Session: [Date]

**Goals:**
1. [Primary goal]
2. [Secondary goal]
3. [Nice to have]

**Context:**
[What's the current state? What's changed?]

**Success Criteria:**
[How do we know we're done?]

**Estimated Time:**
[How long do we have?]
```

### Progress Update Template
```markdown
## Progress Update

**Completed:**
- [Task 1] ✓
- [Task 2] ✓

**In Progress:**
- [Task 3] - 60% done

**Blocked:**
- [Task 4] - waiting on [dependency]

**Next:**
- [Task 5]
```

### Decision Request Template
```markdown
## Decision Needed: [Topic]

**Context:**
[What's the situation?]

**Options:**
1. [Option A] - Pros: ... Cons: ...
2. [Option B] - Pros: ... Cons: ...

**Recommendation:**
I recommend [option] because [reason]

**Impact if we delay:**
[What happens if we don't decide now?]
```

---

## Continuous Improvement

**Review these patterns:**
- After each project (what worked?)
- When starting new collaboration (what to adapt?)
- When something goes wrong (what to change?)
- When something goes right (what to keep?)

**This document should evolve as you discover what works for your style and projects.**
