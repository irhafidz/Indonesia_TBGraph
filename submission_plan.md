# TBGraph Action Plan – From BPJS Dataset to Winning Submissions

## Overview

- Main goal (now): Submit TBGraph case study to **Information Sciences – Special Issue: Graph-based solutions for Artificial Intelligence** by **1 June**.
- Secondary goals (after June):  
  - Extend XAI analysis for a dedicated **XAI journal** article.  
  - Prepare a shorter, impact‑focused paper for **ICML 2026 “AI for Good” (Seoul)**, using the same pipeline and results.

---

## Part A – Information Sciences Special Issue (Deadline: 1 June)

### Week-by-week task breakdown (now → 1 June)

| Week (2026) | Task Theme | What to Do / Achieve | Concrete Targets |
|------------|------------|----------------------|------------------|
| Week 1 (Apr 25 – May 1) | Finalise cohort & labels | 1. Decide ONE clear prediction label: (a) TB treatment outcome (good vs bad) or (b) high-cost TB episode (e.g., top 20–25% of total TB claim cost). 2. In the existing BPJS `.ipynb`, build `patients_df` (1 row per PSTV01) with age, sex, class (PSTV09), PBI status (PSTV10), region/province. 3. Build `episodes_df` summarising visits/referrals per patient (counts of FKTP visits, presence of FKRTL referral, highest hospital class accessed, time FKTP→FKRTL, total TB cost using FKL47). 4. Use ICD‑10 lookup (`df8`) to filter to TB codes A15–A19 only. 5. Drop records missing key dates/outcomes required for the label. | - One clean CSV: `tb_patients_cohort.csv` with features + final label column `y`. - Short note: exact label definition (e.g., “y=1 if total TB cost is in top 25% of distribution”). |
| Week 2 (May 2 – May 8) | Implement TBGraph KG | 1. Fix KG schema in code based on metadata sheet: node types (`patient`, `fktp`, `fkrtl`, `region`), edge types (`patient`–`visit_fktp`–`fktp`, `patient`–`referral_fkrtl`–`fkrtl`, `fktp`–`located_in`–`region`, `fkrtl`–`located_in`–`region`). 2. In Python, create integer ID maps for each node type. 3. From FKTP and FKRTL dataframes, construct edge lists with timestamps and basic attributes (e.g., visit date, referral date). 4. Build node feature matrices: patient socio‑economic features; facility tier; hospital class and region. 5. Use PyTorch Geometric to create a `HeteroData` object with node features, edge indices, and `y` on `patient` nodes. | - One saved file: `tbgraph_hetero.pt`. - Text summary for paper: number of patients, facilities, hospitals, regions; edges per type; average degree. |
| Week 3 (May 9 – May 15) | Baselines + first GNN | 1. From `tb_patients_cohort.csv`, produce `X_tabular` (aggregated patient features) and `y`. 2. Train baseline models (logistic regression, Random Forest/XGBoost) with 5‑fold cross‑validation. 3. Record performance: AUC‑ROC, F1, precision, recall. 4. Load `tbgraph_hetero.pt` and split patient nodes into train/validation/test (70/15/15, stratified). 5. Implement a simple R‑GCN or HeteroConv GNN (2–3 layers, hidden size 32–64, dropout ~0.3) for node classification on `patient` nodes. 6. Train with early stopping on validation AUC; evaluate on test set. 7. Compare GNN performance to best baseline. | - Results table: `Model | AUC | F1 | Precision | Recall` including baselines and GNN. - At least 1 ROC curve figure for the GNN. - Choose your “main” GNN model for the paper (the one with best and most stable metrics). |
| Week 4 (May 16 – May 22) | XAI & equity analysis | 1. Integrate GNNExplainer (or PyG `Explainer`) for your chosen GNN. 2. Select 10–20 patients: high‑risk, low‑risk, misclassified, different SES groups (PBI vs non‑PBI, Java–Bali vs outer islands). 3. For each selected patient node, run GNNExplainer to obtain important neighbors and features. 4. Visualise explanation subgraphs (patient, facility, hospital, region) using NetworkX or PyG plotting tools; export as high‑resolution images. 5. Define equity groups based on PBI status, region, and hospital class; compute outcome rates, average predicted risk, mean time‑to‑referral, and total cost per group. 6. Create 2–3 key tables/plots showing disparities and relate them to the explanation subgraphs. | - 3–4 clean explanation subgraph figures with captions (e.g., “High‑risk PBI patient routed through low‑class hospital with long referral delay”). - 2–3 equity tables/plots (group statistics). - Clear text in notes describing at least one “equitable” and one “inequitable” TB pathway pattern. |
| Week 5 (May 23 – May 31) | Writing, polishing & submission | 1. Draft full paper with sections: Introduction, Related Work, Data & TBGraph Construction, Methods (baselines + GNN + XAI), Results, Discussion, Conclusion. 2. Integrate evidence from your lit review spreadsheet to highlight the gap (no TB KG + claims + GNN + XAI in LMICs). 3. Insert all figures (schema diagram, ROC curve, XAI subgraphs, equity plots) and tables (dataset statistics, model metrics, equity metrics). 4. Write 150–200 word abstract stressing (a) graph‑based AI, (b) equity and TB governance, (c) explainable GNN. 5. Prepare highlights and a graphical abstract (adapt your three‑phase TBGraph figure). 6. Check Information Sciences Guide for Authors (format, word limits, reference style) and implement required formatting. 7. Get feedback from supervisors, revise once, and upload via the Elsevier/Information Sciences submission portal under the special issue “Graph-based solutions for Artificial Intelligence.” | - Complete manuscript (LaTeX or Word) + all figures, tables, highlights, graphical abstract. - Submission confirmation email from the journal before or on 1 June. |

