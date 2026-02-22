# Meta-Learning Framework: Learning How to Learn

**Purpose:** Capture and improve how we work together, learn from projects, and evolve our processes over time.

**Last Updated:** February 21, 2026

---

## Core Concept

**Meta-learning** is learning about learning. It's stepping back from the work to understand:
- What patterns lead to success?
- What mistakes do we repeat?
- How can we improve how we improve?

**This framework is project-agnostic and applies to any software development context.**

---

## Reflection Frameworks

### After-Action Review (AAR)

**When to use:** After completing any significant task, feature, or sprint

**Four Questions:**

1. **What was supposed to happen?**
   - What were our goals?
   - What did we expect?
   - What was the plan?

2. **What actually happened?**
   - What did we deliver?
   - How did it differ from plans?
   - What surprised us?

3. **Why was there a difference?**
   - What caused deviations?
   - What assumptions were wrong?
   - What did we miss?

4. **What will we do next time?**
   - What should we keep doing?
   - What should we stop doing?
   - What should we start doing?

**Example:**
```markdown
## AAR: Multiplayer Feature Implementation

**Supposed to happen:**
- 2-week implementation
- Simple room-based system
- Support 10 concurrent users

**Actually happened:**
- 3-week implementation (50% overrun)
- Complex state management needed
- Supports 50+ concurrent users

**Why the difference:**
- Underestimated WebSocket complexity
- Didn't account for race conditions
- Over-delivered on scalability (gold-plating)

**Next time:**
- Add 50% buffer for distributed systems
- Design for concurrency from start
- Clarify "good enough" vs "perfect"
- Set explicit scope boundaries
```

**Real Example from Practice:**
```markdown
## AAR: Web UI CLI Parity Implementation (2026-02-21)

**Supposed to happen:**
- Read formatters.py reference implementation
- List all CLI output fields
- Implement everything in one pass
- Show user for verification

**Actually happened:**
- Built incrementally without reading reference first
- User had to request missing pieces 3-4 times
- Each iteration added more fields
- Took ~4 iterations instead of 1

**Why the difference:**
- Didn't read reference implementation before coding
- Assumed "match CLI" meant "similar" not "exact"
- Underestimated importance of every detail
- Missed that format strings, labels, emojis all matter

**Next time:**
- ALWAYS read reference implementation first
- When user says "match exactly," read source code line-by-line
- Create completeness checklist before implementing
- Ask "What am I missing?" before showing user
- Assume every detail is intentional

**Impact:**
- Feature works correctly ✓
- User had to repeat themselves (frustrating)
- Could have saved ~90 minutes with upfront analysis
- Learned valuable pattern for future replication work
```

### 5 Whys Analysis

**When to use:** After bugs, failures, or unexpected problems

**Process:** Ask "why" five times to get to root cause

**Example:**
```
Problem: Production database ran out of connections

Why? Too many connections open at once
Why? Connections not being closed properly
Why? Database sessions leaked in error paths
Why? No finally block to ensure cleanup
Why? Didn't follow cleanup pattern established earlier

Root cause: Process not followed
Fix: 1) Add cleanup to current code
      2) Add linter rule to catch missing finally blocks
      3) Document pattern more visibly
```

### Start-Stop-Continue

**When to use:** Regular team retrospectives, process reviews

**Three Categories:**

**START:**
- What new practices should we adopt?
- What are we missing?

**STOP:**
- What's not working?
- What's wasting time?

**CONTINUE:**
- What's working well?
- What should we keep doing?

**Example:**
```markdown
## Sprint Retrospective

**START:**
- Writing tests before complex code
- Documenting decisions in DECISIONS.md
- Daily progress updates

**STOP:**
- Committing without running tests
- Ad-hoc architecture changes
- Working in isolation for days

**CONTINUE:**
- Code reviews before merge
- Clear commit messages
- Updating docs with code
```

---

## Learning Capture Patterns

### Pattern 1: Real-Time Learning Log

**Approach:** Document insights as they occur

**Template:**
```markdown
## Learning: [Short Title]
**Date:** YYYY-MM-DD
**Context:** What were we doing?
**Observation:** What did we notice?
**Insight:** What did we learn?
**Application:** How will we use this?
```

