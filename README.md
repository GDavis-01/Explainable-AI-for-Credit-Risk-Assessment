# Explainable AI for Credit Risk Assessment

Code and intermediate results for an MSc dissertation examining whether post-hoc explanations of credit-scoring models are reliable.

Four models (logistic regression, XGBoost, and matched neural networks in TensorFlow and PyTorch) are fitted on two credit datasets of deliberately contrasting size and explained using three attribution methods: SHAP, LIME and Integrated Gradients.

Three properties are tested:

- Whether the methods agree with one another
- Whether the deep learning framework alters the explanation
- Whether an explanation registers demographic disparate impact

## What's in here?

### Notebooks (`notebooks/`)

There are two main analysis notebooks, one for each dataset:

- `Dissertation_Taiwan.ipynb`
- `Dissertation_German.ipynb`

Each runs the full pipeline:

- Fetches the data
- Fits the four models
- Computes the explanations
- Produces the outputs needed to answer all three research questions

A third notebook, `Experimental_Design_Figure.ipynb`, draws the experimental design schematic used as Figure 3.1 in the dissertation.

### Statistical Analysis (`r/`)

`Dissertation_Data_Analysis.Rmd` performs the formal statistical testing, including the Friedman and Nemenyi model comparison, concordance analysis, and four-fifths ratios with bootstrap confidence intervals.

It also produces the statistical figures used in the dissertation.

### Intermediate Results (`data/`)

Five CSV files per dataset are written by the Python notebooks and read by the R script. They contain:

- Per-fold AUCs
- Test predictions with protected attributes
- Feature importances for each model and explanation method
- Per-seed training diagnostics
- SHAP importances from the framework comparison

Keeping these files in the repository means the statistical stage can be reproduced without having to rerun the Python notebooks, which can take some time.

### Figures (`images/`)

The final figures used in the dissertation are:

- `fig_experimental_design.png`
- `fig_framework_taiwan.png`
- `fig_framework_german.png`
- `fig_cv_auc.png`
- `fig_concordance_heatmap.png`
- `fig_top_features.png`
- `fig_age_gradient.png`
- `fig_fairness_ratios.png`

No raw data are stored here. Both datasets are fetched at runtime from the UCI Machine Learning Repository, so their provenance is clear and local copies cannot drift from the published versions.

## Running the Analysis

The two stages are separate by design:

- Python does the modelling and explanation
- R does the formal statistical testing
- The CSV files are the hand-off between them

Keeping the stages separate also means the intermediate results can be inspected directly.

### 1. Python

Open either notebook in Google Colab and run all cells.

Each notebook fetches its dataset, fits the four models, computes the explanations, and writes the CSV exports used by the R analysis.

The RQ2 multi-seed section is the slowest part. Ten seeds are trained under each deep learning framework, with SHAP explanations calculated for each run.

### 2. R

Place the exported CSV files alongside the R Markdown file and knit `Dissertation_Data_Analysis.Rmd`.

This runs the Friedman and Nemenyi comparison, concordance analysis, and four-fifths ratio analysis with bootstrap confidence intervals. It also produces the statistical figures.

## Dependencies

The notebooks install their own dependencies in the first cell. Outside Google Colab, the main Python dependencies are:

- shap
- lime
- captum
- ucimlrepo
- xgboost
- tensorflow
- torch
- scikit-learn
- pandas
- numpy
- scipy
- matplotlib

## A Note on Reproducibility

Random seeds are set throughout the analysis wherever possible, but exact numerical reproduction is not guaranteed for every output.

PyTorch is generally stable under the seeds used here. TensorFlow execution is not fully deterministic under the current setup, so repeated runs can return slightly different neural-network attribution and concordance values. The effect is more noticeable for the Taiwan dataset than for the German dataset.

Small differences can also occur in stochastic explanation procedures such as LIME.

The main conclusions remain stable across reruns. In particular, the Bonferroni-adjusted RQ2 comparison gives the same headline result: 0 of 23 features are significantly different between TensorFlow and PyTorch for the Taiwan dataset, compared with 2 of 20 for the German dataset.

Because of this, small numerical differences from the values reported in the dissertation should be expected when rerunning the full pipeline.

## Citation

Davis, G. (2026) *Explainable AI for Credit Risk Assessment*. MSc dissertation, Loughborough University.

## Data Sources

Yeh, I.-C. and Lien, C.-H. (2009) 'The comparisons of data mining techniques for the predictive accuracy of probability of default of credit card clients', *Expert Systems with Applications*, 36(2), pp. 2473-2480.

Hofmann, H. (1994) *Statlog (German Credit Data)* [Dataset]. UCI Machine Learning Repository.
