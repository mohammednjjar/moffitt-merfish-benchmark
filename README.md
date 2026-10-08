# Supervised vs. Unsupervised Cell-Type Recovery on the Moffitt (2018) MERFISH Dataset

Code for the paper *Supervised vs. Unsupervised Cell-Type Recovery on the
Moffitt (2018) MERFISH Hypothalamic Preoptic Dataset: A Feature-Contribution
Benchmark* (Al Najjar and Rada, Bahçeşehir University).

The benchmark asks how well the 15 published cell-type labels of
Moffitt et al. (2018) can be recovered from MERFISH measurements of the mouse
hypothalamic preoptic region, and which input feature blocks carry the
cell-type signal. Two supervised classifiers (Random Forest, LightGBM) and three
unsupervised methods (Leiden, Louvain, Gaussian Mixture Model) are scored on
one Hungarian-matched macro-F1 scale, with ARI and NMI as assignment-free
checks, across eight feature representations and three class-set
granularities (K = 8, 9, 15).

## Repository layout

```
├── notebooks/
│   ├── 01_data_cleaning.ipynb   raw release -> two cleaned CSVs
│   └── 02_benchmark.ipynb       the full benchmark (Steps 0–17)
├── data/
│   └── README.md                where to download the raw files
├── results/                     output tables from the published run
├── requirements.txt             exact library versions
└── LICENSE
```

## How to reproduce

1. **Install** Python 3.11 or 3.12 and the pinned libraries:
   ```bash
   pip install -r requirements.txt
   ```
2. **Download** the three raw files into `data/raw/` (see
   [`data/README.md`](data/README.md)).
3. **Clean** the data: run `notebooks/01_data_cleaning.ipynb`. It writes
   `data/clean/cells_cleaned.csv` (874,768 cells) and
   `data/clean/cells_boundaries_clean.csv` (35,522 cells).
4. **Run the benchmark**: run `notebooks/02_benchmark.ipynb` top to bottom.
   Tables go to `outputs/tables/`, figures to `outputs/figures/`.

Both notebooks run from the repository root or from `notebooks/`, locally or
on Google Colab (clone the repository there and place the data as above).

**Compute.** The published run used a single Google Colab A100 high-RAM
instance. The full benchmark takes several days, dominated by Optuna tuning
(Steps 8–9) and full-graph clustering on ~870,000 cells (Steps 6, 12, 13).
Each step writes its own tables, so steps can be run in separate sessions.

**Reproducibility.** A single seed (42) is applied to Python, NumPy, every
estimator, every cross-validation splitter, the neighbour-graph construction
and every Optuna sampler. LightGBM and the neighbour-graph routines admit small
run-to-run variation under multithreading, so results reproduce to within
about 10⁻³ rather than bit-exactly.

## Data split

| | K = 8 | K = 9 | K = 15 |
|---|---:|---:|---:|
| Train (80% of the boundary subset) | 26,436 | 27,125 | 28,417 |
| Internal test (20%) | 6,609 | 6,782 | 7,105 |
| External holdout (never used in training or tuning) | 752,818 | 773,346 | 839,246 |

The training pool is the 35,522-cell subset with repaired boundary polygons
(one animal); the external holdout is every other cleaned cell. For each K,
the K most frequent classes in the training pool are kept.

## Where each result comes from

| Paper | Notebook step | Output table (`outputs/tables/`) |
|---|---|---|
| Table 2 — split sizes | Step 6 (printed) | — |
| Table 3 — macro-F1, untuned baseline | Step 6 | `master_summary_all_K.csv`, `K{K}_pivot_f1_macro.csv` |
| Table 4 — per-class precision / recall / F1 | Step 8 | `K15_report_LGBM_tuned_genes_only_external_839k.csv` |
| Table 5 — ARI and NMI | Step 6 | `master_summary_all_K.csv` |
| Table 6 — reproduced Louvain vs. Leiden vs. LightGBM | Step 6 | `master_summary_all_K.csv` |
| Table 7 — tuned vs. baseline at K = 15 | Step 10 | `K15_optuna_tuned_vs_baseline_summary.csv` |
| Table 8 — three clustering regimes | Steps 6, 12b, 13 | `master_summary_all_K.csv`, `graph_kmatched_summary.csv`, `graph_resolution_sweep_res07.csv` |
| Morphology ablation | Step 7 | `shape_summary_all_K.csv` |
| Tuned search spaces and selected values | Steps 8–9 | `K15_optuna_best_params_*.json` |
| Transfer of K = 15 parameters to K = 8 / 9 | Steps 11–12 | `tuned_K8_K9_summary.csv` |
| Leave-two-section-out class loss | Step 14 | `leave_2_bregma_class_loss_874k.csv` |
| Neighbourhood size, k = 3 vs. 10 | Step 16 | `neighbourhood_k10_vs_k3.csv` |
| Figure 1 — class distribution | Step 17b | figure `class_distribution_counts_pct_874k.png` |
| Figure 2 — raw counts per section and class | Step 17c | figure `bregma_class_rawcounts_874k.png` |
| Gene importance (top 25 of 155 genes) | Step 17a | `lgbm_gain_importance_genes_K15.csv` |

## Environment of the published run

Python 3.12.13 on Google Colab, NumPy 1.26.4, pandas 2.2.2, scikit-learn 1.5.1,
SciPy 1.13.1, LightGBM 4.5.0, Optuna 3.6.1, Scanpy 1.10.3, AnnData 0.10.8,
python-igraph 0.11.6, leidenalg 0.10.2, louvain 0.8.2. Leiden uses Scanpy's
`igraph` flavour; Louvain uses the `vtraag` flavour (the `louvain` package).

## Data source

Moffitt, J. R., Bambah-Mukku, D., Eichhorn, S. W., Vaughn, E., Shekhar, K.,
Perez, J. D., … Zhuang, X. (2018). Molecular, spatial, and functional
single-cell profiling of the hypothalamic preoptic region. *Science*,
362(6416), eaau5324. https://doi.org/10.1126/science.aau5324 —
data: https://doi.org/10.5061/dryad.8t8s248

## License

Code is released under the MIT License (see `LICENSE`). The data are not
redistributed here; they remain under the terms of their original release.
