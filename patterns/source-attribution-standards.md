# Source Attribution Standards

## Overview

Requirements for citing sources when AI assistants provide information from knowledge bases, ensuring verifiability and building trust.

## The Problem

**Without proper attribution:**
- Can't verify claims
- Can't trace back to original source
- Can't assess source quality
- Can't share findings with confidence
- Compliance/audit trail missing

**Example of poor attribution:**
```
The policy changed last year.
[No source, can't verify, can't share]
```

## The Solution

Implement consistent citation standards that make information verifiable and traceable.

---

## Minimum Requirements

### 1. Inline Citations

**Every claim needs a citation number:**

```markdown
The policy changed in June 2025 [1]. This reversed the previous
enforcement rule [2] and gave customers more flexibility [3].
```

**Not:**
```markdown
The policy changed in June 2025. This reversed the previous rule.
[Where did this come from? Can't verify]
```

### 2. Sources Section

**Every response needs a Sources section at the end:**

```markdown
### Sources

[1] "Policy update announcement" - https://company.com/updates/2025-06

[2] "Original enforcement policy" - https://company.com/policies/2025-02

[3] "Customer flexibility improvements" - https://company.com/blog/changes
```

### 3. Source Components

**Each source must include:**
- **Citation number:** [1], [2], etc.
- **Quote or context:** Brief excerpt or description
- **URL:** Actual link to discussion/document
- **Title/type** (if relevant): Document name, discussion thread, etc.

---

## Format Standards

### Internal Discussions (Slack, Teams, etc.)

```markdown
[1] "User asked about policy change timeline..."
- https://internal.slack.com/archives/CHANNEL/p1234567890
```

**Include:**
- Relevant quote from discussion
- Direct link to message/thread
- Optional: Date if not in URL

### Official Documentation

```markdown
[2] Product Lifecycle Policy
- https://company.com/docs/lifecycle
```

**Include:**
- Document title
- Direct URL to page
- Optional: Section reference if long doc

### Multiple Sources for Same Claim

```markdown
The feature is available in all tiers [1, 2, 3].

### Sources
[1] "Pricing documentation" - https://company.com/pricing
[2] "Feature matrix" - https://company.com/features
[3] Internal discussion confirming - https://internal.slack.com/...
```

---

## Depth Levels

### Quick Responses

**Minimal citation acceptable:**

```markdown
Support ends: May 31, 2027

[Product lifecycle documentation]
```

### Balanced Responses

**Inline numbers, source mentions (URLs optional):**

```markdown
Support ends May 31, 2027 [1]
Extended support available until 2035 [2]

[1] Product lifecycle policy
[2] Extended support options
```

### Deep Responses

**Full sourcing required:**

```markdown
Support ends May 31, 2027 [1]. Extended support is available as
an add-on until 2035 [2], but requires purchase 6 months before
the deadline [3].

### Sources

[1] "Maintenance Support phase ends on May 31, 2032"
- https://company.com/docs/lifecycle#maintenance-phase

[2] "Extended Life Cycle Support (ELS) Add-on available"
- https://company.com/docs/els-addon

[3] "Customers must purchase ELS 6 months prior to deadline"
- https://internal.slack.com/archives/SUPPORT/p9876543210
```

---

## Special Cases

### Synthesized Information

**When combining multiple sources:**

```markdown
The recommended approach is to start with Option A, then migrate
to Option B when you reach scale [1, 2, 3].

### Sources

[1] "Start with SQLite for MVP" - Discussion thread
[2] "Migrate to PostgreSQL at ~1000 users" - Architecture docs
[3] "Migration path documentation" - Internal wiki

Note: This recommendation synthesizes guidance from multiple sources
```

### Contradictory Sources

**When sources disagree:**

```markdown
The policy may vary by region:
- North America: Rule A applies [1]
- Europe: Rule B applies [2]
- APAC: Check with regional team [3]

### Sources
[1] NA policy doc - https://...
[2] EU policy doc - https://...
[3] APAC escalation thread - https://...

Note: Sources show regional variation - confirm for your region
```

