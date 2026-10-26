# Mid-Workflow Changes

## Overview

Users may ask to change the Discovery workflow while it is running. This document provides guidance on handling these requests safely and effectively.

State markers below refer to the Stage Progress block in core-workflow.md: pending = `[ ]`, skipped or not in this path = `[-]` with the reason, in progress = **Current Stage**.

Discovery stage dependencies (later stages consume earlier artifacts):

```
Envision (PR/FAQ) --> Solution Analysis --> Prototype & Validation (A.1)
                                      \--> Use Case Intake (A.2, pre-filled)
Use Case Intake --> Use Case Prioritization --> Prototype Context Generation --> Prototype Building
Prototype & Validation / Prototype Building --> Product Strategy --> Go-to-Market
```

---

## Types of Mid-Workflow Changes

### 1. Adding a Skipped Stage

**Scenario**: User wants to run a stage that was skipped

**Example**: "I handed off the PROTOTYPE files, but now I want to build one here after all"

**Handling**:
1. **Confirm Request**: "You want to add Prototype Building for {use case}. This will set up and run the prototype locally. Confirm?"
2. **Check Dependencies**: Verify the stage's prerequisites exist
3. **Update State**: Mark the stage as `[ ]` and set it as **Current Stage** in `aiplc-state.md`
4. **Execute Stage**: Follow normal stage execution
5. **Log Change**: Document in `audit.md` with timestamp and reason

**If Discovery had already ended**: see "Reopening After Discovery Has Ended" below.

