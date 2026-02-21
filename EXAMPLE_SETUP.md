# Example: Setting Up a New Project with This Framework

**Scenario:** You're starting nexus_roller_web (multiplayer web app) and want to use this framework.

---

## Option 1: Lightweight Reference (Recommended for Fast-Moving Projects)

**What:** Keep framework in separate folder, reference it, only copy what you need.

```bash
# In your new project
cd ~/Dropbox/nexus_roller_web

# Create minimal project docs
mkdir -p docs
touch DECISIONS.md
touch LESSONS.md
touch DEVELOPMENT.md

# Add reference to framework
cat > docs/FRAMEWORK_REFERENCE.md << 'EOF'
# Governance Framework Reference

This project uses patterns from: `~/Dropbox/collaborative-dev-framework`

## What We're Using:
- Decision template from GOVERNANCE.md
- After-Action Review from META_LEARNING_FRAMEWORK.md
- Work session patterns from COLLABORATION_PATTERNS.md

## What We Adapted:
- Simplified decision format (moving fast on MVP)
- Weekly retrospectives instead of daily (small project)
- Lightweight lessons capture (big failures only)

## See Also:
- Full framework: ~/Dropbox/collaborative-dev-framework
- This project's decisions: ../DECISIONS.md
- This project's lessons: ../LESSONS.md
EOF
```

**Pros:**
- Minimal overhead
- Don't copy docs you won't use
- Framework stays in one place, easier to update
- Project stays lean

**Cons:**
- Have to reference external folder
- Can't customize templates easily

---

## Option 2: Copy Core Docs (Recommended for Longer Projects)

**What:** Copy framework docs to project, customize as needed.

```bash
cd ~/Dropbox/nexus_roller_web

# Copy framework docs
mkdir -p docs/framework
cp ~/Dropbox/collaborative-dev-framework/GOVERNANCE.md docs/framework/
cp ~/Dropbox/collaborative-dev-framework/COLLABORATION_PATTERNS.md docs/framework/
cp ~/Dropbox/collaborative-dev-framework/META_LEARNING_FRAMEWORK.md docs/framework/

# Create project-specific docs
touch DECISIONS.md
touch LESSONS.md
touch DEVELOPMENT.md

# Customize templates
cat > DECISIONS.md << 'EOF'
# Architecture Decisions

## Active Decisions
(Log decisions here using template from docs/framework/GOVERNANCE.md)

---

## Template (customize as needed)

### Decision XXX: [Title]
**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Deprecated

**Context:** What's the situation?

**Decision:** What we chose and why.

**Consequences:** What this means going forward.
EOF

cat > LESSONS.md << 'EOF'
# Lessons Learned

## Format
**Date:** YYYY-MM-DD
**Type:** Bug / UX Issue / Performance / Architecture / Process
**What happened:**
**What we missed:**
**What we learned:**
**Action taken:**

---

## Log
(Lessons appear here)
EOF
```

**Pros:**
- All docs in one place
- Can customize templates
- Project is self-contained
- Easy for collaborators to find

**Cons:**
- More files to maintain
- Framework updates don't propagate automatically

---

## Option 3: Symlink (Advanced)

**What:** Symlink framework docs so they stay in sync.

```bash
cd ~/Dropbox/nexus_roller_web
mkdir -p docs

# Symlink framework (always up-to-date)
ln -s ~/Dropbox/collaborative-dev-framework docs/framework

# Create project-specific docs
touch DECISIONS.md
touch LESSONS.md
touch DEVELOPMENT.md
```

**Pros:**
- Framework updates automatically
- No copying needed
- Can reference templates easily

**Cons:**
- Can't customize framework templates
- Symlinks might confuse some tools
- Breaking changes to framework affect project

---

## Practical Example: First Week of nexus_roller_web

### Day 1: Project Setup

**1. Create project structure**
```bash
cd ~/Dropbox/nexus_roller_web
mkdir -p backend/app frontend/src docs
touch DECISIONS.md LESSONS.md
```

