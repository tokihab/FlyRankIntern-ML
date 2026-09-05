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

Usage Examples

Once the pipeline runs, the agent ingests daily performance metrics and outputs an editorial action playbook.

Example Input Data:
A raw CSV containing search history, impressions, click-through rates, and word counts (fact_content_daily_performance).

Example Output (Ranked Queue):
The model assigns a win_chance score to each page and categorizes them into actionable tiers:

    High Priority (Update Now): win_chance > 0.6 AND search_volume > 50

        Example Reason Code generated: "Stuck on Page 2+ (Needs a rankings push)"

    Medium Priority (Quick Fixes): win_chance > 0.4

        Example Reason Code generated: "Content is too thin (Needs more details)"

    Low Priority: Leave as is.

Simple Architecture Sketch

    Data Source: FlyRank Internship Warehouse (March 2026 production slice, 176,738 active rows).

    Features: word_count, avg_position, competition, search_volume, intent_flags.

    Model Engine: Random Forest Classifier (100 trees, max depth 5).

    Validation Strategy: Strict client-grouped cross-validation (pages from the same client are never split between training and test sets) to prevent data leakage.

v2 Evaluation Results

    Accuracy: 0.748 (74.8%)

    Recall: 0.789 (The model successfully catches the vast majority of truly declining pages).

    ROC-AUC (Honest Grouped Split): 0.507 (Compared to a naive random split of 0.562, proving the necessity of grouped validation).

Limitations List

    Directional, not causal: Identifying a declining trend does not guarantee that simply adding words or refreshing the metadata will restore its Google rankings.

    Modest Generalization: The grouped AUC of 0.507 indicates that search signals alone carry limited predictive power when applied to entirely unseen clients. It acts best as a triage tool within the same training cohort.

    High False Positives: Because the model optimizes for high recall (catching failing pages), it inherently flags some healthy pages as declining. Human editorial review is mandatory before deleting or rewriting any content.

AI Transparency

This project was built with AI acting as a pair programmer and thought partner. I used large language models to help debug Python scripts, structure the GitHub Actions CI pipeline, and refine the professional formatting of the final research paper. All architectural decisions, leakage audits, and cross-validation strategies were directed and manually verified by me.

📖 [Read my Final Internship Retrospective here](retrospective.md)
