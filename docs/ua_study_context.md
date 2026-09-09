---
layout: default
title: U&A Study Context
---

# Ocarina — U&A Study Module

## PURPOSE

This module defines the analytical framework, metric logic, and interpretation rules for **Usage & Attitudes (U&A)** studies across any FMCG or consumer category.

U&A studies are inherently open in structure — sections and variables differ by category and client brief. This module provides generalized logic flexible enough to apply to any U&A design.

This module extends:
- study_context.md
- sig_test_rendering_module.md
- table_format.md
- rwa_rendering_module.md

---

# 1. STUDY TYPE OVERVIEW

## Objective
Understand **who** the consumer is, **what** they buy and use, **how** they behave, and **why** they make decisions — at category and brand level.

## Typical Section Structure

U&A studies commonly contain some or all of the following sections:

| Section | Core Purpose |
|---|---|
| Screener / Demographics | Who is the respondent |
| Category & Brand Usage | What brands are known / used |
| Purchase Behavior | How and where they buy |
| Usage Behavior | How they actually use the product |
| Brand Image & Importance | How they perceive the category and brands |
| Packaging / Concept Evaluation | What they think of specific stimuli |

Not all sections appear in every study. Ocarina operates in MANUAL MODE — only analyze sections explicitly requested.

---

# 2. DEMOGRAPHICS & SAMPLE PROFILING

## Standard Metrics
- Age: group into meaningful brackets (e.g. 18–25, 26–30, 31–35, 36–40, 40+) + mean age
- Domicile / Area: % of total base
- SES: % of total base — always flag if distribution is upper-skewed or incomplete

## SES Calculation
Indonesian studies typically use composite scoring:
- S4 (household expenditure) + S5 (water source) + S6 (cooking fuel) = Total SES score
- Map total score to SES tier (A through E)

## Output Format
- Tables: % of total base + n
- Flag any skew in sample composition that may limit generalizability

## Interpretation Rules
- Report composition factually — do not editorialize
- Flag if SES distribution is incomplete (e.g. only A/B/C1 present) as this limits representativeness
- Age bimodality (e.g. cluster at youth + mature) is worth noting as it may imply two distinct segments

---

# 3. CATEGORY & BRAND USAGE

## Standard Variables
- **Awareness (TOM / Aided)**: which brands does respondent know
- **Usage (past 1 month / 3 months)**: which brands used recently — MA
- **Most Often Used (MOU)**: single brand — primary brand driver analysis

## Output Format
- Awareness & Usage: % of total base (MA)
- MOU: % of total base, sorted descending — render as horizontal bar chart

## Base Rules
- Awareness = total base (n)
- Usage = total base (n)
- MOU = total base (n), SA

## Interpretation Rules
- MOU is the most important brand metric in U&A — it defines the competitive landscape
- Gap between Awareness and Usage signals conversion friction
- Gap between Usage and MOU signals loyalty vs. repertoire behavior

---

# 4. PURCHASE BEHAVIOR

## Standard Variables

### Purchase Frequency (SA)
- Report: % of total base per frequency option
- Always compute **estimated monthly frequency** using weighted scoring:
  - Assign estimated times/month per code (e.g. daily=30, weekly=4, monthly=1)
  - Mean = weighted average across sample
- Flag: mean is driven by outliers if few heavy buyers inflate the score

### Purchase Volume (Integer / Open-ended)
- Report: mean units per purchase occasion
- Optional: distribution of 1/2/3+ units

### Purchase Channel (SA)
- Report: % of total base, sorted descending
- Key channels to watch: Modern trade (minimarket/supermarket) vs. traditional trade (warung) vs. e-commerce

### Consideration Factors (MA + Ranking)

**MA (P4-type):**
- Report: % of total base, sorted descending
- Use as primary metric for factor importance — directly reflects what consumers consider

**Ranking (P5-type):**
- Preferred method: **Rank-Ordered Logit (Exploded Logit)**
- Output: utility % per factor, normalized to 100%, sorted descending
- Always report McFadden Pseudo R²
- Tiering: High / Mid / Low using natural break method (default: equal share ± 1pp as anchor)

### Pseudo R² Interpretation Rules
| R² Range | Interpretation |
|---|---|
| < 0.05 | Very weak — data likely random (common in dummy data) or high heterogeneity |
| 0.05–0.15 | Weak but present — use with caution |
| 0.15–0.30 | Moderate — results are meaningful |
| 0.30+ | Strong consensus |

**Critical rule:** Never inflate R² by removing "outliers" to hit a target. Removing respondents who disagree with the majority is data fabrication, not cleaning. Low R² from ranking data is a valid finding — it means consumer preferences are heterogeneous.

**When R² is low:** Rely on MA (consideration factors) as primary importance signal. Report ranking results as directional only.

### Conditional / Follow-up Questions
- Always report on correct sub-base (e.g. P6 only among those who selected "Packaging" at P4)
- State sub-base n clearly in table header

---

# 5. USAGE BEHAVIOR

## Standard Variables
- Usage frequency: % of total base, tabulated
- Usage occasion / context: if present, MA table
- Usage amount per occasion: mean if numeric

## Interpretation Rules
- Heavy users (daily or more) signal high category engagement → higher expectations on product and packaging
- Infrequent users signal low involvement → packaging needs stronger shelf impact to trigger purchase

---

# 6. BRAND IMAGE & IMPORTANCE

## Two Standard Components

### 6.1 Importance (BI1-type) — Rating Scale
**What it measures:** How important each image attribute is to the consumer when choosing a brand in this category.

**Typical scale:** 5-point (1=Not important, 5=Very important)

**Recommended method:** Mean score normalized to 100% as importance weight

**Calculation:**
1. Compute mean score per attribute
2. Sum all means
3. Divide each mean by total sum × 100 = normalized importance %

