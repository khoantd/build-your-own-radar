# ThoughtWorks Radar – Architecture Overview

*Document status: **draft** – review required by the technical‑writer team.*

---

## 1. Overview

ThoughtWorks Radar is a lightweight web application that generates an interactive technology radar from a data source supplied by the user. The primary data source is a **public Google Sheet**; alternative formats (CSV, JSON) are supported for advanced use‑cases. The application renders the radar in the browser and can be hosted as a static site (e.g., GitHub Pages) or run in a containerised environment.

---

## 2. Context

---

## 3. Key Decisions

---

## 4. System Landscape

### 4.1 Containers &amp; Services

```
+-------------------+        +-------------------+
|  Browser (Client) | <----> |  Google Sheets    |
|  - Radar UI       |        |  (public CSV)    |
|  - JS fetch logic |        +-------------------+
+-------------------+

(Optional) +-------------------+
            |  Static Host      |
            |  (GitHub Pages,  |
            |   Netlify, etc.) |
            +-------------------+
```

- **Browser** – Loads `index.html`, fetches the public CSV export of the Google Sheet via a simple `fetch` request, parses the data, and renders the radar using the bundled JavaScript library.
- **Google Sheets** – Acts as the authoritative data store. The sheet is published as a CSV file (`https://docs.google.com/spreadsheets/d/<ID>/export?format=csv`).
- **Static Host** – Serves the static assets (`index.html`, CSS, JS). No runtime code executes on the server.

### 4.2 Data Flow (User → Radar)

1. **User edits** the Google Sheet (adds/updates entries).
2. **Sheet is published** as a public CSV URL (automatically updated on each edit).
3. **Browser fetches** the CSV URL on page load (or on manual refresh).
4. **JS parser** converts CSV rows into radar items.
5. **Radar renderer** draws the interactive visualization.

The entire flow is **pull‑based**; there is no push or webhook mechanism.

---

## 5. Radar Application Context
