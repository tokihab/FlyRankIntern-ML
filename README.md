# FlyRank ML Capstone: Content Refresh Predictor

## What It Does and For Whom
This machine learning pipeline helps SEO managers and content editors prioritize website updates. Instead of relying on gut feelings or lagging traffic reports, this tool evaluates historical search signals (word count, average position, competition, and search volume) to predict which pages are entering a traffic decline, generating an actionable, ranked queue for the content team.

## Architecture Sketch
- **Data Source:** FlyRank Internship Warehouse (March 2026 slice, 176k+ rows).
- **Features:** `word_count`, `avg_position`, `competition`, `search_volume`, `intent_flags`.
- **Model:** Random Forest Classifier (100 trees, max depth 5).
- **Validation Strategy:** Client-grouped cross-validation to prevent data leakage.

## Evaluation Results
- **Accuracy:** 0.748 (74.8%)
- **Recall:** 0.789 (catches the majority of truly declining pages)
- **ROC-AUC (Honest Grouped Split):** 0.507

## Limitations
1. **Directional, not causal:** Identifying a declining trend does not guarantee that adding words or refreshing the page will restore its rankings.
2. **Modest Generalization:** The grouped AUC of 0.507 indicates that the feature set carries limited predictive power when applied to entirely unseen clients. It works best as a triage tool within the same training cohort.
3. **High False Positives:** The model optimizes for recall (catching failing pages), which results in healthy pages being flagged. Human editorial review is mandatory.

## Setup & Reproducibility
A stranger can reproduce this entirely in Google Colab with no local setup:
1. Clone this repo: `git clone https://github.com/tokihab/FlyRankIntern-ML`
2. Install dependencies: `pip install -r requirements.txt`
3. Run the pipeline: `python scripts/run_all.py`

## AI Transparency
This project was built with AI acting as a pair programmer and thought partner. I used LLMs to help debug Python scripts, structure the GitHub Actions CI pipeline, and refine the professional formatting of the final research paper. All architectural decisions, leakage audits, and cross-validation strategies were directed and manually verified by me.

📖 [Read my Final Internship Retrospective here](retrospective.md)
