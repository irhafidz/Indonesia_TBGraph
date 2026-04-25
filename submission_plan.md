# TBGraph Submission Plan

This is a working plan for three TBGraph submissions based on the current April 2026 timeline: **ICML 2026 AI4GOOD workshop in Seoul (deadline 3 May 2026 AoE)**, **Information Sciences special issue Graph-based solutions for Artificial Intelligence (deadline 30 June 2026)**, and a parallel **XAI-focused journal target (deadline 30 June 2026)**.
The TBGraph concept already defines a three-phase pipeline ontology design, data/KG construction, and GNN reasoning with explainability which provides the backbone for all three submissions.



## Short aim plan

| Priority | Venue / Track | Deadline | Short aim plan |
|---|---|---:|---|
| 1 | ICML 2026 AI4GOOD Workshop, Seoul | 3 May 2026 AoE | Submit a short, impact-first paper with one clean TB prediction task, a minimal BPJS-based TBGraph, one baseline vs one graph model, and 1-2 XAI figures showing inequitable TB pathways. |
| 2 | Information Sciences “Special Issue Graph-based solutions for Artificial Intelligence | 30 June 2026 | Submit the main full paper with reproducible cohort construction, heterogeneous KG building, baseline and GNN comparison, XAI, and quantitative equity analysis. |
| 3 | XAI-focused journal target | 30 June 2026 (your target) | Submit a distinct XAI-centered version emphasizing explanation quality, fairness, and interpretability over raw predictive novelty. |

## Working principle

The right strategy is to treat the three submissions as **one shared pipeline with three different levels of completeness**.  
The ICML workshop paper should be the fastest and leanest version, the Information Sciences paper should be the complete graph-AI paper, and the XAI journal should be a more interpretability-centered derivative only if it becomes sufficiently distinct in contribution and framing.

## Part 1 ”ICML 2026 AI4GOOD Workshop" (deadline 3 May 2026)

The workshop deadline is extremely close, so this paper must be deliberately compact.
The goal is to show that anonymized BPJS claims can be converted into a graph-based AI pipeline for equitable TB governance in Indonesia, not to complete every experiment you would want for a journal paper.

### ICML target paper structure

| Section | What to include | What to leave out |
|---|---|---|
| Problem | TB burden, BPJS uniqueness, need for equity-aware pathway modeling | Long literature review |
| Data | Short BPJS TB cohort description, key variables, anonymization note | Full ontology detail |
| Graph construction | Minimal patient-facility-pathway graph schema | Full OWL/Neo4j implementation narrative |
| Modeling | One baseline and one graph model | Multiple graph architectures |
| XAI | 1-2 explanation subgraphs with short interpretation | Deep explainer benchmarking |
| Impact | Why this matters for TB governance and AI for Good | Overclaiming deployment readiness |

### ICML detailed steps

| Step | Task | What exactly to do | Deliverable |
|---|---|---|---|
| 1 | Freeze one prediction target | Choose only one label for ICML: e.g., high-cost TB episode, referral delay risk, or adverse pathway risk. Pick the one you can derive fastest and explain most clearly. | One final target definition written in one sentence. |
| 2 | Build the minimum TB cohort | In the BPJS notebook, filter ICD-10 A15-A19 TB cases; create one row per patient; keep only key features such as PBI status, insurance class, region, facility sequence, hospital class, and cost/outcome variables| `patients_df` and `episodes_df` ready for modeling. |
| 3 | Define trainable label | Derive `y` clearly and reproducibly, such as `high_cost = 1 if total episode cost >= 75th percentile`. Keep this simple and document the threshold. | A label column in your cohort file plus a note in Methods. |
| 4 | Build minimal graph schema | Create nodes for `patient`, `fktp`, and `fkrtl`; optionally add `region` if it is easy to encode. Create edges `patient->fktp` and `patient->fkrtl`, and attach a few meaningful node features.| One graph object and one schema figure for the paper. |
| 5 | Run one baseline | Train logistic regression or XGBoost on patient-level aggregated features. Use a simple split or 5-fold CV. | One baseline metric table. |
| 6 | Run one graph model | Use one graph model only, ideally GraphSAGE, GAT, or hetero R-GCN if your graph is stable. Do not spend time on many variants. | One graph-model metric table. |
| 7 | Generate one explanation figure | Use GNNExplainer or attention weights for 2-3 patient examples. Focus on one clear inequitable pathway pattern, such as disadvantaged patients routed through lower-tier facilities with longer delays. | One clean XAI figure and short explanation. |
| 8 | Draft the paper | Write 4-6 pages with a strong impact-first framing: TB, Indonesia, BPJS, graph AI, equitable pathways. Keep the claims modest and honest. | Draft paper PDF or source. |
| 9 | Supervisor revision | Simplify wording, tighten abstract, improve title, and verify all claims match available evidence. | Submission-ready version. |
| 10 | Submit | Upload before 3 May AoE and keep a clean archive of the exact submitted version. | Final ICML AI4GOOD submission.|

