# Question Format Guide

## MANDATORY: All Questions Must Use This Format

### Rule: Questions Go in Question Files
**CRITICAL**: Every question that asks the user to make a choice or provide information MUST be written to a question file — never asked only in chat. This applies on every host, even when the host offers its own interactive question UI: question files are recorded in the workspace, can be answered at the user's pace, and let any host resume the session.

### Allowed in Chat (the only exceptions)
1. **Status and progress messages** that need no answer
2. **Approval gates** after an artifact is presented ("Please review ... and confirm to proceed") — the user approves or requests changes in chat
3. **"Let me know when you're done"** prompts after a question file is created
4. **Iteration requests** on a running prototype ("make the header darker") during Prototype Building or Prototype & Validation iteration — log them in `iteration-log.md`

Anything else — including yes/no decisions, selections, counts, and free-text inputs — goes in a question file. Credentials are never requested in chat or in question files.

### Question File Location and Naming
- Write question files in the current stage's directory under `aiplc-docs/discovery/` (see the directory tree in core-workflow.md). Each stage file names its question file.
- Name: `{topic}-questions.md` (for example `solution-analysis-questions.md`, `build-questions.md`)
- Follow-up clarifications: `{topic}-clarification-questions.md` in the same directory

### Two Question Types

#### Choice Question
Meaningful options plus "Other" as the last option. "Other" is **always** labeled `X)`, whatever the number of options, so it never collides with a real option letter.

```markdown
## Question [Number]
[Clear, specific question text]

A) [First meaningful option]
B) [Second meaningful option]
[...additional options as needed...]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

#### Open Question
For information that has no meaningful options (a URL, a count, a list of use cases, sample interactions). No options; the user writes free text after the tag.

```markdown
## Question [Number]
[Clear, specific question text]
[Optional: fields to cover or an example of the expected format]

[Answer]: 
```

**CRITICAL**:
- "Other" (`X)`) is MANDATORY as the LAST option for every choice question
- Only include meaningful options - don't make up options to fill slots
- Use as many or as few options as make sense (minimum 2 + Other)
- When a choice needs a short detail (for example "B) I use an AWS CLI profile — profile name"), the user writes the letter followed by the detail after `[Answer]:`

### Complete Example

```markdown
# Use Case Intake Questions

Please answer the following questions. Fill in your answer after each [Answer]: tag.

## Question 1
How many use cases do you want to evaluate?

[Answer]: 

## Question 2
Please describe each use case. For each one, include: name, problem it solves, target users,
business value (High/Medium/Low), technical complexity (High/Medium/Low), type
(Agentic or Application), 2-3 key capabilities, and any constraints.
You can paste a list or a table.

[Answer]: 

## Question 3
Which device should the prototypes target by default?

A) Desktop browser
B) Mobile phone
C) Both (responsive design)
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

### User Response Format
Users answer by filling in the letter choice (choice question) or text (open question) after the [Answer]: tag:

```markdown
## Question 3
Which device should the prototypes target by default?

A) Desktop browser
B) Mobile phone
C) Both (responsive design)
X) Other (please describe after [Answer]: tag below)

[Answer]: C
```

### Reading User Responses
After user confirms completion:
1. Read the question file
2. Extract answers: an answer is everything after `[Answer]:` up to the next question heading — it may span several lines
3. Validate all questions are answered
4. Proceed with analysis based on responses

### Multiple Choice Guidelines

#### Option Count
- Minimum: 2 meaningful options + "Other"
- Typical: 3-4 meaningful options + "Other"
- Maximum: 5 meaningful options + "Other"
- **CRITICAL**: Don't make up options just to fill slots - only include meaningful choices

#### Option Quality
- Make options mutually exclusive
- Cover the most common scenarios
- Only include meaningful, realistic options
- **ALWAYS include "Other" (`X)`) as the LAST option** (MANDATORY)
- Be specific and clear
- Where a stage defines an intelligent default, put it first as Option A

#### Good Example:
```markdown
## Question 5
What revenue model fits this product best?

A) Subscription ← Suggested based on your pain point analysis
B) Usage-based pricing
C) Freemium with paid tiers
D) One-time purchase
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

#### Bad Example (Avoid):
```markdown
## Question 5
What revenue model will you use?

A) Yes
B) No
C) Maybe

