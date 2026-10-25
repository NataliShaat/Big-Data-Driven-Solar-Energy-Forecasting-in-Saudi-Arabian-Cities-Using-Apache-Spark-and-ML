# Big Data-Driven Solar PV Forecasting in Saudi Arabian Cities

A reproducible, leakage-controlled benchmark of **13 forecasting models** for daily photovoltaic (PV) output across five Saudi cities (Riyadh, Jeddah, Mecca, Medina, Dammam), using **Apache Spark** for ingestion and **five fused input modalities** (meteorological, sky state, geospatial, hardware configuration, temporal).

## Key results (held-out test set, 2,196 rows)

| Rank | Model | RMSE (kWh) | MAE (kWh) | R² |
|---:|---|---:|---:|---:|
| 1 | XGBoost | 0.450 | 0.382 | 0.950 |
| 2 | Decision Tree | 0.453 | 0.362 | 0.949 |
| 3 | Gradient Boosting | 0.482 | 0.327 | 0.942 |
| 4 | Random Forest | 0.593 | 0.456 | 0.913 |
| 5 | Bi-GRU (best deep model) | 1.117 | 0.886 | 0.690 |
| 11 | CNN-LSTM-Transformer | 2.744 | 2.410 | −0.868 |

The full table for all 13 models is in [`results/t5_master_table.csv`](results/t5_master_table.csv).

- **Data audit:** the simulated target is constant within every City × Month × Mount cell. The 10,980 rows therefore contain only **113 distinct targets**, and the test set only **30**.
- **Significance:** tested at the level of independent information (30 cells), XGBoost beats every linear and deep model, but is statistically tied with the other tree ensembles.
- **Modality ablation:** fusing configuration and sky-state inputs with meteorology lowers RMSE by **28%** relative to meteorology alone.
- **Scalability:** Spark overtakes single-process pandas beyond about **2.1 M rows**, tested up to 5.49 M rows on two cores.

## Repository structure

```
├── notebooks/
│   └── ksa_solar_pv_benchmark.ipynb   # full pipeline: Spark → EDA → models → tests → figures
├── data/
│   └── KSAsolar2024.csv               # public dataset (CC BY 4.0), see data/README.md
├── results/                           # all tables produced by the notebook (CSV)
├── figures/                           # all figures produced by the notebook (PNG, 300 dpi)
├── requirements.txt
└── LICENSE
```

## How to run

**Locally**

```bash
git clone <this-repo-url>
cd ksa-solar-pv-benchmark
pip install -r requirements.txt
jupyter notebook notebooks/ksa_solar_pv_benchmark.ipynb
```

Apache Spark requires Java 17 or newer. Run all cells from the top. A full run takes about 20–30 minutes on a laptop CPU, most of it spent training the deep models.

**Google Colab:** open the notebook and run all cells. You will be prompted to upload `data/KSAsolar2024.csv`.

## Protocol in brief

- **Split:** chronological 80/20 (8,784 training rows, 2,196 test rows; test period 19 Oct – 31 Dec 2024). Every model is scored on the same test rows.
- **Leakage control:** `Output_Power_W` is excluded as an input, and all scalers are fitted on training rows only.
- **Deep models:** 14-day windows (days t−13 … t) within each City × Panel × Mount series. The last 10% of training dates are used for early stopping.
- **Statistics:** Wilcoxon signed-rank test on 30 cell-level MAEs, plus a Diebold–Mariano test with Newey–West variance on date-level loss differentials.
- **Seed:** 42.

## Dataset

S. Liaquat, *2024 Solar Dataset of Top 5 Cities of KSA*, Kaggle, 2024, CC BY 4.0:
https://www.kaggle.com/datasets/shahlaliaquat/2024-solar-dataset-of-top-5-cities-of-ksa

## License

Code: MIT (see `LICENSE`). Dataset: CC BY 4.0 (original author).
