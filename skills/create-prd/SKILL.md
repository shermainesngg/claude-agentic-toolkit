---
name: create-prd
description: Generate a comprehensive Product Requirements Document (PRD) from conversation context. Use this skill whenever the user wants to create a PRD, write product requirements, document a product spec, plan an MVP, or turn a product discussion into a structured document. Also trigger when the user says "write a PRD", "create requirements doc", "document this product", "spec this out", or asks to formalize a product idea into a structured plan.
---

# Create PRD: Generate Product Requirements Document

Generate a comprehensive Product Requirements Document (PRD) based on the current conversation context and requirements discussed.

## Why this matters

A good PRD is the bridge between a product idea and its implementation. It forces clarity on scope, surfaces hidden assumptions, and gives the whole team a shared reference point. Without one, teams build the wrong thing, scope creeps silently, and success is undefined.

## Before writing: Extract requirements

Review the entire conversation history and gather:
- Explicit requirements and implicit needs
- Technical constraints and preferences
- User goals and success criteria
- Any decisions already made

If critical information is missing, ask clarifying questions before generating. A PRD built on guesses is worse than no PRD.

## PRD structure

Create the PRD with the following sections. Adapt depth based on what's known — emphasize architecture for technical products, user stories for consumer-facing ones.

### 1. Executive Summary
- Concise product overview (2-3 paragraphs)
- Core value proposition
- MVP goal statement

### 2. Mission
- Product mission statement
- Core principles (3-5 key principles)

### 3. Target Users
- Primary user personas
- Technical comfort level
- Key user needs and pain points

### 4. MVP Scope
- **In Scope:** Core functionality for MVP (use checkboxes)
- **Out of Scope:** Features deferred to future phases (use checkboxes)
- Group by categories (Core Functionality, Technical, Integration, Deployment)

### 5. User Stories
- 5-8 primary user stories in format: "As a [user], I want to [action], so that [benefit]"
- Include concrete examples for each story
- Add technical user stories if relevant

### 6. Core Architecture & Patterns
- High-level architecture approach
- Directory structure (if applicable)
- Key design patterns and principles
- Technology-specific patterns

### 7. Features
- Detailed feature specifications
- For agents: tool designs with purpose, operations, and key features
- For apps: core feature breakdown

### 8. Technology Stack
- Backend/frontend technologies with versions
- Dependencies and libraries
- Third-party integrations

### 9. Security & Configuration
- Authentication/authorization approach
- Configuration management (environment variables, settings)
- Security scope (in-scope and out-of-scope)
- Deployment considerations

### 10. API Specification (if applicable)
- Endpoint definitions
- Request/response formats
- Authentication requirements
- Example payloads

### 11. Success Criteria
- MVP success definition
- Functional requirements (use checkboxes)
- Quality indicators
- User experience goals

### 12. Implementation Phases
- Break into 3-4 phases
- Each phase: Goal, Deliverables (checkboxes), Validation criteria
- Realistic timeline estimates

### 13. Future Considerations
- Post-MVP enhancements
- Integration opportunities
- Advanced features for later phases

### 14. Risks & Mitigations
- 3-5 key risks with specific mitigation strategies

### 15. Appendix (if applicable)
- Related documents
- Key dependencies with links
- Repository/project structure

## Style guidelines

- **Tone:** Professional, clear, action-oriented
- **Format:** Use markdown extensively — headings, lists, code blocks, tables
- **Checkboxes:** Use them for in-scope and out-of-scope items to make status scannable
- **Specificity:** Concrete examples over abstract descriptions
- **Consistency:** Use the same terminology throughout

## Quality checks before finishing

Verify the PRD passes these checks:
- All relevant sections are present and substantive
- User stories have clear benefits
- MVP scope is realistic and well-defined
- Technology choices are justified
- Implementation phases are actionable
- Success criteria are measurable
- No contradictions between sections

## Output

Write the PRD to a file (default: `PRD.md` in the current directory). After creating it:
1. Confirm the file path
2. Summarize the key points
3. Call out any assumptions made due to missing information
4. Suggest next steps (review, refinement, implementation planning)
