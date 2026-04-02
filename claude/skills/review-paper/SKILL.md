---
name: review-paper
description: Review a research paper and extract structured notes relevant to this project. Use when asked to review, summarize, or analyze an academic paper.
---

# Review Paper Skill

Review a research paper and extract structured notes relevant to this project.

## Arguments
- First argument: path to the paper file (`.tex`, `.md`, or `.pdf`)
- Optional second argument: specific questions or claims to focus on

## Steps

1. **Read project context first:**
   - Read `docs/knowledge/INDEX.md` (if it exists) for the knowledge repo structure
   - Read the project's config file (e.g., `config.R`, `code/config.do`) for project context
   - Read relevant topic files as needed (e.g., methodology decisions, identification strategy) for current methodology and results
   - **Note the project's geographic focus, time period, data types, and methodology** — you will need these in Step 3b to extract project-relevant findings
   - This context informs your understanding but does NOT go into the paper notes

2. **Read the paper — handle file type correctly:**
   - `.tex` files: Read as plain text using the Read tool. NEVER treat as PDF.
   - `.md` files: Read as plain text using the Read tool.
   - `.pdf` files under 10 pages: Read directly with the `pages` parameter.
   - `.pdf` files over 10 pages: Run `pdftotext <file> /tmp/paper_text.txt` first, then read the text file in sections (200 lines at a time). If pdftotext is not available, read the PDF in page ranges (pages 1-10, then 11-20, etc.).
   - If the file is too large to process, stop and ask the user for relevant page numbers or excerpts.

3. **Extract structured notes (PURE SUMMARY ONLY):**

   **3a. General notes** with these sections:
   - **Citation**: Authors, title, year, journal
   - **Research Question**: What the paper investigates
   - **Identification Strategy**: How they establish causality
   - **Key Assumptions**: What must hold for identification to work
   - **Key Variables**: Variable names, definitions, construction
   - **Data Sources**: Datasets, sample, time period
   - **Main Findings**: Core results — see Numerical Fidelity and Tables rules below
   - **Methodological Contributions**: New estimators, diagnostics, or frameworks introduced
   - Do NOT include any project-specific commentary in the paper notes

   **3b. Project-relevant findings:**
   - From the project context identified in Step 1, identify the project's geographic focus, time period, data sources, or methodology
   - Search the paper for ALL mentions, data, and findings specific to that focus — including appendix tables, supplementary tables, methodology exceptions, and categorizations
   - Add a dedicated section: **"Findings for [Geography/Topic]"** (e.g., "Findings for Philadelphia")
   - This section is still a PURE SUMMARY — it extracts what the paper says about the project's focus area, not project-specific commentary
   - Include: city/topic-specific results tables, methodology exceptions, categorization/grouping of the focus area, benchmarking data

4. **Write notes to `docs/knowledge/papers/<paper_name>.md`:**
   - Derive the filename from the paper (e.g., `lyons_et_al_2025.md`, `nguyen_2019.md`)
   - One file per paper
   - If the file already exists, ask the user whether to overwrite or append

5. **Present the notes and ask about project implications:**
   - Show the structured notes
   - Ask which claims, methods, or sections the user wants to examine more closely
   - Ask which knowledge repo topic files should be updated with project-specific implications (e.g., `identification.md`, `methods.md`, `interpretation.md`)
   - Update those topic files with the implications — this is where project-specific commentary goes

## Rules

### File handling
- NEVER attempt to read a .tex file as a PDF. This is a text file. Use the Read tool.
- Read the entire paper! No exceptions.

### Numerical fidelity
- **Exact numbers only.** Never round, approximate, or paraphrase numerical claims. If the paper says "almost three quarters", write "almost three quarters." If it says "73%", write "73%." If it says "3.16%", do not write "3%."
- **Page references required.** Every factual claim with a specific number must include a page reference: e.g., "real rents rose 60% from 1890 to 2006 (p. 15)."
- **Direct quotes for key claims.** When the paper states a finding with a specific magnitude, quote the exact sentence and cite the page. Use blockquote format:
  > "quote from paper" (p. X)
- **No paraphrasing of numbers.** The review must not introduce any variation or error when translating the paper to markdown. If you are uncertain of a number, re-read the relevant section before writing it down.

### Tables over prose
- **Use comparison tables** to present quantitative results, not prose descriptions. Tables preserve exact numbers and make comparisons scannable.
- Build tables for: period-by-period results, method comparisons, cross-group comparisons, or any set of parallel numerical findings the paper reports.

### Depth
- **Depth over brevity.** The review should be comprehensive enough that a reader does not need to return to the paper for any factual claim. Cover every section of the paper, not just highlights.
- **Every empirical finding needs its magnitude.** Growth rates, coefficients, sample sizes, R-squared, confidence intervals — include all reported numbers.
- **Period breakdowns.** When the paper reports results by time period, reproduce the full breakdown, not just the overall figure.

### Separation of concerns
- **Paper notes are PURE SUMMARIES.** No project-specific commentary, no "implications for our results," no "how this relates to our analysis." Project implications go in the relevant knowledge repo topic file (e.g., `docs/knowledge/identification.md`, `docs/knowledge/methods.md`).
- The "Findings for [Geography/Topic]" section (Step 3b) is still a pure summary — it extracts what the paper says, filtered to the project's focus area.
- Ask before assuming which aspects of the paper matter most.
- If context is getting heavy, prioritize the sections most relevant to the project.
