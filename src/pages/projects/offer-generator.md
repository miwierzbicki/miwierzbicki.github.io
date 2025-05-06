---
layout: ../../layouts/MarkdownLayout.astro
title: Custom offer generator
author: Mirosław Wierzbicki
date: 2025-02-01 08:30
status: finished ✅
repo: /404
---



### Overview
Web app built to help a small IT outsourcing company create sales quotes more easily and consistently. The main goal was to speed up the quoting process and keep everything organized.

### Key Features

*   **Client Management:**
    *   Pick existing clients from a list.
    *   Add new clients with their basic info (like NIP/VAT ID).<br><br>

*   **Quote Building:**
    *   Simple form to add items to a quote.
    *   Input fields for item name, net/gross price, VAT rate, margin, and quantity.
    *   Live preview of totals as items were added.
    *   Easy to add or remove items.<br><br>

*   **Custom File Names & Unique IDs:**
    *   Option to set a standard way to name the output PDF files (e.g., ClientName_Date).
    *   **Each generated quote was assigned a unique ID**, allowing for clear identification both internally and for clients.<br><br>

*   **PDF Generation:**
    *   Created a standard PDF quote which included the unique quote ID, company logo, and client details.
    *   Contained a table showing all items, prices, VAT, and margins.
    *   Displayed clear final amounts for the client and VAT breakdown.
    *   Had a summary section and space for any notes.

### Tech Stack

*   **Frontend:** **Vue.js** with **Bootstrap** for the user interface.
*   **Backend:** **Node.js** (providing API endpoints).
*   **Database:** **MariaDB**.
*   **Deployment:** Ran on a lightweight **Debian VM** in **Proxmox**.

### Impact
This tool really helped the team:

*   **Faster Quotes:** Made generating quotes much quicker.
*   **Consistency:** All quotes looked the same and had a standard format.
*   **Better Organization & Tracking:** Standard file names and unique quote IDs made it easier to find, reference, and track specific quotes.

