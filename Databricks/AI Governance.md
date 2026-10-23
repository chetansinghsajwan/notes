**AI Governance** is the set of rules, processes, and responsibilities that ensure AI/ML models are **reliable, fair, explainable, secure, and compliant** throughout their lifecycle.
---
**The AI lifecycle it covers:**
```
Data Collection → Model Training → Model Evaluation → Deployment → Monitoring → Retirement
      ↑                                                                              |
      └──────────────────────── Governed at every stage ────────────────────────────┘
```
---
**Key pillars:**
|Pillar|What it means|Example|
|---|---|---|
|Access Control|Who can build, deploy, or invoke a model|Only MLOps team can push to production|
|Lineage|What data trained this model|Model v3 was trained on dataset from Jan 2025|
|Versioning|Track every version of a model|Roll back to v2 if v3 performs poorly|
|Explainability|Why did the model make this decision|Loan rejected because income < threshold|
|Bias and Fairness|Is the model discriminating|Does fraud model flag one region more than others|
|Drift Monitoring|Is the model still accurate over time|Accuracy dropped from 94% to 78% after 3 months|
|Reproducibility|Can you recreate the exact model|Same code + same data + same params = same model|
|Compliance|Does the model meet regulatory requirements|EU AI Act, RBI guidelines for BFSI models|
|Auditability|Complete log of model decisions|Which model version made which prediction when|
---
**Without AI governance — real risks:**
- A model makes a biased hiring or lending decision — **legal liability**
- A model drifts silently and gives wrong predictions — **business loss**
- You cannot explain a model decision to a regulator — **compliance failure**
- Someone deploys an untested model to production — **system failure**
---
**Tools that implement AI governance:**
|Tool|What it does|
|---|---|
|MLflow (Databricks)|Model versioning, experiment tracking, registry|
|Unity Catalog|Access control and lineage for models|
|Model Monitoring|Drift detection, accuracy tracking|
|EU AI Act / ISO 42001|Regulatory frameworks for AI compliance|
---
**BFSI relevance for you:**  
In India's BFSI sector, RBI and SEBI are increasingly requiring explainability and auditability of AI models used in credit scoring, fraud detection, and algorithmic trading — making AI governance critical, not optional.
---
**In short:** AI governance asks — _who built this model, on what data, does it work fairly, can we explain it, and is it still working correctly?_