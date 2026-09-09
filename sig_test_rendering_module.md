# Ocarina — Significance Test Rendering Module

## PURPOSE

This module standardizes how significance test results are displayed in tables.

It ensures:

* Consistent formatting
* Clear interpretation
* Client-ready outputs

This module works together with:

* study_context.md (analysis logic)
* sig_test_skill.md (calculation engine)

---

# 1. TABLE STRUCTURE

Column labels are **dynamic and must adapt to input data**.

Do NOT hardcode labels like RAN00–RAN04.

Instead:

* Use column names exactly as provided in the dataset
* Assign letter indices dynamically (a, b, c, d, …) based on column order

---

## 1.1 DYNAMIC COLUMN MAPPING

Example:

| Metric | Product A | Product B | Concept X | Concept Y |
| ------ | --------- | --------- | --------- | --------- |

Then mapping becomes:

* Product A → (a)
* Product B → (b)
* Concept X → (c)
* Concept Y → (d)

Rendered header:

| Metric | Product A (a) | Product B (b) | Concept X (c) | Concept Y (d) |

---

## 1.2 RULE

* Column letters must ALWAYS follow left-to-right order
* Letters are ONLY used for significance markers
* Display names must remain unchanged (no renaming)

--------|----------|----------|----------|----------|----------|

---

# 2. CELL FORMAT

Each cell must follow a two-line structure:

Line 1 → Value
Line 2 → Significance markers (if any)

---

## 2.1 MEAN ROWS

Example:

5.32
B D

---

## 2.2 PERCENTAGE ROWS

Example:

19%
B

---

# 3. MARKER RULE (CRITICAL)

* Marker letter represents the LOWER column
* Marker is ALWAYS placed in the HIGHER column
* A column must NEVER contain its own letter

---

## EXAMPLE

RAN01 = 8%
RAN02 = 19%

→ RAN02 is higher
→ Marker = B
→ Placed in RAN02

Correct:

RAN01: 8%
RAN02: 19%
B

---

# 4. MULTIPLE SIGNIFICANCE

If one column is higher than multiple others:

30%
B D

Meaning:

* Higher than RAN01 (b)
* Higher than RAN03 (d)

---

# 5. SIGNIFICANCE LEVELS

| Level | Marker        |
| ----- | ------------- |
| 90%   | lowercase (b) |
| 95%   | UPPERCASE (B) |
| 99%   | B+            |
| 99.9% | B++           |

---

# 6. TABLE BLOCK STRUCTURE

Each attribute must be displayed in blocks:

Example:

Smoothness on throat

|            | a    | b    | c    | d    | e    |
| ---------- | ---- | ---- | ---- | ---- | ---- |
| Mean score | 5.07 | 4.87 | 5.28 | 4.83 | 5.28 |
|            | —    | —    | B D  | —    | B D  |
| TB%        | 13%  | 8%   | 18%  | 6%   | 14%  |
|            | —    | —    | B D  | —    | —    |

---

# 7. RENDERING EXCLUSIONS (MANDATORY)

The following rows are NEVER rendered in sig-test output tables:

* **Std Dev**
* **Sample Variance**

These rows are skipped entirely — no value row, no marker row.

Rows to render (in order):

* Base (n)
* TB%
* T2B%
* T3B% *(7-pt and 9-pt scale only)*
* BB%
* B2B%
* B3B% *(7-pt and 9-pt scale only)*
* Mean score

---

# 8. GROUPING STRUCTURE

Tables must be grouped into:

## 8.1 KEY MEASURES

* Overall Liking
* Purchase Intention

## 8.2 DIAGNOSTICS — HEDONIC

* Aroma
* Smoothness
* Taste

## 8.3 DIAGNOSTICS — INTENSITY

* Sweetness
* Bitterness
* Cooling

---

# 9. WORKFLOW INTEGRATION

Ocarina must follow this sequence:

1. Receive user input (data + test type)
2. Run significance test using sig_test_skill.md
3. Store results in structured markers dictionary
4. Print verification log (mandatory)
5. Render table using THIS format (never manual typing)

---

# 10. INTERPRETATION LAYER (MANDATORY)

After rendering table, always extract:

## DRIVERS

Attributes that are:

* High score
* Significantly higher than others

## BARRIERS

Attributes that are:

* Low score
* Significantly lower

## TENSIONS

* High absolute score but no significance
* Indicates weak differentiation

## OPPORTUNITIES

* Close to leaders but not yet significant

---

# 11. NON-NEGOTIABLE RULES

* NEVER place marker in lower column
* NEVER inline markers (e.g. "19% (B)")
* ALWAYS use two-line cell format
* ALWAYS render from computed structure (no manual edits)
* ALWAYS include verification log before table
* NEVER render Std Dev or Sample Variance rows in sig-test tables

---

# 12. ROLE IN SYSTEM

This module bridges:

Calculation → Visualization → Interpretation

Without this module:

* Output is technically correct but hard to read

With this module:

* Output becomes decision-ready

---

# END OF MODULE
