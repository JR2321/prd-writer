---
name: prd-writer
description: Write, review, or improve Product Requirements Documents (PRDs). Use when the user asks to "write a PRD", "create a product spec", "draft requirements", "review my PRD", "improve my product spec", mentions "product requirements", "feature spec", "engineering spec", or discusses product planning, feature definition, requirements gathering, or turning ideas into implementable specs. Also triggers on "PRD", "product spec", "requirements document", or "feature requirements".
---

# PRD Writer

_(Inspired by Stripe, Linear, Notion, and Amazon)_

> "A great PRD removes ambiguity. If an engineer has to guess, the PRD failed."

## Your Role

When this skill is active:
- Write PRDs that engineers (or their coding agents) can implement directly
- Review existing PRDs against the 10 principles below
- Identify gaps, ambiguities, and missing acceptance criteria
- Ensure every feature is scoped to a shippable increment
- Provide specific, actionable feedback when reviewing
- Output PRDs in clean markdown that can live in a repo or docs system

---

## **1. Start With the Problem, Not the Solution**

**Principle**

Every PRD opens with a clear problem statement.

> Who has this problem? How painful is it? What happens if we do nothing?

Not: feature wishlists, solution descriptions, or technology choices.

**GREAT PRDs look like**

```markdown
## Problem

Merchants processing >$1M/month cannot reconcile payouts with individual
orders. They spend 8-12 hours per week on manual matching. Three enterprise
prospects have cited this as the reason they chose a competitor.
```

Then:
- Who specifically is affected (segment, persona)
- How you know this is real (data, quotes, support tickets)
- What the cost of inaction is

**Anti-pattern**

"We need to build a reconciliation dashboard with filtering, export, and real-time updates."

That's a solution spec pretending to be a problem statement.

---

## **2. Define Success Before Defining Features**

**Principle**

State measurable outcomes before listing a single feature.

Success criteria answer: "How will we know this shipped well?"

**GREAT PRDs look like**

```markdown
## Success Criteria

- Reconciliation time drops from 8-12 hrs/week to <1 hr/week
- 90% of payouts auto-matched within 24 hours of settlement
- Zero enterprise prospects lost to reconciliation gaps in Q3
```

**Anti-pattern**

"Success: dashboard is launched and available to all merchants."

Launching is not success. Outcomes are success.

---

## **3. Write for the Engineer Who Will Build It**

**Principle**

The primary audience is the person (or agent) implementing this.

Every section should answer: "Does an engineer know exactly what to build after reading this?"

**Rules**
- Use precise language ("the system must" not "it would be nice if")
- Define every noun (what is a "merchant"? what is a "payout"?)
- Avoid marketing language, vision statements, and aspirational fluff
- Include constraints: performance, security, compliance, compatibility

---

## **4. Scope Ruthlessly: One Shippable Increment**

**Principle**

A PRD covers one deliverable unit. Not the roadmap. Not the vision.

If it can't ship in 2-6 weeks, it's too big. Break it down.

**GREAT PRDs look like**

```markdown
## Scope

### In Scope
- Auto-matching engine for Stripe payouts to internal orders
- Match confidence scoring (exact, fuzzy, unmatched)
- Manual review queue for low-confidence matches

### Out of Scope (Future)
- Multi-PSP support (Phase 2)
- Accounting system integrations (Phase 3)
- Real-time streaming reconciliation
```

**Anti-pattern**

A 40-page document covering three quarters of roadmap with no clear cut line.

---

## **5. Acceptance Criteria for Every Requirement**

**Principle**

Each requirement needs testable acceptance criteria. If you can't write a test for it, the requirement is too vague.

**GREAT PRDs look like**

```markdown
### Requirement: Auto-Match Engine

The system must automatically match settled payouts to orders.

**Acceptance Criteria:**
- Given a payout with a charge ID that matches exactly one order,
  the system marks it as "exact match" with 100% confidence
- Given a payout with amount matching multiple orders,
  the system marks it as "fuzzy match" and ranks candidates by date proximity
- Given a payout with no matching order within 5% amount tolerance,
  the system marks it as "unmatched" and adds it to the review queue
- Matching runs within 5 minutes of payout webhook receipt
```

**Anti-pattern**

"The system should match payouts to orders accurately."

---

## **6. Show the User Journey, Not Just Data Models**

**Principle**

Walk through what the user actually does, step by step.

Engineers build better products when they understand the human flow, not just the API contract.

**GREAT PRDs look like**

```markdown
## User Journey

1. Merchant logs in, sees "Reconciliation" tab with today's summary
   - "142 matched, 3 needs review, 1 unmatched"
2. Clicks "Needs Review" -> sees table of fuzzy matches
   - Each row shows: payout amount, top 3 candidate orders, confidence %
3. Clicks a row -> side panel shows payout details and order details
4. Clicks "Confirm Match" or "Mark Unrelated"
5. At end of session, clicks "Export" -> downloads CSV for accounting
```

**Anti-pattern**

A list of API endpoints with no context on how a human interacts with the system.

---

## **7. Specify the Edges: Errors, Limits, and Empty States**

**Principle**

The happy path is 30% of the work. The edges are 70%.

Explicitly cover: what happens when things break, when there's no data, when limits are hit.

**GREAT PRDs look like**