**Example:**
```markdown
## Learning: Database Indexes Matter A Lot
**Date:** 2026-02-20
**Context:** Room listing was slow with 100+ rooms
**Observation:** Query took 2 seconds without index, 10ms with index
**Insight:** Foreign key columns need indexes for joins
**Application:** Always add indexes to foreign keys from now on
```

### Pattern 2: Post-Incident Report

**Approach:** Comprehensive analysis after significant issues

**Template:**
```markdown
## Incident: [Title]

**Summary:**
[One-paragraph overview]

**Timeline:**
- HH:MM - [What happened]
- HH:MM - [What happened]
- HH:MM - [Resolution]

**Impact:**
- Users affected: [number]
- Duration: [time]
- Data loss: [yes/no, details]

**Root Cause:**
[What fundamentally caused this?]

**Resolution:**
[How was it fixed?]

**Action Items:**
1. [ ] [Immediate fix]
2. [ ] [Prevent recurrence]
3. [ ] [Improve detection]

**Lessons:**
- [What did we learn?]
```

### Pattern 3: Weekly Digest

**Approach:** Regular summary of progress and learnings

**Template:**
```markdown
## Week of [Date]

**Shipped:**
- [Feature/fix 1]
- [Feature/fix 2]

**Learned:**
- [Lesson 1]
- [Lesson 2]

**Challenges:**
- [Challenge 1] - [How addressed]
- [Challenge 2] - [How addressed]

**Next Week:**
- [Priority 1]
- [Priority 2]
```

---

## Feedback Loop Design

### Short Loops (Minutes to Hours)

**Examples:**
- Tests run after each code change
- Code review before merge
- Local testing before commit

**Benefits:**
- Catch errors early
- Low cost to fix
- Fast iteration

**Implementation:**
```
Write code → Run tests → See results → Adjust
        ↑___________________________________|
                  (< 1 minute)
```

### Medium Loops (Days to Weeks)

**Examples:**
- Sprint retrospectives
- Weekly progress reviews
- Feature demos

**Benefits:**
- Course correction
- Process improvement
- Team alignment

**Implementation:**
```
Plan sprint → Execute → Review → Adjust process
         ↑___________________________________|
                   (1-2 weeks)
```

### Long Loops (Months to Years)

**Examples:**
- Quarterly roadmap reviews
- Annual technology reassessment
- Multi-project pattern analysis

**Benefits:**
- Strategic direction
- Technology evolution
- Career growth

**Implementation:**
```
Set strategy → Execute projects → Evaluate outcomes → Refine strategy
           ↑______________________________________________|
                         (3-12 months)
```

---

## Knowledge Management

### Personal Knowledge Base

**What to capture:**
- Commands you use frequently
- Patterns that work well
- Mistakes to avoid
- Resources that helped
- Decisions and why

**Organization:**
```
knowledge/
├── commands.md          # CLI snippets, recipes
├── patterns.md          # Code patterns that work
├── antipatterns.md      # Things to avoid
├── resources.md         # Helpful links, docs
└── decisions/           # Why we chose X over Y
    ├── 001-database.md
    ├── 002-auth.md
    └── ...
```

### Project Knowledge Base

**What to document:**
- How things work (architecture)
- Why things are this way (decisions)
- What went wrong (lessons)
- How to do common tasks (runbooks)

**Organization:**
```
docs/
├── ARCHITECTURE.md      # How it works
├── DECISIONS.md         # Why we chose this
├── LESSONS.md           # What we learned
├── RUNBOOKS.md          # How to do X
└── TROUBLESHOOTING.md   # Common issues
```

### Transferable Knowledge

**Extract patterns that apply across projects:**

**From specific:**
> "In the nexus_roller project, we used SQLite for the MVP and documented when to migrate to PostgreSQL"

**To general:**
> "For MVPs, use simpler technology with a documented migration path to production-grade alternatives. Document the migration triggers upfront."

---

## Continuous Improvement Cycles

### Daily Improvement

**Micro-habits:**
- Before commit: "Is this the clearest way to write this?"
- After fixing bug: "How can I prevent this class of bugs?"
- End of day: "What did I learn today?"

### Weekly Improvement

**Review:**
- What shipped this week?
- What's taking longer than expected?
- What patterns are emerging?
- What should change?

**Actions:**
- Update processes
- Share learnings
- Adjust priorities

### Monthly Improvement

**Assessment:**
- Progress toward goals?
- Technical debt manageable?
- Team satisfaction?
- Process effectiveness?

