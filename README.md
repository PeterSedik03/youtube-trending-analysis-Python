# Trending YouTube Analysis (Pandas Project)

This repository contains my project for analyzing **Trending YouTube videos** using **Python (pandas)**. The core deliverable is a Jupyter notebook that answers the assignment’s set of queries and produces the corresponding outputs (tables and aggregations). For convenience and for grading, I also include an exported **HTML version** of the notebook so that results can be viewed without running any code.

## Project overview

The goal of the project is to explore a multi-country dataset of trending YouTube videos and compute a series of statistics such as:
- counts of videos under specific conditions (e.g., missing tags, removed videos),
- aggregated metrics by channel (e.g., total views),
- time-based groupings (e.g., publish time clustered into 10-minute intervals),
- tag-level analysis (splitting and exploding tags),
- “top video” selections by day/month and country,
- integration of a JSON categories file to validate/flag non-assignable categories.

The notebook implements all required queries step by step and prints the outputs directly in the cells.

## Dataset and preprocessing (Zstandard → CSV)

The original dataset files were provided in **`.zst` (Zstandard-compressed)** format. Before running the analysis, I converted all `.zst` files to `.csv` in one batch using **PowerShell**, so that pandas could load them easily.

In practice, the workflow was:
1. Place all `.zst` files in a folder (one file per country).
2. Run a PowerShell batch conversion that outputs `.csv` files with matching names.
3. Use Python/pandas to load and concatenate all CSV files into a single dataframe, adding a `country` column based on each filename.

> Note: The exact PowerShell command can vary depending on the tool available on the system (e.g., `zstd.exe`). The key point is that all `.zst` files were converted to `.csv` automatically in one pass, without manual file-by-file work.

## What the notebook does

The notebook:
- reads and concatenates all country CSV files into one dataframe (adding `country`),
- cleans or filters out excluded rows (e.g., removed videos, disabled comments/ratings),
- computes derived metrics (e.g., `like_ratio` with safe handling for `dislikes == 0`),
- performs tag parsing (`split` + `explode`) and tag-level aggregations,
- selects maximum-view videos per `(trending_date, country)` and per `(month, country)`,
- loads the categories JSON files and merges them with the main dataframe,
- produces the final outputs for every query in a clear, reproducible way.

To avoid biased counts caused by repeated appearances of the same video across multiple trending days, some aggregations use a deduplicated view of the dataset (e.g., by `["video_id", "country"]`) when the intent is to count *unique videos* rather than *rows*.

## Repository contents

- `ProgettoPS944958.ipynb`  
  The main notebook containing all queries and outputs.

- `ProgettoPS944958.html`  
  HTML export of the notebook (recommended for quick review of results).

- `requirements.txt`  
  Python dependencies required to run the notebook.

- `data/` (not necessarily included in the repo)  
  Folder expected to contain the converted CSV files and the JSON category files, depending on the course rules about sharing datasets.

## How to run

1. Create and activate a Python environment (optional but recommended).
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
