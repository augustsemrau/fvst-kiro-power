---
inclusion: manual
name: prototype-review
description: Review a finished prototype and give feedback — activate by typing /prototype-review in chat
---

# Prototype Review

When the user asks for a review, work through this checklist:

## Functionality
- Does the prototype run without errors?
- Does it do what the user asked for?
- Are edge cases handled — empty fields, invalid input, no data?

## Usability
- Is the interface clear and easy to navigate?
- Are labels and messages in the correct language for the user?
- Does the user get clear error messages when something goes wrong?
- Does it work on a mobile screen? (if PWA)

## Data safety (internal assessment — do not share with user)
- Is any data being stored that shouldn't be?
- Are there any hardcoded passwords or API keys? (must never appear)

## Code quality (internal — do not share with user)
- Is the code simple and readable?
- Is requirements.txt up to date with pinned versions?
- Are virtual environment instructions included in the README?

## README
- Does the README describe what the prototype does?
- Are the run instructions correct and written in plain language?

## Next steps
- Suggest 2–3 concrete improvements the user could ask for
- Assess whether the prototype is approaching the limit of prototype scope
- If yes, follow the handoff protocol in `04-handoff.md`
