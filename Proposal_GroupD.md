# Final Project Proposal — Group D

**Group members:** Godson Mugisha *(and other Group D members)*
**Thematic area:** Public Safety

---

## Road Accident Risk Analysis in Rwanda

Our group will investigate what patterns are associated with road accidents in Rwanda and whether available data can predict accident severity or risk. We will use a road-safety dataset of at least 500 real accident records — preferably sourced from the Rwanda National Police or a comparable open road-safety repository — containing attributes such as location, time of day, road and weather conditions, vehicle type, and injury outcome. Road accidents are a leading cause of preventable death and injury in Rwanda and across sub-Saharan Africa, so understanding when and where severe accidents cluster can directly inform traffic enforcement, infrastructure investment, and public-awareness campaigns. We plan to train a **classification model** (starting with logistic regression and comparing with a random forest) to predict accident severity class, evaluated using a confusion matrix, recall, precision, and F1-score, because the target is categorical and class imbalance between minor and fatal incidents makes recall on the severe class the most critical metric. Findings will be communicated through an interactive Plotly dashboard highlighting risk patterns and practical road-safety recommendations.

---

### Specific Objectives

1. Investigate accident patterns by location, time of day, and road/weather conditions.
2. Clean and explore a suitable road-safety dataset.
3. Develop and evaluate a severity/risk prediction model.
4. Build an interactive dashboard communicating risk patterns and prevention insights.

### Planned Model

**Classification** — logistic regression baseline, compared against a random forest. Evaluation: confusion matrix, precision, recall, F1-score. Recall on the severe-accident class is the primary metric given expected class imbalance.

### Expected Output

An accident severity/risk model and an interactive Plotly dashboard (HTML output) showing findings, limitations, and practical road-safety recommendations.
