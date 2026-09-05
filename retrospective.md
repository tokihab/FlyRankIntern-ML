# Internship Retrospective: From Naive Splits to Honest Data

If I could sit down with the Week 1 version of myself—who was just starting to stare at 79 million rows of production search data—I would tell him to stop worrying about getting a high accuracy score and start worrying about how the data is lying to him. 

When I set out to build this content refresh predictor, my goal was to create an algorithm that could definitively tell an SEO editor what to fix. I wanted a magic bullet. In the early weeks, I thought I had found one. My initial model runs were kicking back feature importances of 99% and incredibly high accuracy. Week 1 me would have shipped that to production and called it a massive success. 

What changed was my understanding of data leakage and honest validation. I learned the hard way that my 99% feature importance was caused by `views_after`—a metric from the future that the model logically wouldn't have at decision time. Later, I realized that my standard 80/20 random split was allowing the model to memorize client-specific quirks because pages from the same website were bleeding into both the training and test sets. 

The biggest pivot of my internship was abandoning that "good" naive split (AUC 0.562) for an honest, client-grouped split. My ROC-AUC dropped to 0.507. It was humbling to see the model barely outperform random chance on unseen clients. But that drop represented reality. It forced me to rewrite my claims from "this AI perfectly predicts traffic" to "this is a directional decision-support tool."

**The three most transferable things I learned are:**
1. **Grouped Cross-Validation:** Standard splits are dangerous in real-world business data. Grouping by client or entity is the only way to test true generalization.
2. **Translating Business to ML:** I learned how to turn a messy human problem ("which of these 10,000 pages should I rewrite today?") into a structured classification task with an actionable, ranked output queue.
3. **Honest Claim Framing:** Engineers lose credibility when they overpromise. Stating limitations clearly—like acknowledging high false positives—actually builds trust with stakeholders.

If I were to build this next (v2), I wouldn't add more complex models. I would build an automated feedback loop to track the actual outcomes of the refreshed pages at 30, 60, and 90 days, replacing my derived proxy label with real, causal ground truth.
