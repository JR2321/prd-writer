# PRD Writer

A Claude Code / OpenClaw skill for writing world-class Product Requirements Documents that engineers (or their coding agents) can implement directly.

Inspired by how Stripe, Linear, Notion, and Amazon think about product specs.

## Install

```bash
# Clone into your skills directory
git clone https://github.com/JR2321/prd-writer.git ~/.claude/skills/prd-writer
```

The skill is now available in all Claude Code sessions.

## Usage

**Direct command:**
```
/prd-writer
```

**Natural language triggers:**
- "Write a PRD for [feature]"
- "Review my product spec"
- "Draft requirements for [project]"

## The 10 Principles

1. **Start With the Problem, Not the Solution** — who, how painful, cost of inaction
2. **Define Success Before Features** — measurable outcomes, not "we launched it"
3. **Write for the Engineer Who Will Build It** — precise language, defined terms, no fluff
4. **Scope Ruthlessly: One Shippable Increment** — 2-6 weeks, clear cut line
5. **Acceptance Criteria for Every Requirement** — given/when/then, testable
6. **Show the User Journey** — step-by-step flow, not just data models
7. **Specify the Edges** — errors, limits, empty states (70% of the real work)
8. **Include Non-Functional Requirements** — perf, security, scale, compliance targets
9. **Provide Context, Not Mandates** — share what/why, let engineers decide how
10. **Make It a Living Document** — lives in repo, tracked questions, post-launch results

## North Star Question

> "Could an engineer or their coding agent start building from this PRD tomorrow without a single follow-up question?"

If not, the PRD isn't done.

## What's Included

- **10 principles** with examples, anti-patterns, and "GREAT PRDs look like" sections
- **Full PRD template** ready to fill in
- **Review framework** for auditing existing PRDs principle-by-principle
- **Packaged `.skill` file** for easy distribution

## Credits

Built by [JR](https://github.com/JR2321), inspired by the [dev-docs-reviewer](https://github.com/csmoove530/dev-docs-reviewer) skill format.
