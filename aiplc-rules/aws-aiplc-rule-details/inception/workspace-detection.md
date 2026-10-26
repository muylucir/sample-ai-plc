# Workspace Detection

**Purpose**: Determine whether this is a new Discovery workflow, a resumed one, or a build from existing PROTOTYPE-*.md files.

This workspace runs the Discovery phase only. Do NOT scan for application source code, do NOT classify the workspace as greenfield or brownfield, and do NOT route to any Inception, Construction, or Operations stage — those run in a separate developer workspace.

## Step 1: Check for an Existing AI-PLC Session (FIRST)

Check if `aiplc-docs/aiplc-state.md` exists.

- **If it exists**: This is a resumed session. Follow `common/session-continuity.md` and resume from the recorded stage. **STOP HERE** — do not run Steps 2-5. (PROTOTYPE-*.md files generated earlier in this same session must not restart the workflow at Prototype Building.)
- **If it does not exist**: Continue to Step 2.

## Step 2: Check for Existing PROTOTYPE-*.md Files (Entry Point 1)

Scan for prototype specification files:

**Path pattern**: `aiplc-docs/discovery/prototypes/*/PROTOTYPE-*.md`

### IF PROTOTYPE-*.md file(s) found:

This is **Entry Point 1** - Build from existing prototype specifications.

**Typical scenario**: Workshop environment where teams receive pre-generated PROTOTYPE-*.md files.

1. Count the files found
2. Extract use case names from filenames
3. Present to user:

```
I found {N} prototype specification file(s) in your workspace:

1. 📄 PROTOTYPE-{use-case-1-slug}.md
2. 📄 PROTOTYPE-{use-case-2-slug}.md (if multiple)
[...]

These files contain complete specifications for building prototypes.

I'll skip the discovery phase and build prototypes directly from 
these specifications.
```

4. **SKIP ALL DISCOVERY STAGES BEFORE PROTOTYPE BUILDING**:
   - Skip pain point analysis
   - Skip PR/FAQ generation
   - Skip use case intake
   - Skip prioritization
   - Skip context generation

5. Create the state file (Step 4) with **Entry Point** set to `Entry Point 1`.

6. **JUMP DIRECTLY TO**: Prototype Building stage (see `discovery/prototype-building.md`)

7. After prototypes built, continue to Product Strategy → GTM

### IF NO PROTOTYPE-*.md files found:

Continue to Step 3.

## Step 3: Check for Orphaned Discovery Artifacts

If `aiplc-docs/discovery/` contains artifacts but `aiplc-state.md` does not exist (state file deleted or never written):

1. List the artifacts found
2. Follow "Corrupted State File" in `common/error-handling.md` to rebuild the state file from the artifacts
3. Resume from the first stage whose artifacts are missing

If `aiplc-docs/discovery/` is empty or does not exist, this is a new Discovery workflow.

## Step 4: Create Initial State File

Create `aiplc-docs/aiplc-state.md`:

```markdown
# AI-PLC State Tracking

## Session Information
- **Start Date**: [ISO timestamp]
- **Current Stage**: Workspace Detection

[Stage Progress block — copy from "State File" in core-workflow.md, with Workspace Detection marked [x]]
```

## Step 5: Present Completion Message

**For Entry Point 1** (PROTOTYPE-*.md found): use the message in Step 2.

**For a new Discovery workflow:**
```markdown
# 🔍 Workspace Detection Complete

• **No existing AI-PLC session found** — starting a new Discovery workflow
• **Next Step**: Proceeding to **Discovery Mode Selection** to determine how to start (pain points or use cases)...
```

## Step 6: Automatically Proceed

- **No user approval required** - this is informational only
- Automatically proceed to next stage:
  - **Resumed session**: The recorded stage (see `common/session-continuity.md`)
  - **PROTOTYPE-*.md found**: Prototype Building (Entry Point 1)
  - **New workflow**: Discovery Mode Selection → User chooses Path A (Envision) or Path B (Use Case Intake)