### No Source Available

**When answering from general knowledge:**

```markdown
This is a general software engineering principle (not specific to
this organization). [General knowledge - no internal source]

For organization-specific guidance, consult: [relevant doc/person]
```

---

## Quality Standards

### Good Citations

✅ **Specific and verifiable:**
```markdown
[1] "The all-or-nothing rule was reversed in June 2025"
- https://internal.slack.com/archives/CHANNEL/p1234567890
- Thread: Policy Update Discussion
```

✅ **Includes context:**
```markdown
[2] Product Manager confirmed this approach in planning meeting
- https://company.com/meetings/notes/2025-03-04
- See: Section "Database Selection"
```

✅ **Multiple perspectives:**
```markdown
Feature availability confirmed by:
[1] Product documentation
[2] Engineering team discussion
[3] Customer-facing FAQ
```

### Poor Citations

❌ **Too vague:**
```markdown
[1] Someone mentioned this somewhere
```

❌ **No URL:**
```markdown
[2] Internal policy document (no link provided)
```

❌ **Broken/inaccessible link:**
```markdown
[3] https://internal.server/document-that-moved
```

---

## Benefits

### For Users
- **Verifiable:** Can check claims independently
- **Shareable:** Can forward with confidence
- **Traceable:** Can find original context
- **Trustworthy:** Evidence-based, not opinion

### For Teams
- **Audit trail:** Clear provenance of decisions
- **Knowledge transfer:** Easy to onboard new members
- **Consistency:** Same standards across team
- **Accountability:** Know where info came from

### For Compliance
- **Documentation:** Required for many industries
- **Traceability:** Can prove decision basis
- **Reproducibility:** Others can verify findings

---

## Anti-Patterns

❌ **No sources:**
```markdown
The policy changed last year.
[Can't verify, can't trust]
```

❌ **Generic sources:**
```markdown
[1] Company documentation
[Where? Which doc? Which section?]
```

❌ **Inconsistent formatting:**
```markdown
[1] https://example.com
[2] "Quote here" https://example.com/other
[3] Document title - https://example.com/another "quote"
[Hard to scan, looks unprofessional]
```

❌ **Too many sources for simple claims:**
```markdown
The product was released in 2022 [1, 2, 3, 4, 5, 6, 7, 8].
[Overkill - one authoritative source is enough]
```

---

## Integration with Other Patterns

**Tiered Response System:**
- Quick: Minimal citation
- Balanced: Inline numbers + basic sources
- Deep: Full sourcing with URLs and quotes

**Memory-Based Preferences:**
```markdown
## Citation Standards
- Always cite with URLs
- Use numbered format [1], [2]
- Include Sources section for Balanced/Deep responses
```

---

## Tools and Automation

### Link Validation

**Periodically verify links aren't broken:**
```bash
# Check all markdown files for broken links
find . -name "*.md" -exec grep -o 'https://[^)]*' {} \; | sort -u | while read url; do
  curl -s -o /dev/null -w "%{http_code} $url\n" "$url"
done
```

### Citation Format Linter

**Check for consistent formatting:**
- All citations numbered
- All numbers have corresponding sources
- Sources section present when citations used
- URLs are valid format

---

## Success Metrics

- **Verifiability rate:** % of claims that can be traced to source
- **Link validity:** % of source URLs that work
- **User trust:** Reduction in "where did you get this?" questions
- **Shareability:** Increase in outputs being shared with team

**Red flags:**
- Frequent "I can't find that source" feedback
- Broken links in sources
- Vague citations that don't help
- Inconsistent formatting across responses

---

## Related Patterns

- Tiered Response System - Different citation depth by tier
- Memory-Based Preferences - Store team's citation requirements

---

**Remember:** The best source citation is specific enough to verify, but concise enough to scan quickly.
