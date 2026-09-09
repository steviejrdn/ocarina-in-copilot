# Ocarina — RWA Chart Rendering Module

## PURPOSE

This module standardizes how RWA driver analysis results are rendered as charts.

**Output format: HTML rendered in-chat via `show_widget`. No PNG, no separate file.**

It governs:
- Chart type, orientation, and sorting
- Color coding by tier
- Label format and positioning
- R² display placement
- HTML/CSS implementation spec
This module extends:
- `rwa_rendering_module.md` (table structure and interpretation rules)
- `rwa-skill/SKILL.md` (execution workflow and data source)
---

# 1. CHART TYPE

**Horizontal bar chart — mandatory.**

No substitutions. Vertical bars, pie charts, or bubble charts are not permitted
for RWA output regardless of user preference, unless explicitly overridden.

---

# 2. SORTING

- Always sort **descending by Importance score** (top driver at top of chart)
- Never preserve input order
- Never sort alphabetically
---

# 3. COLOR CODING BY TIER

Colors are assigned per attribute based on Importance tier, not rank position.

| Tier | Importance Range | Hex Color |
|---|---|---|
| Highest | ≥ 80 | `#E82020` (Red) |
| High | ≥ 60 AND < 80 | `#F5A623` (Orange) |
| Mid | ≥ 40 AND < 60 | `#F5E642` (Yellow) |
| Low | ≥ 20 AND < 40 | `#42D4F5` (Cyan) |
| Lowest | < 20 | `#42F55A` (Green) |

These colors are fixed. Do not substitute based on aesthetic preference.

---

# 4. LABEL FORMAT

### Bar label (at end of bar)
- Value: Importance score as **whole integer** (e.g. `100`, `92`, `63`)
- Position: Right of bar end
- Font: Bold, color `#1A1A6E` (dark navy)
### Y-axis labels (attribute names)
- Use original attribute names exactly as provided in data
- No truncation, no relabeling
- Text-align: right, fixed width column
### No axis lines or gridlines
- Clean layout — bars and labels only
---

# 5. CHART TITLE

Format:
```
Driver Analysis — Overall Liking
{Product Name}   |   R² = XX.X%
```

- Line 1: Fixed header
- Line 2: Product name + R² (1 decimal place)
- Font: Bold, color `#1A1A6E`
- Displayed above the chart
If running pooled (all products combined), replace product name with `All Products`.

---

# 6. LEGEND

Position: Below chart or bottom-right of chart area.

Always show all 5 tiers regardless of which tiers appear in data:
- 🟥 Highest
- 🟧 High
- 🟨 Mid
- 🟦 Low
- 🟩 Lowest
---

# 7. BAR WIDTH SCALING

- Max bar width maps to Importance = 100 → 100% of bar container
- All other bars scale proportionally: `width = {importance}%`
- Container max-width: 700px, full width responsive
---

# 8. FIGURE HEIGHT

Scale with number of attributes:
```
row_height = 44px per attribute
min total height = 200px
```

---

# 9. HTML IMPLEMENTATION (CANONICAL)

Use this template when calling `show_widget`. Inject data from the JSON output of the
Python script (`__CHART_DATA_START__` ... `__CHART_DATA_END__`).

```html
<div style="font-family: sans-serif; max-width: 700px; padding: 16px;">

  <!-- Title -->
  <div style="color:#1A1A6E; font-weight:bold; font-size:14px; margin-bottom:4px;">
    Driver Analysis — Overall Liking
  </div>
  <div style="color:#1A1A6E; font-size:12px; margin-bottom:16px;">
    {PRODUCT_NAME} &nbsp;|&nbsp; R² = {R2}%
  </div>

  <!-- Bars -->
  {ROWS}

  <!-- Legend -->
  <div style="display:flex; gap:16px; margin-top:16px; flex-wrap:wrap; font-size:11px;">
    <span><span style="display:inline-block;width:12px;height:12px;background:#E82020;margin-right:4px;"></span>Highest</span>
    <span><span style="display:inline-block;width:12px;height:12px;background:#F5A623;margin-right:4px;"></span>High</span>
    <span><span style="display:inline-block;width:12px;height:12px;background:#F5E642;margin-right:4px;"></span>Mid</span>
    <span><span style="display:inline-block;width:12px;height:12px;background:#42D4F5;margin-right:4px;"></span>Low</span>
    <span><span style="display:inline-block;width:12px;height:12px;background:#42F55A;margin-right:4px;"></span>Lowest</span>
  </div>

</div>
```

### Row template (repeat per attribute, sorted descending):

```html
<div style="display:flex; align-items:center; margin-bottom:8px;">
  <!-- Attribute label -->
  <div style="width:180px; text-align:right; padding-right:10px;
              font-size:12px; font-weight:bold; color:#1A1A6E; flex-shrink:0;">
    {ATTRIBUTE}
  </div>
  <!-- Bar container -->
  <div style="flex:1; background:#f0f0f0; border-radius:3px; height:26px; position:relative;">
    <div style="width:{IMPORTANCE}%; background:{COLOR}; height:100%; border-radius:3px;"></div>
  </div>
  <!-- Value label -->
  <div style="width:40px; text-align:right; padding-left:8px;
              font-size:12px; font-weight:bold; color:#1A1A6E;">
    {IMPORTANCE}
  </div>
</div>
```

---

# 10. RENDERING WORKFLOW

1. Script runs (Step 2 of SKILL.md) → outputs JSON
2. Extract JSON from `__CHART_DATA_START__` ... `__CHART_DATA_END__`
3. For each product in JSON array:
   - Build HTML string using template above
   - Inject: product name, R², attribute rows (sorted descending), colors per tier
   - Call `show_widget` with the HTML
4. One `show_widget` call per product — never batch multiple products into one widget
---

# 11. RENDERING EXCLUSIONS

The following are NEVER shown in the chart:
- Raw weights (internal calculation only)
- Standard deviation, variance
- Sample size (n)
- % Contribution (shown in terminal table only, not chart)
---

# 12. MULTI-PRODUCT WORKFLOW

When dataset contains multiple products:

1. Loop through each product in JSON array
2. Render one HTML chart per product via `show_widget`
3. Add interpretation layer after each chart
4. Do NOT combine multiple products into a single chart unless explicitly requested
---

# 13. TEXT BAR FALLBACK

Only if `show_widget` is unavailable:

```
Moisturizing Feel  ████████████████████  100   Highest
Foam Richness      ███████████████████    97   Highest
Scalp Feel         ██████████████████     92   Highest
Ease of Rinse      ██████████████         72   High
Hair Smoothness    █████████████          70   High
...
R² = 11.61%
```

This is a last resort only. Always attempt `show_widget` first.

---

# 14. NON-NEGOTIABLE RULES

- ALWAYS color by tier, never by rank position
- ALWAYS show all 5 tier colors in legend, even if some tiers are absent from data
- ALWAYS display Importance as whole integer in bar labels
- NEVER show decimal in bar label (e.g. `92` not `91.8`)
- NEVER use PNG output — always render in-chat HTML
- NEVER sort ascending or alphabetically
- ALWAYS include R² in chart title
---

# END OF MODULE