---

## Part B – Follow‑up XAI Journal Submission (after June, more XAI depth)

### High‑level aim

Transform TBGraph into a more XAI‑focused paper for a specialised XAI journal (e.g., focusing on GNN explanations, fairness, or healthcare explainability). You will **reuse the same dataset and main model**, but deepen the XAI and evaluation side.

| Stage | Task Theme | What to Do / Achieve | Targets |
|-------|------------|----------------------|---------|
| Stage 1 (June – July) | Expand explanation methods | 1. Add at least one more GNN XAI method: gradient‑based (Integrated Gradients), PGExplainer, or attention‑weight analysis. 2. Implement a unified pipeline to run multiple explainers on the same patients. 3. Compare stability and agreement between methods (e.g., which edges/features are consistently highlighted). | - Table/plot showing overlap in top‑k important features/edges between methods. - Example cases where explanations agree vs disagree. |
| Stage 2 (July – August) | Human‑interpretability & fairness | 1. Define human‑meaningful explanation metrics (e.g., simplicity, faithfulness) based on available literature. 2. Conduct a small “expert review” session if possible (e.g., TB clinician or public health expert rates whether highlighted pathways make sense). 3. Strengthen fairness analysis: use metrics such as demographic parity difference or equal opportunity for groups defined by PBI status or region. | - Quantitative fairness metrics table. - Short qualitative summary from expert feedback (if feasible). |
| Stage 3 (August – September) | XAI‑centric manuscript | 1. Re‑structure the paper around XAI: TBGraph as case study, but main contribution is methodology for explainable GNN on health‑equity graphs. 2. Emphasise (a) taxonomies of explanations, (b) evaluation of explanation quality, (c) how explanations reveal inequities. 3. Submit to an XAI‑specific journal or special issue aligned with explainable graph learning in healthcare. | - Second manuscript focused on XAI, referencing but not duplicating the Information Sciences paper. |

---

## Part C – ICML 2026 “AI for Good” (Seoul) – TBGraph as Impact Story

### High‑level aim

Prepare a shorter conference paper highlighting **policy impact and methodology** for TBGraph as an AI‑for‑Good solution in Indonesia, based on your final GNN + XAI pipeline.

Assume ICML 2026 workshop/track notifications early 2026, with submission around **January 2026** (exact date to be checked on the ICML call for papers when available).

| Stage | Task Theme | What to Do / Achieve | Targets |
|-------|------------|----------------------|---------|
| Stage 1 (Oct – Nov 2025) | Policy & case-study framing | 1. Summarise how TBGraph can support equitable TB governance decisions: e.g., identifying bottleneck facilities, regions with long delays, or high catastrophic costs. 2. Prepare 2–3 “scenario” narratives: “What would MoH/BPJS do with these insights?” 3. Select the most impactful XAI subgraphs and equity plots as visuals for non‑technical audience (policy‑makers, workshop attendees). | - Short 1–2 page concept note emphasising social impact and policy relevance. - Clear mapping from TBGraph outputs to potential interventions. |
| Stage 2 (Dec 2025) | ICML‑style technical summary | 1. Compress the technical methodology: TBGraph construction, GNN, and explanations, emphasizing novelty relative to general GNN or health applications. 2. Prepare new experiments if needed: temporal generalisation (train on earlier years, test on later years), or robustness to missing data. 3. Write a 6–8 page ICML‑style paper (or workshop format) with clear problem statement, method, experiments, and impact. | - Draft ICML paper with AI‑for‑Good emphasis, reusing but shortening material from your journal paper plus additional robustness results. |
| Stage 3 (Jan 2026) | Submission & presentation prep | 1. Finalise and submit to the appropriate ICML 2026 “AI for Good” track/workshop (Korea). 2. Prepare slides or a poster focusing on (a) TB burden in Indonesia, (b) BPJS as unique data resource, (c) TBGraph method, (d) key findings. 3. Design a simple demo script or static storyboard in case you get a spotlight talk. | - Submitted ICML paper. - Completed slide deck or poster template ready for revisions after reviews. |

---

## How to Use This Plan

- Save this file as `tbgraph_plan.md` in your project folder or Git repo so you can re‑read it every day.
- At the start of each week, highlight the current row in the Week table and tick off tasks as you complete them.
- When you feel stuck, focus on **one cell** in the table (e.g., “build `patients_df` and `episodes_df`”) and ask for concrete code help for that exact cell.

You already have strong ingredients: unique BPJS TB dataset, a clear gap in the literature, and a powerful graph‑plus‑XAI story.[file:341][file:339] This plan is designed so that, if you execute each row, you will have a solid, reviewer‑friendly first PhD paper and a path toward two more publications.
