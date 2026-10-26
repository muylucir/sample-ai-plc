# DISCOVERY PHASE WORKFLOW (Product Manager Workspace)

## Purpose
This workspace is exclusively for Product Managers to complete the Discovery phase. The output is a comprehensive Discovery Document that will be provided to developers for Inception and Construction phases in a separate workspace.

## Workflow Principle
**The workflow adapts to the work, not the other way around.**

The AI model intelligently assesses which Discovery stages are needed based on:
1. Whether PROTOTYPE-*.md files already exist
2. Whether user wants to start from pain points or use cases
3. Number of use cases to evaluate
4. User's goals (handoff files vs build prototypes)

## MANDATORY: Rule Details Loading
**CRITICAL**: When performing any phase, you MUST read and use relevant content from rule detail files.

Rule details location (paths relative to the workspace root, matching the README setup for each tool):
- **Kiro**: `.kiro/aws-aiplc-rule-details/` (this file is in `.kiro/steering/aws-aiplc-rules/`)
- **Claude Code and other tools**: `./aws-aiplc-rule-details/` (this file is the workspace's `CLAUDE.md` or equivalent)

Use whichever of the two directories exists. If neither exists, tell the user the rule details are missing and point them to the setup steps in the README — do not continue without them. All rule paths in these rules (`common/...`, `discovery/...`, `inception/...`) are relative to that directory.

**Common Rules**: ALWAYS load common rules at workflow start:
- Load `common/process-overview.md` for workflow overview
- Load `common/session-continuity.md` for session resumption guidance
- Load `common/content-validation.md` for content validation requirements
- Load `common/question-format-guide.md` for question formatting rules
- Load `common/security-guardrails.md` for URL fetching, input file, prototype execution, and credential rules (apply to every stage and path)
- Reference these throughout the workflow execution

**On-demand Rules**: Load only when the situation arises:
- Load `common/error-handling.md` when an error occurs or artifacts are missing/inconsistent
- Load `common/workflow-changes.md` when the user asks to skip, add, redo, or pause a stage

## MANDATORY: Content Validation
**CRITICAL**: Before creating ANY file, you MUST validate content according to `common/content-validation.md` rules:
- Validate Mermaid diagram syntax
- Validate ASCII art diagrams (see `common/ascii-diagram-standards.md`)
- Escape special characters properly
- Provide text alternatives for complex visual content
- Test content parsing compatibility

## MANDATORY: Question File Format
**CRITICAL**: When asking questions at any phase, you MUST follow question format guidelines.

**See `common/question-format-guide.md` for complete question formatting rules including**:
- Choice questions (A, B, C... plus `X) Other`) and open questions
- [Answer]: tag usage
- Where question files live, and the only things that may be asked in chat
- Answer validation and ambiguity resolution

## MANDATORY: Custom Welcome Message
**CRITICAL**: When starting ANY Discovery workflow, you MUST display the welcome message.

**How to Display Welcome Message**:
1. Load the welcome message from `common/welcome-message.md`
2. Display the complete message to the user
3. This should only be done ONCE at the start of a new workflow
4. Do NOT load this file in subsequent interactions to save context space

---

# DISCOVERY PHASE WORKFLOW

## Overview

The Discovery Phase has **THREE ENTRY POINTS**:

1. **Entry Point 1 (Highest Priority)**: Existing PROTOTYPE-*.md files → Build prototypes directly
2. **Entry Point 2 (Path A)**: Start from pain points → PR/FAQ → Solution Analysis
3. **Entry Point 3 (Path B)**: Start from use cases → Prioritization → Prototype Context Generation

---

## Workspace Detection (ALWAYS EXECUTE FIRST)

1. **MANDATORY**: Log initial user request in audit.md with complete raw input
2. Load all steps from `inception/workspace-detection.md`
3. Execute workspace detection:
   - **FIRST**: Check for existing `aiplc-docs/aiplc-state.md` — if found, resume the recorded stage (see `common/session-continuity.md`)
   - **THEN**: Check for existing PROTOTYPE-*.md files (Entry Point 1)
     - Path: `aiplc-docs/discovery/prototypes/*/PROTOTYPE-*.md`
     - If found: Skip all discovery, jump to Prototype Building
   - Check for Discovery artifacts without a state file (rebuild state, then resume)
4. Determine next stage:
   - If resuming: The recorded stage
   - If PROTOTYPE-*.md found: Prototype Building (Entry Point 1)
   - Otherwise: Discovery Mode Selection (Entry Point 2 or 3)
5. **MANDATORY**: Log findings in audit.md
6. Present completion message to user
7. Automatically proceed to next phase

---

## ENTRY POINT 1: Existing PROTOTYPE-*.md Files

**Execute IF**: Workspace Detection finds PROTOTYPE-*.md files

**Purpose**: Build prototypes from pre-generated specifications (typical workshop scenario)

**Flow**:
1. Announce found PROTOTYPE-*.md files
2. Confirm the user trusts them (they or their team created them) — see `inception/workspace-detection.md`
3. **SKIP ALL DISCOVERY PHASES**
4. **JUMP TO**: Prototype Building (see below)
5. After prototypes built, continue to Product Strategy → GTM

---

## ENTRY POINT 2 & 3: Discovery Mode Selection

**Execute IF**: No PROTOTYPE-*.md files found

Load `discovery/discovery-mode-selection.md`

Ask user (in `aiplc-docs/discovery/discovery-mode-questions.md`):
```
How would you like to start Discovery?

A) Start from customer pain points (create PR/FAQ)
B) I already have use cases to prioritize
X) Other (please describe after [Answer]: tag below)

[Answer]:
```

- If [A]: Proceed to **PATH A - Envision**
- If [B]: Proceed to **PATH B - Use Case Intake**

---

## PATH A: Start from Pain Points

### Step 1: Envision

Load `discovery/envision.md`

**Execution**:
1. **MANDATORY**: Log start of Envision in audit.md
2. Determine pain point gathering mode (interactive or URL-based)
3. If URL mode: read ONLY the user-provided URL
4. Gather and confirm customer pain points
5. Generate PR/FAQ document using Working Backwards format
6. **Wait for Explicit Approval**: Present PR/FAQ for review
7. **MANDATORY**: Log user's response in audit.md

### Step 2: Solution Analysis

Load `discovery/solution-analysis.md`

**Execution**:
1. Analyze PR/FAQ to identify solutions
2. Determine: Single solution OR Multiple solutions

**Branch A.1: Single Solution**
- One clear solution from PR/FAQ
- Proceed to Prototype & Validation (single prototype flow)
- Load `discovery/prototype-validation.md` (for an agentic solution it uses the LLM, credential, and Strands steps of `discovery/prototype-building.md`)
- Build prototype, iterate, validate
- Continue to Product Strategy → GTM

**Branch A.2: Multiple Solutions**
- PR/FAQ suggests N different solution options
- Extract use cases from PR/FAQ
- **MERGE WITH PATH B** at Use Case Intake (pre-filled from the identified solutions — the user confirms instead of re-entering them), then Use Case Prioritization

---

## PATH B: Start from Use Cases

### Step 1: Use Case Intake

Load `discovery/use-case-intake.md`

**Execution**:
1. **MANDATORY**: Log start in audit.md
2. Ask: "How many use cases?" (could be 3, 5, 10, N)
3. Gather details for all N use cases
4. Categorize: Agentic vs Application
5. Document in `aiplc-docs/discovery/use-case-intake/use-cases.md`

### Step 2: Use Case Prioritization

Load `discovery/use-case-prioritization.md`

**Execution**:
1. Apply prioritization frameworks:
   - Agentic framework for agentic use cases
   - Application framework for application use cases
2. Generate scores for all N use cases
3. Present ranked list
4. User confirms or adjusts
5. Select top 3 for prototyping
6. Document in `aiplc-docs/discovery/prioritization/`

### Step 3: Prototype Context Generation

Load `discovery/prototype-context-generation.md`

**Execution**:
1. For each of top 3 use cases:
   - Gather design context (brand URL)
   - Define requirements (LLM, tools, features)
   - Specify frontend (device, screens)
   - Generate PROTOTYPE-{use-case-slug}.md file
2. Create 3 PROTOTYPE-*.md files in `aiplc-docs/discovery/prototypes/`

### Step 4: Decision Point

Ask user (in `aiplc-docs/discovery/prototypes/next-step-questions.md`):
```
What would you like to do next?

A) Build all 3 prototypes now (in this session)
B) Stop here - hand off PROTOTYPE-*.md files to teams
C) Build only specific prototypes
X) Other (please describe after [Answer]: tag below)

[Answer]:
```

**If [A] - Build now**: Proceed to Prototype Building
**If [B] - Hand off**: Optionally continue to Product Strategy → GTM for one use case (ask which and record it as the selected use case for strategy); otherwise end Discovery (see "End of Discovery" in Artifact Contract)
**If [C] - Build specific**: Ask which ones, proceed to Prototype Building

---

## Prototype Building (All Paths Converge Here)

Load `discovery/prototype-building.md`

**Execute IF**:
- Entry Point 1: PROTOTYPE-*.md files found
- Path A.2 or Path B: User chose to build prototypes

(Path A.1 builds its single prototype in Prototype & Validation instead.)

**Execution**:
1. For each PROTOTYPE-*.md file:
   - Read specification
   - Show defaults, ask for LLM provider (ALWAYS ask)
   - Check API credentials from the AI tool's own shell (credentials set after the tool started are not visible to it)
   - Request permission to build
   - Set up Strands (if agentic): host integration such as Kiro's Strands Power when available, otherwise install or mock
   - Build prototype
   - Deploy locally
   - User validates and iterates
2. All prototypes complete

### Selection (if multiple prototypes)

If multiple prototypes built, ask user to select winner for Product Strategy. Record the selection in `aiplc-state.md`.

---

## Product Strategy

Load `discovery/product-strategy.md`

**Execute**: For selected use case

**Execution**:
1. Capture positioning and differentiation
2. Define business model
3. Document strategy decisions
4. **Wait for Explicit Approval**

---

## Go-to-Market

Load `discovery/go-to-market.md`

**Execute**: For selected use case

**Execution**:
1. Marketing strategy
2. Sales approach
3. Launch planning
4. **Wait for Explicit Approval**

---

## Discovery Complete

**Output**: `aiplc-docs/discovery/discovery-document.md`

This comprehensive document contains the five parts defined in **Artifact Contract** below. Parts that a path does not produce are marked as not executed, so every Discovery Document has the same structure.

**Next Steps for Developers**:
This Discovery Document will be provided to developers in a separate workspace for:
- Inception Phase (Requirements Analysis, Workflow Planning, etc.)
- Construction Phase (Code Generation, Build & Test)
- Operations Phase (Deployment, Monitoring)

---

## Key Principles

- **PM-Focused**: This workspace is for Product Managers only
- **Discovery Only**: No Inception, Construction, or Operations phases
- **Three Entry Points**: Flexible starting points based on situation
- **Workshop-Friendly**: PROTOTYPE-*.md files are portable and shareable
- **Scalable**: Supports any number of use cases (N)
- **Transparent**: Always show defaults, ask for LLM provider
- **Standard AI-PLC**: Uses question files with [Answer]: tags throughout
- **Handoff-Ready**: Can stop after generating PROTOTYPE-*.md files
- **Complete Output**: Discovery Document ready for developers

## Audit Logging

**MANDATORY**: Log ALL user inputs and AI responses in `aiplc-docs/audit.md`:
- Capture user's COMPLETE RAW INPUT exactly as provided
- Never summarize or paraphrase user input
- Log every interaction with timestamp
- Use ISO 8601 format (YYYY-MM-DDTHH:MM:SSZ). Get the current time from the shell for **each** entry (`date -u +%Y-%m-%dT%H:%M:%SZ`; PowerShell: `(Get-Date).ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ssZ")`). Never reuse an earlier timestamp or invent one; if the host cannot run a command, write `unknown`

**NEVER log the following in audit.md** (see also `common/security-guardrails.md`)**:**
- API keys, tokens, or secrets of any kind
- AWS credentials (access keys, secret keys, bearer tokens)
- Any value that appears to be a credential (strings starting with `AKIA`, `sk-`, `bedrock-api-key-`, `goog_`, etc.)
- If a user accidentally pastes a credential in chat or an answer file, redact it before logging — replace with `[CREDENTIAL REDACTED]`
- Log only "credentials configured: yes/no" — never the actual values

## Directory Structure

```
aiplc-docs/
├── discovery/
│   ├── discovery-document.md          # Main output (all paths)
│   ├── envision/                      # Path A
│   │   ├── business-context-questions.md
│   │   ├── business-context.md
│   │   ├── mode-selection-questions.md
│   │   ├── pain-point-questions.md    # Interactive / hybrid mode
│   │   ├── pain-points-from-url.md    # URL / hybrid mode
│   │   ├── pain-point-analysis.md
│   │   └── prfaq-clarifying-questions.md  # If needed
│   ├── solution-analysis/             # Path A
│   │   └── identified-solutions.md
│   ├── use-case-intake/               # Path B or Path A.2
│   │   └── use-cases.md
│   ├── prioritization/                # Path B or Path A.2
│   │   ├── framework.md
│   │   ├── scoring.md
│   │   └── ranking.md
│   ├── prototypes/                    # All paths
│   │   ├── {use-case-slug}/           # Entry Point 1, Path A.2, Path B
│   │   │   ├── PROTOTYPE-{use-case-slug}.md  ★ Shareable
│   │   │   ├── design-context.md
│   │   │   └── iteration-log.md
│   │   └── {solution-slug}/           # Path A.1 (Prototype & Validation)
│   │       ├── prototype-spec.md
│   │       ├── design-context.md
│   │       ├── build-instructions.md
│   │       ├── iteration-log.md
│   │       ├── validation-plan.md
│   │       ├── validation-feedback.md
│   │       └── validation-results.md
│   ├── product-strategy/
│   │   └── strategy-questions.md
│   └── go-to-market/
│       └── gtm-questions.md
├── aiplc-state.md
└── audit.md
```

Each stage also writes its question files (`*-questions.md`) in its own directory — see `common/question-format-guide.md` and the "Question Files" section of each stage. `aiplc-docs/resume-questions.md` holds the welcome-back question when a session resumes.

## Artifact Contract

### Discovery Document Parts

`aiplc-docs/discovery/discovery-document.md` always has the same five parts, whichever path ran. The first stage that writes to it creates the file from the skeleton below; every later stage writes **only its own section** of its part (replacing that section if the stage is re-run, never the other sections of the part) and updates the `Status` line. Shared parts and their sections:
- Part 1: Envision (Path A) or Use Case Intake (Path B)
- Part 2: **Identified Solutions** (Solution Analysis), **Prioritization** (Use Case Prioritization)
- Part 3: **Prototype Specifications** (Prototype Context Generation; Prototype Building on Entry Point 1), **Build Results** (Prototype Building), or **Prototype & Validation** (Path A.1)
- Part 4: Product Strategy, including **Strategy Candidates Not Covered** when several use cases were selected

```markdown
# Discovery Document

**Product**: [Product or selected use case name]
**Date**: [ISO date]
**Entry Point / Path**: [Entry Point 1 / Path A.1 / Path A.2 / Path B]
**Status**: In Progress — [last completed stage] | Complete

---

# Part 1: Problem & Opportunity
_Not yet completed_

---

# Part 2: Solution Selection
_Not yet completed_

---

# Part 3: Prototypes
_Not yet completed_

---

# Part 4: Product Strategy
_Not yet completed_

---

# Part 5: Go-to-Market
_Not yet completed_
```

**End of Discovery** — whenever Discovery ends (after Go-to-Market, when the user ends at handoff, or after a Kill decision in Prototype & Validation), after all writing for the final stage is done:
1. Replace any remaining `_Not yet completed_` with `_Not executed in this path — [reason]_`
2. Set the document `Status` to `Complete` (or `Ended — Kill decision`)
3. In `aiplc-state.md`, mark every remaining `[ ]` stage as `[-]` with the reason, and set **Current Stage** to `Discovery Complete` (or `Ended`)

### Stage Inputs and Outputs

| Stage | Runs in | Requires | Produces | Writes Part |
|---|---|---|---|---|
| Envision | Path A | — | `envision/business-context.md`, `envision/pain-point-analysis.md` | 1 (business context, pain points, PR/FAQ) |
| Solution Analysis | Path A | Part 1 (PR/FAQ) | `solution-analysis/identified-solutions.md` | 2 (identified solutions) |
| Use Case Intake | Path B, A.2 | — (A.2: `identified-solutions.md`) | `use-case-intake/use-cases.md` | 1 (Path B: use case summary) |
| Use Case Prioritization | Path B, A.2 | `use-cases.md` | `prioritization/*.md` | 2 (ranking, selection) |
| Prototype Context Generation | Path B, A.2 | `ranking.md` | `prototypes/{slug}/PROTOTYPE-{slug}.md` | 3 (specifications) |
| Prototype Building | Entry Point 1, Path B, A.2 | PROTOTYPE-*.md | prototype code, `iteration-log.md` | 3 (build results, selection); Entry Point 1 also writes Parts 1-2 as not executed |
| Prototype & Validation | Path A.1 | Part 1, `identified-solutions.md` | `prototypes/{solution-slug}/*` | 3 (prototype, validation, build decision) |
| Product Strategy | All paths | One selected use case for strategy | `product-strategy/strategy-questions.md` | 4 |
| Go-to-Market | All paths | Part 4 | `go-to-market/gtm-questions.md` | 5 |

All paths are relative to `aiplc-docs/discovery/`. A stage must not require an artifact that its path does not produce; when an optional input is absent, the stage states the limitation instead of stopping.

### State File

`aiplc-docs/aiplc-state.md` uses one Stage Progress block for every path. Each stage updates **only its own line**: `[x]` complete, `[ ]` pending, `[-]` not applicable to this path or skipped (add the reason).

- When a stage **starts**, set **Current Stage** to it. Resume always continues from **Current Stage**.
- When the path is known (Workspace Detection for Entry Point 1, Discovery Mode Selection, Solution Analysis), mark the stages that the path does not run as `[-]` right away.
- **Shortlisted Use Cases** are the use cases chosen for prototyping (Use Case Prioritization). **Selected Use Case(s) for Strategy** are the ones Product Strategy and Go-to-Market are for (the winner of Prototype Building, the single solution on Path A.1, or the one the user names at handoff).

```markdown
## Stage Progress
- **Current Stage**: [Stage name]
- **Entry Point / Path**: [Entry Point 1 / Path A.1 / Path A.2 / Path B]
- **Shortlisted Use Cases**: [Names, or "None"]
- **Selected Use Case(s) for Strategy**: [Names, or "Not yet selected"]

### 🟣 DISCOVERY PHASE
- [ ] Workspace Detection
- [ ] Discovery Mode Selection
- [ ] Envision
- [ ] Solution Analysis
- [ ] Use Case Intake
- [ ] Use Case Prioritization
- [ ] Prototype Context Generation
- [ ] Prototype Building
- [ ] Prototype & Validation
- [ ] Product Strategy
- [ ] Go-to-Market
```

## Success Criteria

✅ Discovery Document complete with all sections
✅ PROTOTYPE-*.md files generated (if applicable)
✅ Product Strategy documented
✅ Go-to-Market plan documented
✅ All artifacts ready for developer handoff
✅ Complete audit trail in audit.md

## Handoff to Developers

When Discovery is complete, provide developers with:
1. Complete `aiplc-docs/discovery/` directory
2. Especially `discovery-document.md` (main artifact)
3. All PROTOTYPE-*.md files (if generated)

Developers will use these in their workspace for Inception and Construction phases.
