# ThoughtWorks Radar – Architecture Overview

*Document status: **draft** – review required by the technical‑writer team*  

---

## 1. Overview

ThoughtWorks Radar is a lightweight, client‑side web application that visualises a technology radar from a user‑provided data source.  

- **Primary data source** – a **public Google Sheet** published as a CSV file.  
- **Alternative data sources** – CSV or JSON files supplied by the user (requires manual hosting).  
- **Deployment options** – static hosting (GitHub Pages, Netlify, etc.) or a containerised runtime.

The application runs entirely in the browser; no server‑side code is required to process the data.

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
|  - Radar UI       |        |  (public CSV)     |
|  - Fetch logic    |        +-------------------+
+-------------------+

(Optional) +-------------------+
            |  Static Host      |
            |  (GitHub Pages,   |
            |   Netlify, …)     |
            +-------------------+
```

- **Browser** – Loads `index.html` and all static assets, then fetches the CSV export of the Google Sheet.  
- **Google Sheets** – Authoritative data store; published as `https://docs.google.com/spreadsheets/d/<ID>/export?format=csv`.  
- **Static Host** – Serves the HTML, CSS, and JavaScript bundle. No runtime code executes on the server.

### 4.2 Data Flow (User → Radar)

1. **User edits** the Google Sheet (adds, updates, or removes entries).
2. **Sheet is published** as a public CSV URL (automatically reflects the latest edits).
3. **Browser fetches** the CSV URL on page load or on manual refresh.
4. **JS parser** converts each CSV row into a radar item (quadrant, ring, label, etc.).
5. **Radar renderer** draws the interactive visualization in the browser.

The flow is **pull‑based**; there is no push, webhook, or server‑side processing.

---

## 5. Detailed Components

---

## 6. Non‑Functional Considerations

- **Privacy** – Data is publicly accessible; users must ensure no confidential information is stored in the sheet (ADR‑0001).  
- **Availability** – The radar depends on Google’s CSV export service; outages or API changes could temporarily break data retrieval.  
- **Performance** – CSV size is typically small (&lt; 100 KB); fetch and parse complete within a few hundred milliseconds on modern browsers.  
- **Scalability** – As a static client‑side app, the solution scales automatically with the number of page views; the only bottleneck is the Google Sheets export latency.

---

## 7. Operational Runbook

---

## 8. References

- **ADR‑0001** – *Use Google Sheets as Primary Data Source* (accepted)  
- **ADR‑0000** – *ThoughtWorks Radar – Architecture Overview* (draft)  
- Repository: [https://github.com/khoantd/build-your-own-radar](https://github.com/khoantd/build-your-own-radar) (master)

---

*End of document*
