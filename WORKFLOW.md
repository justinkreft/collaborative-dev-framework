# Cross-Project Workflow: How This Framework Evolves

**The goal: Learn from each project, refine the framework, apply improved patterns to the next project.**

---

## The Cycle

```
┌─────────────────┐
│  Start Project  │
│   (copy docs)   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Work on Code   │◄──────┐
│  (use patterns) │       │
└────────┬────────┘       │
         │                │
         ▼                │
┌─────────────────┐       │
│ Capture Lessons │       │
│  (LESSONS.md)   │       │
└────────┬────────┘       │
         │                │
         ▼                │
┌─────────────────┐       │
│   Reflect &     │       │
│   Retrospect    │       │
└────────┬────────┘       │
         │                │
         ▼                │
┌─────────────────┐       │
│  Update Local   │       │
│     Patterns    │───────┘
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Project Wraps   │
│  Up or Pauses   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Extract Generic │
│    Patterns     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Update Central  │
│   Framework     │
└────────┬────────┘
         │
         ▼
    (Next Project)
```

---

## Step-by-Step Workflow

### 1. Starting a New Project

**Copy framework docs to project:**
```bash
# Option A: Copy all framework docs to new project
cp ~/Dropbox/collaborative-dev-framework/*.md ~/new-project/docs/

# Option B: Symlink for live updates (advanced)
ln -s ~/Dropbox/collaborative-dev-framework ~/new-project/docs/framework

# Option C: Reference only (lightweight)
echo "See: ~/Dropbox/collaborative-dev-framework" > ~/new-project/docs/GOVERNANCE_REF.md
```

**Create project-specific files:**
```bash
cd ~/new-project
touch DECISIONS.md    # Log architecture decisions
touch LESSONS.md      # Capture what goes wrong and right
touch DEVELOPMENT.md  # Technical setup, testing, deployment
```

**Adapt the framework:**
- Read GOVERNANCE.md - decide which quality gates apply
- Read COLLABORATION_PATTERNS.md - pick session patterns to try
- Drop what doesn't fit your project scale/style
- Keep it lightweight at first, expand as needed

### 2. During Development

**Use patterns from the framework:**
- When starting a feature → Use work session patterns (COLLABORATION_PATTERNS.md)
- When making big decisions → Use decision template (GOVERNANCE.md)
- When something goes wrong → Use 5 Whys or AAR (META_LEARNING_FRAMEWORK.md)
- When reviewing progress → Use Start-Stop-Continue (META_LEARNING_FRAMEWORK.md)

**Capture lessons in real-time:**
```markdown
# In project's LESSONS.md

## 2026-02-21: TUI Button Visibility Issue

**Type:** UX Discovery
**What happened:** User couldn't find explosion buttons in TUI
**What we missed:** Buttons in scrollable sidebar, not where user's eyes were
**What we learned:** Place actions near the information they act on
**Action taken:** Moved buttons to result display

**Generic pattern:** Put UI where users look
**Framework update needed:** Add to COLLABORATION_PATTERNS.md under UX principles
```

**Update project-local copies when you discover improvements:**
- If a pattern works really well → Document it in project LESSONS.md
- If you refine a template → Update your project's copy
- If you find anti-patterns → Add to project LESSONS.md

### 3. Weekly/Monthly Reflection

**Review what's working:**
```markdown
# Weekly check-in questions (from META_LEARNING_FRAMEWORK.md)

- What patterns from the framework did we use?
- Which ones helped? Which ones felt forced?
- What new patterns emerged organically?
- What should we add to the framework?
```

**Update project docs:**
- Add successful patterns to project DECISIONS.md
- Document failures in LESSONS.md
- Refine templates to match your style

### 4. Project Wrap-Up or Major Milestone

**Extract generic patterns:**

Review project LESSONS.md and DECISIONS.md. For each lesson, ask:
1. **Is this specific to this project or generally applicable?**
2. **Can I reword it to be project-agnostic?**
3. **Would future me benefit from knowing this?**

**Example transformation:**

**Project-specific (LESSONS.md):**
> "In nexus_roller TUI, explosion buttons were invisible in the scrollable control panel. Moving them to the result display (where user's eyes were) fixed discoverability."