**Output:**
- Horizontal bar chart, sorted descending
- Show: Normalized %, Mean score, TB%, T2B% as supporting metrics
- Apply High / Mid / Low tiering using natural break

**Tiering logic (default):**
- Equal share = 100% ÷ number of attributes
- High: ≥ equal share + meaningful gap above
- Mid: within ~1pp of equal share
- Low: clearly below equal share
- Apply natural break method — adjust thresholds to gaps in the data, not fixed formulas

**Key caveat:** Rating scale allows respondents to rate all attributes highly — normalization shows relative importance, not absolute. For absolute importance, MaxDiff or conjoint is more defensible.

**Reverse-coded attributes:**
- Always flag attributes that are negative (e.g. "terlihat kuno/lawas")
- Interpret separately — low importance score on a negative attribute is expected and correct
- Never compare directly with positive attributes

### 6.2 Brand Association (BI2-type) — MA per Attribute
**What it measures:** Which brands consumers associate with each image attribute.

**Base rule:** % of **aware base per brand** (not total base)
- Each brand column has its own base = n respondents aware of that brand
- Never use total base as denominator for BI2

**Output:** Cross-tabulation table
- Rows = image attributes
- Columns = brands (sorted by aware base, descending)
- Focal brand should be visually highlighted

**Significance testing:**
- Run proportion z-test between brands per attribute
- Markers placed in higher column referencing lower column
- Follow sig_test_rendering_module.md rules

**Interpretation:**
- Identify attributes where focal brand over/under-indexes vs. competitors
- Flag reverse-coded attributes separately
- Always caveat that dummy data produces artificially flat distributions

---

# 7. PACKAGING / STIMULUS EVALUATION

## Quantitative Elements (Closed-ended)
- Closed-ended ratings: use table format per table_format.md
- MA selections (e.g. which packaging elements attract): % of relevant base

## Qualitative Elements (Open-ended)
**Standard approach: Thematic Coding**

### Step 1 — Read all verbatims
Pull all responses. Do not summarize before reading.

### Step 2 — Code into themes
Group responses by:
- **Packaging element** (preferred for PE3/PE4-type questions): color, shape, typography, imagery, label/information, closure mechanism, overall design
- **Thematic concept** (preferred for PE1/PE2-type ideal-world questions): ergonomics, aesthetics, functionality, storage, premium cues, etc.

### Step 3 — Count mentions
- Metric: **% of total base** (not raw count)
- One respondent = one dominant theme per question
- If multi-theme coding is needed, note that total will exceed 100%

### Step 4 — Render
- **PE1/PE2-type (ideal):** Horizontal card layout, left = most mentioned, right = least mentioned
- **PE3/PE4-type (reaction to stimulus):** Grouped by packaging element, separate liked vs. disliked sections
- Include verbatim quotes (2–3 per theme) for credibility
- Include sub-count breakdown if theme aggregates multiple distinct verbatims

### Interpretation Rules
- Look for tensions: same element appearing in both liked and disliked = nuanced signal (e.g. color liked for visibility but disliked for premium feel)
- Look for gaps: elements heavily mentioned in disliked but absent in liked = clear improvement area
- Cluster related disliked themes: e.g. "outdated design" + "old-fashioned font" + "looks dated" = one modernity problem
- Do not over-interpret single-digit % themes — flag as directional only

---

# 8. CROSS-SECTION INTERPRETATION

After individual sections are complete, Ocarina should check:

### Consistency Check
- Does purchase frequency align with usage frequency? Misalignment may indicate stockpiling or multi-product households.
- Do stated consideration factors (MA) align with brand image importance (BI1)? Inconsistency = research artifact or genuine tension.

### Strategic Tensions
Common patterns to flag:

| Pattern | Implication |
|---|---|
| High awareness, low MOU share | Conversion problem — brand known but not chosen |
| High importance attribute, low brand association | Opportunity if brand can own that space |
| Attribute in both liked (PE3) and disliked (PE4) | Nuanced finding — do not simplify |
| Low R² on ranking | Consumer heterogeneity — segment-level analysis may be needed |
| SES skew in sample | Findings may not represent full target market |

### Comment / Insight Validation
Before finalizing any narrative insight, Ocarina should verify:
1. Is the claimed driver actually in the top 3 of the relevant metric?
2. Is the label accurate? (e.g. "fragrance variant" ≠ "product variant")
3. Is the direction correct? (e.g. "less important" vs. "lower priority but still relevant")
4. Does it hold across both the MA and ranking data, or only one?

If a comment is not fully supported, Ocarina should flag the discrepancy and suggest a corrected version rather than silently accepting the claim.

---

# 9. OUTPUT STANDARDS

| Metric type | Primary format | Supporting format |
|---|---|---|
| Single-answer categorical | Table (% of base + n) | Bar chart if many categories |
| Multi-answer | Table (% of base, MA noted) | — |
| Numeric / integer | Mean score | Distribution table |
| Rating scale importance | Normalized % bar chart + tier | Mean, TB%, T2B% |
| Brand association | Cross-tab table | — |
| Ranking | Rank-ordered logit bar chart | MA table as validation |
| Open-ended qualitative | Thematic card layout | Verbatim quotes |

---

# 10. NON-NEGOTIABLE RULES

- NEVER fabricate data or remove respondents to inflate model fit
- NEVER conflate MA % with ranking importance — they measure different things
- NEVER use total base as denominator for brand-specific metrics (BI2)
- ALWAYS flag reverse-coded attributes
- ALWAYS state sub-base when question is conditional
- ALWAYS validate narrative comments against actual data before accepting
- ALWAYS caveat dummy data outputs — distributions will be artificially flat

---

# END OF MODULE
