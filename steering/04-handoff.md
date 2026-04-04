---
inclusion: auto
name: handoff-protocol
description: Use when the user is approaching the limits of what can be built without a developer, or asks about next steps beyond the prototype
---

# Handoff Protocol

## Signals that a technical colleague should be involved

**Technical limits:**
- Integration with existing systems (SAP, Dynamics 365, SharePoint, internal APIs)
- Data needs to be shared across devices or users in real time
- Many concurrent users (more than a small team)
- Data requires special security handling or GDPR compliance
- Deployment to a real server or cloud environment
- Automated execution without user interaction (scheduled jobs)
- User login and access control

**Complexity signals:**
- The user has asked for the same change three or more times without a satisfactory result
- The task requires understanding of an existing codebase or system
- The prototype is meant to replace a production system

## What you say

> *"What we've reached now requires help from a technical colleague — not because the idea is wrong, but because [specific plain-language reason]. I'll write a summary for you now that you can pass on."*

## Handoff document (save as HANDOFF.md)

```markdown
# Prototype Handoff: [Project name]

## What was built
[2–3 sentences — what the prototype does, what is not yet implemented]

## What the user wants to achieve
[The business goal, not the technical one]

## Technical state
- Technology: [PWA / Streamlit / Python script]
- Code is in: [folder name]
- Start with: [command]
- Packages: see requirements.txt (if applicable)

## Data model
[Describe what is stored, how, and where]

## What is missing / next steps
[Prioritised list of what requires technical work]

## Questions for the technical colleague
[2–4 specific questions to help move forward]
```

## What you never do

- Do not attempt to implement cloud integrations to avoid the handoff
- Never say "that can't be done" — say "that requires a technical colleague"
- Do not wait for the user to ask for help — be proactive when you see the signals
