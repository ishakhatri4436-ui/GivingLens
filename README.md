# GivingLens: Smarter Blood Donation Outreach

Prepared for **Isha Khatri** — AICTE | IBM SkillsBuild Data Analytics with AI Internship 2026 | BharatCares.

## Overview
GivingLens asks which historical donor records a volunteer team should review first when its review capacity is limited. It combines exploratory analysis, group-based model evaluation and an outreach-budget comparison. Its custom decision layer compares a learned ranking with a recent-first rule and random selection.

This is an academic prototype using a public historical dataset. It does not determine medical eligibility or claim real campaign results. The dataset and prediction task are public; the notebook, report and budget analysis are tailored for this project.

## Submission files
1. `IshaKhatri_GivingLens.ipynb` — complete code, explanations and executed outputs.
2. `requirements.txt` — Python dependencies.
3. `IshaKhatri_ProjectReport.docx` — complete documentation and measured results.
4. `README.md` — project overview, dataset credit and run instructions.

## Dataset and licence
Yeh, I. (2008). **Blood Transfusion Service Center** [Dataset]. UCI Machine Learning Repository.
- Dataset: https://archive.ics.uci.edu/dataset/176/blood+transfusion+service+center
- DOI: https://doi.org/10.24432/C5GS39
- Download: https://archive.ics.uci.edu/static/public/176/blood+transfusion+service+center.zip
- Licence: **Creative Commons Attribution 4.0 International (CC BY 4.0)**.
- Records: 748; source: Hsin-Chu City, Taiwan.
- Outcome: donation in March 2007, not current willingness or outreach response.
- Inputs: recency, donation count, blood volume and tenure. Volume is not money.

A compressed copy of the original CSV is embedded in the notebook. No separate dataset file or live download is required. The notebook verifies SHA-256 `96c8e1091b9c037bcaf25a19b24b49d07771cd88689ffd273e056e9e8845ffe7` and exports a readable CSV. Credit is preserved here, in the notebook and in the report.

## Technology
Python 3.11+ (tested on 3.12), NumPy, Pandas, Scikit-learn, Matplotlib, IPython and JupyterLab. No API keys or paid services are required. Dependencies use tested core-library versions; Python-docx is not needed to run the delivered notebook.

## Setup and run
Keep all four files in the same folder. Open a terminal in that folder.

```bash
python -m venv .venv
```

Activate the environment:
- Windows PowerShell: `.venv\Scripts\Activate.ps1`
- Windows Command Prompt: `.venv\Scripts\activate.bat`
- macOS/Linux: `source .venv/bin/activate`

```bash
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open `IshaKhatri_GivingLens.ipynb`, then choose **Restart Kernel and Run All Cells**. Installing dependencies needs internet access; executing the analysis does not. The notebook creates `givinglens_outputs/` in its working folder. A hosted Jupyter environment such as Colab can also open the notebook; install the requirements first if its library versions differ.

## Method
1. Verify the dataset, check missingness and explore the outcome distribution.
2. Remove redundant volume; derive two ratios without using outcomes.
3. Group identical input profiles. Reserve the first fold of a seeded five-fold stratified group split for testing.
4. Compare the majority baseline, logistic regression, random forest and gradient boosting with five-fold grouped CV on training records only.
5. Select by mean CV average precision; use the fixed 0.5 threshold for classification.
6. Evaluate the held-out set and compare fixed contact budgets.
7. Export results and validate an illustrative input profile.

## Measured results
- Selected model: **Gradient boosting**; selection lead over logistic regression is only about 0.002 CV AP.
- Training/test records: **599 / 149**; test positives: **35**.
- Test ROC-AUC: **0.736**; average precision: **0.428**.
- Accuracy: **75.8%**; default-threshold recall: **14.3%**.
- An all-negative baseline obtains **76.5% accuracy**, slightly above the selected model; ranking is the focus.
- At 30 reviews: **15** historical positives with the model, **12** with recent-first, and **7.05** expected at random. Precision@30 is **50%**, recall@30 is **42.9%**, and lift is **2.13×**.
- The model ties the recent-first rule at some budgets. No statistical significance or extra real-world donations are claimed.

## Outputs
`source_data.csv`, `segment_summary.csv`, `model_comparison.csv`, `outreach_budget.csv`, `historical_review_queue.csv`, `results_summary.json`, and three PNG charts. These are produced when the notebook runs; they are not extra required submission files. `source_row` is a row number, not a real donor identity. The scoring demo uses an illustrative profile and the same train-only fitted model used for evaluation.

## Verification and limitations
All eight code cells passed sequential execution in a fresh in-process IPython session. Notebook schema validation and data, split and input checks passed. A separate Jupyter kernel could not be launched in the authoring environment because sockets were restricted; a browser JupyterLab session was not tested.

Identical-profile grouping reduces leakage but cannot establish donor-level independence without IDs. The data is small, historical and from one centre. There is no chronological validation, calibration for a new population, consent information, medical eligibility or demographic fairness assessment. Stable original row order breaks score ties. Retrospective capture is not causal campaign impact. Use authorised local data and appropriate prospective evaluation before operational use.

## Before submission
Run the notebook, review the results and practise explaining the feature choices, grouping, metrics and lift. Keep the report's factual claims aligned with the code. Update personal or institutional details if needed and describe your own contribution accurately.
