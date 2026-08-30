---
name: product-spec-generator
description: Generate comprehensive Product Requirements Documents (PRDs) and Epics using the approved project template. Use when the user wants to write a PRD, draft feature specifications, define user stories, outline acceptance criteria, or analyze feature scope.
---

# Product Specification Generator

You are tasked with generating Epics and Product Requirement Documents (PRDs) for the project.

## MANDATORY TEMPLATE

Whenever you are asked to generate an Epic or PRD, you MUST use the following exact structure and headings. This template is the "ideal" format approved for this project.

```markdown
# [Epic Name]

**By [Author]**

## User Story
[Describe the core user stories and roles. Explain who needs this feature and why.]

## Business Value
[List the business benefits as bullet points. Example: Automation of processes, time savings, scalability.]

## Current Problems
[List the current problems, limitations, or pain points that this Epic solves. Example: The system currently does not allow X, Y, Z.]

## Scope
[Numbered list of scope items with detailed descriptions.]
### 1. [Scope Item Name]
[Description and business logic]
### 2. [Scope Item Name]
[Description and business logic]

## Dependencies
[List external systems, other epics, technical modules, or teams this Epic depends on. Example: Epic depends on Universal Editor.]

## Future Scope
[List items that are considered for future phases but are not part of the MVP.]

## Out of Scope
[Explicitly list what is NOT being done in this Epic to prevent scope creep.]

## Acceptance Criteria
[List the precise acceptance criteria that must be met for this Epic to be considered complete. Formatted as bullet points or checkboxes.]
```

## Workflows

### Generating a PRD or Epic
1. **Gather Information**: Ask the user for the high-level goal, target users, and key features if not provided.
2. **Draft the Document**: Use the MANDATORY TEMPLATE above. Do not skip any sections. If a section is empty, explicitly write "None".
3. **Format**: Use clear markdown formatting, nested lists, and bold text for readability.
4. **Review & Refine**: Check for ambiguity or untested assumptions. Ensure the MVP scope is clearly separated from "Future Scope" and "Out of Scope".
