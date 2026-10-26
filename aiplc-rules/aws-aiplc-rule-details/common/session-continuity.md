# Session Continuity Templates

## Welcome Back Prompt Template
When a user returns to continue work on an existing AI-PLC project, present this prompt:

```markdown
**Welcome back! I can see you have an existing AI-PLC Discovery session in progress.**

Based on your aiplc-state.md, here's your current status:
- **Product / Use Case**: [product or selected use case name]
- **Entry Point / Path**: [Entry Point 1 / Path A.1 / Path A.2 / Path B]
- **Current Stage**: [Stage Name]
- **Last Completed**: [Last completed step]
- **Next Step**: [Next step to work on]

**What would you like to work on today?**

A) Continue where you left off ([Next step description])
B) Review a previous stage ([Show available stages])

[Answer]: 
```

## MANDATORY: Session Continuity Instructions
1. **Always read aiplc-state.md first** when detecting existing project
2. **Parse current status** from the state file to populate the prompt
3. **MANDATORY: Load Previous Stage Artifacts** - Before resuming any stage, automatically read all relevant artifacts from previous stages that exist in the workspace:
   - **All stages**: `discovery/discovery-document.md` (parts completed so far)
   - **Envision**: `discovery/envision/pain-point-analysis.md`, `discovery/envision/pain-points-from-url.md` (if URL mode)
   - **Solution Analysis**: `discovery/solution-analysis/identified-solutions.md`
   - **Use Case Intake**: `discovery/use-case-intake/use-cases.md`
   - **Use Case Prioritization**: `discovery/prioritization/scoring.md`, `discovery/prioritization/ranking.md`
   - **Prototype Context Generation / Prototype Building**: `discovery/prototypes/*/PROTOTYPE-*.md`, `design-context.md`, `iteration-log.md`
   - **Prototype & Validation**: `discovery/prototypes/{solution-slug}/prototype-spec.md`, `design-context.md`, `build-instructions.md`, `iteration-log.md`, `validation-plan.md`, `validation-results.md`
   - **Product Strategy**: `discovery/product-strategy/strategy-questions.md`
   - **Go-to-Market**: `discovery/go-to-market/gtm-questions.md`
   - All paths above are relative to `aiplc-docs/`
4. **Load only what the current stage needs**: Earlier-stage artifacts that feed the current stage, plus the current stage's own working files. Do not load prototype source code unless resuming Prototype Building or iteration.
5. **Adapt options** based on the recorded path and stage
6. **Show specific next steps** rather than generic descriptions
7. **Log the continuity prompt** in audit.md with timestamp
8. **Context Summary**: After loading artifacts, provide brief summary of what was loaded for user awareness
9. **Asking questions**: ALWAYS ask clarification or user feedback questions by placing them in .md files. DO NOT place the multiple-choice questions in-line in the chat session.

## Error Handling
If artifacts are missing or corrupted during session resumption, see [error-handling.md](error-handling.md) for guidance on recovery procedures.
