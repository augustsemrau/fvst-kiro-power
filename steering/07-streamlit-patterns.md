---
inclusion: auto
name: streamlit-patterns
description: Use when building a Streamlit application — patterns for layout, data, file handling, and common pitfalls
---

# Streamlit Patterns and Best Practices

Use these patterns when building a Streamlit app for FVST users.

---

## Project structure

Always start with this layout:

```
project/
├── app.py               ← the main application
├── requirements.txt     ← pinned versions (e.g. streamlit==1.45.1)
├── data/                ← folder for SQLite database and user files
├── exports/             ← folder for exported files
└── README.md            ← plain language, no jargon
```

---

## App layout — keep it simple

```python
import streamlit as st

st.set_page_config(
    page_title="App name here",
    page_icon="📋",
    layout="centered"       # Use "wide" only if the app displays tables or charts
)

st.title("App name here")
```

For apps with multiple sections, use tabs — not multiple pages. Tabs keep everything visible and simple:

```python
tab1, tab2, tab3 = st.tabs(["Add new", "View all", "Settings"])

with tab1:
    st.subheader("Add a new entry")
    # input form here

with tab2:
    st.subheader("All entries")
    # data table here

with tab3:
    st.subheader("Settings")
    # settings here
```

Only use `st.sidebar` if the app truly needs persistent navigation. For most prototypes, tabs are simpler.

---

## Data storage with SQLite

For any app that stores data, use SQLite — it is a single file, no server, no setup:

```python
import sqlite3
import os

DB_PATH = os.path.join("data", "app.db")
os.makedirs("data", exist_ok=True)

def get_connection():
    conn = sqlite3.connect(DB_PATH)
    conn.row_factory = sqlite3.Row    # Access columns by name
    return conn

def init_db():
    conn = get_connection()
    conn.execute("""
        CREATE TABLE IF NOT EXISTS entries (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            created_at TEXT DEFAULT (datetime('now', 'localtime')),
            title TEXT NOT NULL,
            description TEXT
        )
    """)
    conn.commit()
    conn.close()
```

Call `init_db()` at the top of `app.py` — it only creates the table if it doesn't exist yet.

---

## Forms — always use st.form

Wrap input fields in `st.form` to prevent the app from reloading on every keystroke:

```python
with st.form("add_entry"):
    title = st.text_input("Title")
    description = st.text_area("Description")
    submitted = st.form_submit_button("Save")

    if submitted:
        if not title.strip():
            st.error("Please enter a title.")
        else:
            conn = get_connection()
            conn.execute("INSERT INTO entries (title, description) VALUES (?, ?)", (title, description))
            conn.commit()
            conn.close()
            st.success("Saved!")
            st.rerun()
```

**Important:** Always use `?` placeholders for values — never put user input directly into SQL strings.

---

## Displaying data

```python
import pandas as pd

conn = get_connection()
df = pd.read_sql_query("SELECT * FROM entries ORDER BY created_at DESC", conn)
conn.close()

if df.empty:
    st.info("No entries yet. Add one above.")
else:
    st.dataframe(df, use_container_width=True, hide_index=True)
```

---

## Data export — always include this

Every Streamlit app that stores data must include an export button so the user cannot lose their data:

```python
import io

def export_data():
    conn = get_connection()
    df = pd.read_sql_query("SELECT * FROM entries", conn)
    conn.close()

    if df.empty:
        st.warning("Nothing to export yet.")
        return

    # CSV export
    csv = df.to_csv(index=False).encode("utf-8")
    st.download_button(
        label="Download as CSV",
        data=csv,
        file_name="export.csv",
        mime="text/csv"
    )
```

Place the export button in a visible location — a "Settings" tab or at the bottom of the data view.

---

## File upload — importing data

For apps that need to import data from Excel or CSV:

```python
uploaded = st.file_uploader("Upload a file", type=["csv", "xlsx"])

if uploaded is not None:
    try:
        if uploaded.name.endswith(".csv"):
            df = pd.read_csv(uploaded)
        else:
            df = pd.read_excel(uploaded)
        st.dataframe(df)
        st.success(f"Loaded {len(df)} rows.")
    except Exception:
        st.error("Could not read the file. Make sure it is a valid CSV or Excel file.")
```

---

## Session state — remembering things between clicks

Streamlit reruns the entire script on every interaction. Use `st.session_state` to keep values between reruns:

```python
# Initialize once
if "counter" not in st.session_state:
    st.session_state.counter = 0

if st.button("Count"):
    st.session_state.counter += 1

st.write(f"Clicked {st.session_state.counter} times")
```

Use session state for: edit mode toggles, selected items, form prefills, multi-step workflows.

---

## Error handling

Always catch errors from file operations and database queries. Show plain-language messages:

```python
try:
    conn = get_connection()
    conn.execute("INSERT INTO entries (title) VALUES (?)", (title,))
    conn.commit()
    conn.close()
    st.success("Saved!")
except Exception:
    st.error("Something went wrong while saving. Please try again.")
```

Never let the user see a Python traceback — it will confuse and alarm them.

---

## Running the app

Always include these instructions in README.md:

```markdown
## How to run

1. Open a terminal in the project folder
2. Create a virtual environment (first time only):
   python3 -m venv venv
3. Activate it:
   - Mac/Linux: source venv/bin/activate
   - Windows: venv\Scripts\activate
4. Install packages (first time only):
   pip install -r requirements.txt
5. Start the app:
   streamlit run app.py
6. The app opens automatically in your browser
```

---

## Sharing on the local network

If others on the same WiFi need to use the app:

```bash
streamlit run app.py --server.address 0.0.0.0
```

Include these instructions in README.md:

```markdown
## Sharing with colleagues (same WiFi)

1. Start the app with: streamlit run app.py --server.address 0.0.0.0
2. Find your IP address:
   - Mac: System Settings → Network → IP address
   - Windows: Run `ipconfig` in terminal → IPv4 Address
3. Colleagues open http://[your-ip]:8501 in their browser
```

**Important limitation:** When sharing this way, all users share the same database file. This is fine for a small team, but inform the user: *"Everyone who opens the app sees the same data. If two people save at the exact same moment, one change could be lost — but for a small team this is very rare."*

---

## When sharing beyond the local network is requested

If the user asks to share the app with someone not on the same WiFi or network, this exceeds prototype scope. Follow the handoff protocol in `04-handoff.md`. Explain:

> *"Sharing this app with people outside your network requires setting up a server — that's something a technical colleague can help with. I'll write a summary for them so they can set it up."*

Do not attempt to configure Streamlit Community Cloud, ngrok, or any tunnelling service — these require accounts and introduce complexity that non-technical users cannot maintain.