**2. Make first decision**
```markdown
# In DECISIONS.md

## Decision 001: Use FastAPI + React for MVP
**Date:** 2026-02-21
**Status:** Accepted

**Context:**
Need to build multiplayer dice rolling web app quickly.
Already familiar with Python, need real-time updates.

**Decision:**
- Backend: FastAPI (Python, async, WebSocket support)
- Frontend: React (component-based, fast iteration)
- Database: SQLite for MVP (will migrate to Postgres later)

**Why:**
- Fast development (familiar stack)
- WebSocket support built-in (FastAPI)
- Easy deployment (both have good hosting options)
- Can iterate quickly

**Trade-offs:**
- Python might not be fastest (but good enough for MVP)
- SQLite won't scale forever (documented migration path)
- Not using TypeScript (prioritizing speed over type safety for now)

**Migration triggers:**
- SQLite → Postgres: When we hit 10k+ users or need replication
- Add TypeScript: When team grows or bugs from types become issue
```

**3. Start work session**

Use patterns from COLLABORATION_PATTERNS.md:
- "Exploration Then Plan" pattern for complex features
- "Direct Implementation" for simple features

### Day 3: Hit First Issue

**Something goes wrong: CORS errors in development**

**1. Capture in LESSONS.md**
```markdown
## 2026-02-23: CORS Configuration

**Type:** Configuration / Bug
**What happened:**
Frontend (localhost:5173) couldn't call backend (localhost:8000).
Getting CORS errors in browser console.

**What we missed:**
FastAPI needs CORS middleware configured for local development.

**What we learned:**
CORS middleware must be added before routes are registered.
Need to allow credentials for WebSocket connections.

**Action taken:**
Added CORSMiddleware in main.py:
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Generic pattern:**
For web dev with separate frontend/backend:
- Configure CORS early in backend setup
- Use environment variable for allowed origins
- Document in DEVELOPMENT.md for future contributors
```

**2. Update DEVELOPMENT.md**
```markdown
# In DEVELOPMENT.md

## Running Locally

### Backend
```bash
cd backend
uvicorn app.main:app --reload --port 8000
```

### Frontend
```bash
cd frontend
npm run dev  # Runs on port 5173
```

**Important:** CORS is configured in backend/app/main.py.
If you get CORS errors, check:
1. Frontend URL matches CORS_ORIGINS in config
2. Middleware is added BEFORE routes
3. allow_credentials=True for WebSocket
```

### End of Week 1: Retrospective

**Use Start-Stop-Continue from META_LEARNING_FRAMEWORK.md**

```markdown
## Week 1 Retrospective: 2026-02-21 to 2026-02-27

**START:**
- Writing tests for WebSocket handlers (found bugs in planning)
- Documenting API endpoints as we build them
- Quick LESSONS.md entries when we hit issues

**STOP:**
- Trying to do frontend and backend simultaneously (context switching)
- Skipping CORS setup until "later" (bit us on Day 3)
- Overthinking database schema (YAGNI - build what we need now)

**CONTINUE:**
- Using DECISIONS.md for architecture choices (helpful reference)
- Exploration-then-plan pattern for new features
- Daily check-ins on progress

**Learnings:**
1. CORS should be first thing you set up, not last
2. WebSocket testing requires different approach than REST
3. Simplifying TUI taught us to simplify web UI too

**Next Week:**
- Focus on backend completion before heavy frontend work
- Add integration tests for room creation/joining
- Document WebSocket protocol in API docs
```

---

## What Gets Captured Where

### DECISIONS.md (Project-Specific)
- "Use FastAPI for backend" ✓
- "SQLite for MVP, migrate to Postgres at 10k users" ✓
- "WebSocket for real-time vs. polling" ✓

### LESSONS.md (Project-Specific)
- "CORS middleware must be configured early" ✓
- "WebSocket testing needs special client" ✓
- "Room codes should be 6 chars (tested with users)" ✓

### Framework Update (After Project / Milestone)

**Extract generic patterns from LESSONS.md:**

```markdown
# Add to collaborative-dev-framework/COLLABORATION_PATTERNS.md

### Pattern: Configuration Before Code

**When to use:** Starting new web project with separate frontend/backend

**Process:**
1. Set up CORS/auth/middleware FIRST
2. Then build routes/features
3. Document configuration in DEVELOPMENT.md

**Why:** Configuration issues block all development. Handle upfront.

**Example:** FastAPI CORS middleware must be added before routes to work.
```

