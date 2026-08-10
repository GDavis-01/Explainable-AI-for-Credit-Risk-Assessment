# Explainable-AI-for-Credit-Risk-Assessment

Code and intermediate results for an MSc dissertation examining whether post-hoc explanations of credit-scoring models are reliable.

Four models (logistic regression, XGBoost, and matched neural networks in TensorFlow and Pytorch) are fitted on two credit datasets of deliberately contrasting size, and explained with three attribution methods (SHAP, LIME and Integrated Gradients). Three properties are tested: 
- Whether the methods agree with one another
- Whether deep learning framework alters the explanation
- Whether an explanation registers demographic disparate impact.

What's in here?

Notebooks (notebooks/)

Two analysis notebooks, one per dataset. These are Dissertation_Taiwan.ipynb and Dissertaton_German_Dataset.ipynb each run the full pipeline: 
- Fetches the data
- Fits the four models
- Computes the explanations
- Answers all three research questions.

A third, Design_Figure.ipynb, draws the schematic used as Figure 3.1 in the report.

Statistical Analysis (r/) 

Dissertation_Data_Analysis.Rmd performs the formal testing: the Friedman and Nememyi model comparison, the concordance analysis, and the four-fifths rations with bootstrap confidence intervals. It also produces the statistical figures.

Intermediate results (data/)

Five CSV files per dataset, written by the Python notebooks and read by the R script. They hold the:
- per-fold AUCs
- the test predictions with protected attributes
- the feature importances for ever model and method
- the per-seed training diagnostics
- SHAP importances from the framework comparison.

By keeping these in the repository, the statistical stage can be reproduced without running the python notebooks, which can take some time. 

Figures (images/)

The figures reproduced in the report.

No raw data is stored here. Both datasets are fetched at run time from the UCI Machine Learning Repository, so their provenance is unambiguous and the files cannot drift from their published versions.

Running the analysis

The two stages are separate by design:
- Python does the modelling and explanation
- R does the formal statistical testing
- CSV files above are the hand-off between them.

Running the stages in different environments force the intermediate results into an explicitly inspectable form.

1. Python. Open either notebook in Google Colab and run all cells. Each fetches it's dataset, fits the four models, computes the explanations, and writes it's CSV exports. The RQ2 multi-seed section is the slow part, ten seeds trained under each framework with a SHAP explanation computed for every one.
2. R. Place the exported CSVs alongside the R Markdown file and knit it. It runs the Friedman and Nemenyi comparison, the concordance analysis, and the four fifths ratios with bootstrap confidence intervals, and produces the statistical figures.

The ntebooks install their own dependencies in the first cell. Outside Colab you will need:
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

A note on reproducibility

PyTorch reproduces exactly under the seeds set here. TensorFlow does not - seeding a session does not guarantee deterministic execution, and repeated runs retun slightly different concordance figures. The effect is documented within the report, and is larger on the Taiwan data than on the German.

Analyses that average over the ten seeds, which includes the framework comparison reproduce exactly. Those resting on a single fit, which includes the network concordance figures, vary by a few hundredths between runs. Expect small difference from the values printed in the report for the TensorFlow rows.

Citation

Davis, G. (2026) Explainable AI for Credit Risk Assessment. MSc dissertation, Loughborough University.

Data Sources 

Yeh, I.-C. and Lien, C.-H. (2009) 'The comparisons of data mining techniques for the predictive accuracy of probability of default of credit card clients', Expert Systems with Applications, 36(2), pp. 2473-2480.

Hofmann, H. (1994) Statlog (German Credit Data) [Dataset]. UCI Machine Learning Repository.
