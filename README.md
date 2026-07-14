# Fintech Customer Intelligence & Churn Prediction

A fintech customer analytics project for exploring customer behaviour, predicting churn risk, identifying churn drivers, and recommending retention actions.

The repository currently defines the project structure and planning documents. It does **not** include a dataset, trained model, or modelling code.

## Project structure

```text
fintech-customer-churn-prediction/
├── configs/
├── data/
│   ├── raw/
│   ├── processed/
│   └── external/
├── docs/
├── notebooks/
├── reports/
│   └── figures/
├── scripts/
├── src/
│   ├── data/
│   ├── features/
│   ├── models/
│   ├── visualization/
│   └── utils/
├── tests/
├── .gitignore
└── README.md
```

## Folder guide

| Folder | Purpose |
| --- | --- |
| `configs/` | Project settings such as paths, model parameters, and experiment options. |
| `data/raw/` | Original datasets, kept unchanged for traceability. |
| `data/processed/` | Cleaned or transformed datasets created during analysis. |
| `data/external/` | Optional supporting data from outside sources. |
| `docs/` | Project notes, assumptions, definitions, and business context. |
| `notebooks/` | Jupyter notebooks for exploration, analysis, and explanation. |
| `reports/` | Final summaries, findings, and presentation material. |
| `reports/figures/` | Charts and images used in reports or documentation. |
| `scripts/` | Command-line scripts for repeatable project tasks. |
| `src/` | Reusable Python source code. |
| `src/data/` | Data loading, validation, and preparation code. |
| `src/features/` | Feature engineering logic for customer behaviour signals. |
| `src/models/` | Model training, prediction, and evaluation code once modelling begins. |
| `src/visualization/` | Reusable plotting and dashboard helpers. |
| `src/utils/` | Shared helper functions. |
| `tests/` | Automated tests for project code. |

## Project documents

| File | Purpose |
| --- | --- |
| `.gitignore` | Excludes local environments, caches, generated models, and real data files from version control. |
| `.gitkeep` files | Keep empty project folders tracked until real files are added. |
| `README.md` | Summarises the project purpose and structure. |
| [`docs/data_dictionary.md`](docs/data_dictionary.md) | Defines the planned customer, transaction, app activity, and subscription tables, including the churn rule. |

## Current scope

- The data dictionary is a planning document only.
- No dataset has been created or downloaded.
- No model has been trained.
- No prediction code or notebook analysis has been added.
