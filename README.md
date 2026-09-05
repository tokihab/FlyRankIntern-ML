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

# Weekly Voice AI Tech Brief Agent

## What It Does and For Whom
This automated workflow acts as a research agent for Senior Voice AI Architects and engineers. It replaces 6 to 8 hours of manual deep-work by gathering the latest industry news, filtering out marketing fluff, and synthesizing the technical updates into concise, 3-bullet-point briefs[cite: 11]. 

## Setup Steps (Stranger-Friendly)
This is a no-code agent built on Activepieces[cite: 11]. To reproduce it:
1.  **Clone the Flow:** Create a new project in Activepieces (Cloud Builder)[cite: 11].
2.  **Configure the Trigger:** Add a "Web Form" trigger to accept human input for the specific search topic[cite: 11].
3.  **Add SerpApi:** Add a "Google Search (SerpApi)" node and input your API key. Map the search query to `{{trigger.searchTopic}} latest tech news 2026`[cite: 11].
4.  **Add Google Gemini:** Add a "Chat Gemini" node and input your API key[cite: 11]. Map the output of the SerpApi node into the prompt.
5.  **Add Destination:** Add a "Create Document (Google Docs)" or "Slack/Email" node to route the formatted markdown brief to your workspace[cite: 11].

## Usage Examples
**Example Input (Web Form):** 
`Native Audio LLMs`[cite: 11]

**Example Output (Generated Brief):**
*   **Architectural Shift to End-to-End Processing:** Native Audio LLMs are evolving into true end-to-end models that process audio directly, moving beyond traditional segmented Speech-to-Text (STT), LLM, and Text-to-Speech (TTS) pipelines[cite: 11]. 
*   **Advanced Reasoning and Expanded Context:** New models like GPT-Realtime-2 integrate "GPT-5-class reasoning" directly into live audio loops and feature expanded context windows[cite: 11].
*   **Specialized Models for Real-time Voice Agents:** Key models emerging in 2026, such as Deepgram Nova-3 and GPT-Realtime-2, are specifically optimized for real-time performance in voice agents[cite: 11].

## Simple Architecture Sketch
The workflow is a linear 4-step pipeline executed via Activepieces[cite: 11]:
*   **Step 1: Trigger (Web Form):** Collects the target Voice AI topic from the user[cite: 11].
*   **Step 2: Gather (SerpApi):** Queries Google using the target topic and extracts a JSON list of the top organic articles and snippets[cite: 11].
*   **Step 3: Synthesize (Google Gemini 1.5 Flash):** Processes the raw JSON against a strict system prompt to extract core engineering updates (focusing on architecture, latency, and new models)[cite: 11].
*   **Step 4: Format & Route (Google Docs / Slack):** Converts the LLM output into a clean markdown document for easy consumption[cite: 11].

## v2 Evaluation Results
*   **Execution Speed:** The pipeline successfully runs a topic end-to-end in approximately 5 seconds[cite: 11].
*   **Time Saved:** Automates a manual research process that typically takes 6 to 8 hours per week[cite: 11].
*   **Setup Cost:** Initial workflow design, JSON mapping, and query spacing fixes took 1.5 hours upfront[cite: 11].

## Limitations List
1.  **Query Fragility:** If the search query lacks a space before the date filter, SerpApi returns zero results, which subsequently breaks the Gemini node[cite: 11].
2.  **LLM Hallucinations on Hard Metrics:** The LLM can occasionally mix up specific millisecond latency metrics or pricing models[cite: 11]. 
3.  **Mandatory Human Review:** Because of potential metric hallucinations, a human architect must verify exact numbers before making a hard engineering or purchasing decision based on the brief[cite: 11].

## AI Transparency
This workflow was designed with AI serving as a strategic thought partner. I utilized LLMs to help craft the rigid prompt instructions used in the Gemini synthesis node and to format this documentation cleanly. All workflow steps, API node configurations, and limitation audits were executed and validated manually by me.

📖 [Read my Final Internship Retrospective here](retrospective.md)