**Generic pattern (for framework):**
> "**Principle:** Place interactive UI elements near the information they act upon, not in separate scrollable panels. Users' visual focus follows content, not chrome."
>
> **Category:** UX Design
> **Document:** COLLABORATION_PATTERNS.md
> **Section:** Problem-Solving Patterns > UI/UX

### 5. Update Central Framework

**Process:**
```bash
cd ~/Dropbox/collaborative-dev-framework
git pull  # Get latest changes

# Review project lessons
vim ~/finished-project/LESSONS.md

# Add generic patterns to framework
vim COLLABORATION_PATTERNS.md  # Add UX principles
vim GOVERNANCE.md              # Update quality standards if needed
vim META_LEARNING_FRAMEWORK.md # Add new reflection techniques

# Document the change
git add -A
git commit -m "Add UX principle: Place UI where users look

Learned from nexus_roller TUI development:
- Buttons in scrollable sidebar were invisible to users
- Moving buttons near content they act on improved discoverability
- General principle: UI follows user's visual focus

Added to COLLABORATION_PATTERNS.md under Problem-Solving > UX"

git push
```

**Version the framework:**
```bash
# Optional: Tag major updates
git tag -a v1.1 -m "Added UX design principles from nexus_roller project"
git push --tags
```

### 6. Next Project Starts

**You now have an improved framework:**
- New patterns from previous project
- Refined templates that match your style
- Anti-patterns documented to avoid
- Better questions to ask during development

**Repeat the cycle:**
1. Copy updated framework to new project
2. Use patterns during work
3. Capture new lessons
4. Extract generic patterns
5. Update framework
6. (Loop)

---

## What to Update in the Framework vs. Project Docs

### Update Central Framework When:
- ✅ Pattern applies to **any** project (project-agnostic)
- ✅ You've used it successfully **2+ times** (validated)
- ✅ It's a fundamental principle (transparency, quality, etc.)
- ✅ It's a common anti-pattern to avoid
- ✅ It's a template or checklist others could use

### Keep in Project Docs Only When:
- ❌ Pattern is **specific to this tech stack** (e.g., "FastAPI WebSocket setup")
- ❌ Decision is **context-dependent** (e.g., "We chose SQLite for MVP because...")
- ❌ Lesson is **domain-specific** (e.g., "Dice explosion chains can be recursive")
- ❌ You're **still experimenting** (not yet validated)

### Gray Area (Document Both):
- **Project:** Specific implementation details
- **Framework:** Generic principle extracted

**Example:**
- **Project DECISIONS.md:** "Chose WebSockets over polling for multiplayer because latency requirements (<100ms) and bidirectional communication needed"
- **Framework COLLABORATION_PATTERNS.md:** "When choosing real-time tech: WebSockets for bidirectional + low latency, SSE for one-way updates, polling for simplicity"

---

## Keeping Rules Fluid

### The Framework is NOT:
- ❌ A rigid process to follow step-by-step
- ❌ A checklist you must complete
- ❌ A set of rules you can't break
- ❌ Something that slows you down

### The Framework IS:
- ✅ A collection of **patterns that worked before**
- ✅ A set of **questions to consider**
- ✅ A **starting point** you adapt
- ✅ A **learning log** that grows with you
- ✅ **Always negotiable** based on context

### How to Keep It Fluid:

**1. Start minimal, expand as needed**
```
Week 1: Just use DECISIONS.md
Week 2: Add LESSONS.md when you hit first bug
Week 3: Try a retrospective template from META_LEARNING_FRAMEWORK.md
Week 4: Adopt patterns that helped, drop ones that didn't
```

**2. Question everything periodically**
```markdown
# Every 2 weeks, ask:

- What framework docs did I open this week?
  → If you didn't open it, remove it from the project

- What patterns did I actually use?
  → Keep those, archive the rest

- What's missing that I keep wishing existed?
  → Add it to the framework

- What feels forced or bureaucratic?
  → Drop it immediately
```

**3. Adapt templates to your style**

Don't like the decision template? Change it:

```markdown
# Your style: Minimal
Decision: Use SQLite
Why: Fast to set up, good enough for MVP
Trade-off: Will migrate to Postgres later
```

vs.

```markdown
# Framework's formal template
## Decision: Database Technology
Date: 2026-02-21
Context: Need persistence for MVP...
Options Considered:
1. SQLite - pros/cons
2. PostgreSQL - pros/cons
...
```

