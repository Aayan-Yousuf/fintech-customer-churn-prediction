# Fintech Customer Intelligence & Churn Prediction

An end-to-end, beginner-friendly fintech customer analytics and machine-learning project for exploring customer behaviour, predicting churn risk, explaining the main churn drivers, and recommending practical retention actions.

This repository is being built step by step. The current version only sets up a professional project structure. It does **not** include a dataset, trained model, or modelling code yet.

## Current project structure

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

## What each folder is for

| Folder | Simple explanation |
| --- | --- |
| `configs/` | Stores project settings later, such as file paths, model parameters, or experiment options. Keeping settings here makes the project easier to adjust without rewriting code. |
| `data/raw/` | Will hold the original, untouched dataset once a dataset is added. Raw data should stay unchanged so results can be traced back to the source. |
| `data/processed/` | Will hold cleaned or transformed data created from the raw data. This keeps prepared data separate from the original data. |
| `data/external/` | Will hold extra data from outside sources if needed, such as public economic indicators or reference tables. |
| `docs/` | Stores written project notes, assumptions, definitions, and business explanations. This helps recruiters and reviewers understand the thinking behind the work. |
| `notebooks/` | Will contain beginner-friendly Jupyter notebooks for exploration, analysis, and storytelling. Notebooks are useful for showing the reasoning process step by step. |
| `reports/` | Will contain final project outputs, such as summaries, business insights, and presentation-ready material. |
| `reports/figures/` | Will contain generated charts and images used in reports or the README. |
| `scripts/` | Will contain small command-line scripts for repeatable tasks, such as preparing data or running checks. |
| `src/` | Will contain reusable Python source code. This keeps the project more professional than putting all logic inside notebooks. |
| `src/data/` | Will contain code for loading, validating, and preparing datasets. |
| `src/features/` | Will contain feature engineering code, such as creating customer activity metrics or transaction behaviour signals. |
| `src/models/` | Will contain future model training, prediction, and evaluation code. This folder is empty for now because the model is not being built yet. |
| `src/visualization/` | Will contain reusable plotting code for charts and dashboards. |
| `src/utils/` | Will contain shared helper functions used across the project. |
| `tests/` | Will contain automated tests to check that important project code works correctly. |

## Files added at this stage

| File | Simple explanation |
| --- | --- |
| `.gitignore` | Tells Git which files should not be committed, including local virtual environments, Python cache files, generated model files, and real data files. This protects the repository from becoming messy or accidentally exposing data. |
| `.gitkeep` files | Empty placeholder files used so Git tracks folders that do not contain real project files yet. They can be removed later when each folder has real content. |
| `README.md` | Explains the project purpose and the initial folder structure in plain English. |

## What has intentionally not been added yet

- No dataset has been created or downloaded.
- No model has been trained.
- No prediction code has been written.
- No notebook analysis has been started.

This keeps the first step focused only on creating a clean, professional foundation.
