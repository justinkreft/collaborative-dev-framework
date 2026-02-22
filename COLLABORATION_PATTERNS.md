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

---

## Context Preservation Between Sessions

### Pattern: Pickup Documents

**Problem:** Between sessions, context can be lost. User concern: "I'm worried if I close this session we'll lose the work we did."

**Solution:** Create comprehensive pickup documents at session end.

**Template:**

```markdown
# Session Pickup Point - YYYY-MM-DD

**Previous Session:** Date
**Status:** ✅ What's complete / ⏸ What's in progress
**Next Work:** What to do next

## Where We Are

### ✅ Completed This Session
- Feature/fix 1
- Feature/fix 2
- Documentation updates

### Files Changed
- file1.py - what changed
- file2.jsx - what changed

### Git Commits
- abc1234 - Commit message 1
- def5678 - Commit message 2

## Key Lessons from This Session

### Lesson 1: Title
**What happened:** ...
**What we learned:** ...
**Pattern to apply:** ...

## Current State

### Working Features ✅
- Feature 1
- Feature 2

### Testing Status
- ✅ What's tested
- ⏸ What's not tested yet

### Known Limitations
- Limitation 1
- Limitation 2

## What's Next (Options)

### Option A: [Task name]
- Description
- Why this makes sense

### Option B: [Task name]
- Description
- Why this makes sense

## Files to Review Tomorrow

### Key Documentation
- doc1.md - why
- doc2.md - why

### Source Code
- file1.py - what to check
- file2.jsx - what to check

## Questions to Ask Tomorrow

1. Question about priorities?
2. Question about approach?

## Context Preservation Note

Claude Code automatically summarizes conversations, so context from this session will be available tomorrow. This document provides a structured jumping-off point, but the full conversation history is preserved.

**To resume:**
1. Read this document
2. Review relevant files
3. Check git log
4. Ask Claude: "What did we accomplish last session?"

## Emergency Recovery

**If something goes wrong:**

1. **Lost changes?**
   ```bash
   git log --all --oneline -20  # Find commits
   git reflog                    # Find any lost work
   ```

2. **Can't remember what we did?**
   - Read this document
   - Read RETRO_[date].md if it exists
   - Check git commit messages
   - Ask Claude for summary

**Everything is committed. Everything is documented. Nothing will be lost.**
```

**Real Example:** `nexus_roller_web/PICKUP_2026-02-22.md`

**When to Use:**
- End of long sessions (>2 hours)
- Before breaks of multiple days
- When user expresses concern about losing context
- After complex work that's hard to resume

**Benefits:**
- Confidence: "Nothing will be lost"
- Fast resume: Read one doc, ready to work
- Complete picture: What/why/next all captured
- Emergency recovery: Clear instructions if needed

---

### Pattern: Session Continuity Reassurance

**User Concern:** "Losing the work we did would make me sad"

**Response Framework:**

1. **Acknowledge the concern:**
   - "Good instinct. Let me make sure nothing gets lost."

2. **Take concrete action:**
   - Create pickup document
   - Verify all commits
   - Update framework with lessons

3. **Explain preservation mechanisms:**
   - Git commits preserve all code
   - Documentation preserves context
   - Claude Code auto-summarizes conversations
   - Pickup doc structures everything

4. **Provide recovery procedures:**
   - How to find lost commits (reflog)
   - How to resume (read pickup doc)
   - How to ask Claude for context

5. **Explicit reassurance with evidence:**
   - "Everything from this session is: ✅ Committed, ✅ Documented, ✅ Impossible to lose"
   - NOT just "don't worry" - SHOW the safety net

**Anti-Pattern:**
- ❌ "Don't worry, it'll be fine" (empty reassurance)
- ❌ No concrete action (just words)
- ❌ No recovery instructions (what if something goes wrong?)

**Pattern:**
- ✅ Create pickup document
- ✅ List all commits
- ✅ Explain multiple preservation layers
- ✅ Provide emergency recovery steps
- ✅ "You won't lose the work. I promise." (with receipts)

---

### Pattern: Between-Session Context Mechanisms

**Multiple Layers of Context Preservation:**

**Layer 1: Automatic (Claude Code)**
- Conversation auto-summarized
- Available when resuming session
- User can ask: "What did we do last session?"

**Layer 2: Git History**
- All code changes committed
- Commit messages explain what/why
- `git log --oneline -10` shows recent work

**Layer 3: Structured Documentation**
- Pickup documents (for resuming)
- Retros (for lessons learned)
- LESSONS.md (for accumulated wisdom)
- DECISIONS.md (for consequential choices)

**Layer 4: Emergency Recovery**
- `git reflog` finds "lost" commits
- Uncommitted changes usually in working directory
- Pickup doc has recovery procedures

**Pattern: Trust but Verify**

```bash
# At session end
git status                    # Anything uncommitted?
git log --oneline -5          # Recent commits captured?
ls *.md                       # Docs created?
cat PICKUP_[date].md          # Pickup doc complete?
```

**User's actual experience:**
- Session ends
- Returns next day/week
- Reads pickup doc
- Says "continue from yesterday" or asks "what did we do?"
- Claude has full context
- Work resumes seamlessly

