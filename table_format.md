---
layout: default
title: Table Format
---

# Ocarina — Table Formatting Module (Hedonic, Intensity, JAR)

## PURPOSE

Standardize table structure for:

* Hedonic scale questions
* Intensity scale questions
* JAR (Just About Right) questions

Ensures:

* Consistent outputs across studies
* Compatibility with sig-test rendering
* Clean downstream interpretation

---

# 1. GENERAL RULE

* Columns are **dynamic** → defined by user input
* Preserve column names exactly as provided
* All percentages are calculated **per column total (base)**
* Row order is **fixed and non-negotiable**
* **All numerical outputs must be displayed to exactly 2 decimal places** — no exceptions (see Section 1.1)

---

## 1.1 NUMERICAL PRECISION — MANDATORY

**All numerical values in output tables must be formatted to exactly 2 decimal places, without exception.**

This applies to:

| Value Type | Example |
|---|---|
| Percentage (TB, T2B, T3B, BB, B2B, B3B, Less, JAR, More) | 38.00% / 7.50% |
| Mean score | 5.32 / 4.80 |
| Standard Deviation | 1.24 / 0.87 |
| Sample Variance | 1.54 / 0.75 |
| RWA % Contribution | 39.90% / 8.30% |
| Any other derived metric | — |

**Rules:**
* Always apply `round(value, 2)` before rendering
* Never display raw integers for % metrics (e.g. `38` → must be `38.00`)
* Never display 1 decimal place (e.g. `5.3` → must be `5.30`)
* Base (n) is the ONLY exception — display as whole integer

---

# 2. HEDONIC & INTENSITY SCALE TABLE FORMAT

## 2.1 COLUMN STRUCTURE

| Metric | [Col1] | [Col2] | [Col3] | ... |

Columns must:

* Represent products / concepts / groups
* Be mapped dynamically (no hardcoding)

---

## 2.2 ROW STRUCTURE (STRICT ORDER)

1. Base (n)
2. TB (%)
3. T2B (%)
4. T3B (%)
5. BB (%)
6. B2B (%)
7. B3B (%)
8. Mean score
9. Standard Deviation
10. Sample Variance

---

## 2.3 DEFINITIONS

### TB (Top Box)

* 5-point: 5
* 6-point: 6
* 7-point: 7
* 9-point: 9

### T2B (Top 2 Box)

* 5-point: 4–5
* 6-point: 5–6
* 7-point: 6–7
* 9-point: 8–9

### T3B (Top 3 Box) — ONLY for 7 & 9 scale

* 7-point: 5–7
* 9-point: 7–9

### BB (Bottom Box)

* All scales: 1

### B2B (Bottom 2 Box)

* All scales: 1–2

### B3B (Bottom 3 Box) — ONLY for 7 & 9 scale

* 7-point: 1–3
* 9-point: 1–3

---

## 2.4 CONDITIONAL ROW LOGIC

* If scale = 5 or 6:

  * REMOVE T3B and B3B rows
* If scale = 7 or 9:

  * INCLUDE all rows

---

## 2.5 CALCULATION RULES

* Base = number of respondents per column
* % metrics = (count in category / base) * 100
* Mean = weighted average of scale
* Std Dev = standard deviation of responses
* Variance = square of std dev

---

# 3. JAR (JUST ABOUT RIGHT) TABLE FORMAT

## 3.1 SCALE

* ALWAYS 5-point scale

---

## 3.2 COLUMN STRUCTURE

| Metric | [Col1] | [Col2] | [Col3] | ... |

---

## 3.3 ROW STRUCTURE (STRICT ORDER)

1. Base (n)
2. Less (%) → Scale 1–2
3. JAR (%) → Scale 3
4. More (%) → Scale 4–5

---

## 3.4 CALCULATION RULES

* All % calculated from column base
* Categories must be mutually exclusive and exhaustive

---

# 4. SIG-TEST COMPATIBILITY

This module is designed to integrate with:

* sig_test_skill.md
* sig_test_rendering_module.md

Rules:

* % rows → use proportion test (Z-test)
* Mean rows → use mean test (T-test)
* Std Dev & Variance → DO NOT test

---

# 5. RENDERING RULES

* Maintain strict row order
* Do NOT rearrange rows based on values
* Do NOT drop rows unless dictated by scale rules
* Ensure clean alignment for sig-test overlay
* **Apply 2 decimal places to ALL numerical outputs before rendering** (see Section 1.1)
* Base (n) is exempt — render as whole integer

---

# 6. COMMON FAILURE TO AVOID

* Mixing row order
* Incorrect scale mapping (e.g., wrong T2B range)
* Calculating % from total sample instead of column base
* Including T3B/B3B for 5-point scale
* Running sig-test on variance or std dev
* **Displaying % or numeric values without 2 decimal places** (e.g. `38%` or `5.3` instead of `38.00%` or `5.30`)

---

# 7. ROLE IN SYSTEM

This module defines:

"How data is structured before analysis"

It feeds into:

* Significance testing
* RWA analysis
* Penalty analysis (JAR)

Without this module:
→ Data becomes inconsistent and non-testable

With this module:
→ Data is analysis-ready and standardized

---

# END OF MODULE