**Strategic changes:**
- Tool adoption/retirement
- Process overhaul
- Skill development
- Roadmap adjustment

### Quarterly Improvement

**Deep dive:**
- Architecture review
- Technology refresh
- Skills gap analysis
- Strategic planning

**Major changes:**
- Re-architecture
- Platform migration
- Team restructuring
- Product pivot

---

## Learning Taxonomies

### Types of Knowledge

**1. Factual (What)**
- How to write a SQL query
- What's the syntax for X?
- Which library does Y?

**Capture:** Documentation, cheat sheets

**2. Conceptual (Why)**
- Why use WebSockets vs polling?
- Why is this architecture better?
- Why did this bug occur?

**Capture:** DECISIONS.md, LESSONS.md

**3. Procedural (How)**
- How to deploy the app
- How to debug WebSocket issues
- How to optimize database queries

**Capture:** RUNBOOKS.md, DEVELOPMENT.md

**4. Metacognitive (When/Strategy)**
- When to refactor vs rewrite?
- When is technical debt okay?
- When to ask for help?

**Capture:** This document, GOVERNANCE.md

### Learning Levels

**Level 1: Awareness**
"I know this exists"
- Action: Bookmark, add to reading list

**Level 2: Understanding**
"I know how this works"
- Action: Document in own words

**Level 3: Application**
"I can use this in my project"
- Action: Implement, test, verify

**Level 4: Analysis**
"I can debug this and understand trade-offs"
- Action: Document edge cases, limitations

**Level 5: Creation**
"I can design new solutions using this"
- Action: Teach others, write guides

**Level 6: Evaluation**
"I can judge when to use this vs alternatives"
- Action: Document decision criteria

---

## Measuring Learning

### Leading Indicators (Predict future success)

**Process metrics:**
- Time spent documenting decisions
- Frequency of retrospectives
- Knowledge base growth
- Cross-project pattern identification

**Behavioral metrics:**
- Asking "why" before implementing
- Documenting before forgetting
- Sharing learnings proactively
- Referring to past lessons

### Lagging Indicators (Show past results)

**Outcome metrics:**
- Bugs per release (decreasing?)
- Time to implement similar features (decreasing?)
- Onboarding time for new contributors (decreasing?)
- Repeat mistakes (decreasing?)

**Quality metrics:**
- Test coverage (increasing?)
- Documentation completeness (increasing?)
- Code review findings (decreasing?)
- Production incidents (decreasing?)

---

## Anti-Patterns in Learning

### ❌ Learning Theater

**What:** Going through motions without actual learning
**Example:** Writing retrospectives but never reading them
**Instead:** Act on insights, review past lessons regularly

### ❌ Not Invented Here

**What:** Rejecting solutions because "we didn't think of it"
**Example:** Rewriting a library instead of using established one
**Instead:** Learn from others, adapt what works

### ❌ Reinventing the Wheel

**What:** Not learning from past solutions
**Example:** Solving the same problem differently each time
**Instead:** Document patterns, reuse solutions

### ❌ Analysis Paralysis

**What:** Over-analyzing without doing
**Example:** Spending weeks researching instead of prototyping
**Instead:** Learn by doing, iterate quickly

### ❌ No Time to Learn

**What:** Always "too busy" to improve
**Example:** Never documenting because "we need to ship"
**Instead:** Learning IS part of shipping, allocate time

### ❌ Learning Silos

**What:** Knowledge trapped in individuals
**Example:** Only one person knows how deployment works
**Instead:** Document, share, cross-train

---

## Templates

### Learning Template
```markdown
## Learning: [Title]

**Context:**
What were we working on?

**What Happened:**
What did we observe or discover?

**Why It Matters:**
What's the significance?

**Insight:**
What's the underlying principle?

**Application:**
How will we use this going forward?

**Related:**
- Links to similar learnings
- Connections to other concepts
```

### Retrospective Template
```markdown
## Retrospective: [Sprint/Feature/Period]

**Wins 🎉**
What went well?

**Challenges 😅**
What was difficult?

**Surprises 😲**
What was unexpected?

**Learnings 💡**
What did we discover?

**Actions ✅**
- [ ] Start doing: ...
- [ ] Stop doing: ...
- [ ] Continue doing: ...
```