**Both are valid.** Use what works for you.

**4. Version your adaptations**

Track how the framework evolves for YOUR workflow:

```bash
cd ~/collaborative-dev-framework
git branch my-workflow  # Personal adaptations
git checkout my-workflow

# Simplify templates, add custom patterns, etc.
# Keep main branch as "canonical" version
# Merge back improvements that could help others
```

---

## Real-World Example: nexus_roller → nexus_roller_web

### What We Learned (nexus_roller project):

**From LESSONS.md:**
- TUI buttons invisible in sidebar → Move UI near content
- Rich markup syntax errors → Test markup strings in isolation
- Threshold field confusing in TUI → Not every feature needs every interface
- Test coverage gaps → Evaluate coverage, add critical tests
- Magic numbers everywhere → Extract to constants

**What We Updated in Framework:**

**COLLABORATION_PATTERNS.md:**
- Added: "Put UI where users look" principle
- Added: "Simplify for the common case" (TUI vs CLI complexity)
- Added: "Trust user feedback over test results"

**META_LEARNING_FRAMEWORK.md:**
- Added: Example of systematic code quality evaluation
- Added: Pattern of using parallel agents for analysis

**GOVERNANCE.md:**
- Reinforced: "Test coverage is a signal, not a goal"
- Added: Pattern of targeted edge case testing (one well-chosen test > ten random)

### What We Applied to Next Project (nexus_roller_web):

**Started with:**
- GOVERNANCE.md principles (transparency, quality, iteration)
- COLLABORATION_PATTERNS.md session flow (exploration → plan → execute)
- Decision templates from GOVERNANCE.md

