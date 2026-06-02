# ⚕️ Medical Coding: HCC & RxHCC Rapid Search

## Overview
Standard medical coding workflows often require manually cross-referencing patient ICD-10 diagnoses across multiple, massive CMS datasets to identify both CMS-HCC (Medicare Advantage) and RxHCC (Pharmacy) risk adjustment categories. This process is traditionally slow, prone to formatting errors, and requires navigating cumbersome government spreadsheets.

This application is a **custom, high-performance search tool** built to instantly unify and filter these data sources into a single, lightning-fast UI for daily production use.

## The Architecture
To ensure maximum speed and a $0 hosting footprint, this project leverages a modern, static-first data architecture. 

* **The Data Pipeline (DuckDB):** Instead of hosting a live SQL database that costs monthly overhead, this app uses a local DuckDB pipeline. DuckDB reads, cleans, and joins hundreds of thousands of rows of raw CMS CSV crosswalk files locally, handling "dirty" data (like ragged rows and header line-breaks) to output a highly optimized, lightweight JSON payload.
* **The Frontend (Astro):** Built using the Astro web framework, the application ships zero unnecessary JavaScript to the browser. The frontend simply fetches the pre-compiled JSON payload and uses native DOM filtering to search the dataset instantly upon keystroke.
* **Hosting & CI/CD (Vercel):** The repository is connected to Vercel for continuous integration. Any updates to the master branch trigger an automatic build and deployment of the static assets globally.

## Key Features
* **Instant Sub-string Matching:** Search by partial ICD-10 code (e.g., "E11") or by diagnosis description (e.g., "Diabetes").
* **Unified Risk Categories:** Simultaneously displays both CMS-HCC (Red) and RxHCC (Purple) badges if a specific diagnosis triggers one or both models.
* **Resilient Data Parsing:** The backend pipeline is configured to automatically ignore formatting errors, null-pad ragged rows, and bypass strict CSV modes common in raw government exports.

## Future Roadmap
* **Data Pipeline Automation:** Implement a script to automatically ingest and re-compile the JSON payload when CMS releases the next fiscal year's crosswalk updates.
* **UX Enhancements:** Add quick-clear functionality ("X" button) and "Copy to Clipboard" toggles to further reduce friction between patient charts.

---
*Architecture inspired by modern data engineering patterns discussed on the Spicy Data blog.*