### ICML coding checklist

| Module | Minimum code you need |
|---|---|
| Data prep | TB filter, patient episode aggregation, label generation |
| Graph builder | Patient/facility node maps, edge indices, node features |
| Baseline | Logistic regression or XGBoost |
| Graph model | One GNN only |
| XAI | One explainer run on a few examples |
| Figures | One metrics table, one XAI graph figure |

## Part 2 ”Information Sciences journal (deadline 30 June 2026)

This should be your **main submission** and the most complete scientific version of TBGraph.
Here you should fully realize the pipeline shown in your methodology figure: ontology-guided schema, graph construction from BPJS tables, graph learning, explainability, and equity interpretation.

### Information Sciences paper target structure

| Section | What must be strong |
|---|---|
| Introduction | TB governance problem, graph-based AI motivation, LMIC/BPJS relevance, contribution bullets |
| Related work | Clear gap: no combined TB + claims + KG + GNN + XAI study in this setting.|
| Data | Reproducible cohort creation, variables, anonymization, cohort statistics |
| TBGraph construction | Schema, entity/relationship logic, graph statistics, implementation details |
| Modeling | Baselines, GNN architecture, training and evaluation setup |
| Explainability | Method used, example explanations, pathway interpretation |
| Equity analysis | PBI, region, class/tier disparities with quantitative evidence |
| Discussion | Policy implications, limitations, and next steps |

### Information Sciences detailed steps

| Week / phase | Task | What exactly to do | Deliverable |
|---|---|---|---|
| 24 Apr-1 May | Finalize cohort and target | Decide the primary journal target, define inclusion/exclusion criteria, and document all preprocessing assumptions. This should be the cohort definition you will keep stable for the rest of the project. | Final cohort CSV and cohort flow note. |
| 2-8 May | Formalize graph schema | Translate the metadata sheet into executable schema: patient, FKTP, FKRTL, diagnosis, region, and socioeconomic attributes where feasible.| Schema diagram and schema description text. |
| 2-8 May | Implement graph construction | Build node ID dictionaries, edge lists, feature matrices, and a saved graph object such as a PyTorch Geometric `HeteroData`. | `tbgraph_hetero.pt` or equivalent. |
| 9-15 May | Train tabular baselines | Build patient-level aggregate features and run logistic regression, random forest, and/or XGBoost. Record AUC, F1, recall, and calibration if possible. | Baseline results table. |
| 16-22 May | Train primary graph model | Train one main graph model carefully, tune modestly, and compare with baselines. Keep notes on split strategy and random seeds for reproducibility. | Main model metrics table and ROC/PR figure. |
| 23-29 May | Add XAI pass 1 | Run GNNExplainer or attention-based explanation on representative patients and export 3â€“4 readable subgraph figures. | Set of explanation figures. |
| 30 May-5 Jun | Add equity analysis | Stratify results by PBI status, region, hospital class, and pathway pattern. Quantify disparities in predicted risk and observed outcomes. | Equity tables and plots. |
| 6-12 Jun | Robustness checks | Test a second split strategy, a sensitivity analysis for label threshold, or a simplified graph ablation. Choose one or two checks only. | Robustness subsection and small results table. |
| 13-19 Jun | Write manuscript v1 | Write the full paper from intro to discussion. Use the literature review spreadsheet to support the novelty claim carefully. | Full draft. |
| 20-26 Jun | Revise manuscript v2 | Strengthen figures, sharpen contributions, reduce weak claims, and align terminology with the special issue graph-based AI theme.| Revised final draft. |
| 27-30 Jun | Final package and submit | Prepare cover letter, highlights, author metadata, and final files. Submit the journal paper and archive the submission package. | Final journal submission.|

### Information Sciences coding checklist

