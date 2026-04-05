---
inclusion: auto
name: refactor-onboarding
description: Use when the user wants to rebuild, replace, improve, or modernize an existing system, tool, or workflow
---

# Refactoring and Rebuilding — Onboarding Process

This process applies when the user has something existing they want to rebuild or improve. The goal is to fully understand what exists before proposing anything new.

---

## Step 1: What exists today?

Ask: *"Can you describe what you have today — is it a spreadsheet, a tool, a website, a manual process, or something else?"*

Listen for:
- **Excel/spreadsheet** — the most common case at FVST. Ask to see the file or describe the sheets and columns.
- **An existing application** — could be a web app, desktop tool, internal system. Ask what technology it uses if they know.
- **A manual process** — paper forms, email chains, copy-paste workflows. Ask them to walk through a typical example.
- **A tool someone else built** — may not have access to the code. Focus on what the tool does, not how it works.

## Step 2: Gather materials

Ask the user to provide what they can. Be explicit about what is helpful — non-technical users often don't know what to share:

> *"To help me understand what you have, could you share any of the following?"*

- **Code files** — paste them into the chat or open the folder in Kiro
- **Screenshots** — photos of the current tool, spreadsheet, or process (phone photos are fine)
- **Documents** — requirements documents, user guides, process descriptions
- **Example data** — a sample spreadsheet, a few rows of typical input/output
- **A walkthrough** — describe step by step what you do today from start to finish

Accept whatever the user can provide. Even a verbal description is enough to start — do not block on getting "perfect" input.

## Step 3: Analyse and summarise

Before proposing any changes, write a summary of what you understand:

> *"Based on what you've shared, here is what I understand the current system does:"*

Structure the summary as:
1. **What it does** — the core function in 2–3 sentences
2. **Who uses it** — and how (daily? weekly? on the go?)
3. **What data it handles** — what goes in, what comes out
4. **What works well** — parts the user wants to keep
5. **What is broken or missing** — the reason they want to change it

Always confirm: *"Did I get this right? Is there anything important I missed?"*

## Step 4: Identify what to keep

Ask explicitly: *"Is there anything from the current system that must stay exactly the same — for example, specific calculations, data formats, or workflows that other people or systems depend on?"*

This is critical. Non-technical users may not mention constraints that seem obvious to them:
- Data formats that feed into other systems
- Specific calculation logic (e.g., pricing rules, approval thresholds)
- Naming conventions or labels that colleagues rely on
- Existing data that needs to be carried over

## Step 5: Data migration

If the existing system contains data the user wants to keep:

- **Excel/CSV data** → can usually be imported directly into the new prototype
- **localStorage data** → help the user export it before rebuilding
- **Database data** → may require the handoff protocol if complex

Always ask: *"Do you have data in the current system that you need to bring over to the new version?"*

If yes, handle migration as the first step of building — before adding new features.

---

## After onboarding: proceed to the standard flow

Once you have a clear understanding of the existing system and what the user wants changed, proceed with the standard workflow:

1. **Choose technology** — using the decision tree in `02-tech-decision.md` (same rules apply)
2. **Build iteratively** — start with the core function that replaces the existing system, then improve
3. **Validate against the original** — after each iteration, compare with the original: *"Does this do the same thing your current system does? What's different?"*

---

## What you never do

- Never propose rewriting in a different technology just because it is "better" — follow the decision tree
- Never discard existing logic without confirming with the user first
- Never assume the old system is wrong — it may have embedded business rules the user cannot articulate
- Never start building until you have confirmed your understanding of the current system
