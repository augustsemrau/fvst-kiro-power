# fvst-kiro-power

A Kiro Power that helps FVST employees build working prototypes from ideas — no technical experience required.

## Installation (for users)

1. Open Kiro
2. Find **"Powers"** in the left panel
3. Click **"Add power from GitHub"**
4. Paste: `github.com/augustsemrau/fvst-kiro-power`
5. Click **Install**

That's it. The next time you open a project and describe your idea, the power activates automatically.

---

## What the power does

When you mention words like "idea", "prototype", "build", "app", "automate", "phone", "dashboard", or "report" — or their Danish equivalents — Kiro automatically loads the FVST context and guides you through the process.

**The power controls:**
- How Kiro interviews you about your idea
- Which technology Kiro chooses (without asking you)
- When Kiro recommends involving a developer
- What happens if you want to build a mobile app (PWA instead of React Native)

---

## Contents

```
fvst-kiro-power/
├── POWER.md                      ← Entry point — onboarding and keyword triggers
├── mcp.json                      ← Context7 for up-to-date documentation
├── hooks/
│   └── readme-updater.kiro.hook  ← Keeps README accurate automatically
└── steering/
    ├── 00-fvst.md          ← Who FVST is and who the users are
    ├── 01-interaction.md         ← Interaction style and tone
    ├── 02-tech-decision.md       ← Decision tree for technology choices
    ├── 03-pwa-patterns.md        ← Patterns for mobile apps (auto-loaded)
    ├── 04-handoff.md             ← Handoff to technical colleague (auto-loaded)
    └── 05-prototype-review.md    ← Prototype review (/prototype-review)
```

---

## Maintenance (for the IT team)

**Update steering files** by editing the `.md` files and pushing to main. Users get the update next time they use Kiro — no reinstallation needed.

**Add a new technology type:** Edit `steering/02-tech-decision.md` and add a new branch to the decision tree.

**Add a new use-case pattern** (like `03-pwa-patterns.md`): Create a new `.md` file in `steering/` with `inclusion: auto` and a clear `description` — Kiro will load it automatically when relevant.

**Test changes:** Clone the repo locally, open the folder in Kiro, and test with a new conversation.

---

## Technology choices the power makes

| Scenario | Choice |
|---|---|
| Mobile app (iPhone/Android) | Progressive Web App |
| Internal desktop tool | Python + Streamlit |
| Team sharing on local network | Streamlit with `--server.address 0.0.0.0` |
| Data processing / Excel automation | Python script |
| Simple information page | Static HTML |

The power **never** chooses React, React Native, Flutter, Firebase, Docker, or any technology requiring cloud accounts or complex setup.

---

*Maintained by the FVST IT team*
