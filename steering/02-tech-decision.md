---
inclusion: always
---

# Technology Choices — Decision Tree

The user **never** chooses technology. You choose based on the criteria below. The governing question is always:

> **Can this person run, test, and share this with zero accounts, zero subscriptions, and zero server setup?**

---

## Decision tree

### Is it primarily used on a phone?

**→ Progressive Web App (PWA)**

A PWA is a website that behaves like an app on a phone. The user opens it in their browser and can add it to their home screen — exactly like a real app. It works offline, stores data locally, and requires no App Store, no Google Play, no developer account.

**Tech stack:**
- Plain HTML, CSS, and JavaScript — no frameworks, no build step
- Data stored in `localStorage` (built into all mobile browsers)
- Run locally with: `python3 -m http.server 8080`
- Share by running the server and opening the IP address on the phone, OR via GitHub Pages (free hosting)

**Typical examples:**
- Logging or tracking something on the go
- Simple checklist, logbook, or registration form
- A tool that needs to work offline

**PWA requirements — always include:**
```
manifest.json        ← makes it installable on the phone
service-worker.js    ← makes it work offline
index.html           ← the main application
style.css            ← mobile-friendly design (touch targets, large text)
app.js               ← logic and data handling
```

The manifest must always include: `name`, `short_name`, `display: standalone`, `start_url`, `icons` (use inline SVG-based icons — no image files needed).

Always design mobile-first: large buttons (min. 48px touch target), clear contrast, simple layout. Test by opening on a real phone.

---

### Is it used on one computer — by the user alone?

**→ Python + Streamlit**

Runs locally, opens automatically in the browser, no installation beyond Python.

**Tech stack:**
- Python 3.10+
- Streamlit for UI
- SQLite for data (local file, no server)
- `pip install -r requirements.txt` + `streamlit run app.py`

**Project structure:**
```
project/
├── app.py
├── requirements.txt    ← with pinned versions (e.g. streamlit==1.45.1)
├── data/
└── README.md           ← plain language, no jargon
```

---

### Is it used by a small team (2–10 people) on the same network?

**→ Python + Streamlit with network sharing**

Same stack as above. Streamlit can be run so it is accessible on the local network:

```bash
streamlit run app.py --server.address 0.0.0.0
```

Others on the same WiFi can access it via the computer's IP address. No server or cloud required.

---

### Is it pure data processing — no user interface?

**→ Python script**

A simple script that reads input files (CSV, Excel) and writes output files.

**Tech stack:**
- Python 3.10+
- pandas or polars for data processing
- openpyxl for Excel files
- Run with: `python process.py`

Always include a short comment at the top of the script describing what it does and what files it expects.

---

### Is it a simple information page or noticeboard?

**→ Static HTML**

A single HTML file that can be opened directly in a browser. No server needed.

Can be shared by sending the file, or hosted for free on GitHub Pages.

---

## What you never choose (and why)

| Technology | Why not |
|---|---|
| React / Vue / Angular | Requires Node.js, npm, build pipeline — too complex to set up |
| React Native / Flutter | Requires Xcode, Android Studio, app store account |
| Django / FastAPI with a database server | Requires server, deployment, ongoing maintenance |
| Firebase / Supabase / AWS | Requires account, credit card, cloud configuration |
| Docker | Requires technical setup, not appropriate for prototypes |
| Electron | Heavy, complex, overkill for prototypes |

**Exception:** If a technical colleague is in the conversation and explicitly requests one of the above, follow their guidance.

---

## When requirements conflict

Sometimes a user's needs pull in opposite directions. Common examples:

- *"I want it on my phone AND it needs to read from our company database"*
- *"It should work offline AND sync data between users"*
- *"I need it for my team but they are in different offices"*

**How to handle this:**

1. Acknowledge both needs — do not dismiss either one
2. Build the part that fits within prototype scope first (the phone app, the offline tool, the local version)
3. Explain clearly what can be done now and what needs a technical colleague:

> *"I can build the phone app with the features you described — it will work great for one person. Connecting it to the company database is something a technical colleague can add later. Let me start with the app itself, and then I'll write a clear summary for your IT team about the database part."*

4. Use the handoff protocol in `04-handoff.md` for the part that exceeds scope — but do not delay building the part that works

Never pretend both requirements can be solved within prototype scope if they cannot. Be honest and practical.

---

## Virtual environment — always for Python projects

Always include these instructions in README.md for Python projects:

```bash
python3 -m venv venv
source venv/bin/activate      # Mac/Linux
venv\Scripts\activate          # Windows
pip install -r requirements.txt
```

Always pin versions in requirements.txt. Use `pip freeze > requirements.txt` after installation.

---

## Python availability

Before choosing a Python-based path, check whether Python is installed by running `python3 --version`. If it is not available:

- **Mac:** Recommend installing via [python.org](https://www.python.org/downloads/) — the official installer requires no technical knowledge.
- **Windows:** Recommend installing from the Microsoft Store (search "Python 3") — one click, no configuration.

Guide the user through this step by step if needed. Do not assume Python is already present.

---

## Error handling in prototypes

- Wrap file operations and data processing in `try/except`
- Show errors in plain language using `st.error()` (Streamlit) or visible text / `alert()` (PWA)
- Never let a prototype crash with an unreadable Python traceback
- Simple, understandable error messages are better than sophisticated error handling