### Decision Retrospective
```markdown
## Decision Review: [Decision Name]

**Original Decision:**
What did we decide? (link to DECISIONS.md)

**Expected Outcomes:**
What did we think would happen?

**Actual Outcomes:**
What actually happened?

**Analysis:**
- What was right?
- What was wrong?
- What did we miss?

**Update:**
- [ ] Decision still valid
- [ ] Decision needs update
- [ ] Decision deprecated

**Lessons:**
What would we do differently?
```

---

## Integration with Daily Work

### Morning Ritual
1. Review yesterday's learnings
2. Check for blockers from past lessons
3. Plan with past insights in mind

### During Work
1. Notice when you solve a problem
2. Quick note: "Fixed X by doing Y"
3. Tag for later documentation

### End of Day
1. What did I learn today?
2. What surprised me?
3. What should I remember?
4. 5-minute documentation

### End of Week
1. Review daily notes
2. Extract patterns
3. Update knowledge base
4. Share with team (if applicable)

---

## Scaling Learning

### Solo Developer
- Personal knowledge base
- Regular reflection
- Cross-project pattern extraction
- External learning (blogs, courses)

### Small Team (2-5)
- Shared knowledge base
- Weekly sync on learnings
- Pair programming for knowledge sharing
- Collaborative retrospectives

### Larger Team (5+)
- Documented processes
- Knowledge sharing sessions
- Internal tech talks
- Mentorship programs

### Organization
- Learning culture
- Time allocated for improvement
- Knowledge management systems
- Communities of practice

---

## Making It Stick

### Principles for Effective Learning

**1. Active vs Passive**
❌ Reading about it
✅ Implementing it

**2. Spaced Repetition**
❌ Cram all learning into one session
✅ Review and apply over time

**3. Varied Practice**
❌ Always solving same problems
✅ Apply patterns to different contexts

**4. Retrieval Practice**
❌ Re-reading notes
✅ Explaining without notes

**5. Elaboration**
❌ Memorizing facts
✅ Connecting to what you know

**6. Concrete Examples**
❌ Abstract principles only
✅ Real examples from your work

### Building a Learning Habit

**Start small:**
- 5 minutes of reflection daily
- One insight captured per week
- Monthly review of learnings

**Build momentum:**
- Celebrate small wins
- Share learnings with others
- Track growth over time

**Make it sustainable:**
- Integrate into workflow
- Keep it simple
- Focus on high value

---

## Evolution of This Framework

**This framework should evolve as you learn more about learning.**

**Questions to ask periodically:**
- What's working?
- What's not being used?
- What's missing?
- What's changed in how we work?

**Signs it needs updating:**
- Processes feel forced
- Documentation isn't referenced
- Retrospectives feel rote
- No new insights emerging

**Keep what works, drop what doesn't, add what's missing.**

---

## Final Thoughts

**The goal isn't perfect process—it's continuous improvement.**

**The goal isn't documenting everything—it's capturing what matters.**

**The goal isn't spending time on meta-work—it's making real work better.**

**Use this framework to help you learn faster, make better decisions, and build better software.**

---

## Captured Lessons (Project Examples)

### Lesson: Read the Reference Implementation First (2026-02-21)

**Context:** Implementing web UI to match existing CLI/TUI output

**What Happened:**
- Task: Make web UI output match CLI exactly
- Approached incrementally: built features, then iterated based on feedback
- User had to request missing pieces 3-4 times: threshold, explosion indicator, status messages, etc.
- Each iteration added more fields to match CLI

**What Should Have Happened:**
1. Read `formatters.py` (the reference implementation) FIRST
2. List every field, label, emoji, and format string
3. Implement everything in one pass
4. Show user for verification

**Why This Matters:**
When user says "match exactly," they mean:
- Every field in the original
- Every label with exact wording
- Every emoji and symbol
- Order and formatting
- Status messages and variations

**Pattern to Apply:**

**Replication Workflow Checklist:**
- [ ] Locate reference implementation (source code, not just output)
- [ ] Read it line-by-line
- [ ] List all outputs, fields, conditionals
- [ ] Map to new implementation
- [ ] Cross-reference before showing user
- [ ] Ask: "Is there anything in the reference I haven't captured?"

**File Reference:** `nexus_roller_web/RETRO_CLI_PARITY.md`

---

### Lesson: "Match Exactly" Means EVERYTHING (2026-02-21)

