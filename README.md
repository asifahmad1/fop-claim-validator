# Front-of-Pack (FoP) Rating & Claim Validator

By **Dr. Asif Ahmad** — Asif SaaS Studio

Category **Food Science** · Version **1.0** · MIT License

Screen a product's nutrient profile three ways at once: which nutrition claims it may make under **Codex CAC/GL 23-1997** (with the **EU protein variation** from Regulation (EC) No 1924/2006), a predicted **Nutri-Score** under the **2023 updated algorithm**, and a predicted **Health Star Rating** (AU/NZ) in any of the six official categories. **100% in your browser**: no uploads, no accounts, and no data ever leaves your device.

**▶ Live on Asif SaaS Studio:** open the tools catalogue and choose *Front-of-Pack (FoP) Rating & Claim Validator* (slug `fop-claim-validator`).

## Run it

Open `index.html` in any modern browser — or from this repo via any static host. No build step, no dependencies, no server. The app opens with a worked example product (wholegrain muesli bar) already screened.

## What it does

1. **Claim engine** — all **24 Codex nutrient content claims** evaluated on every keystroke: low / free energy, fat, saturated fat (including the ≤10%-of-energy sub-rule), sugars, sodium (low / very low / free), no added sugars, source & high protein, source & high fibre, and source & high calcium, iron, vitamin C and vitamin D at 15% / 30% of NRV. Protein and fibre claims are judged on all their Codex routes (per 100 g, per 100 ml **and** per 100 kcal) and the app names the route met. Every FAIL shows the size of the gap ("needs to fall by 1.20 g"). A jurisdiction switch re-bases the two protein claims to the EU energy-share rule (source ≥ 12% of energy, high ≥ 20%).
2. **Caution flags** — when a product passes claims but is high in sodium (≥ 500 mg), sugars (≥ 15 g/100 g), saturated fat (≥ 5 g) or energy (≥ 400 kcal), amber cautions say so — worded as cautions, not Codex disqualifications, because many markets restrict claims on such products even where Codex does not.
3. **Nutri-Score 2023** — general foods and beverages, with the full auditable points table per component. Implements the 2023 computation as published by Santé publique France: for foods with N ≥ 11, **protein points are not counted** (the 2017 FVL-points condition was dropped in 2023 — the app follows the 2023 Q&A text, not the older 2017 form); the red-meat protein cap of 2 points; beverage sweeteners +4 N; plain water = A by rule.
4. **Health Star Rating** — all **six categories** (1, 1D, 2, 2D, 3, 3D), baseline points (Tables 1 & 2, including the Table 2 sugars cap of 10 and the extended sodium scale), V points from FVNL % (both Table 3 columns), protein points subject to the **≥13 rule** (protein counts only if baseline < 13 or V ≥ 5), and fibre points (not applicable in beverage categories).
5. **Batch screening** — paste or upload a CSV of products (`name, form, energy_kcal, protein, fibre, sugars, fat, satfat, sodium_mg, fvn_pct, calcium, iron, vitc, vitd`); tolerant parser (comma/semicolon/tab, header matched by column name, positional fallback, bad lines reported by line number). Results: claims passed, Nutri-Score grade, HSR stars; click a row to load it into the main form.
6. **Linked fields that remove classic errors** — energy in kcal **and** kJ (1 kcal = 4.184 kJ), sodium mg **and** salt g (sodium mg = salt g × 400), edited either way. Live validation: negatives, nutrients > 100 g, FVNL outside 0–100, saturated fat above total fat.
7. **Outputs** — copyable text summary and a full CSV download (ratings, every claim with status, value and requirement, caution flags).

## Sources & honest scope

- **Sources:** Codex CAC/GL 23-1997 (Table of conditions for nutrient content claims; NRVs: protein 50 g, Ca 800 mg, Fe 14 mg, vitamin C 100 mg, vitamin D 5 µg); Regulation (EC) No 1924/2006, Annex (EU protein claims); Santé publique France *Nutri-Score Questions & Answers*, 10 July 2023 (Tables 5, 6, 9, 10) and Scientific Committee update reports 2022 & Jan 2023; *Guide for industry to the Health Star Rating Calculator* (summarised scoring tables, 2015 update).
- Predictions are **guidance, not legal clearance** — national implementations vary (e.g. some markets add a trans-fat condition to saturated-fat-free) and final claim legality needs regulatory review.
- The app takes a **nutrient profile** as input; it does not compute a profile from a recipe (the companion LP Recipe & Cost Optimizer covers formulation).
- Nutri-Score's special **cheese** rule and **added fats / nuts & seeds** category are **not** implemented (general foods + beverages only) — classifying cheese or oils with the general tables will mis-grade them, and the in-app scope note says so.
- HSR Table 4 edge: the protein step at 3.2 g is printed "≥3.2" in the summarised table while all other steps use ">" — the app uses ">" uniformly, so a product at exactly 3.2 g protein may differ by one P point from a strict reading.
- No backend is needed or faked — everything runs in-page: no server, no storage, no API keys.

## Files

- `index.html` — the complete app, self-contained: all CSS and JavaScript inline, 44,421 bytes. No localStorage, no service workers, no browser permissions, no parent-window access — safe in a sandboxed iframe, and it works fully offline.
- `fop-claim-validator-test-report.md` — the full test report, sources & versions table, honest-scope notes and the Wix listing details.
- Documentation (PDF) → `docs/fop-claim-validator-docs.pdf`

## Testing

110 automated checks run against the shipped PURE CORE — all passed. Every boundary value was derived **by hand from the published tables before the harness ran**: claim thresholds at exact-at-threshold PASS and +ε FAIL (26 checks), the saturated-fat 10%-of-energy sub-rule flips, protein/fibre routes, NRV boundaries, the EU/Codex divergence in both directions, Nutri-Score rule cases (protein exclusion at N = 11 with maximum FVL points; red-meat cap; beverage sweetener flip; water rule), HSR rule cases (the ≥13 protein rule in both branches; category effects; Table 2 sugars cap; concentrated-FVNL column), and the batch parser. Published/official worked examples reproduced exactly: chocolate-hazelnut spread → Nutri-Score **E** (N 31, score 30); semi-skimmed milk → **B**; whole milk → **C**; orange juice → **C**; cola → **E**; the official HSR Guide soft-drink example → **1 star** (baseline 2, final 2). Full report: `fop-claim-validator-test-report.md`.

## Documentation

Complete user guide and technical documentation — the claims/ratings compliance problem, the sources & versions table, all 24 claim conditions with routes, the EU protein divergence with constructed examples, caution-flag wording and rationale, the Nutri-Score 2023 protein rule as implemented and why, the HSR protein rule, batch screening, honest scope, the testing summary, and the tested core verbatim: `docs/fop-claim-validator-docs.pdf`.

Created by Dr. Asif Ahmad · Asif SaaS Studio · v1.0
