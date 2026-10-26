# Go-to-Market

**Assume the role** of a go-to-market strategist

**Phase**: DISCOVERY PHASE — final stage (all paths), after Product Strategy

**Purpose**: Cover marketing, sales, and launch planning. This is the final stage of Discovery — completing the single living document that developers use as input context in their own workspace.

## Prerequisites
- Product Strategy stage must be complete and approved
- `aiplc-docs/discovery/discovery-document.md` must exist with Part 4 (Product Strategy) populated

## Execution Steps

### Step 1: Load Prior Discovery Context

Load `aiplc-docs/discovery/discovery-document.md` (Parts 1-4) and the same path-dependent artifacts listed in Step 1 of `product-strategy.md` (whichever exist).

Use these to inform all Go-to-Market decisions. Every recommendation should be grounded in the evidence that exists and the product strategy — not assumptions. Where Part 4 flags assumptions not backed by customer research, carry that flag into GTM.

### Step 2: Gather Go-to-Market Inputs

Create `aiplc-docs/discovery/go-to-market/gtm-questions.md` with questions covering the areas below.

**CRITICAL — Intelligent Defaults**: Every question MUST include an intelligent default as Option A, drawn from the Discovery context loaded in Step 1. The PM confirms or overrides rather than writing from scratch.

**MANDATORY question areas:**

#### Marketing Strategy
- What is the primary marketing channel for reaching the beachhead market?
- What is the messaging framework? (key messages, tagline, elevator pitch)
- What content or assets are needed for launch? (landing page, demo, case study, etc.)
- What is the pre-launch awareness strategy?

#### Sales Approach
- What is the sales model? (self-serve, inside sales, field sales, partner/channel, PLG)
- What is the expected sales cycle length?
- What are the key objections prospects will raise, and how do we address them?
- What sales enablement materials are needed?

#### Launch Planning
- What is the target launch date? (informed by PRFAQ summary paragraph)
- What is the launch strategy? (big bang, soft launch, beta/early access, phased rollout)
- What are the launch milestones and dependencies?
- What is the MVP scope for launch vs. post-launch roadmap?

#### Success Metrics & Monitoring
- What are the launch success criteria? (first 30/60/90 days)
- What metrics will be tracked from day one?
- What is the feedback collection mechanism post-launch?
- What are the kill criteria? (conditions under which to pivot or stop)

Use the standard AIPLC question format (multiple choice with [Answer]: tags, mandatory "Other" option).

```markdown
## Question [Number]
[Question text]

A) [Intelligent default from Discovery context] ← Suggested based on your Discovery work
B) [Alternative option]
C) [Alternative option]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

### ⛔ GATE: Await User Answers
DO NOT proceed until all GTM questions are answered and validated.
**MANDATORY**: Analyze ALL answers for ambiguities and create follow-up questions if needed.

### Step 3: Write Go-to-Market to Living Document

Write **Part 5: Go-to-Market** in `aiplc-docs/discovery/discovery-document.md`:

```markdown
# Part 5: Go-to-Market

## Marketing Strategy
- **Primary Channel**: [Channel]
- **Messaging Framework**:
  - **Tagline**: [Tagline]
  - **Elevator Pitch**: [2-3 sentences]
  - **Key Messages**: [List]
- **Pre-Launch Strategy**: [Description]
- **Launch Assets Needed**: [List]

## Sales Approach
- **Sales Model**: [Self-serve/Inside sales/Field sales/Partner/PLG]
- **Expected Sales Cycle**: [Duration]
- **Key Objections & Responses**:
  | Objection | Response |
  |---|---|
  | [Objection 1] | [Response] |
  | [Objection 2] | [Response] |
- **Sales Enablement Materials**: [List]

## Launch Plan
- **Target Launch Date**: [Date]
- **Launch Strategy**: [Big bang/Soft launch/Beta/Phased]
- **MVP Scope**: [What's in vs. out for launch]
- **Post-Launch Roadmap**: [Key features planned after launch]
- **Launch Milestones**:
  | Milestone | Target Date | Dependencies |
  |---|---|---|
  | [Milestone 1] | [Date] | [Dependencies] |
  | [Milestone 2] | [Date] | [Dependencies] |

## Success Metrics & Monitoring
- **30-Day Success Criteria**: [Criteria]
- **60-Day Success Criteria**: [Criteria]
- **90-Day Success Criteria**: [Criteria]
- **Key Metrics from Day One**: [List]
- **Feedback Collection**: [Mechanism]
- **Kill Criteria**: [Conditions to pivot or stop]
```

Update the document status header to `**Status**: Complete`, and replace any part still marked `_Not yet completed_` with `_Not executed in this path — [reason]_`.

### Step 4: Present Discovery Phase Completion

```markdown
# 🟣 Discovery Phase Complete

All stages of Discovery are now complete.

## Discovery Document Summary
- **Product**: [Product name]
- **Target Customer**: [Segment]
- **Primary Pain Point**: [Top pain point, or "Not researched in this path"]
- **Prototype**: [Validated with X users / Built, not validated / Skipped]
- **Revenue Model**: [Model]
- **Launch Strategy**: [Strategy]
- **Target Launch Date**: [Date]

## Artifacts
- `aiplc-docs/discovery/discovery-document.md` — Complete living document (all parts)
- `aiplc-docs/discovery/envision/` — Envision working files (Path A)
- `aiplc-docs/discovery/use-case-intake/`, `prioritization/` — Use case working files (Path A.2, Path B)
- `aiplc-docs/discovery/prototypes/` — PROTOTYPE-*.md files and prototype working files
- `aiplc-docs/discovery/product-strategy/` — Product Strategy working files
- `aiplc-docs/discovery/go-to-market/` — Go-to-Market working files

## Next Step
Discovery is complete in this workspace. Hand off the `aiplc-docs/discovery/` directory — especially `discovery-document.md` — to developers, who use it as input context in their own workspace (for example, the Inception phase of AI-DLC).

**Please review the complete Discovery Document and confirm it is ready for handoff.**
```

### ⛔ GATE: Await User Approval
DO NOT mark Discovery complete until the user explicitly approves the complete Discovery Document.

### Step 5: Update State Tracking

In `aiplc-docs/aiplc-state.md`, mark `Go-to-Market` as `[x]` and every stage that did not run in this path as `[-]`.

### Step 6: Log and End

- Log completion in `aiplc-docs/audit.md`
- End the workflow. Do NOT start Inception, Construction, or Operations stages in this workspace.
