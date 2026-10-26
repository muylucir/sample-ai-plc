# Error Handling and Recovery Procedures

## General Error Handling Principles

### When Errors Occur
1. **Identify the error**: Clearly state what went wrong
2. **Assess impact**: Determine if the error is blocking or can be worked around
3. **Communicate**: Inform the user about the error and options
4. **Offer solutions**: Provide clear steps to resolve or work around the error
5. **Document**: Log the error and resolution in `audit.md`

### Error Severity Levels

**Critical**: Workflow cannot continue
- Missing required files or artifacts
- Invalid user input that cannot be processed
- System errors preventing file operations

**High**: Stage cannot complete as planned
- Incomplete answers to required questions
- Contradictory user responses
- Missing dependencies from prior stages

**Medium**: Stage can continue with workarounds
- Optional artifacts missing
- Non-critical validation failures
- Partial completion possible

**Low**: Minor issues that don't block progress
- Formatting inconsistencies
- Optional information missing
- Non-blocking warnings

## Stage-Specific Error Handling

### Workspace Detection Errors

**Error**: Cannot read workspace files
- **Cause**: Permission issues, missing directories
- **Solution**: Ask user to verify workspace path and permissions
- **Workaround**: Proceed with user-provided information only

**Error**: Existing `aiplc-state.md` is corrupted
- **Cause**: Manual editing, incomplete previous run
- **Solution**: Ask user if they want to start fresh or attempt recovery
- **Recovery**: Create backup, rebuild state file (see "Corrupted State File" below)

### Envision Errors

**Error**: User-provided URL fails validation or cannot be fetched
- **Cause**: Non-HTTPS scheme, private/internal address, site blocks automated access, page requires login
- **Solution**: Explain why the URL was rejected or failed, ask for a different public HTTPS URL
- **Workaround**: Switch to interactive or free-form input mode

**Error**: Fetched page has little or no usable customer information
- **Cause**: Marketing-only page, dynamic content not rendered, unrelated page
- **Solution**: Show what was extracted and ask the user to confirm, add, or provide another source
- **Do Not Proceed**: Do not fill gaps from prior knowledge without the user's permission

**Error**: Contradictory pain point or business context answers
- **Cause**: Unclear understanding, multiple stakeholders answering
- **Solution**: Create follow-up questions to resolve contradictions
- **Do Not Proceed**: Until contradictions are resolved or the user explicitly asks to proceed (record the assumption made)

### Use Case Intake and Prioritization Errors

**Error**: Bulk use case input cannot be parsed reliably
- **Cause**: Free-form paste with mixed formats, missing fields
- **Solution**: Show the parsed list and ask the user to confirm or correct it
- **Workaround**: Gather the missing fields one use case at a time

**Error**: Not enough information to score a criterion
- **Cause**: Use case description lacks business value, tool, or data details
- **Solution**: Ask targeted questions for the missing criteria
- **Workaround**: Score with a stated assumption and mark the score as low-confidence in `scoring.md`

### Prototype Context Generation and Building Errors

**Error**: PROTOTYPE-*.md file is missing required sections
- **Cause**: Hand-edited or older specification file
- **Solution**: List the missing required sections (see `discovery/prototype-md-format.md`) and ask the user for them
- **Workaround**: Use the documented defaults for optional sections only

**Error**: LLM credentials not found or verification fails
- **Cause**: Credentials not set, set in a process the AI tool cannot see, expired, or lacking model access
- **Solution**: Follow the credential steps in `discovery/prototype-building.md`
- **Workaround**: Offer the mocked-agent prototype or a different provider

**Error**: Package installation or prototype start-up fails
- **Cause**: Missing runtime, network restrictions, version conflicts, port already in use
- **Solution**: Show the error summary, fix what can be fixed inside the project's virtual environment, or choose another port
- **Escalation**: If it requires admin rights or system changes, mark it as a **HUMAN TASK** with instructions and wait

### Product Strategy and Go-to-Market Errors

**Error**: No selected solution or use case
- **Cause**: Handoff path chosen, or selection not recorded in `aiplc-state.md`
- **Solution**: Ask the user which solution or use case the strategy is for, then record it
- **Do Not Proceed**: Until one is selected

## Recovery Procedures

### Partial Stage Completion

**Scenario**: Stage was interrupted mid-execution

**Recovery Steps**:
1. Read the stage's progress in `aiplc-state.md` and its working files (question files, partial artifacts)
2. Identify the last completed step (answered question file, approved artifact)
3. Resume from the next uncompleted step
4. Verify all prior steps are actually complete
5. Continue execution normally

### Corrupted State File

**Scenario**: `aiplc-state.md` is corrupted or inconsistent

**Recovery Steps**:
1. Create backup: `aiplc-state.md.backup`
2. Ask user which stage they're actually on
3. Regenerate state file from scratch
4. Mark completed stages based on existing artifacts
5. Resume from current stage

### Missing Artifacts

**Scenario**: Required artifacts from prior stage are missing

**Recovery Steps**:
1. Identify which artifacts are missing
2. Determine if they can be regenerated
3. If yes: Return to that stage, regenerate artifacts
4. If no: Ask user to provide information manually
5. Document the gap in `audit.md`

### User Wants to Restart Stage

**Scenario**: User is unhappy with stage results and wants to redo

