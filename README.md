# NPV Estimation Tool — Activera Consulting

## 1. What This Project Is

This project is a self-contained, browser-based **Net Present Value (NPV) estimation tool** built for Activera Consulting. It lets a user model the financial case for a project or investment — inputting costs, benefits, timing, and risk assumptions — and instantly see whether that project is expected to create or destroy value.

The tool requires no installation and no server. It runs entirely in the browser, using Python (via Pyodide) for all the financial math and charting, and plain HTML/CSS/JavaScript for the interface. A user simply opens `index.html`, clicks through to the calculator (`control.html`), fills in their assumptions, and receives a full valuation — including charts, a written summary, and downloadable reports.

## 2. What Is NPV?

**Net Present Value (NPV)** is a core financial metric used to evaluate whether an investment or project is worth pursuing. It works by:

1. Estimating all the cash inflows (benefits) and outflows (costs) a project will generate over its lifetime.
2. "Discounting" each future cash flow back to today's dollars, using a discount rate that reflects the time value of money and the riskiness of the investment (a dollar received five years from now is worth less than a dollar today).
3. Summing all of these discounted cash flows, including the upfront investment (as a negative), into a single number.

- **NPV > 0** — the project is expected to generate more value than it costs, after accounting for the time value of money. It creates value.
- **NPV < 0** — the project is expected to cost more than it returns. It destroys value.
- **NPV = 0** — the project exactly breaks even.

NPV is widely preferred over simpler metrics (like payback period or ROI) because it accounts for both the timing and the magnitude of cash flows, and reflects risk through the discount rate.

## 3. What This Tool Is Used For

This tool is designed for **business case and project valuation work**, particularly for evaluating internal initiatives (e.g., technology transformations, process improvements, automation projects) where costs are upfront and benefits accrue over time. It is used to:

