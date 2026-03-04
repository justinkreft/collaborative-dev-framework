# Tiered Response System

## Overview

A pattern for controlling information depth in knowledge work, preventing over-engineering simple questions while ensuring thorough analysis when needed.

## The Problem

When working with AI assistants or knowledge bases:
- Simple questions get buried in unnecessary detail (wastes time)
- Complex questions get shallow answers (misses critical info)
- No clear contract about depth/speed trade-offs
- User can't control effort level

## The Solution

Implement a three-tier system where users choose response depth upfront:

### Tier 1: Quick (⚡ ~30 seconds)
**Use for:** Facts, dates, yes/no questions, simple lookups

**Delivers:**
- Direct answer with key facts only
- Minimal context
- Basic citation if relevant
- "Want more detail?" offer at end

**Example:**
```
Question: "When does support end for Product X?"

Quick Answer:
Support ends: May 31, 2027

Want more detail?
```

---

### Tier 2: Balanced (📊 1-2 minutes)
**Use for:** Most questions, understanding concepts, decision support

**Delivers:**
- Answer with context
- Why it matters
- Inline citations [1], [2]
- Clear sections
- Key implications

**Example:**
```
Question: "What's the Product X support timeline?"

## Support Timeline
- Full Support ends: May 31, 2027
- Maintenance ends: May 31, 2032
- Extended support available until 2035

## What This Means
Full Support includes new features and updates [1]
Maintenance is security fixes only [2]

## Key Decision Points
Choose extended support 6 months before 2032 deadline

[1] Product lifecycle documentation
[2] Maintenance policy guide
```

---

### Tier 3: Deep (🔍 3-5 minutes)
**Use for:** Documentation, sharing with team, complex analysis, compliance

**Delivers:**
- Complete analysis with all details
- Multiple structured sections
- Full Sources section with:
  - Quotes/context
  - Actual URLs to discussions/documents
  - Citation numbers throughout
- Trade-offs and implications
- Related considerations

**Example:**
```
Question: "What's the Product X support timeline?"

## Complete Support Lifecycle

### Phase 1: Full Support (2022-2027)
[Detailed breakdown with all features]

### Phase 2: Maintenance (2027-2032)
[What's included, what's excluded, migration paths]

### Phase 3: Extended Support (2032-2035)
[Pricing, limitations, use cases]

### Decision Framework
[When to upgrade, when to extend, when to migrate]

### Sources
[1] "Full Support includes..." - https://company.com/docs/lifecycle
[2] "Maintenance phase provides..." - https://company.com/support/policy
[3] "Extended support pricing..." - https://company.com/pricing
```

---

## Implementation

### 1. Always Prompt for Depth

Never assume which level the user needs. Always ask unless explicitly specified.

**Prompt format:**
```
How much detail do you need?

1. ⚡ Quick (30 sec) - Just the answer
2. 📊 Balanced (1-2 min) - Answer + context
3. 🔍 Deep (3-5 min) - Full analysis + sources
```

### 2. Accept Multiple Input Methods

Allow specification via:
- **Numbers:** `1`, `2`, `3`
- **Text:** `quick`, `balanced`, `deep`
- **Inline:** "Quick - what is X?" or "Give me deep analysis of Y"

### 3. Match Effort to Need

**Performance principle:** Deliver minimum viable detail that satisfies the user's need.

| Question Type | Suggested Default | Rationale |
|--------------|-------------------|-----------|
| "When did X happen?" | Quick | Factual, no context needed |
| "How does X work?" | Balanced | Needs context to be useful |
| "Explain X for documentation" | Deep | Will be shared/referenced |

### 4. Offer Upgrades

Always end Quick/Balanced responses with upgrade path:
- Quick: "Want more detail/sources?"
- Balanced: "Want the full analysis with all sources?"

---

## Benefits

### For Users
- **Time savings:** Don't wait for unnecessary detail
- **Control:** Choose effort based on current need
- **Predictability:** Know what to expect from each level

### For Teams
- **Consistency:** Same structure across all knowledge queries
- **Efficiency:** Reduces back-and-forth clarifications
- **Scalability:** Works for novices and experts

### For AI Assistants
- **Clear expectations:** Unambiguous success criteria
- **Resource optimization:** Don't waste compute on unwanted detail
- **Better UX:** Responsive to user time constraints

---

## Anti-Patterns

❌ **Always defaulting to Deep**
- Wastes time on simple questions
- User asked "when" not "why, how, and implications"

❌ **Assuming user intent**
- "This seems important, I'll do Deep"
- Let the user decide importance

❌ **Inconsistent structure**
- Quick responses with Deep-level sourcing
- Deep responses with no Sources section

❌ **No upgrade path**
- User gets Quick, wants more, has to re-ask entire question
- Always offer "Want more?"

---

## Variations by Context

### Documentation Context
Default to **Balanced** or **Deep** - written content benefits from thoroughness

### Real-time Troubleshooting
Default to **Quick** - speed matters during incidents

### Learning/Onboarding
Default to **Balanced** - new team members need context

### Compliance/Audit
Always use **Deep** - full sourcing required

---

## Success Metrics

- Time to first answer (should decrease)
- Follow-up question rate (should decrease)
- User satisfaction with depth level
- Actual depth chosen by users (shows preferences)

**Goal:** <5% of responses require depth adjustment after initial delivery

---

## Related Patterns

- Memory-Based Preferences - Persist user's default depth preferences
- Source Attribution Standards - How to cite sources in Deep responses

---

**Remember:** The goal isn't always maximum detail—it's the right detail for the user's current need.