[Answer]: 
```

### Workflow Integration

#### Step 1: Create Question File
```markdown
Create the stage's question file (for example aiplc-docs/discovery/solution-analysis/solution-analysis-questions.md) with all questions
```

#### Step 2: Inform User
```
"I've created {file} with [X] questions. 
Please answer each question after the [Answer]: tag — a letter for multiple-choice questions, text for open questions. 
If none of the options match your needs, choose X (Other) and describe your preference. Let me know when you're done."
```

#### Step 3: Wait for Confirmation
Wait for user to say "done", "completed", "finished", or similar.

#### Step 4: Read and Analyze
```
Read the question file
Extract all answers
Validate completeness
Proceed with analysis
```

### Error Handling

#### Missing Answers
If any [Answer]: tag is empty:
```
"I noticed Question [X] is not answered. Please provide an answer for all questions before proceeding."
```

#### Invalid Answers
If a choice question's answer does not start with one of its letters:
```
"Question [X] has an invalid answer '[answer]'. 
Please use one of the letter choices provided, or X (Other) with a description."
```

#### Ambiguous Answers
If user provides explanation instead of letter on a choice question:
```
"For Question [X], please provide the letter choice that best matches your answer. 
If none match, choose X (Other) and add your description after the [Answer]: tag."
```

### Contradiction and Ambiguity Detection

**MANDATORY**: After reading user responses, you MUST check for contradictions and ambiguities.

#### Detecting Contradictions
Look for logically inconsistent answers, for example:
- Target mismatch: "Enterprise IT buyers" but "Self-serve consumer sign-up"
- Pricing mismatch: "Premium positioning" but "Free forever, no paid tier"
- Scope mismatch: "Prototype in one day" but "Five integrated external systems"

#### Detecting Ambiguities
Look for unclear or borderline responses:
- Answers that could fit multiple options
- Responses that lack specificity ("depends", "maybe", "mix of")
- Conflicting indicators across multiple questions

#### Creating Clarification Questions
If contradictions or ambiguities detected:

1. **Create clarification file**: `{topic}-clarification-questions.md` in the same directory
2. **Explain the issue**: Clearly state what contradiction/ambiguity was detected
3. **Ask targeted questions**: Use the formats above to resolve the issue
4. **Reference original questions**: Show which questions had conflicting answers

**Example**:
```markdown
# [Stage Name] Clarification Questions

I detected contradictions in your responses that need clarification:

## Contradiction 1: [Brief Description]
You indicated "[Answer A]" (Q[X]:[Letter]) but also "[Answer B]" (Q[Y]:[Letter]).
These responses are contradictory because [explanation].

### Clarification Question 1
[Specific question to resolve contradiction]

A) [Option that resolves toward first answer]
B) [Option that resolves toward second answer]
C) [Option that provides middle ground]
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Ambiguity 1: [Brief Description]
Your response to Q[X] ("[Answer]") is ambiguous because [explanation].

### Clarification Question 2
[Specific question to clarify ambiguity]

A) [Clear option 1]
B) [Clear option 2]
X) Other (please describe after [Answer]: tag below)

[Answer]: 
```

#### Workflow for Clarifications

1. **Detect**: Analyze all responses for contradictions/ambiguities
2. **Create**: Generate clarification question file if issues found
3. **Inform**: Tell user about the issues and clarification file
4. **Wait**: Do not proceed until user provides clarifications
5. **Re-validate**: After clarifications, check again for consistency
6. **Proceed**: Only move forward when all contradictions are resolved, or the user explicitly asks to proceed — then record the assumption made in the stage artifact and `audit.md`

### Best Practices

1. **Be Specific**: Questions should be clear and unambiguous
2. **Be Comprehensive**: Cover all necessary information
3. **Be Concise**: Keep questions focused on one topic
4. **Be Practical**: Options should be realistic and actionable
5. **Be Consistent**: Use same format throughout all question files

## Summary

**Remember**: 
- ✅ Always create question files (exceptions: status messages, approval gates, "let me know when done", prototype iteration requests)
- ✅ Choice questions: options plus `X) Other` as the last option
- ✅ Open questions: free text after `[Answer]:`
- ✅ Only include meaningful options - don't make up options to fill slots
- ✅ Always use [Answer]: tags
- ✅ Always wait for user completion
- ✅ Always validate responses for contradictions
- ✅ Always create clarification files if needed
- ❌ Never ask decision or input questions only in chat
- ❌ Never ask for credentials in chat or question files
- ❌ Never make up options just to have A, B, C, D
- ❌ Never proceed without answers
- ❌ Never make assumptions about ambiguous responses without recording them