**User Quote:**
> "I designed the CLI and TUI very carefully. ALL functions and information needs to be there. I want 'exploded dice' called out in the UI exactly the way the CLI does."

**What This Taught Us:**

**Surface Level vs Deep Understanding:**
- Surface: "Make it look similar"
- Deep: "Every detail matters because the design is intentional"

**Details That Matter:**
- Label text: "Overfill (wasted pips)" not just "Overfill"
- Format strings: `#01` vs `#1` (zero-padding matters)
- Delimiters: `[5, 3, 2]` vs `5 + 3 + 2` (commas vs plus signs)
- Status variations: Three different "OPTIMAL" messages depending on how optimality was achieved
- Threshold display: Only show when != default value
- Legend: Show what colors mean

**User Perspective:**
- They spent time designing the CLI/TUI carefully
- Every field has a purpose
- Every format choice is intentional
- Missing pieces feel like their work wasn't respected

**Communication Pattern:**
When user says "match exactly" or "don't make me ask again":
- STOP and do research first
- READ the reference implementation
- ASK: "What am I missing?" before building
- VERIFY completeness before showing

**Application:**
Before implementing anything that "matches" existing work:
1. Understand the original designer's intent
2. Assume every detail is intentional
3. Cross-reference exhaustively
4. Better to over-deliver than under-deliver

---

### Lesson: Effective User Feedback Patterns (2026-02-21)

**What Worked:**

**1. "Don't make me ask again"**
- Clear escalation
- Signaled: "I've said this enough times"
- Triggered deeper analysis on our end

**2. "I designed CLI/TUI very carefully. ALL functions..."**
- Provided context for why completeness matters
- Helped us understand the stakes
- Shifted from "nice to have" to "must have"

**3. "Single and multiplayer should look the same"**
- Caught divergence early
- Clear, testable requirement
- Easy to verify

**4. "I want it to look MORE like the CLI"**
- Direction was clear
- Comparative ("more like") helpful
- Led to reading reference code

**What This Teaches Us:**

**Directness Works:**
- Users don't need to be "nice" - they need to be clear
- Escalation ("don't make me ask again") is valid feedback
- Context ("I designed it carefully") helps us understand intent

**As AI Collaborators:**
- Don't wait for escalation - ask clarifying questions early
- When user says "match X," immediately ask: "Should I read X's source code first?"
- Verify completeness before showing: "I've implemented A, B, C. Am I missing anything from the original?"

**Pattern:**
```
User: "Make it match X"
AI: "I'll read X's implementation first to capture all details. One moment."
AI: [reads source]
AI: "I found these N components in X. Before implementing, should I list them for verification?"
User: "Yes" or "Just build it"
AI: [implements]
AI: "Built with all N components. Testing against reference..."
```

**Application:**
Add to collaboration patterns:
- "Read first, build second"
- "Completeness checklist before showing"
- "Direct feedback is good feedback"


---

## Framework Update Pattern

### Pattern: Extract Lessons to Framework

**When:** After completing a project session that revealed project-agnostic patterns

**Flow:**

```
1. Work on project
   ↓
2. Capture project-specific lessons (LESSONS.md)
   ↓
3. Conduct retro (identify patterns)
   ↓
4. Extract generic patterns
   ↓
5. Update framework documents
   ↓
6. Commit to framework repo
```

### Decision: Which Framework Doc to Update?

**SOCIAL_CONTRACT.md:**
- Communication principles
- Working relationship rules
- Error handling protocols
- Example: "Don't make me ask again" as effective escalation

**GOVERNANCE.md:**
- Decision-making processes
- Quality standards
- Commit message formats
- Example: Multi-paragraph commit message standards

**COLLABORATION_PATTERNS.md:**
- Tactical work patterns
- Session management
- Problem-solving approaches
- Example: Stop protocol, status updates, context preservation

**META_LEARNING_FRAMEWORK.md:**
- How to learn from work
- Reflection frameworks
- Lesson capture patterns
- Example: "Read reference implementation first"

**WORKFLOW.md:**
- How framework itself evolves
- Cross-project patterns
- Framework maintenance
- Example: This pattern itself!

### Example: 2026-02-21 Session

**What happened:**
- Worked on nexus_roller_web
- Made web UI match CLI exactly
- Had to iterate 3-4 times
- User said "don't make me ask again"
- Realized: should have read formatters.py first