**Recovery Steps**:
1. Confirm user wants to restart (data will be lost)
2. Archive existing artifacts: `{artifact}.backup`
3. Reset stage status in `aiplc-state.md`
4. Re-execute stage from beginning

### User Wants to Skip Stage

**Scenario**: User wants to skip a stage that was planned

**Recovery Steps**:
1. Confirm user understands implications
2. Document skip reason in `audit.md`
3. Mark stage as "SKIPPED" in `aiplc-state.md`
4. Proceed to next stage
5. Note: Later stages fall back to the inputs that exist (see each stage's prerequisites)

## Escalation Guidelines

### When to Ask for User Help

**Immediately**:
- Contradictory or ambiguous user input
- Missing required information
- Technical constraints AI cannot resolve
- Decisions requiring business judgment

**After Attempting Resolution**:
- Repeated errors in same step
- Complex technical issues
- Unusual project structures
- Integration with external systems

### When to Suggest Starting Over

**Consider Fresh Start If**:
- Multiple stages have errors
- State file is severely corrupted
- Product direction has changed significantly
- The starting path (pain points vs use cases) needs to change
- User cannot provide missing information
- Artifacts are inconsistent across stages

**Before Starting Over**:
1. Archive all existing work
2. Document lessons learned
3. Identify what to preserve
4. Get user confirmation
5. Restart from Discovery Mode Selection

## Session Resumption Errors

### Missing Artifacts During Resumption

**Error**: Required artifacts from previous stages are missing
- **Cause**: Files deleted, moved, or never created
- **Solution**: 
  1. Identify which stage created the missing artifacts
  2. Check if stage was marked complete in aiplc-state.md
  3. If marked complete but artifacts missing: Regenerate that stage
  4. If not marked complete: Resume from that stage
- **Recovery**: Return to the stage that creates missing artifacts and re-execute

**Error**: Artifact file exists but is empty or corrupted
- **Cause**: Interrupted write, manual editing, file system issues
- **Solution**:
  1. Create backup of corrupted file
  2. Attempt to regenerate from stage that creates it
  3. If cannot regenerate: Ask user for information to recreate
- **Recovery**: Re-execute the stage that creates the artifact

### Inconsistent State During Resumption

**Error**: aiplc-state.md shows stage complete but artifacts don't exist
- **Cause**: State file updated but artifact generation failed
- **Solution**:
  1. Mark stage as incomplete in aiplc-state.md
  2. Re-execute the stage to generate artifacts
  3. Verify artifacts exist before marking complete
- **Recovery**: Reset stage status and re-execute

**Error**: Artifacts exist but aiplc-state.md shows stage incomplete
- **Cause**: Artifact generation succeeded but state update failed
- **Solution**:
  1. Verify artifacts are complete and valid
  2. Update aiplc-state.md to mark stage complete
  3. Proceed to next stage
- **Recovery**: Update state file to reflect actual completion

**Error**: Multiple stages marked as "current" in aiplc-state.md
- **Cause**: State file corruption, manual editing
- **Solution**:
  1. Review artifacts to determine actual progress
  2. Ask user which stage they're actually on
  3. Correct aiplc-state.md to show single current stage
- **Recovery**: Rebuild state file based on existing artifacts

### Context Loading Errors

**Error**: Cannot load required context from previous stages
- **Cause**: Missing files, corrupted content, wrong file paths
- **Solution**:
  1. List which artifacts are needed for current stage
  2. Check which ones are missing or corrupted
  3. Regenerate missing artifacts or ask user for information
- **Recovery**: Complete prerequisite stages before resuming current stage

**Error**: Loaded artifacts contain contradictory information
- **Cause**: Manual editing, multiple people working, incomplete updates
- **Solution**:
  1. Identify contradictions and present to user
  2. Ask user which information is correct
  3. Update artifacts to resolve contradictions
- **Recovery**: Reconcile contradictions before proceeding

### Resumption Best Practices

1. **Always validate state**: Check aiplc-state.md matches actual artifacts
2. **Load incrementally**: Load artifacts stage-by-stage, validate each
3. **Fail fast**: Stop immediately if critical artifacts are missing
4. **Communicate clearly**: Tell user exactly what's missing and why it's needed
5. **Offer options**: Regenerate, provide manually, or start fresh
6. **Document recovery**: Log all recovery actions in audit.md

## Logging Requirements

### Error Logging Format

```markdown
## Error - [Stage Name]
**Timestamp**: [ISO timestamp]
**Error Type**: [Critical/High/Medium/Low]
**Description**: [What went wrong]
**Cause**: [Why it happened]
**Resolution**: [How it was resolved]
**Impact**: [Effect on workflow]

---
```

### Recovery Logging Format

```markdown
## Recovery - [Stage Name]
**Timestamp**: [ISO timestamp]
**Issue**: [What needed recovery]
**Recovery Steps**: [What was done]
**Outcome**: [Result of recovery]
**Artifacts Affected**: [List of files]

---
```

## Prevention Best Practices

1. **Validate Early**: Check inputs and dependencies before starting work
2. **Checkpoint Often**: Update `aiplc-state.md` immediately after completing steps
3. **Communicate Clearly**: Explain what you're doing and why
4. **Ask Questions**: Don't assume - clarify ambiguities immediately
5. **Document Everything**: Log all decisions and changes in `audit.md`
