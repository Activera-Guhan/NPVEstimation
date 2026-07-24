# [NPV Estimation Tool — Activera Consulting](https://activera-guhan.github.io/NPVEstimation/)

## Table of Contents
1. [About](#about)
2. [What Is NPV](#what-is-npv)
3. [What This Tool Is Used For](#what-this-tool-is-used-for)
4. [How To Use This Tool](#how-to-use-this-tool)
5. [Outputs](#outputs)
6. [For Editors](#for-editors)
   - [Publishing Changes to an Existing Repository](#publishing-changes-to-an-existing-repository)
   - [Creating a New Repository](#creating-a-new-repository)

## About
This tool lets a user model the financial case for a project or investment by inputting costs, benefits, timing, and risk assumptions, and instantly see whether that project is expected to create or destroy value. It's built for business-case and project-valuation work at Activera Consulting — evaluating things like technology transformations, process improvements, or automation initiatives where costs land upfront and benefits accrue over time.

It runs entirely client-side in the browser — no installation, no server, no data ever leaves the user's machine. The interface is plain HTML/CSS/JavaScript, and all of the financial math and chart rendering is done in Python, running directly inside the browser via [Pyodide](https://pyodide.org/).

To use it, open `index.html` (the landing page) and click through to the calculator (`control.html`), or open `control.html` directly. Fill in the project's assumptions, click **Go**, and the tool generates a full valuation — base-case NPV, sensitivity analysis, and (optionally) a Monte Carlo simulation — along with a written summary and downloadable reports.

## What Is NPV
**Net Present Value (NPV)** is a core financial metric used to evaluate whether an investment or project is worth pursuing. It works by:
1. Estimating all the cash inflows (benefits) and outflows (costs) a project will generate over its lifetime.
2. "Discounting" each future cash flow back to today's dollars, using a discount rate that reflects the time value of money and the riskiness of the investment (a dollar received five years from now is worth less than a dollar today).
3. Summing all of these discounted cash flows, including the upfront investment (as a negative), into a single number.

- **NPV > 0** — the project is expected to generate more value than it costs. It creates value.
- **NPV < 0** — the project is expected to cost more than it returns. It destroys value.
- **NPV = 0** — the project exactly breaks even.

NPV is widely preferred over simpler metrics (like payback period or ROI) because it accounts for both the timing and the magnitude of cash flows, and reflects risk through the discount rate.

## What This Tool Is Used For
* **Inform go/no-go decisions:** Quantify whether a proposed project is financially justified before committing budget to it.
* **Capture the full benefit case:** Combine **hard financial benefits** (direct dollar savings/revenue) with **soft benefits** — time saved, reduced turnover, avoided downtime, and more — translating soft benefits into dollar terms automatically.
* **Model realistic adoption:** Ramp benefits up gradually over a configurable adoption period, instead of assuming full benefit starts on day one.
* **Stress-test the result:** Run **sensitivity analysis** (how much does NPV move if one assumption changes?) and a **Monte Carlo simulation** (what's the realistic range of outcomes given uncertainty across several assumptions at once?).
* **Produce client-ready output:** Generate a polished written summary and charts, exportable as PDF, PNG, or CSV, for use in presentations or business cases.

## How To Use This Tool
**1.** Click on the link at the very top of the instructions, and press "Find your NPV now!"

**2.** Fill in **Basic Details**: Project Name and Company Name.

**3.** Choose your **Analysis Options** — which outputs you want generated:
- Overall Summary *(always included)*
- Cash Flow Diagram
- Sensitivity Spider Chart
- Monte Carlo Distribution

**4.** Fill in the **Input Options** — the core financial assumptions:
- Initial Investment ($) and Annual Benefit ($)
- Duration (yrs) and Implementation (yrs)
- Annual Expenses ($)
- Discount Rate, Inflation Rate, Tax Rate, and Overhead Rate

**5.** (Optional) Expand **Optional Inputs** to enable:
- **Project Adoption Ramp Up** — spreads benefit realization over a number of years instead of starting at 100% immediately.
- One or more **Soft Benefit** categories (Time Saved / Productivity Gain, Reduced Manager Oversight, Faster Cycle Times, Revenue Uplift, Reduced Churn, and more) — each converts its own inputs into an annualized dollar value automatically.

**6.** Click **Go**. The tool runs entirely in-browser (via Pyodide) to compute the cash flow schedule, base-case NPV, sensitivity analysis, and — if selected — the Monte Carlo simulation.

**7.** Review the results on the right: the Overall Summary plus any charts you selected. On the Sensitivity Spider Chart, you can change which variables are shown or highlighted and it will redraw live.

**8.** Choose a **File Type** (PDF / PNG / CSV) and click **Download** to export your results.

### Additional Elements
- **Info bubbles (i):** Hover for guidance on what a specific field means and how it affects the model.
- **Clear:** Resets all fields and clears the results panel.
- **Download:** Grayed out until a calculation has been run; becomes active once results exist.

## Outputs
The tool generates the following, based on what's selected under Analysis Options:

- **Overall Summary** *(always generated)* — a one-page visual summary covering project metadata, base-case NPV and verdict, top sensitivity drivers, and (if run) Monte Carlo probabilistic results.
- **Cash Flow Diagram** — annual net cash flow and cumulative present value over the project's lifetime, with the final NPV annotated.
- **Sensitivity Spider Chart** — how NPV responds to a ±20% swing in each key input (investment, benefit, opex, duration, discount rate), so you can see which assumption the result is most sensitive to.
- **Monte Carlo Distribution** — 10,000 simulated scenarios with randomized inputs (within realistic uncertainty ranges), showing the distribution of possible NPV outcomes, P10 / P50 / P90 values, and the probability that NPV is positive.

These can be exported in three formats:

| Format | Contents |
|---|---|
| **PDF** | A formatted, multi-page report with a cover page, table of contents, the Analysis Summary, and a dedicated page per selected chart — ready to share or present. |
| **PNG** | A ZIP file containing each selected chart as an individual high-resolution PNG image. |
| **CSV** | The full year-by-year cash flow table, plus (if run) the sensitivity analysis swing table and the Monte Carlo probabilistic summary — for further analysis in Excel or other tools. |

---

## For Editors
**IMPORTANT: Create a copy of `control.html` (and `index.html`) before making any changes, to avoid accidental alterations to the working version.**

### Project Construction
This project is built entirely with **HTML, CSS, and JavaScript** on the front end, with **Python integrated directly into the browser via Pyodide** — there is no backend server or build process.

- **`index.html`** — the landing/splash page.
  - Flat, solid background (no gradient): light `#f1f5f9`, with `#acc9e9` (mid) and `#32568b` (dark) as the accent/text colors.
  - Typeface is Helvetica Neue throughout (regular for body text, bold for the heading and button), no all-caps text transforms.
  - Brand logo pinned to the top-left, brand icon pinned to the top-right at 50% opacity — both `position: fixed` and sized with `vw`/`clamp()` so they scale cleanly on mobile without separate breakpoints.
  - Links to `control.html`.
- **`control.html`** — the full calculator application. This single file contains:
  - The **HTML structure** for all inputs (basic details, analysis options, financial inputs, optional soft benefits, output selection, download controls).
  - **CSS** (embedded in a `<style>` block) controlling layout, theming, and responsiveness — using the same color tokens and Helvetica Neue typeface as `index.html` for a consistent look. The brand logo sits inline with the "Basic Details" title; the brand icon appears as a 50%-opacity watermark behind the placeholder text in the empty results panel.
  - **JavaScript** that handles UI interactivity (form state, dynamic soft-benefit rows, checkbox toggles) and orchestrates calls into Python.
  - A large embedded **Python codebase** (loaded as a string and executed inside Pyodide) that performs all the actual financial modeling: building the cash flow schedule, computing NPV, running sensitivity analysis, running the Monte Carlo simulation, rendering all chart images (via Matplotlib, returned to JavaScript as base64-encoded PNGs), and building the downloadable PDF/PNG-ZIP/CSV files.
- **Pyodide** is loaded from a CDN and, on first run, loads the `numpy`, `pandas`, and `matplotlib` packages into the browser's Python environment.
- **`activera_consulting_logo_short.png`** and **`activera_consulting_icon.png`** must stay in the same folder as `index.html` and `control.html` — both are referenced via relative paths, and the logo is also embedded into the downloadable PDF report.
- Because everything runs client-side, no data ever leaves the user's browser — there is no server component and no data storage.

### Key Python entry points (inside `control.html`)
- `run_from_js(params)` — main entry point called from JavaScript when the user clicks Go. Builds the cash flow table, computes NPV, sensitivity analysis, and (optionally) Monte Carlo results, and returns chart images + summary data back to JS.
- `redraw_spider(display_vars, highlight_var)` — re-renders the Sensitivity Spider Chart when the user changes which variables to display/highlight, without re-running the full calculation.
- `prepare_download(filetype)` — assembles the final PDF, PNG ZIP, or CSV for download, using the most recently computed results.

### Things to Watch For When Editing
- The Python code lives inside a large JavaScript string (`ENGINE_CODE`) in `control.html` — indentation and escaping matter, since it's Python source embedded in a JS template literal.
- Chart colors and formatting are centralized in the `COLORS` dictionary and `plt.rcParams` block in the Python engine — this is separate from the page's own CSS color tokens, so re-theming the page does not automatically re-theme the charts.
- The page's own color palette lives in CSS custom properties near the top of `control.html`'s `<style>` block (and as `:root` variables in `index.html`) — update these in one place to re-theme the interface consistently.
- The PDF export uses a fixed 8.5"×11" page layout with its own header/footer/cover-page logic; changes to chart content will generally flow through automatically, but page-count logic (`total_pages`) assumes the current set of chart types.

### Suggested AI Prompt Template for Changes
> This project is an NPV Estimation tool, built in HTML/CSS/JS + Python integrated via Pyodide. I have uploaded the necessary file(s) — please do not make any stylistic changes, only the specific changes mentioned below:
>
> [Describe the specific functional change you want here]

### Publishing Changes to an Existing Repository
This project has multiple files (`index.html`, `control.html`, `activera_consulting_logo_short.png`, `activera_consulting_icon.png`), so update each file that changed individually:
1. Log in to GitHub.
2. Find the repository you are working in.
3. Click the file you need to update (e.g. `control.html`).
4. Click the edit button (pen icon).
5. Make/paste your changes.
6. Press the green "Commit changes" button.
7. Repeat for any other files that changed (e.g. an updated logo/icon PNG, or `index.html`).
8. Changes will automatically be updated on the live site.

*Note: images (like the logo/icon PNGs) can't be edited in the browser like text files — instead, delete the old file and upload the replacement with the exact same file name, so the existing relative-path references in the HTML keep working.*

### Creating a New Repository
**If you want one based on another repository:**
- Go to the dashboard of the repository you want to clone.
- Click the green "Code" button.
- Click "Download ZIP".
- Open your File Explorer app.
- Right-click the ZIP file you downloaded and click "Extract All".
- Go to the GitHub homepage.
- Click the "New" button (to create a new repository).
- Fill in the details and click "Create repository".
- Click "Add file."
- Click "Upload file".
- Select all the files from the unzipped folder (including both PNG assets).
- Upload them.

**If you want a brand new blank repository (skip if you've already created one):**
- Click the GitHub cat logo at the top left (homepage).
- Click the green "New" button at the top left.
- Fill out the details and click the green "Create repository" button.
- Click "Add file" → "Upload file" and upload `index.html`, `control.html`, and both PNG assets together.

**Then, either way:**
1. Click "Settings", then "Pages".
2. Under "Branch", change "None" to "main".
3. Press "Save".
4. Wait a minute, then refresh the page — the link will show up.
5. The new page will have the changes; use the new link from now on.

*Note: once a repository is created, any file can be edited by the owner and the page will automatically update. A new repository only needs to be created if access isn't granted to the current one and changes need to be published somewhere new. Since `index.html` links to `control.html` by a relative path, both files (and the two PNG assets) need to live in the same repository, at the same folder level, for the site to work correctly.*

---

<sub>Made by Guhan Gargya at Activera Consulting</sub>
