# Trending YouTube Analysis (Pandas Project)

This repository contains a Python (**pandas**) project that analyzes a multi-country dataset of **Trending YouTube videos**.
The main deliverable is a Jupyter notebook that answers the assignment’s required queries and prints the corresponding outputs (tables and aggregations).
For easier grading/review, I also include an exported **HTML version** of the notebook so results can be inspected without running code.

## Dataset

Source (Kaggle): https://www.kaggle.com/datasets/datasnaek/youtube-new
> Note: The dataset files are not included in this repository (size/licensing). Please download them from Kaggle and extract them locally.

## Project overview

The notebook explores the dataset and computes statistics such as:
- counts of videos under specific conditions (e.g., missing tags, removed videos),
- aggregated metrics by channel (e.g., total views),
- time-based groupings (e.g., publish time clustered into 10-minute intervals),
- tag-level analysis (splitting and exploding tags),
- “top video” selections by day/month and country,
- integration of JSON category files to flag non-assignable categories.

## How to run

1. Download and extract the dataset locally (CSV + category JSON files).
2. Open `ProgettoPS944958.ipynb` in Jupyter.
3. Update the dataset folder path in the notebook (the `cartella = ...` cell) to point to your local dataset directory.
4. Run all cells.

## Preprocessing note (Zstandard → CSV)

In my case, the raw files were provided in **`.zst` (Zstandard-compressed)** format, so I batch-converted them to `.csv` before running the analysis.
If your download already contains CSV files, you can skip this step.

## What the notebook does

- reads and concatenates all country CSV files into a single dataframe (adding a `country` column inferred from the filename),
- filters out excluded rows (e.g., removed videos, disabled comments/ratings),
- computes derived metrics (e.g., `like_ratio` with safe handling for `dislikes == 0`),
- performs tag parsing (`split` + `explode`) and tag-level aggregations,
- selects maximum-view videos per `(trending_date, country)` and per `(month, country)`,
- loads the categories JSON files, merges them with the main dataframe, and flags non-assignable categories,
- prints the outputs for every required query directly in the notebook cells.

To avoid biased counts caused by repeated appearances of the same video across multiple trending days, some aggregations use a deduplicated view of the dataset (e.g., `["video_id", "country"]`) when the intent is to count **unique videos** rather than **rows**.

## Repository contents

- `ProgettoPS944958.ipynb` — main notebook (all queries + outputs)
- `ProgettoPS944958.html` — HTML export (quick review without running code)
