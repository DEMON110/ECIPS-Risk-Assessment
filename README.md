ECIPS: Leakage-Resistant Metaverse Transaction Risk Classification
A reproducible machine-learning research workflow for auditing and predicting Kaggle-provided transaction risk labels in a metaverse transactions dataset. The project emphasizes label integrity, feature provenance, leakage-resistant evaluation, temporal robustness, cost-sensitive high-risk screening, and transparent reporting.
> **Research caveat:** The prediction target is a dataset-provided risk label, **not independently verified fraud ground truth**. Results should not be interpreted as real-world fraud detection performance.
Project overview
This repository documents a nine-notebook investigation, progressing from dataset intake and leakage audits to controlled experiments, ensemble learning, and final statistical validation.
Research questions
Are the provided labels reliable, and can features inadvertently reveal how they were generated?
How do models perform when evaluated using chronological, leakage-conscious splits?
How sensitive are findings to feature selection, label definitions, and time windows?
What trade-offs arise between multiclass classification and high-risk screening?
How robust and explainable are the final results?
Dataset
Source: Metaverse transactions dataset (Kaggle-provided labels)
Records: 78,600
Columns: 14
Audited source SHA-256: `c7eae68df7ff627089b44df6123c4974e71065f4c342b45a4bf4664bdea6beb0`
A copy of `metaverse_transactions_dataset.csv` is present in `paper 1/result 5/` in the supplied archive. Some notebooks use a different default filename or location: adjust their `DATA_PATH` to point to the same audited CSV. Later notebooks may validate the source checksum and stop if it differs.
Review the dataset's original licensing and redistribution terms before publishing the CSV to a public repository.
Notebook workflow
The notebooks are in `paper 1/` and should be explored in numerical order.
Notebook	Focus
`01_dataset_intake_validation.ipynb`	Source intake, schema, missingness, and structural validation
`02_label_integrity_leakage_audit.ipynb`	Label integrity and leakage audit
`03_feature_provenance_split_design.ipynb`	Feature provenance and evaluation split design
`04_baseline_model_after_audit.ipynb`	Post-audit baseline modeling
`05_Label_Generation_and_Feature_Provenance_Investigation.ipynb`	Label-generation and provenance investigation
`06_Controlled_Feature_Set_Experiments_and_Label_Rule_Sensitivity.ipynb`	Controlled feature sets and label-rule sensitivity
`07_Leakage_Resistant_Model_Development_and_Robustness_Evaluation.ipynb`	Leakage-resistant model development and robustness
`08_Advanced_Ensemble_Optimization_Cost_Sensitive_Learning_and_Explainability.ipynb`	Ensemble optimization, screening costs, and explainability
`09_Final_Model_Validation_Statistical_Robustness_and_Research_Consolidation.ipynb`	Final comparisons, calibration, bootstrap intervals, temporal validation, and synthesis
The archive also contains notebook copies with saved execution results, intermediate CSV/JSON artifacts, and exported output ZIP files. These are research artifacts, not separate pipeline stages.
Methods
Evaluation: Chronological train / validation / test split (70% / 10% / 20% in the final evaluation), with additional forward-chaining temporal checks.
Models: Extra Trees, Random Forest, LightGBM, and Logistic Regression.
Multiclass metrics: Accuracy, balanced accuracy, macro-F1, and weighted-F1.
High-risk screening: One-vs-rest precision, recall, F1, average precision, ROC-AUC, and validation-selected decision thresholds.
Robustness: Bootstrap confidence intervals, calibration analysis, temporal-window comparisons, and feature/provenance investigations.
Selected results
The following numbers are from `paper 1/ecips_notebook09_outputs/09_results_summary.md` and refer to the final held-out test set.
Multiclass classification
Model	Accuracy	Balanced accuracy	Macro-F1
ExtraTrees (balanced)	0.7607	0.7791	0.6597
LightGBM (balanced)	0.7178	0.8400	0.6558
Random Forest (balanced)	0.8386	0.6782	0.6359
ExtraTrees (unweighted)	0.8558	0.5809	0.5539
Logistic Regression (balanced)	0.6009	0.7937	0.5410
Validation-selected champions: ExtraTrees (balanced) for multiclass macro-F1; ExtraTrees (unweighted) for high-risk average precision. Different metrics favor different models, so accuracy alone should not determine the preferred model.
High-risk screening
Using validation-selected thresholds on the final test set:
Model	High-risk recall	High-risk precision	Average precision
ExtraTrees (balanced)	0.9976	0.2414	0.2533
ExtraTrees (unweighted)	0.9945	0.2414	0.2525
LightGBM (balanced)	1.0000	0.2416	0.2507
Very high recall comes with low precision and substantial false-positive volume. Screening results therefore require explicit operational cost and alert-volume consideration.
Getting started
Clone or download the repository and open a terminal in the project directory.
Create a Python environment and install the core analysis packages:
```bash
   python -m venv .venv
   # Windows PowerShell:
   .venv\Scripts\Activate.ps1
   # macOS/Linux:
   # source .venv/bin/activate

   pip install jupyter numpy pandas matplotlib scikit-learn lightgbm
   ```
Open the notebooks:
```bash
   jupyter notebook "paper 1"
   ```
Set each notebook's `DATA_PATH` to the audited dataset file. Check for notebook-specific configuration and optional dependencies before execution.
Run notebooks 01 through 09 in order. Existing exported artifacts can be used for comparison, but paths to earlier results may need updating for your environment.
The package list above covers the main libraries observed in the research workflow; it is not a tested, version-locked environment specification.
Reproducibility and artifacts
The supplied archive includes, among other outputs:
`paper 1/ecips_notebook08_outputs/` — validation/model comparisons, test metrics, threshold curves, feature importance, and reproducibility metadata.
`paper 1/ecips_notebook09_outputs/` — final model comparisons, calibration, bootstrap intervals, temporal robustness, research limitations, and results summary.
`paper 1/result 4/` through `paper 1/result 7/` — intermediate research results.
For comparable results, preserve the original dataset version, feature exclusions, chronological splits, and validation-only threshold selection. Do not tune on the final test set.
Limitations
Labels are dataset-provided proxies rather than independently adjudicated fraudulent transactions.
Features may encode or closely approximate the label-generation process; provenance and leakage audits are central to interpreting results.
Chronological evaluation improves the realism of the experiment but does not establish generalization to other datasets or live deployments.
Ordinary row-level bootstrap confidence intervals do not fully account for temporal dependence.
High-risk screening can generate many false positives even when recall is excellent.
Intended use
This repository is intended for research, education, and methodological evaluation. It is not a production fraud-detection service or a validated basis for consequential financial decisions.
Citation and license
If you use this work, cite the repository and the original dataset source. Add the corresponding paper citation, author information, repository URL, and a `LICENSE` file when those details are finalized. No software license is implied by this README.
