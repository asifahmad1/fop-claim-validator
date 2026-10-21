# Front-of-Pack (FoP) Rating & Claim Validator v1.0 — Test Report

**What it is:** A single-file browser app for the Asif SaaS Studio catalogue that screens a product's nutrient profile (per 100 g or 100 ml) three ways: (a) which nutrition claims it may make under **Codex CAC/GL 23-1997** — with the **EU protein variation** from Regulation (EC) No 1924/2006 — showing PASS/FAIL for every claim with the product's value against the threshold; (b) a predicted **Nutri-Score** under the **2023 updated algorithm** (general foods and beverages), with a full points breakdown; and (c) a predicted **Health Star Rating** (AU/NZ) in any of the **six official categories**, also with a points breakdown. Caution flags warn when a passing product is high in sodium, sugars, saturated fat or energy. A batch mode screens a pasted/uploaded CSV of products at once. Target users: food formulators, QA/regulatory staff, food science students. By Dr. Asif Ahmad.

## Features

1. **Product & settings** — name; Solid (per 100 g) / Beverage (per 100 ml) toggle; optional serving size (shown in the summary); FVNL (fruit/vegetables/nuts/legumes) % with a "concentrated" toggle for the HSR V-points column; red-meat toggle (Nutri-Score 2023 protein cap of 2 points); "contains added sugars" checkbox (drives the No Added Sugars claim); non-nutritive-sweetener checkbox (Nutri-Score beverages +4 N points); plain-water checkbox (Nutri-Score class A by rule); jurisdiction selector Codex / EU (re-bases the two protein claims); HSR category selector with all six official categories.
2. **Nutrient profile** — energy in kcal **and** kJ (linked fields, 1 kcal = 4.184 kJ), protein, fibre, total sugars, total fat, saturated fat, sodium mg **and** salt g (linked fields, sodium mg = salt g × 400), calcium, iron, vitamin C, vitamin D. Live validation: negatives, >100 g nutrients, FVNL outside 0–100, and saturated fat exceeding total fat all raise inline messages.
3. **Claim engine** — evaluates **24 claims** on every keystroke: low energy; energy-free (n/a for solids); low fat; fat-free; low saturated fat (**including the ≤10%-of-energy sub-rule**); saturated-fat-free; low sugars; sugars-free; no added sugars (with the "contains naturally occurring sugars" statement note when natural sugars are present); low / very low / sodium-free; source of & high protein (Codex per-100 g, per-100 ml **and per-100-kcal routes**, the route used is named; EU basis = ≥12% / ≥20% of energy from protein); source of & high fibre (per-100 g / per-100 ml / per-100-kcal routes); source of & high calcium, iron, vitamin C, vitamin D at 15% / 30% of NRV per 100 g (7.5% / 15% per 100 ml). Every FAIL shows how far off the product is (e.g. "needs to fall by 1.20 g").
4. **Caution flags** — if any claim passes and the profile is high in sodium (≥500 mg), sugars (≥15 g/100 g, solids), saturated fat (≥5 g) or energy (≥400 kcal), an amber caution states that many markets restrict claims on such products. Worded as cautions, not Codex disqualifications.
5. **Nutri-Score 2023** — A–E chip row with the earned grade highlighted; final score with N and P totals; expandable auditable table (each component's value → points) plus the rule applied (protein exclusion at N ≥ 11; red-meat cap; beverage computation; water rule).
6. **Health Star Rating** — star display (★ characters, halves shown), final score and baseline; expandable table of baseline points (energy, saturated fat, sugars, sodium), V/P/F modifying points, the protein-counting rule outcome, and the category applied.
7. **Batch screening** — paste CSV or upload a file; tolerant parser (comma/semicolon/tab, optional header matched by column name, positional fallback, bad lines reported by line number, never silently dropped). Results table: claims passed count, Nutri-Score grade, HSR stars per product (HSR category defaults: beverages → 1, solids → 2). Clicking a row loads that product into the main form.
8. **Outputs** — Copy summary (clipboard with manual fallback) and Download CSV of the full evaluation (ratings, every claim with status/value/requirement, cautions).

## Sources & table versions (every threshold used)

| Engine | Source | Version / detail |
|---|---|---|
| Claims | Codex Alimentarius **CAC/GL 23-1997**, *Guidelines for Use of Nutrition and Health Claims*, Table of conditions for nutrient content claims | Conditions exactly as specified for low/free/source/high claims; NRVs used: Ca 800 mg, Fe 14 mg, vitamin C 100 mg, vitamin D 5 µg, protein 50 g |
| EU protein claims | Regulation (EC) **No 1924/2006**, Annex | "Source of protein" ≥12% of energy; "High protein" ≥20% of energy |
| Nutri-Score foods | Santé publique France, *Nutri-Score Questions & Answers*, version **10 July 2023**, Tables 5 (N) & 6 (P); Scientific Committee update report **2022**, §1.4 | N: energy kJ thresholds 335…3350 (10 pts), sugars 3.4…51 (15 pts), saturates 1…10, salt 0.2…4.0 g (20 pts). P: protein 2.4…17 (7 pts), fibre 3.0…7.4 (5 pts), FVL ≤40/>40/>60/>80 = 0/1/2/5. Bands: A ≤0, B 1–2, C 3–10, D 11–18, E ≥19 |
| Nutri-Score computation rule | SPF Q&A (July 2023), "Calculation of the nutritional score — general case" | N < 11 → score = N − P; **N ≥ 11 → protein points not counted** (score = N − fibre − FVL points). Note: the 2017 FVL-points condition on this rule was dropped in the 2023 algorithm — the app follows the 2023 text and says so |
| Nutri-Score beverages | SPF Q&A Tables 9 (N) & 10 (P); Scientific Committee beverages update report, **31 Jan 2023**, §6.7.3 | Energy 30…390 kJ (non-linear), sugars 0.5/2/3.5/5/6/7/8/9/10/11 g, saturates 1…10 g, salt as foods, non-nutritive sweeteners present = +4; protein 1.2…3.0 g (7 pts, linear), fibre as foods, FVL max **6** points (0/2/4/6); score = N − P (protein cap removed); bands: water = A, B ≤2, C 3–6, D 7–9, E ≥10 |
| Health Star Rating | *Guide for industry to the Health Star Rating Calculator* (healthstarrating.gov.au), tables as in the official **Summarised NPSC and HSR Scoring Tables** (2015 update) | Baseline Table 1 (Cats 1, 1D, 2, 2D) and Table 2 (Cats 3, 3D — sugars capped at 10 pts, sodium extended by 90 mg steps); FVNL Table 3 (both columns); protein/fibre Table 4; final-score → stars Table 5 for **all six categories** |

## Honest scope

Predictions are guidance computed from the published tables named above — national implementations vary (e.g. some markets add a trans-fat condition to saturated-fat-free, and per-serving claim routes exist in Codex that this tool does not use beyond the summary display), and **final claim legality needs regulatory review**. The app takes a nutrient profile as input; it does **not** compute a profile from a recipe — ingredient-level computation is a staged v1.1 item (the companion LP Recipe & Cost Optimizer covers formulation). Beverage/solid and HSR-category classification are the user's. Nutri-Score's special **cheese** rule and the **added fats / nuts & seeds** category are **not** implemented (general foods + beverages only) — classifying cheese or oils with the general tables will mis-grade them, and the in-app scope note says so. Two official automatic ratings are handled by rule rather than by the tables: plain water (Nutri-Score A; HSR 5 stars in its own right) — the app covers the Nutri-Score side with the water checkbox; the HSR user should select Category 1 and note fresh/minimally processed produce is likewise auto-5-star in the official system. HSR note: in Table 4 the protein boundary at 3.2 g is printed "≥3.2" in the summarised table while all other steps use ">" — the app uses ">" uniformly, so a product at exactly 3.2 g protein may differ by one P point from a strict reading.

## File

- `~/workspace/your_files/fop-claim-validator/index.html` — **44,421 bytes** (limit 150 KB), all CSS/JS inline, no external assets, no localStorage/service workers/permissions/parent-window access. Responsive two-column layout, labelled inputs, inline validation errors.

## How to run

Open `index.html` in any modern browser (double-click, or paste the file's contents into the Wix CMS `appHtml` field — see below). No build step, no server.

## Testing

**Method:** the app's PURE CORE (conversions, claim engine, both rating algorithms, batch parser) was extracted from the page and run in Node against independently hand-derived expectations — every boundary value was computed from the published tables above by hand before the harness ran. Worked examples use nutrient values from standard product labels and the officially published classifications. The full inline scripts (core + DOM) were extracted and syntax-checked with `node --check` — **OK**.

**Results: 110 / 110 checks passed.**

| Group | Checks | Result |
|---|---|---|
| Salt/sodium conversions (1.0 g salt = 400 mg sodium; 120 mg = 0.3 g salt) | 3 | ✅ |
| Claim boundaries — exact-at-threshold PASS and +ε FAIL (low fat 3.0/3.01 solid & 1.5/1.51 beverage, fat-free 0.5, low sodium 120/121, very low 40/41, sodium-free 5/6, low energy 40/41 & 20 beverage, energy-free 4 & n/a on solids, low sugars 5.0/5.01 & 2.5 beverage, sugars-free 0.5, sat-fat-free 0.1/0.11) | 26 | ✅ |
| Saturated-fat 10%-of-energy sub-rule flips (1.5 g @100 kcal FAIL vs @200 kcal PASS; beverage 0.75 g @50 kcal FAIL) | 3 | ✅ |
| Protein/fibre routes incl. per-100-kcal route (route named in output) | 9 | ✅ |
| Micronutrient NRV boundaries (Ca 120/119 & 240, Fe 2.1, vit C 15, vit D 0.75, beverage Ca 60) | 7 | ✅ |
| No-added-sugars logic + naturally-occurring-sugars note | 3 | ✅ |
| Claim inventory (24 claims in both jurisdictions) | 2 | ✅ |
| EU/Codex divergence (15% protein-energy: EU source PASS / high FAIL, Codex high PASS; 11%: EU source FAIL, Codex source PASS) | 5 | ✅ |
| Caution flags (raised on sodium 600; none on a clean profile) | 2 | ✅ |
| Nutri-Score foods — published example + rules (below) | 15 | ✅ |
| Nutri-Score beverages — official classifications (below) | 10 | ✅ |
| Health Star Rating — official worked example + rules (below) | 17 | ✅ |
| Batch parser (3-product batch, header/semicolon/no-header, bad-line numbering "Line 3", empty input) | 12 | ✅ |

**Published / official worked examples reproduced:**

| Example | Inputs (per 100 g/ml) | Expected | Got |
|---|---|---|---|
| Chocolate-hazelnut spread (Nutella-type, published 2023 grade **E**) | 2252 kJ, sugars 56.3, sat fat 10.6, sodium 43 mg, protein 6.3, fibre 3.4 | N 31, score 30, **E** | N 31, score 30, **E** ✅ |
| Semi-skimmed milk (SPF Q&A: "largely classified as **B**") | 195 kJ, sugars 4.8, sat fat 1.1, sodium 44 mg, protein 3.4 | score 0, **B** | score 0, **B** ✅ |
| Whole milk (SPF Q&A: "classified as **C**") | 265 kJ, sugars 4.7, sat fat 2.4, sodium 44 mg, protein 3.3 | score 3, **C** | score 3, **C** ✅ |
| 100% orange juice (2023 beverages report outcome **C**) | 188 kJ, sugars 8.8, sat fat 0.1, sodium 4 mg, protein 0.7, FVL 100% | score 4, **C** | score 4, **C** ✅ |
| Cola (sugar-sweetened soda) | 180 kJ, sugars 10.6, sodium 4 mg | score 12, **E** | score 12, **E** ✅ |
| Sweetened soft drink, HSR Category 1 (worked example in the official HSR Guide's 5 steps) | 204 kJ, protein 0.2, sat fat 0, sugars 11.5, fibre 0, sodium 15 mg | baseline 2, final 2, **1 star** | baseline 2, final 2, **1 star** ✅ |

**Rule cases verified:** Nutri-Score protein exclusion at N = 11 with FVL points at maximum (protein still excluded under the 2023 rule; score 4 → C, where the 2017 rule would have counted protein); red-meat protein cap (raw 7 pts → capped 2; same product unflagged scores −2/A vs 3/C flagged); Nutri-Score food energy boundary (335 kJ = 0 pts, 336 kJ = 1 pt); beverage sweeteners (+4 N points moves a flavoured water from B to C); plain-water checkbox → A. HSR protein rule (baseline 14: protein excluded with V < 5 → score 14 → 2 stars; counted with V = 5 → score 3 → 3 stars); HSR category effect (identical zero profile: Cat 1 → 2 stars, Cat 1D → 4 stars); HSR Table 2 sugars cap (50 g sugars = 10 baseline points in Cat 3, not 11); HSR concentrated-FVNL column (30% → 1 V point vs 0 non-concentrated); HSR fibre not applicable in beverage categories.

## Known limitations

- Nutri-Score cheese rule and added-fats/nuts/seeds category not implemented (see Honest scope).
- HSR Table 4 protein boundary at exactly 3.2 g uses ">" uniformly (one-point edge difference possible; see Honest scope).
- Per-serving claim routes in Codex (e.g. protein per serving) are not evaluated; serving size is display-only in the summary.
- Batch mode applies default flags (no added sugars / sweeteners / red meat all off; HSR category by form) — batch rows needing those flags should be loaded into the main form and adjusted.
- FVN/FVNL % is a single shared input, although the Nutri-Score definition (fruit/veg/legumes; nuts and certain oils in the fats category) and the HSR FVNL definition differ slightly in edge cases; the user enters the figure appropriate to their product.

## Wix upload steps

1. In the Wix dashboard, open the CMS collection **SaasProducts**.
2. Edit (or add) the item with slug `fop-claim-validator`.
3. Paste the full contents of `index.html` into the **appHtml** field; set Title, Category (Food Science), Version 1.0, Published = on.
4. Save, open the live app page, and run the smoke test with the app's default product (Wholegrain muesli bar, already filled in): expect **Nutri-Score C** (score 10, N 13 − P 3), **Health Star Rating 3.5 stars** (final score 0, baseline 11), **6 of 24 claims passed** (No added sugars; Low sodium; Source of protein; Source of fibre; High fibre; Source of iron) and **two caution flags** (sugars 18 g ≥ 15; energy 410 kcal ≥ 400). Then paste the sample line from the batch box placeholder and click "Screen batch": the Oat bar row should show **6 / 24** claims, Nutri-Score **C**, HSR **3.5 ★**.

## Listing

- **Title:** Front-of-Pack (FoP) Rating & Claim Validator
- **Description:** Screen nutrition claims under Codex (with the EU protein variation) and predict Nutri-Score 2023 and Health Star Ratings from a nutrient profile.
- **Category:** Food Science
- **Slug:** `fop-claim-validator`
- **Version:** 1.0