**Considerations**:
- Only stages on the current path can be added (see each stage's prerequisites); to use another path's stages, see "Switching the Starting Path"
- Later stages that already ran (for example Product Strategy) may need revision to reflect the new evidence

---

### 2. Skipping a Planned Stage

**Scenario**: User wants to skip a stage

**Example**: "Let's skip Go-to-Market for now"

**Handling**:
1. **Confirm Request**: "You want to skip Go-to-Market. The Discovery Document will have no marketing, sales, or launch plan. Confirm?"
2. **Warn About Impact**: Explain what will be missing for the developer handoff
3. **Get Explicit Confirmation**: User must explicitly confirm
4. **Update State**: Mark stage as `[-]` with reason in `aiplc-state.md`
5. **Log Change**: Document in `audit.md` with timestamp and reason

**Considerations**:
- Can be added back later (see "Adding a Skipped Stage")

---

### 3. Restarting the Current Stage

**Scenario**: User is unhappy with the current stage's result

**Example**: "I don't like this PR/FAQ. Can we start over?"

**Handling**:
1. **Understand Concern**: "What specifically would you like to change about the PR/FAQ?"
2. **Offer Options**:
   - **Option A**: Modify the existing artifact (faster, preserves work)
   - **Option B**: Complete restart (clean slate)
3. **If Restart Chosen**:
   - Archive existing artifacts: `{artifact}.backup.{timestamp}`
   - Set the stage as **Current Stage** and mark it `[ ]` in `aiplc-state.md`
   - Re-execute from the beginning of the stage
4. **Log Change**: Document reason for restart and what will change

---

### 4. Restarting a Previous Stage

**Scenario**: User wants to go back and redo a completed stage

**Example**: "I want to change the target customer we chose in Envision"

**Handling**:
1. **Assess Impact**: Identify all stages that consumed the stage's artifacts (see dependencies above)
2. **Warn User**: "Changing Envision affects: Solution Analysis, prototype scope, Product Strategy, and Go-to-Market. Confirm?"
3. **Get Explicit Confirmation**: User must understand full impact
4. **If Confirmed**:
   - Archive all affected artifacts
   - Reset all affected stages in `aiplc-state.md`
   - Return to the stage to restart and re-execute from that point forward
   - If Discovery had already ended, follow "Reopening After Discovery Has Ended" below
5. **Log Change**: Document full impact and reason for restart

### Reopening After Discovery Has Ended

Applies to sections 1 and 4 when **Current Stage** is `Discovery Complete` or `Ended`:
1. Reopen the requested stage **and every later stage that depends on it** (see dependencies above) — for example, reopening Product Strategy also reopens Go-to-Market. Mark them `[ ]`, whether they were `[x]` or `[-]`.
2. For each reopened stage, replace its part or section in `discovery-document.md` with `_Not yet completed_` (archive a completed one first).
3. Set the document `Status` to `In Progress — [first reopened stage]` and **Current Stage** to that stage.
4. **If Product Strategy is reopened**: ask which use case it is for — option A is the current selected use case (redo), followed by any **Strategy Candidates Not Covered** from the archived Part 4 — then set **Selected Use Case(s) for Strategy** and the document's `**Product**:` header to the answer. Carry the remaining candidates into the new Part 4.
5. Run the reopened stages in order. Run "End of Discovery" (core-workflow.md) only after the last reopened stage finishes.


**Considerations**:
- Built prototypes may no longer match the new direction — ask whether to rebuild or keep them as reference

---

### 5. Changing the Selected Use Cases or Solution

**Scenario**: User wants to prototype or strategize for a different use case

**Example**: "Swap use case 3 for the one ranked fifth"

**Handling**:
1. **Confirm Request** and show what changes (new PROTOTYPE-*.md to generate, strategy to redo if already done)
2. **Update** `prioritization/ranking.md` (selected list) and the selection recorded in `aiplc-state.md`
3. **Generate** context for the newly selected use case; keep PROTOTYPE-*.md files for deselected ones unless the user asks to remove them
4. **Log Change** in `audit.md`

---

### 6. Switching the Starting Path

**Scenario**: User started from pain points but now wants to start from a list of use cases (or the reverse)

**Handling**:
1. **Confirm Request** and explain which artifacts carry over (for example, use cases extracted from the PR/FAQ can seed Use Case Intake)
2. **Archive** artifacts of the abandoned path
3. **Update** the path in `aiplc-state.md` and continue at the first stage of the new path
4. **Log Change** in `audit.md`

---

### 7. Pausing the Workflow

**Scenario**: User needs to pause and resume later

**Example**: "I need to stop for now and continue tomorrow"

**Handling**:
1. **Complete Current Step**: Finish the current step in progress if possible
2. **Update State**: Ensure `aiplc-state.md` reflects current status
3. **Log Pause**: Document pause point in `audit.md`
4. **Provide Resume Instructions**: "When you return, I'll detect your existing session and offer to continue from: [current stage, current step]"

**On Resume**: Follow `common/session-continuity.md`.

---

## General Guidelines for Handling Changes

### Before Making Changes

1. **Understand the Request**: Ask clarifying questions about what user wants to change and why
2. **Assess Impact**: Identify all affected stages and artifacts
3. **Explain Consequences**: Clearly communicate what will need to be redone
4. **Offer Alternatives**: Sometimes modification is better than restart
5. **Get Explicit Confirmation**: User must understand and accept the impact

### During Changes

1. **Archive Existing Work**: Always backup before making destructive changes
2. **Update All Tracking**: Keep `aiplc-state.md`, `discovery-document.md`, and `audit.md` in sync
3. **Communicate Progress**: Keep user informed about what's happening

### After Changes

1. **Verify Consistency**: Check that all artifacts are aligned with changes
2. **Log Completely**: Document full change history in `audit.md`
3. **Resume Workflow**: Continue with normal execution from new state

---

## Change Request Decision Tree

```
User requests change
    |
    +-- Is it the current stage?
    |   +-- Yes: Modify or restart current stage
    |   +-- No: Go to next question
    |
    +-- Is it a completed stage?
    |   +-- Yes: Assess impact on dependent stages
    |   |   +-- Low impact: Modify and update dependents
    |   |   +-- High impact: Recommend restart from that stage
    |   +-- No: Go to next question
    |
    +-- Is it adding a skipped stage?
    |   +-- Yes: Check prerequisites, execute
    |   +-- No: Go to next question
    |
    +-- Is it skipping a stage?
    |   +-- Yes: Warn about impact, get confirmation, skip
    |   +-- No: Go to next question
    |
    +-- Is it changing selection or starting path?
        +-- Yes: See sections 5 and 6
        +-- No: Clarify request with user
```

---

## Logging Requirements

### Change Request Log Format

```markdown
## Change Request - [Stage Name]
**Timestamp**: [ISO timestamp]
**Request**: [What user wants to change]
**Current State**: [Where we are in workflow]
**Impact Assessment**: [What will be affected]
**User Confirmation**: [User's explicit confirmation]
**Action Taken**: [What was done]
**Artifacts Affected**: [List of files changed/reset]

---
```

---

## Best Practices

1. **Always Confirm**: Never make destructive changes without explicit user confirmation
2. **Explain Impact**: Users need to understand consequences before deciding
3. **Archive First**: Always backup before making destructive changes
4. **Update Everything**: Keep all tracking files in sync
5. **Log Thoroughly**: Document all changes for audit trail
6. **Be Flexible**: Workflow should adapt to user needs, not force rigid process