```markdown
# Add to collaborative-dev-framework/GOVERNANCE.md

### Quality Gate: DEVELOPMENT.md Must Include Setup

**Before bringing on contributor:**
- [ ] DEVELOPMENT.md exists
- [ ] Local setup instructions tested
- [ ] Common issues documented (CORS, ports, env vars)
- [ ] "It works on my machine" is not acceptable

**Why:** Reduces onboarding friction, captures environment knowledge.
```

---

## Tips for Iterating on Social Rules

### 1. Experiment with Templates

**Try different formats:**

```markdown
# Week 1: Use formal decision template
## Decision: Use FastAPI
Date: 2026-02-21
Context: ...
Options: ...
Decision: ...
Consequences: ...

# Week 2: Feels bureaucratic, simplify
## Decision: Use FastAPI (2026-02-21)
Why: Fast, async, WebSocket support
Trade-off: Will migrate DB later

# Week 3: Found what works for you
Decision: FastAPI
Reason: [one sentence]
Link: [to implementation]
```

**Keep the version that you actually use.**

### 2. Add Friction Where Needed

**If you keep making same mistake:**

```markdown
# In your project

## Pre-Commit Checklist
- [ ] Tests pass
- [ ] No debug prints left in
- [ ] LESSONS.md updated if bug fixed
- [ ] DECISIONS.md updated if architecture changed

## (Add this to GOVERNANCE.md after using it 3 times)
```

### 3. Remove Friction Where It's Not Helping

**If you never open a doc, delete it:**

```bash
# You copied all framework docs but only use DECISIONS.md
rm docs/framework/META_LEARNING_FRAMEWORK.md  # Never opened it
rm docs/framework/COLLABORATION_PATTERNS.md    # Only used once

# Keep only what helps
ls docs/
# DECISIONS.md  LESSONS.md  DEVELOPMENT.md
```

### 4. Create Your Own Patterns

**You discover: "Deployment always breaks because we forget to update .env.example"**

```markdown
# Add to your GOVERNANCE.md

### Quality Gate: Environment Variables

**Before deployment:**
- [ ] .env.example includes all required vars
- [ ] README documents each variable
- [ ] Default values are safe (no secrets)

**Process:**
When adding new env var to code:
1. Add to .env.example immediately
2. Document in README
3. Update deployment docs

**Why:** Prevents "worked in dev, broke in prod"
```

**After using 2-3 times across projects:**

→ Add to central framework (collaborative-dev-framework/GOVERNANCE.md)

---

## Summary: How to Work Like This

**1. Start Simple**
```bash
# New project
mkdir my-project
cd my-project
touch DECISIONS.md LESSONS.md
```

**2. Reference Framework**
```bash
# When you need a pattern
cat ~/Dropbox/collaborative-dev-framework/COLLABORATION_PATTERNS.md | grep "Pattern:"

# When you need a template
cat ~/Dropbox/collaborative-dev-framework/GOVERNANCE.md
```

**3. Capture As You Go**
```bash
# Something went wrong
echo "## $(date): What happened and what we learned" >> LESSONS.md

# Made a decision
echo "## Decision: [Title]" >> DECISIONS.md
```

**4. Reflect Periodically**
```bash
# End of week
vim LESSONS.md  # Review what you learned
vim DECISIONS.md  # Review what you decided

# Ask: What patterns emerged?
# Ask: What should go in central framework?
```

**5. Update Framework**
```bash
# End of project or milestone
cd ~/Dropbox/collaborative-dev-framework

# Add generic patterns
vim COLLABORATION_PATTERNS.md

# Commit
git add -A
git commit -m "Add patterns from my-project"
```

**6. Repeat**

Next project starts with better framework → Learn more → Improve framework → Repeat

---

## The Key Insight

**You're not following a process.**
**You're building a process that works for YOU.**

- Framework gives you starting point
- Projects teach you what works
- You extract patterns
- Framework gets better
- Next project benefits

**This is meta-learning in action.**

---

**Start now:**
```bash
# What's your next project?
cd ~/path/to/next-project

# Copy just what you need
touch DECISIONS.md LESSONS.md

# Link to framework
echo "Governance: ~/Dropbox/collaborative-dev-framework" > GOVERNANCE_REF.md

# Build, learn, improve
```
