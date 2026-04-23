# TBGraph 🔬

**Explainable Knowledge Graph Framework for Tuberculosis Governance Using National Health Insurance Claims at Population Scale**

TBGraph is an end-to-end, explainable knowledge graph framework that transforms population-scale TB insurance claims into governance-ready equity insights. It exposes disparities in tuberculosis care access between subsidized (PBI) and non-PBI patients across hospital tiers and 500+ Indonesian cities/regencies, with full GNN explainability.

---

## Pipeline

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          TBGraph Processing Pipeline                        │
└─────────────────────────────────────────────────────────────────────────────┘

  ┌──────────────────┐
  │  BPJS Kesehatan  │
  │   Claims Data    │   (500+ cities/regencies · PBI & non-PBI patients)
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────┐
  │ Stage 1 · Ontolo │
  │  gy Design      │   RDFLib · OWL/SKOS schema · tbgraph_ontology.ttl
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────┐
  │ Stage 2 · KG     │
  │  Construction    │   NetworkX · RDFLib · entity linking · triple store
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────┐
  │ Stage 3 · GNN    │
  │  Equity Reason-  │   PyTorch Geometric · Graph Attention Network (GAT)
  │  ing             │   PBI vs. non-PBI · hospital tier · regional equity
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────┐
  │ Stage 4 · Expla- │
  │  inability       │   GNNExplainer · subgraph attribution · feature masks
  └────────┬─────────┘
           │
           ▼
  ┌──────────────────┐
  │  Policy Report   │
  │  & Region Stats  │   tbgraph_policy_report.json · tbgraph_region_stats.csv
  └──────────────────┘
```
---

## Dataset

The real BPJS Kesehatan TB claims data used in this research is **restricted** and cannot be redistributed. Access is subject to formal data-sharing agreements with BPJS Kesehatan and the Indonesian Ministry of Health.

**A synthetic data generator is included in the notebook.** Navigate to **Stage 1A** (`TBGraph_Pipeline.ipynb`) to generate a synthetic cohort that mirrors the statistical structure of the original dataset — including patient insurance category (PBI/non-PBI), hospital tier distributions, regional codes, treatment outcomes, and visit frequency. All downstream pipeline stages run identically on synthetic data.

---

## Citation

If you use TBGraph in your research, please cite:

```bibtex
@inproceedings{anonymous2026tbgraph,
  title     = {TBGraph: An Explainable Knowledge Graph Framework for Tuberculosis
               Governance Using National Health Insurance Claims at Population Scale},
  author    = {Hafidz, I, Rakmawati, N.A, Rusdiansyah, A.},
  booktitle = {AI4GOOD Workshop at ICML 2026},
  year      = {2026},
  note      = {Workshop paper}
}
```

---

## License

This project is licensed under the [MIT License](LICENSE).

---

## Acknowledgements

- **BPJS Kesehatan** for facilitating access to anonymized national health insurance claims data under formal data governance agreement.
- **Institut Teknologi Sepuluh Nopember (ITS) Surabaya** for institutional support and research infrastructure.
- **World Health Organization** *Global Tuberculosis Report 2024*, which informed the equity framing and epidemiological context of this work.