**Project-specific lesson (nexus_roller_web/LESSONS.md):**
```markdown
## Lesson: Read Reference Implementation First

Should have read formatters.py before implementing web UI.
Would have captured all fields in first pass.
```

**Extract generic pattern:**
- This applies to ANY project where you're matching existing output
- Not specific to dice rollers or nexus_roller
- Reusable pattern for all future work

**Add to framework (META_LEARNING_FRAMEWORK.md):**
```markdown
### Lesson: Read the Reference Implementation First

**Context:** Implementing feature to match existing implementation

**Pattern:**
1. Locate reference implementation source code
2. Read it line-by-line
3. List all outputs, fields, conditionals
4. Map to new implementation
5. Cross-reference before building

**Replication Workflow Checklist:**
- [ ] Locate reference implementation
- [ ] Read it completely
- [ ] List all components
- [ ] Ask user: "Am I missing anything?"
```

**Commit to framework:**
```bash
cd collaborative-dev-framework
git add META_LEARNING_FRAMEWORK.md
git commit -m "Add 'read reference first' pattern from nexus_roller session"
```

### Extraction Process

**Step 1: Identify the lesson**
- What went wrong or right?
- What would we do differently?
- Is this project-specific or generic?

**Step 2: Generalize it**
- Remove project-specific details
- Identify the core pattern
- Make it applicable to any project

**Step 3: Find the right home**
- Communication → SOCIAL_CONTRACT
- Quality/process → GOVERNANCE
- Tactical patterns → COLLABORATION_PATTERNS
- Learning/reflection → META_LEARNING
- Framework itself → WORKFLOW

**Step 4: Write it up**
- Clear title
- Context when to use
- Pattern/template
- Real example (can be project-specific for illustration)
- Anti-pattern (what NOT to do)

**Step 5: Commit**
- Descriptive commit message
- Reference source project if helpful
- Link to retro doc if it exists

### Multiple Lessons from One Session

**Example: 2026-02-21 Session Extracted:**

1. **META_LEARNING_FRAMEWORK.md**
   - "Read the Reference Implementation First"
   - "Match Exactly Means EVERYTHING"
   - "Effective User Feedback Patterns"

2. **COLLABORATION_PATTERNS.md**
   - Context preservation (pickup documents)
   - Session end protocol
   - User support patterns (self-doubt)

3. **GOVERNANCE.md**
   - Commit message standards
   - Co-authorship pattern

**All from one 24-hour work session.**

### When to Update Framework

**After every project?** No. Too much overhead.

**Update when:**
- Pattern used successfully 2-3 times (proven)
- Lesson was significant (changed how we work)
- Pattern feels generalizable (not project-specific)
- Natural opportunity (session end, retro, cleanup)

**Don't update when:**
- One-time situation
- Project-specific quirk
- Obvious/common knowledge
- No clear pattern emerged

### Framework Health

**Signs framework is healthy:**
- Gets referenced during work
- Patterns are actually used
- Grows slowly but steadily
- Remains readable and navigable

**Signs framework is unhealthy:**
- Never referenced
- Just a dumping ground
- Growing too fast (no curation)
- Too complex to navigate

**Maintenance:**
- Periodically review what's not being used
- Consolidate duplicate patterns
- Archive outdated patterns
- Update INDEX.md when adding sections

### Meta-Meta-Learning

**This pattern is itself an example of meta-learning:**

- Project work teaches lessons
- Lessons teach patterns
- Patterns improve framework
- Framework improves future projects
- **Loop continues forever**

**The goal:** Each project makes the next one easier.

### Real Example: This Document

**Source:** 2026-02-21 nexus_roller_web session

**Extracted lessons:**
- Context preservation (COLLABORATION_PATTERNS)
- Session end protocol (COLLABORATION_PATTERNS)
- Commit standards (GOVERNANCE)
- User support patterns (COLLABORATION_PATTERNS)
- Framework update flow (META_LEARNING) ← This section!

**Process:**
1. Worked on nexus_roller_web (project)
2. Created RETRO_CLI_PARITY.md (project retro)
3. Identified generic patterns (extraction)
4. Updated framework docs (application)
5. Committed to framework repo (preservation)
6. Documented the extraction process itself (meta-meta-learning)

**Result:** Framework is now more complete. Next project will benefit from all these patterns.