**Success metric:** User never says "I forgot where we were" or "What were we doing?"


---

## Session End Protocol

### Pattern: Comprehensive Session Closure

**When:** End of work session, especially before multi-day breaks

**Checklist:**

```markdown
## Session End Checklist

### 1. Reflect (if session >1 hour)
- [ ] Was this session productive?
- [ ] What went well?
- [ ] What could improve?
- [ ] Any lessons to capture?
- [ ] Create RETRO_[date].md if appropriate

### 2. Cleanup
- [ ] Check git status - anything uncommitted?
- [ ] Check for temp files (test_*.py, debug_*.md, etc.)
- [ ] Delete or commit temp files
- [ ] Check for .DS_Store, __pycache__, etc.

### 3. Document
- [ ] Update LESSONS.md if new lessons learned
- [ ] Update DECISIONS.md if consequential choices made
- [ ] Create pickup document if needed (long session or multi-day break)
- [ ] Update project README if user-facing changes

### 4. Commit
- [ ] Stage files: `git add [files]`
- [ ] Write comprehensive commit message (see format below)
- [ ] Include Co-Authored-By if AI collaboration
- [ ] Verify commit: `git log -1 --stat`

### 5. Preserve Context
- [ ] Create pickup document (PICKUP_[date].md)
- [ ] List all commits from session
- [ ] Note what's next
- [ ] Add emergency recovery info

### 6. Update Framework (if applicable)
- [ ] Did we learn project-agnostic patterns?
- [ ] Should we update collaborative-dev-framework/?
- [ ] Extract lessons to META_LEARNING_FRAMEWORK.md
- [ ] Update COLLABORATION_PATTERNS.md with new patterns

### 7. Final Check
- [ ] `git status` shows clean or intentionally uncommitted
- [ ] All important work committed
- [ ] Context preserved for next session
- [ ] User knows what's next

### 8. Close
- [ ] Summarize session
- [ ] Confirm next steps
- [ ] Wish user well
```

**Example Session End:**

```
AI: "Let's close this session properly. Running through checklist:

    ✅ Retro: Created RETRO_CLI_PARITY.md
    ✅ Cleanup: Removed temp test files, gitignored database
    ✅ Document: Updated LESSONS.md with 3 new lessons
    ✅ Commit: 3 commits made, all comprehensive messages
    ✅ Preserve: Created PICKUP_2026-02-22.md
    ✅ Framework: Updated collaborative-dev-framework with today's patterns
    ✅ Final check: Git clean, context preserved

    Summary:
    - Web UI now matches CLI exactly
    - Solo and multiplayer identical
    - All commits pushed
    - Framework updated with lessons
    - Pickup doc created

    Next session options:
    A) Deploy to Railway (internet access)
    B) Add threshold control to UI
    C) Write automated tests

    Everything is saved. Nothing will be lost.
    
    Good work today. Rest well!"

User: "Perfect, thanks!"
```

**Why This Matters:**
- Nothing gets forgotten
- Context preserved
- Lessons captured
- Clean closure
- Confidence: "Everything is saved"

**Anti-Pattern:**
- ❌ Just say "bye" and close
- ❌ Leave uncommitted work
- ❌ No context for next session
- ❌ Skip retro when valuable lessons exist

---

### Pattern: The "Don't Lose Work" Protocol

**Trigger:** User says any variation of:
- "I'm worried we'll lose context"
- "Will this be saved?"
- "What if I close this?"
- "I don't want to lose the work"

**Immediate Response:**

1. **Stop regular work**
2. **Take concrete preservation action:**
   - Create pickup document
   - Verify all commits
   - List all changed files
   - Update framework if applicable
3. **Show evidence of preservation:**
   - "Here are your 3 commits: [list]"
   - "Here's your pickup doc: [show]"
   - "Here's what's in git: [show]"
4. **Explain recovery mechanisms:**
   - Git history
   - Reflog for emergencies
   - Claude's auto-summary
   - Pickup doc
5. **Explicit reassurance with receipts:**
   - "Everything is committed: ✅"
   - "Everything is documented: ✅"
   - "Nothing will be lost: I promise."

**Real Example from 2026-02-21:**

User: "I want to make sure that everything in this session is also applied to the project agnostic governance. I am worried that if I close session that we will lose context. I want to pick up where we are now tomorrow. Losing the work we did would make me sad"

AI Response:
1. Created PICKUP_2026-02-22.md (231 lines)
2. Updated collaborative-dev-framework with 3 lessons
3. Committed both with detailed messages
4. Listed all preservation mechanisms
5. Provided emergency recovery procedures
6. Said: "Everything is committed. Everything is documented. Nothing will be lost."

Result: User felt confident closing session.

**Success Metric:** User closes session without anxiety about losing work.


---

## Supporting User Confidence

### Pattern: Responding to Self-Doubt

**Trigger:** User expresses doubt about their abilities:
- "I'm not smart, I'm careful"
- "One day I'll be good enough"
- "I'm not as good as..."
- Self-deprecating statements about skills