- Quantify whether a proposed project is financially justified.
- Capture both **hard financial benefits** (direct dollar savings/revenue) and **soft benefits** (productivity gains, reduced turnover, avoided downtime, etc.), and translate soft benefits into dollar terms.
- Model realistic adoption timelines using an **S-curve ramp-up**, rather than assuming benefits start at 100% on day one.
- Stress-test the result with **sensitivity analysis** (how much does NPV move if one assumption changes?) and **Monte Carlo simulation** (what's the realistic range of outcomes given uncertainty in several assumptions at once?).
- Produce a polished, client-ready output (charts, written summary, and exportable report) for use in presentations or business cases.

## 4. How to Use This Tool

1. **Open `index.html`** in a web browser. This is the landing page.
2. Click **"Find Your NPV Now!"** to go to the calculator (`control.html`).
3. Fill in the **Basic Info**: Project Name and Company Name.
4. Select which **outputs** you want generated: Overall Summary (always included), Cash Flow Diagram, Sensitivity Spider Chart, and/or Monte Carlo Distribution.
5. Enter the **core financial inputs**:
   - Initial Investment ($)
   - Annual Benefit ($)
   - Duration (years) and Implementation period (years)
   - Annual Expenses / Opex ($)
   - Discount Rate, Inflation Rate, Tax Rate, and Overhead Rate
6. (Optional) Expand **Optional Inputs** to:
   - Enable a **Project Adoption Ramp-Up** (S-curve) and set its speed.
   - Fill in one or more **Soft Benefit** categories (e.g., time saved, reduced turnover, avoided downtime) — the tool converts these into an annualized dollar value automatically.
7. Click the **calculate/run** button. The tool (running Python in the browser via Pyodide) will compute the cash flow schedule, base-case NPV, sensitivity analysis, and — if selected — a Monte Carlo simulation.
8. Review the results on-screen: the Overall Summary, and any charts you selected (Cash Flow Diagram, Sensitivity Spider, Monte Carlo Distribution). If a Sensitivity Spider Chart is shown, you can adjust which variables are displayed or highlighted and it will redraw.
9. Choose a download format (PDF, PNG, or CSV) and download your results for reporting or sharing.

**Note:** The very first calculation may take a few seconds to run, since the browser needs to load the Python environment (Pyodide) and its libraries (NumPy, pandas, Matplotlib) the first time the page is used.

## 5. Outputs

The tool can generate the following, matching whatever was selected in the checkboxes:

- **Overall Summary** *(always generated)* — a one-page visual summary covering project metadata, base-case NPV and verdict, top sensitivity drivers, and (if run) Monte Carlo probabilistic results.
- **Cash Flow Diagram** — a bar-and-line chart showing annual net cash flow and cumulative present value over the project's lifetime, with the final NPV annotated.
- **Sensitivity Spider Chart** — shows how NPV responds to a ±20% swing in each key input (investment, benefit, opex, duration, discount rate), making it easy to see which assumption the result is most sensitive to.
- **Monte Carlo Distribution** — runs 10,000 simulated scenarios with randomized inputs (within realistic uncertainty ranges) and shows the distribution of possible NPV outcomes, along with P10 / P50 / P90 values and the probability that NPV is positive.

These can be exported in three formats:

| Format | Contents |
|---|---|
| **PDF** | A formatted, multi-page report with a cover page, the Analysis Summary, and a dedicated page per selected chart — ready to share or present. |
| **PNG** | A ZIP file containing each selected chart as an individual high-resolution PNG image. |
| **CSV** | The full year-by-year cash flow table, plus (if run) the sensitivity analysis swing table and the Monte Carlo probabilistic summary — for further analysis in Excel or other tools. |

---

## For Editors

### Project Construction

This project is built entirely with **HTML, CSS, and JavaScript** on the front end, with **Python integrated directly into the browser via [Pyodide](https://pyodide.org/)** — there is no backend server or build process.

- **`index.html`** — the landing/splash page. Pure HTML/CSS; links to `control.html`.
- **`control.html`** — the full calculator application. This single file contains:
  - The **HTML structure** for all inputs (basic info, financial inputs, optional soft benefits, output selection, download controls).
  - **CSS** (embedded in a `<style>` block) controlling all layout, theming, and responsiveness.
  - **JavaScript** that handles UI interactivity (form state, dropdowns, checkboxes, dynamic soft-benefit rows) and orchestrates calls into Python.
  - A large embedded **Python codebase** (loaded as a string and executed inside Pyodide) that performs all the actual financial modeling: building the cash flow schedule, computing NPV, running sensitivity analysis, running the Monte Carlo simulation, and rendering all chart images (via Matplotlib, returned to JavaScript as base64-encoded PNGs) as well as building the downloadable PDF/PNG-ZIP/CSV files.
- **Pyodide** is loaded from a CDN (`pyodide.js`) and, on first run, loads the `numpy`, `pandas`, and `matplotlib` packages into the browser's Python environment.
- Because everything runs client-side, no data ever leaves the user's browser — there is no server component and no data storage.

### Key Python entry points (inside `control.html`)

- `run_from_js(params)` — main entry point called from JavaScript when the user runs a calculation. Builds the cash flow table, computes NPV, sensitivity analysis, and (optionally) Monte Carlo results, and returns chart images + summary data back to JS.
- `redraw_spider(display_vars, highlight_var)` — re-renders the Sensitivity Spider Chart when the user changes which variables to display/highlight, without re-running the full calculation.
- `prepare_download(filetype)` — assembles the final PDF, PNG ZIP, or CSV for download, using the most recently computed results.

### Editing Guidance

Because the visual design (layout, colors, typography, spacing) has been carefully tuned, any future edits — by a human or an AI assistant — should be **scoped strictly to functional/logic changes** unless a visual change is explicitly requested. Editors should always re-attach the current `control.html` (and `index.html` if relevant) before requesting changes, so that edits are applied to the actual current file rather than reconstructed from memory.

**Suggested AI prompt template for making changes:**

> This project is an NPV Estimation tool, built in HTML/CSS/JS + Python integrated via Pyodide. I have uploaded the necessary file(s) — please do not make any stylistic changes, only the specific changes mentioned below:
>
> [describe the specific functional change you want here]

### Things to Watch For When Editing

- The Python code lives inside a large JavaScript string (`ENGINE_CODE`) in `control.html` — indentation and escaping matter, since it's Python source embedded in a JS template literal.
- Chart colors and formatting are centralized in the `COLORS` dictionary and `plt.rcParams` block in the Python engine — change these in one place to re-theme all charts consistently.
- The PDF export uses a fixed 8.5"×11" page layout with its own header/footer/cover-page logic; changes to chart content will generally flow through automatically, but page-count logic (`total_pages`) assumes the current set of chart types.
- `index.html` and `control.html` must remain in the same folder as the logo image (`activera_consulting_logo.png`) for the branding to display correctly since it's referenced via a relative path.

---

<sub>Made by Guhan Gargya at Activera Consulting</sub>