| Module | What to code | What good looks like |
|---|---|---|
| Cohort builder | Stable patient episode builder from BPJS tables | Reproducible cohort every run |
| Feature builder | Patient, socioeconomic, facility, and pathway aggregates | Clean tabular matrix |
| KG builder | Node/edge creation and graph serialization | Working graph object for modeling |
| Baselines | Logistic regression, RF/XGBoost | Fair comparison to graph model |
| Graph model | One strong main GNN | Competitive or improved performance |
| Explainer | GNNExplainer or attention workflow | Interpretable pathway figures |
| Equity module | Group disparity summaries | Quantified inequity narrative |
| Figure export | Tables, ROC/PR, XAI graphs | Journal-quality visuals |

## Part 3 XAI-focused journal target (deadline 30 June 2026)

This submission is only realistic if you position it as **different from the Information Sciences paper**.  
It should not be just the same paper with the word XAI in the title; instead, it must foreground explanation quality, fairness, interpretability, and trustworthiness in graph-based TB governance.]

### XAI paper target structure

| Section | What must be emphasized |
|---|---|
| Problem | Why explainability matters for policy-sensitive TB risk modeling |
| Method | Graph model plus explanation pipeline |
| Explanation methods | At least one main explainer and ideally one comparison method |
| Evaluation | Explanation stability, plausibility, and subgroup fairness |
| Use case | How explanations reveal inequitable care pathways |
| Discussion | Trustworthiness and governance relevance |

### XAI detailed steps

| Step | Task | What exactly to do | Deliverable |
|---|---|---|---|
| 1 | Define distinct paper question | Reframe the question from Can graph AI predict TB risk? to How can graph explanations reveal inequitable TB pathways in BPJS claims? | Separate abstract and contribution bullets. |
| 2 | Reuse the same graph/model backbone | Do not rebuild the whole pipeline from scratch. Reuse the cohort, graph object, and best graph model from the main paper. | Shared backbone with the journal paper. |
| 3 | Add a second explanation lens | If feasible, compare GNNExplainer with attention-based explanation or another graph explanation approach. | One explanation-comparison table. |
| 4 | Evaluate explanation quality | Check whether similar patients have similar explanations, whether important nodes/edges are stable across runs, and whether the outputs are policy-interpretable. | One explanation-quality results subsection. |
| 5 | Strengthen fairness link | Compare explanations across PBI vs non-PBI, Java-Bali vs outside Java-Bali, or higher vs lower hospital class pathways. | One fairness/XAI figure or table. |
| 6 | Write focused manuscript | Keep the paper centered on explainability rather than full predictive performance benchmarking. | Draft XAI manuscript. |
| 7 | Decide submit or defer | Submit only if overlap with the graph-AI journal is low enough and the XAI angle is clearly stronger than cosmetic rewording. | Go/no-go submission decision. |

## Shared execution order

To avoid getting stuck, use one fixed implementation order for all three submissions.  
Each stage feeds the next, and most code should be reused rather than rewritten.

| Order | Shared task | Why it comes first |
|---|---|---|
| 1 | Freeze one clear label per submission | Prevents drifting scope |
| 2 | Build clean patient-level cohort | Everything depends on clean cohort logic |
| 3 | Implement graph construction | All graph experiments require this backbone |
| 4 | Run tabular baselines | Gives you a defensible non-graph benchmark |
| 5 | Train one main GNN | Avoids wasting time on too many model variants |
| 6 | Generate XAI outputs | Needed for both workshop and journal narratives |
| 7 | Add equity analysis | Converts technical output into policy relevance |
| 8 | Write while results stabilize | Reduces last-minute manuscript chaos |

## Notes

A PhD submission paper is not the one with the largest number of experiments; it is the one with the **cleanest scope, clearest novelty claim, reproducible pipeline, and most defensible story**.  
For TBGraph, the winning formula is: one well-defined TB task, one clearly documented BPJS-to-graph construction pipeline, one fair baseline comparison, and a few compelling XAI figures that reveal inequitable patient-facility pathways in a way that matters for TB governance.]

## Daily rule until the first deadline

Until **3 May**, work in this order every day: **label -> cohort -> graph -> baseline -> GNN -> 1 XAI figure -> writing**.  
After the ICML submission, switch to:  **robust cohort -> stronger graph -> better experiments -> more XAI -> equity analysis -> journal writing**.
