# Product Strategy

**Assume the role** of a product strategy advisor

**Phase**: DISCOVERY PHASE — after a solution or use case has been selected (all paths)

**Purpose**: Capture positioning, differentiation, and business model decisions. These are informed by the structured discovery work — validated pain points, market research, a PRFAQ that has been through stakeholder review, and optionally validated prototype feedback from real users — rather than assumptions.

## Prerequisites
- `aiplc-docs/discovery/discovery-document.md` must exist (create it from the skeleton in "Artifact Contract", core-workflow.md, if it does not)
- A use case recorded under **Selected Use Case(s) for Strategy** in `aiplc-state.md`. (**Shortlisted Use Cases** do not count — they are the prototyping shortlist.) If none is recorded, ask which one this strategy is for and record it before continuing. A Discovery Document holds one strategy. If more than one is recorded, ask which one to use, set **Selected Use Case(s) for Strategy** to that one, and list the others in Part 4 under **Strategy Candidates Not Covered**. To take another candidate through Product Strategy → Go-to-Market later, copy the whole workspace (including `aiplc-docs/`) to a new folder after this Discovery ends, open it, and reopen Product Strategy (see "Reopening After Discovery Has Ended" in `common/workflow-changes.md`, which asks which candidate to use) — its prototype does not need to be rebuilt

Envision is **not** required — Path B and Entry Point 1 reach this stage without it.

## Execution Steps

### Step 1: Load Prior Discovery Context

Load whichever of these exist (all paths relative to `aiplc-docs/discovery/`):
- `discovery-document.md` (Parts 1-3)
- `envision/business-context.md` and `envision/pain-point-analysis.md` — Path A
- `use-case-intake/use-cases.md` and `prioritization/scoring.md` — Path A.2, Path B
- `prototypes/{selected-slug}/PROTOTYPE-{selected-slug}.md` and `iteration-log.md` — Entry Point 1, Path A.2, Path B
- `prototypes/{solution-slug}/validation-results.md` — Path A.1

Use these to inform all Product Strategy decisions. Every question and recommendation should be grounded in the evidence that exists, not assumptions.

**If validation results exist**: Strategy questions should reference specific validation findings (e.g., "Users validated that X is the top pain point" or "3 of 5 users requested Y").

**If there is no pain point analysis** (Path B, Entry Point 1): Draw defaults from the use case and prototype specification. State in the questions and in Part 4 that positioning, pricing, and willingness-to-pay are assumptions not yet backed by customer research.

### Step 2: Gather Product Strategy Inputs

Create `aiplc-docs/discovery/product-strategy/strategy-questions.md` with questions covering the areas below.

**CRITICAL — Intelligent Defaults**: Every question MUST include an intelligent default as Option A, drawn from the context loaded in Step 1. The PM confirms or overrides rather than writing from scratch.

**MANDATORY question areas:**

#### Positioning
- How should this product be positioned in the market? (e.g., premium, budget, niche specialist, platform)
- What is the one-sentence value proposition?
- What is the primary message customers should associate with this product?

#### Differentiation
- What are the top 3 differentiators vs. existing solutions? (informed by PRFAQ competitive analysis)
- Which differentiator is the most defensible long-term?
- What is the product's unfair advantage or moat?

#### Business Model
- What is the revenue model? (subscription, one-time purchase, freemium, usage-based, marketplace, etc.)
- What is the pricing strategy? (informed by willingness-to-pay data from pain point analysis)
- What are the key cost drivers?
- What is the expected gross margin?

#### Target Market
- What is the initial beachhead market? (the specific segment to win first)
- What is the expansion path after the beachhead? (adjacent segments)
- What are the key customer acquisition channels?

#### Success Metrics
- What are the primary KPIs for the first 6 months?
- What does product-market fit look like for this product?
- What are the leading indicators of success vs. failure?

Use the standard AIPLC question format (multiple choice with [Answer]: tags, mandatory "Other" option).

```markdown
## Question [Number]
[Question text]

A) [Intelligent default from Envision context and prototype validation] ← Suggested based on your Discovery work
B) [Alternative option]
C) [Alternative option]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

### ⛔ GATE: Await User Answers
DO NOT proceed until all strategy questions are answered and validated.
**MANDATORY**: Analyze ALL answers for ambiguities and create follow-up questions if needed.

### Step 3: Write Product Strategy to Living Document

Write **Part 4: Product Strategy** in `aiplc-docs/discovery/discovery-document.md`:

```markdown
# Part 4: Product Strategy

**For**: [Selected use case]

## Positioning
- **Market Position**: [Premium/Budget/Niche/Platform]
- **Value Proposition**: [One sentence]
- **Primary Message**: [What customers should associate with this product]

## Differentiation
| Differentiator | Description | Defensibility |
|---|---|---|
| [Differentiator 1] | [Description] | [High/Medium/Low] |
| [Differentiator 2] | [Description] | [High/Medium/Low] |
| [Differentiator 3] | [Description] | [High/Medium/Low] |

- **Primary Moat**: [Unfair advantage or defensible moat]

## Business Model
- **Revenue Model**: [Subscription/One-time/Freemium/Usage-based/etc.]
- **Pricing Strategy**: [Description, informed by willingness-to-pay data]
- **Key Cost Drivers**: [List]
- **Expected Gross Margin**: [Estimate]

## Target Market
- **Beachhead Market**: [Specific initial segment]
- **Expansion Path**: [Adjacent segments after beachhead]
- **Key Acquisition Channels**: [List]

## Success Metrics
| Metric | Target (6 months) | Leading Indicator |
|---|---|---|
| [KPI 1] | [Target] | [Leading indicator] |
| [KPI 2] | [Target] | [Leading indicator] |
| [KPI 3] | [Target] | [Leading indicator] |

- **Product-Market Fit Signal**: [What PMF looks like for this product]
```

Update the document status header to `**Status**: In Progress — Product Strategy`.

### Step 4: Present Product Strategy Completion

```markdown
# 📊 Product Strategy Stage Complete

## Summary
- **Positioning**: [One-line summary]
- **Revenue Model**: [Model]
- **Beachhead Market**: [Segment]
- **Primary Differentiator**: [Top differentiator]

## Artifacts Updated
- `aiplc-docs/discovery/discovery-document.md` — Living document (Part 4: Product Strategy)
- `aiplc-docs/discovery/product-strategy/strategy-questions.md` — Strategy questions with answers

## Next Step
Proceeding to **Go-to-Market** to cover marketing, sales, and launch planning.

**Please review the Product Strategy section of the Discovery Document and confirm to proceed.**
```

### ⛔ GATE: Await User Approval
DO NOT proceed to Go-to-Market until the user explicitly approves the Product Strategy output.

### Step 5: Update State Tracking

In `aiplc-docs/aiplc-state.md`, mark `Product Strategy` as `[x]`.

### Step 6: Log and Proceed

- Log completion in `aiplc-docs/audit.md`
- Proceed to Go-to-Market stage