**Adapted:**
- Simpler decision format (we're moving fast)
- Lightweight LESSONS.md (capture big failures only)
- Reference framework, don't copy all docs

**Will feed back:**
- WebSocket multiplayer patterns (if successful)
- Room code generation approaches
- Database migration patterns (SQLite → Postgres)

---

## The Meta Pattern

**This workflow document itself follows the framework:**
- It's project-agnostic ✓
- It's based on real experience ✓
- It provides templates you can adapt ✓
- It will evolve as we learn more ✓
- It's not prescriptive, just helpful ✓

**When this workflow doesn't work for you:**
- Document what you did instead (LESSONS.md)
- Extract the generic pattern
- Update this document
- Improve it for next time

---

## Quick Reference

### Starting Project
```bash
cp ~/Dropbox/collaborative-dev-framework/GOVERNANCE.md ~/new-project/docs/
cp ~/Dropbox/collaborative-dev-framework/COLLABORATION_PATTERNS.md ~/new-project/docs/
touch ~/new-project/DECISIONS.md
touch ~/new-project/LESSONS.md
```

### During Project
```bash
# Log decisions
echo "## Decision: [Title]" >> DECISIONS.md

# Capture lessons
echo "## [Date]: [What Happened]" >> LESSONS.md

# Use framework patterns
cat ~/Dropbox/collaborative-dev-framework/COLLABORATION_PATTERNS.md | grep "Pattern:"
```

### After Project
```bash
# Extract patterns
vim LESSONS.md  # Review project lessons
vim ~/Dropbox/collaborative-dev-framework/COLLABORATION_PATTERNS.md  # Add generic patterns

# Commit improvements
cd ~/Dropbox/collaborative-dev-framework
git add -A
git commit -m "Add patterns from [project-name]"
```

---

## Special Workflows

### Replication Workflow: Matching Existing Output

**When to use:** Implementing a feature that must match existing CLI, API, or UI output exactly.

**Pre-Implementation Checklist:**

```markdown
## Before You Code: Replication Prep

- [ ] Locate reference implementation (source code, not just output)
- [ ] Read reference code line-by-line
- [ ] List all outputs, fields, labels, format strings
- [ ] Note all conditionals (when does output vary?)
- [ ] Check for edge cases in reference
- [ ] Map reference fields to new implementation
- [ ] Ask user: "Should I list what I found for verification?"
```

**Implementation Flow:**

```
1. ANALYZE REFERENCE
   └─ Read source code (formatters, views, templates)
   └─ Document all fields, variations, formats
   └─ Note every label, emoji, delimiter
   └─ Check for conditional logic

2. CREATE COMPLETENESS MAP
   └─ List every field in reference
   └─ Note exact formatting (e.g., "#01" vs "#1")
   └─ Document all status message variations
   └─ Map to target implementation structure

3. VERIFY BEFORE CODING
   └─ Show completeness map to user
   └─ Ask: "Am I missing anything from reference?"
   └─ Clarify any ambiguities

4. IMPLEMENT EXHAUSTIVELY
   └─ Build all fields in one pass
   └─ Match formatting exactly
   └─ Include all variations/conditionals

5. CROSS-REFERENCE
   └─ Compare output to reference line-by-line
   └─ Test all conditional branches
   └─ Verify edge cases match

6. SHOW USER
   └─ Demo matching output
   └─ Highlight completeness
   └─ Request final verification
```

**What "Match Exactly" Means:**

- ✅ Every field in original
- ✅ Exact label wording ("Overfill (wasted pips)" not "Overfill")
- ✅ Format strings ("#01" not "#1" if reference uses zero-padding)
- ✅ Delimiters ([5, 3, 2] vs 5 + 3 + 2)
- ✅ Status message variations (all branches)
- ✅ Emojis and symbols (💥 vs ⦿)
- ✅ Field order and grouping
- ✅ Conditional display logic (show X only when Y)
- ✅ Color coding, styling (if applicable)

**Anti-Pattern: Incremental Guessing**

❌ Don't do this:
```
Build basic version → User: "Missing X" → Add X
→ User: "Missing Y" → Add Y
→ User: "Don't make me ask again" → Finally read reference
```

✅ Do this instead:
```
Read reference → List all fields → Verify completeness
→ Build everything in one pass → Show complete result
```

**Why This Matters:**

When user says "match exactly," they usually designed the original carefully:
- Every field has a purpose
- Every format choice is intentional
- Missing pieces feel like their work wasn't respected

**Better to over-deliver than under-deliver on completeness.**

**Real Example:**

- **Task:** Make web UI match CLI output
- **Wrong approach:** Built incrementally, user requested missing pieces 4 times
- **Right approach:** Read `formatters.py`, list 15+ fields, implement all at once
- **Time saved:** ~90 minutes if done correctly from start

**File Reference:** `META_LEARNING_FRAMEWORK.md` lines 736-813 for detailed lessons

---

## Deployment Workflow

**Pattern: Internet Deployment Checklist**

When deploying a web application to the internet, follow this systematic process to avoid common pitfalls.

### Pre-Deployment Planning

**1. Platform Research (30-60 minutes)**

Research BEFORE starting deployment to avoid surprises:

```markdown
For each platform candidate:
- ✅ Pricing: Free tier limits? Monthly cost?
- ✅ Tech stack support: Does it support your runtime (Node.js, Python, etc.)?
- ✅ Build process: Can it build your frontend? Or do you need to commit dist/?
- ✅ Database: SQLite? Postgres? What's included?
- ✅ Deployment speed: How long to deploy? Auto-redeploy on git push?
- ✅ Sleep behavior: Does free tier sleep after inactivity?
- ✅ Domain: Custom domain? HTTPS included?
- ✅ Security: Secrets management? Environment variables?
```

**Common platforms:**
- **ngrok:** Quick demos, expose localhost. NOT for production. Security concerns with long-term exposure.
- **Railway:** Fast deployment, starts at ~$5-8/month. Good for production.
- **Render:** Free tier available, Python + Node.js, sleeps after 15 min. Good for hobby projects.
- **Vercel/Netlify:** Free tier for static sites + serverless. No long-running servers.
- **Heroku:** Paid tiers, established platform, reliable.
- **VPS (Digital Ocean, Linode):** Full control, $5-10/month, requires sysadmin knowledge.

**2. Security Review**

Before exposing anything to internet:
- ✅ No hardcoded secrets in code
- ✅ .env files in .gitignore
- ✅ API keys in environment variables
- ✅ CORS configured correctly (not allow-all in production)
- ✅ Input validation on all endpoints
- ✅ Rate limiting if needed
- ✅ HTTPS enforced

**Anti-Pattern:** "I'll fix security later"
**Pattern:** Security BEFORE deployment

### Git Repository Hygiene

**CRITICAL: Verify git repo location BEFORE first commit**

```bash
# Check current repo location
git rev-parse --show-toplevel

# DANGER: If this shows your home directory or Dropbox root, STOP!
# Example of WRONG location: /Users/you/Dropbox
# Example of RIGHT location: /Users/you/Dropbox/my-project

# If repo is at wrong location:
rm -rf .git  # Delete git repo
cd my-project  # Move to correct directory
git init  # Initialize in correct location
```

**Why this matters:**
- Git repo at parent directory = entire Dropbox/home folder committed to GitHub
- Privacy risk: personal files, documents, credentials exposed
- Performance: huge repo, slow operations
- **ALWAYS init git inside project directory**

**Checklist:**
- ✅ `git init` in project directory, NOT parent
- ✅ `.gitignore` includes: `node_modules/`, `venv/`, `.env`, `__pycache__/`, `*.pyc`, `.DS_Store`
- ✅ Review `git status` before first commit
- ✅ If deploying via GitHub: verify repo contents match project, not entire Dropbox

### Free Tier Pragmatism

**Pattern: Accept free tier constraints**

Free tiers often have limitations. Pragmatic solutions are OK for hobby projects:

**Example: Render free tier can't build frontend (no Node.js in Python environment)**

Options:
1. ❌ **Over-engineering:** Set up Docker, multi-stage builds, separate services
2. ❌ **Complexity:** Deploy frontend to Vercel, backend to Render
3. ✅ **Pragmatic:** Commit `dist/` build artifacts to git, serve from backend

**Decision criteria:**
- Is this a hobby project or production app?
- Hobby → Pragmatic solutions are fine
- Production → Proper build pipeline

**Acceptable for free tier hobby projects:**
- Committing build artifacts (`dist/`, `build/`)
- SQLite instead of Postgres
- No CI/CD pipeline
- Manual deployment

**NOT acceptable even for hobby:**
- Committing secrets
- No input validation
- Allowing security vulnerabilities

### Deployment Steps

**1. Local testing**
```bash
# Test backend
cd backend && python -m app.main
curl http://localhost:8000/health

# Test frontend
cd frontend && npm run build && npm run preview

# Test integration
# Open browser, test all features
```

**2. Environment setup**
```bash
# Create .env.example (template without secrets)
echo "DATABASE_URL=sqlite:///./app.db" > .env.example
echo "CORS_ORIGINS=http://localhost:3000" >> .env.example

# Add to .gitignore
echo ".env" >> .gitignore
echo "*.db" >> .gitignore
```

**3. Platform deployment**
```bash
# Example: Render deployment
1. Create account on render.com
2. Connect GitHub repo
3. Create Web Service
4. Configure:
   - Build command: `pip install -r requirements.txt`
   - Start command: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
   - Environment: Python 3.11
5. Add environment variables
6. Deploy
```

**4. Post-deployment testing**
```bash
# Test API
curl https://your-app.onrender.com/health
curl -X POST https://your-app.onrender.com/api/v1/endpoint

# Test in browser
# - All features
# - Mobile responsive
# - Different browsers
```

**5. Monitor first 24 hours**
- Watch for errors in platform logs
- Test after free tier sleep (15 min on Render)
- Verify database persists across restarts

### Common Deployment Issues

**Issue: "Works on localhost, not on production"**

Causes:
- Hardcoded `http://localhost:8000` URLs
- Environment variables not set
- CORS not configured for production domain
- Database path incorrect

Fix:
```javascript
// Bad
const API_URL = "http://localhost:8000/api"

// Good - auto-detect environment
const API_BASE = window.location.hostname === 'localhost'
  ? 'http://localhost:8000'
  : window.location.origin

const API_URL = `${API_BASE}/api/v1`
```

**Issue: "API returns 422 Validation Error"**

Cause: Frontend sending wrong data types

Fix:
```javascript
// Bad - string gets sent to API expecting int
const diceCount = e.target.value  // "10" (string)

// Good - parse to int
const diceCount = parseInt(e.target.value)  // 10 (number)
```

**Issue: "WebSocket connection failed"**

Cause: Wrong protocol (ws vs wss)

Fix:
```javascript
// Bad - hardcoded ws://
const WS_URL = "ws://localhost:8000/ws"

// Good - auto-detect protocol
const WS_PROTOCOL = window.location.protocol === 'https:' ? 'wss' : 'ws'
const WS_BASE = window.location.host
const WS_URL = `${WS_PROTOCOL}://${WS_BASE}/ws`
```

### Deployment Lessons from Real Projects

**From nexus_roller_web deployment:**

1. **Platform Evaluation**
   - Tried: ngrok → Railway → Render
   - ngrok: Good for quick demos, NOT long-term (security, requires laptop running)
   - Railway: Discovered $5-8/month AFTER starting (should have checked first)
   - Render: Free tier works, accepted constraints

2. **Git Repo Location**
   - Almost committed entire Dropbox to GitHub
   - User caught it: "validate my Dropbox isn't committed"
   - Showed proof via GitHub API
   - Lesson: ALWAYS verify repo location first

3. **Deterministic RNG for User Variations**
   - Bug: "Explode dice" was re-rolling ALL dice (wrong)
   - Expected: Keep same base dice, add explosion rolls (right)
   - Fix: Use seed parameter for deterministic RNG
   - Pattern: When user can "modify" a random result, use seeded RNG to ensure same starting point

4. **Frontend Build on Free Tier**
   - Render Python environment lacks Node.js
   - Can't build frontend during deployment
   - Options: Docker (complex), separate services (overkill), commit dist/ (pragmatic)
   - Chose: Commit dist/ - acceptable for hobby free tier

5. **User Concern Validation**
   - User said: "I don't want my laptop exposed to the internet all the time"
   - Response: Switched from ngrok (laptop-based) to Render (cloud-based)
   - Pattern: Take security concerns seriously, provide evidence-based reassurance

**Metrics:**
- Platforms tried: 3
- Time spent: ~4 hours (could have been 1 hour with better planning)
- Critical bug found: Explosion logic re-rolling dice
- Final result: Deployed successfully, works on internet

**URL:** https://nexus-roller-web.onrender.com (example from real deployment)

### Deployment Checklist Template

```markdown
## Pre-Deployment
- [ ] Platform research complete (pricing, tech stack, limitations)
- [ ] Git repo initialized in correct directory (NOT parent folder)
- [ ] .gitignore includes: .env, node_modules, venv, *.db, __pycache__
- [ ] No secrets in code (API keys, passwords, tokens)
- [ ] Environment variables documented in .env.example
- [ ] Local testing: backend API + frontend UI + integration
- [ ] Input validation on all API endpoints
- [ ] CORS configured for production domain

## Deployment
- [ ] Platform account created
- [ ] GitHub repo connected (if using git-based deployment)
- [ ] Build command configured
- [ ] Start command configured
- [ ] Environment variables set
- [ ] Database configured (SQLite path or connection string)
- [ ] Deploy triggered

## Post-Deployment
- [ ] Health check endpoint works: curl https://your-app.com/health
- [ ] API endpoints work: test with curl or Postman
- [ ] Frontend loads: open in browser
- [ ] All features tested: click through entire app
- [ ] Mobile responsive: test on phone or mobile simulator
- [ ] Different browsers: Chrome, Firefox, Safari
- [ ] After sleep: test after free tier sleep period (15 min for Render)
- [ ] Error monitoring: check platform logs for errors
- [ ] Share with test user: get feedback from real person

## 24-Hour Monitoring
- [ ] Check error logs
- [ ] Verify database persists
- [ ] Test from different networks
- [ ] Mobile testing
- [ ] Performance: any slow endpoints?
```

### When NOT to Deploy to Internet

Sometimes deployment isn't the right choice:

**Deploy later if:**
- ✅ Core features not working yet
- ✅ Major bugs still being fixed
- ✅ No one else needs to access it
- ✅ Still in rapid development (10+ changes/day)
- ✅ Security not reviewed

**Deploy now if:**
- ✅ Need to test with remote users
- ✅ Demo for stakeholders
- ✅ Multiplayer features require internet connectivity
- ✅ Ready for alpha/beta testing
- ✅ Want to learn deployment process

---

## Key Principles

1. **Copy framework → Use it → Learn from it → Improve it → Share back**
2. **Project-specific stays in project, generic goes to framework**
3. **Validate patterns 2+ times before adding to framework**
4. **Drop what doesn't help, keep what does**
5. **Framework serves you, not the other way around**
6. **When replicating: Read reference first, code second**

---

**Remember:** The best framework is the one you actually use. Start small, iterate, evolve.
