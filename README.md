# Predicting ICU length of stay after cervical spinal cord injury

## What this project asks

Can information collected during the first day in the ICU help predict how long an adult patient with a cervical spinal cord injury will stay in intensive care?

The project uses MIMIC-IV as its main dataset. It also repeats the analysis separately in the CareVue part of MIMIC-III. The datasets are not combined, and the MIMIC-III results are not treated as an independent hospital validation.

## Main finding

The answer is: only a little, and not clearly better than the comparison methods.

For the main MIMIC-IV analysis (139 eligible ICU stays), the Random Forest model had an R² of 0.071 for log-transformed length of stay and 0.028 for length of stay in days. Its advantage over a simple median prediction was uncertain. It also was not distinguishable from the APACHE-II comparison. APACHE-II had R² values of 0.079 on the log scale and −0.015 in days.

In the separate MIMIC-III CareVue analysis (88 eligible stays), Random Forest had R² values of 0.183 on the log scale and 0.009 in days. It did better than the median baseline in this comparison, but not clearly better than APACHE-II. These small samples make all estimates uncertain. The analysis does not show that the model reliably predicts an individual patient's ICU stay better than APACHE-II.

APACHE-II is an approximation built from fields available in these data. This project did not calculate APACHE-IV. Although APACHE-IV methods have been published, reproducing it faithfully requires specific diagnosis mappings and model inputs that are not directly available in a usable form here.

## What was done

- Selected admissions with cervical spinal cord injury diagnosis codes: ICD-9 806.0x and, in MIMIC-IV, ICD-10 S14.1xxA.
- Kept the first ICU stay per qualifying hospital admission and restricted the study to adults.
- Used measurements from the first 24 hours to predict total ICU length of stay. The main analysis includes stays lasting at least 24 hours.
- Evaluated predictions with patient-grouped cross-validation so a patient's admissions stay together in validation.
- Compared a median baseline, an APACHE-II approximation, Lasso, Random Forest, and XGBoost.
- Tested four alternatives: include short stays, use a 12-hour window, use a 48-hour window, and remove the 99-day stay.

The notebook caps predictions at the minimum and maximum length of stay in each training fold. It also includes a separate capped-versus-uncapped check. Lasso uses a fixed penalty of 0.01; it is not tuned with inner cross-validation. These choices are disclosed in the notebook and report.

## Files

- `ICU_LOS_cervical_SCI_analysis.ipynb` — the analysis notebook and saved outputs.
- `ICU_LOS_Case_Study_Report.docx` — the plain-language written report.
- `results_FINAL/` — tables and figures produced by the latest notebook run.

The other trial notebooks are earlier or separate versions and are not the final analysis. Raw MIMIC data are not included. Running the notebook requires credentialed access to MIMIC-IV and MIMIC-III and the Python packages listed in the notebook.

Before submission, run the notebook once from top to bottom. In the current saved notebook, the Step 8 results-summary cell has saved output but no execution count; its values match `results_FINAL/results_summary.csv`. The modeling and sensitivity cells have saved outputs, but this one cell should be rerun (or the notebook should be restarted and run all) for a clean execution record.

## Important limitations

The cohorts are small and come from one hospital. ICU stay length is affected by many things that are not captured by this model, including treatment response, complications, death, and discharge practices. Some APACHE-II chronic-health information comes from diagnosis codes recorded by discharge, so it would not necessarily be known at the 24-hour prediction point. The MIMIC-III CareVue analysis is a separate replication, not validation at a different hospital. This project is a case-study analysis, not a clinical decision tool.

## References

- Johnson AEW, et al. MIMIC-IV, a freely accessible electronic health record dataset. *Scientific Data*. 2023. [doi:10.1038/s41597-022-01899-x](https://doi.org/10.1038/s41597-022-01899-x).
- Johnson AEW, et al. MIMIC-III, a freely accessible critical care database. *Scientific Data*. 2016. [doi:10.1038/sdata.2016.35](https://doi.org/10.1038/sdata.2016.35).
- Knaus WA, et al. APACHE II: a severity of disease classification system. *Critical Care Medicine*. 1985;13(10):818–829. [PubMed](https://pubmed.ncbi.nlm.nih.gov/3928249/).
- Verburg IWM, et al. Which models can I use to predict adult ICU length of stay? A systematic review. *Critical Care Medicine*. 2017;45(2):e222–e231. [doi:10.1097/CCM.0000000000002054](https://doi.org/10.1097/CCM.0000000000002054).
