# TBGraph 🔬

**Explainable Knowledge Graph Framework for Tuberculosis Governance Using National Health Insurance Claims at Population Scale**

![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)
![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)
![Framework: PyTorch Geometric](https://img.shields.io/badge/Framework-PyTorch%20Geometric-orange.svg)

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
  │ Stage 1 · Ontol- │
  │  ogy Design      │   RDFLib · OWL/SKOS schema · tbgraph_ontology.ttl
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

## Key Features

- **Population-scale KG construction** Builds a heterogeneous knowledge graph from millions of TB insurance claims spanning 500+ Indonesian cities and regencies using NetworkX and RDFLib.
- **Domain ontology** A custom OWL/SKOS ontology (`tbgraph_ontology.ttl`) encodes TB care concepts, insurance categories (PBI/non-PBI), hospital tiers (FKTP/FKRTL), and regional administrative hierarchies.
- **Equity-focused GAT reasoning** A Graph Attention Network learns node embeddings that capture structural disparities between subsidized poor patients (PBI) and general enrollees across facility tiers and geographies.
- **GNNExplainer attribution** Subgraph-level explanations identify which patient–facility–region relationships most strongly predict care inequity, making model decisions interpretable for policymakers.
- **Governance-ready outputs** Structured JSON policy reports and region-level CSV statistics are generated automatically, formatted for direct use in public health reporting workflows.
- **Synthetic data generation** Because the real BPJS dataset is restricted, the pipeline notebook includes a reproducible synthetic data generator that mirrors the statistical properties of the original claims data.

---

## Repository Structure

```
tbgraph/
├── README.md
├── notebooks/
│   └── TBGraph_Pipeline.ipynb        # Full end-to-end pipeline (Stages 1–4)
├── ontology/
│   └── tbgraph_ontology.ttl          # OWL/SKOS domain ontology
├── outputs/
│   ├── tbgraph_policy_report.json    # Structured equity findings by region
│   └── tbgraph_region_stats.csv      # Per-region descriptive statistics
├── figures/
│   ├── tbgraph_subgraph_sample.png   # Sample KG subgraph visualization
│   ├── tbgraph_descriptive_stats.png # Cohort and claims descriptive plots
│   └── tbgraph_explainer_features.png# GNNExplainer feature attribution map
└── models/
    └── tbgraph_gat_best.pt           # Best GAT checkpoint (saved weights)
```

---

## Requirements

Python 3.9 or higher is required. Install all dependencies with:

```bash
pip install torch torchvision torch-geometric \
            networkx rdflib \
            pandas scikit-learn \
            matplotlib seaborn
```

> **Note:** For `torch-geometric`, match the version to your CUDA/CPU build. See the [PyTorch Geometric installation guide](https://pytorch-geometric.readthedocs.io/en/latest/install/installation.html) for details.

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