```markdown
## Edge Cases

- **No payouts in period:** Show empty state with "No payouts received
  in this period" and link to payout schedule docs
- **Duplicate charge IDs:** Flag as anomaly, surface in review queue,
  do not auto-match
- **Payout amount exceeds $100K:** Require manual review regardless
  of match confidence (compliance requirement)
- **Webhook delivery failure:** Retry 3x with exponential backoff.
  After 3 failures, create alert in ops dashboard.
```

---

## **8. Include Technical Constraints and Non-Functional Requirements**

**Principle**

Performance, security, scalability, and compliance requirements are not optional sections. They directly affect architecture decisions.

**GREAT PRDs look like**

```markdown
## Technical Requirements

- **Latency:** Match results available within 5 min of payout webhook
- **Scale:** Support 50K payouts/day at launch, 500K/day by Q4
- **Data retention:** Match records retained 7 years (SOX compliance)
- **Auth:** Merchant admin role required; read-only for staff role
- **API:** RESTful endpoints for match status; webhook for match completion
- **Idempotency:** Re-processing a payout must not create duplicate matches
```

**Anti-pattern**

"The system should be fast and secure." That tells the engineer nothing.

---

## **9. Provide Context, Not Mandates, on Implementation**

**Principle**

Share relevant technical context. Don't dictate architecture.

The PRD should say what and why. Engineers decide how.

**GREAT PRDs look like**

```markdown
## Technical Context

- Payouts arrive via Stripe webhooks (payout.paid event)
- Order data lives in PostgreSQL (orders table, ~2M rows)
- Existing matching logic in billing-service is unreliable above 10K/day
- Team has experience with Temporal for workflow orchestration
```

**Anti-pattern**

"Use Kafka for event streaming, Redis for caching, and deploy on EKS with Helm charts."

Unless there's a hard constraint, let the builders choose.

---

## **10. Make It a Living Document, Not a Handoff Artifact**

**Principle**

A PRD is not done when it's written. It's done when the feature ships.

**Operational rules**
- PRD lives in the repo (not a forgotten Google Doc)
- Open questions are tracked inline with `[OPEN]` tags
- Decisions are recorded with date and rationale
- Engineers can propose changes via PR to the PRD
- Post-launch: add a "Results" section comparing actuals to success criteria

---

## **PRD Template**

When writing a new PRD, use this structure:

```markdown
# [Feature Name]

**Author:** [name]
**Status:** Draft | In Review | Approved | In Progress | Shipped
**Last Updated:** [date]
**Target Release:** [date or milestone]

---

## Problem

[Who has this problem? How painful? What's the cost of inaction?]

## Success Criteria

- [Measurable outcome 1]
- [Measurable outcome 2]
- [Measurable outcome 3]

## User Journey

1. [Step-by-step flow of what the user does]

## Requirements

### [Requirement 1 Name]

[Description]

**Acceptance Criteria:**
- Given [context], when [action], then [outcome]
- ...

### [Requirement 2 Name]

...

## Scope

### In Scope
- [Item]

### Out of Scope (Future)
- [Item with brief rationale]

## Edge Cases

- **[Scenario]:** [How the system handles it]

## Technical Requirements

- **Performance:** [Latency, throughput targets]
- **Security:** [Auth, encryption, compliance]
- **Scale:** [Current and target volumes]
- **Compatibility:** [Integration points, API contracts]

## Technical Context

[Relevant architecture, data sources, existing systems.
NOT implementation mandates.]

## Open Questions

- [OPEN] [Question that needs resolution before build]
- [RESOLVED 2024-01-15] [Question] -> [Decision + rationale]

## References

- [Link to design doc, research, competitive analysis]
```

---

## **How to Review a PRD**

### 1. The Implementability Test

Read the PRD as if you're the engineer assigned to build it tomorrow.
- Do you know exactly what to build?
- Can you estimate the work?
- Are there requirements you'd need to ask about before starting?

If you'd need a follow-up meeting, the PRD isn't done.

### 2. Principle-by-Principle Audit

**Problem Statement (Principle 1)**
- Is the problem clearly stated with evidence?
- Could you explain the "why" to a new team member?

**Success Criteria (Principle 2)**
- Are outcomes measurable and time-bound?
- Would you know if this succeeded 30 days after launch?

**Engineer Readability (Principle 3)**
- Is every term defined?
- Are requirements precise (not "should be fast")?

**Scope (Principle 4)**
- Can this ship in 2-6 weeks?
- Is the cut line between in/out of scope clear?

**Acceptance Criteria (Principle 5)**
- Can you write tests from the acceptance criteria?
- Are given/when/then patterns used?

**User Journey (Principle 6)**
- Can you trace the full user flow?
- Are all screens/states described?

**Edge Cases (Principle 7)**
- Are errors, empty states, and limits covered?
- What happens at boundaries?

**Non-Functional Requirements (Principle 8)**
- Are performance, security, and scale targets stated?
- Are compliance requirements called out?

**Implementation Context (Principle 9)**
- Is context shared without dictating architecture?
- Are relevant systems and data sources listed?

**Living Document (Principle 10)**
- Are open questions tracked?
- Is the PRD in version control?

### 3. Provide Specific Feedback

Structure feedback as:
- **Issue:** What's missing or ambiguous (cite the principle)
- **Impact:** What goes wrong if this ships as-is
- **Recommendation:** Specific improvement with example

### 4. The North Star Question

> "Could an engineer (or their coding agent) start building from this PRD tomorrow without a single follow-up question?"

If not, the PRD isn't done.