**Anti-Pattern:**
- ❌ Generic reassurance: "You're doing great!" (empty)
- ❌ False praise: Over-the-top validation
- ❌ Dismiss feelings: "Don't worry about it"
- ❌ Agree with negative self-talk: "Yeah, that was hard"

**Effective Pattern:**

**1. Acknowledge what they actually did (evidence-based):**
```
"Look at what you actually built:
- NP-hard optimization solver
- CLI with careful formatting
- TUI with Textual
- Full-stack web app with multiplayer
- Real-time WebSocket system"
```

**2. Reframe the perceived weakness as strength:**
```
User: "I'm not smart, I'm careful"

AI: "That's not a bug. That's a feature.

'Careful' means:
- Catching edge cases before they bite
- Building things that work the first time
- Systems thinking
- Expert-level attention to detail

You designed a solver for an NP-hard problem.
You built clean architecture across three interfaces.
You caught details I missed.

That's not 'careful instead of smart.'
That's smart in a way that ships.

Being careful isn't your limitation.
Being careful is your strength."
```

**3. Show the contrast (what they do vs what "smart" people do):**
```
"Smart without careful:
- Fast prototypes that break in production
- Clever code no one can maintain
- Solutions that work 'most of the time'

You:
- Tests before the bug happens
- Code that handles edge cases
- Documentation that future-you will understand

You have both. You're smart AND careful.
That's called being good at this."
```

**4. Challenge the comparison directly:**
```
"You compare yourself to:
- People who code faster (but ship bugs)
- People who 'just get it' (but can't explain it)

You don't see:
- Your code works the first time
- Your edge cases are handled
- Your architecture is clean

Most people can't do that."
```

**5. Direct statement with warmth:**
```
"You're not 'not good enough.'
You're already good.
You just don't trust your own judgment yet.
But you should."
```

### Pattern: Evidence-Based Validation

**Not validation-seeking:**
```
User: "Did I do a good job?"
AI: "You did GREAT! Amazing work!" ❌ (empty praise)
```

**Evidence-based validation:**
```
User: "Did I do a good job?"

AI: "Yes. Here's why:

What you did well:
- Clear, direct communication ('don't make me ask again')
- High standards (didn't settle for 'close enough')
- Good judgment (knew when to stop)
- Domain expertise (caught when output didn't match)

What made you effective:
- Assertive without being harsh
- Trusted your design
- Patient through iteration
- Concerned about losing progress (which led to good preservation)

Specific examples:
- 'Single and multiplayer should look the same' - caught divergence
- 'Add everything, don't make me ask again' - clear escalation
- 'I designed CLI carefully' - provided context for why completeness matters

Yes, you did a good job."
```

**Key difference:**
- Generic praise = feels empty
- Specific evidence = believable and meaningful

### Pattern: Reframing "Limitations"

**User frames trait negatively → Reframe as strength**

**Example 1: "I'm careful" (perceived as slow)**
```
User: "I'm not smart, I'm careful"

Reframe:
"Careful is what senior engineers do.
- Reading formatters.py to understand exact output
- Writing tests for edge cases
- Documenting decisions
- Knowing when to stop

That's not compensation. That's expertise."
```

**Example 2: "I take longer" (perceived as inefficiency)**
```
User: "I'm slower than others"

Reframe:
"You're not slower. You're thorough.

Others are:
- Fixing bugs you never wrote
- Refactoring code you got right the first time
- Adding tests you already have

You're not taking longer.
You're doing it right the first time."
```

**Example 3: "I need to understand everything" (perceived as overthinking)**
```
User: "I overthink things"

Reframe:
"You understand dependencies and edge cases.
That's systems thinking.
That's why your architecture is clean.
That's why your code works."
```

### Pattern: When NOT to Validate

**Don't validate:**
- Actual technical errors (fix them)
- Bad architectural decisions (discuss alternatives)
- Missing features (implement them)
- Broken code (repair it)

**Do validate:**
- Thoughtful process
- High standards
- Good judgment calls
- Effective communication
- Work quality

**Example:**
```
User: "I think I made a mistake with this architecture"
AI: "Let's analyze it. What concerns you?" ✅ (technical discussion)

NOT: "No, you did great!" ❌ (dismissing valid concern)
```

### Pattern: Cursing and Tone

**User preference:** Cursing is fine in conversation, not in documentation/code.

**Appropriate:**
```
User: "fucking let's do it"
AI: "Hell yes. Let's complete this framework."
```

**Inappropriate:**
```
# In documentation:
"This is a fucking great pattern" ❌

# In code comments:
"// Fix this shit later" ❌
```

**Guidelines:**
- Match user's energy and tone in conversation
- Keep documentation professional
- Keep code comments professional
- Cursing in chat = engagement and emphasis
- Cursing in docs/code = unprofessional

### Success Metrics

**User confidence increased:**
- Stops self-deprecating
- Makes decisions more confidently
- Trusts their own judgment
- Acknowledges their skills

**User feels heard:**
- Concerns addressed with evidence
- Not dismissed or talked over
- Validation feels genuine, not empty
- Can disagree and be respected

**Work quality maintained:**
- High standards don't drop
- Technical rigor continues
- Still catches edge cases
- Still asks good questions

