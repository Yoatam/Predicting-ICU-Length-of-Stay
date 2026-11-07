# Predicting ICU Length of Stay After Cervical Spinal Cord Injury

## Research question

Can information recorded early in an ICU stay help predict the total ICU length of stay for adults with a cervical spinal cord injury?

The main analysis uses MIMIC-IV. MIMIC-III CareVue is analyzed separately as a secondary replication. The datasets are not combined, and the MIMIC-III analysis is not independent validation at a different hospital.

## Main results

The models predicted length of stay only weakly.

In the primary MIMIC-IV analysis, the cohort included 139 ICU stays from 138 patients. Random Forest had an R² of 0.071 for log-transformed length of stay, an R² of 0.028 in days, and a mean absolute error of 6.6 days. Its performance was not clearly different from the median baseline or the APACHE-II-based linear comparison. APACHE-II had an R² of 0.079 on the log scale and -0.015 in days.

In the separate MIMIC-III CareVue analysis, the primary cohort included 88 stays from 88 patients. Random Forest had an R² of 0.183 on the log scale, an R² of 0.009 in days, and a mean absolute error of 6.7 days. It performed better than the median baseline, but was not clearly different from the APACHE-II-based linear comparison.

The results do not show that the machine-learning model reliably predicts an individual patient's ICU stay better than APACHE-II. The cohorts are small, so the estimates are uncertain.

## Approach

- Selected adults with cervical spinal cord injury diagnosis codes: ICD-9 806.0x and, in MIMIC-IV, ICD-10 S14.1xxA.
- Kept the first ICU stay of each qualifying hospital admission. A patient can still have more than one qualifying hospital admission, so the analysis has one row per stay, not one row per patient.
- Used vital signs and event counts from the first 24 hours. Laboratory values could be collected up to six hours before ICU admission and through the analysis window.
- Kept patients together when assigning stays to training and testing folds, to prevent the same patient appearing on both sides.
- Compared a median baseline, an APACHE-II-based linear model, Lasso, Random Forest, and XGBoost.
- Tested four alternative settings: include short stays, use a 12-hour window, use a 48-hour window, and remove the 99-day stay.

Predictions were capped to the training-fold range. A separate check compares results with and without this cap. Lasso uses a fixed penalty of 0.01; it was not tuned with inner cross-validation.

## Limitations

The study uses small samples from one hospital. Length of stay also depends on events and decisions that are not fully represented in the model, including treatment response, complications, death, and discharge practices. Some APACHE-II chronic-condition information comes from diagnosis codes finalized after day one.

The APACHE-II score is an approximation based on available fields and a project-defined ICD-code mapping. The implementation does not double creatinine points for acute renal failure. APACHE-IV was not calculated. Its method is published, but key diagnosis mappings and inputs are not directly available in a usable form here.

The literature range cited in the case-study brief is context, not a direct benchmark: the reported metric is labeled differently across the brief and the paper abstract. This analysis is for a case study and is not a clinical decision tool.

## Files

- V4_ICU_LOS_cervical_SCI_analysis.ipynb — analysis code, saved results, and figures.
- results_FINAL/ — tables and figures produced by the notebook.

Raw MIMIC data are not included. Reproducing the analysis requires approved access to MIMIC-III and MIMIC-IV and the Python packages used in the notebook. Run the notebook from top to bottom; scanning the large clinical tables can take time.

## References

- Johnson AEW, et al. MIMIC-IV, a freely accessible electronic health record dataset. Scientific Data. 2023. [doi:10.1038/s41597-022-01899-x](https://doi.org/10.1038/s41597-022-01899-x).
- Johnson AEW, et al. MIMIC-III, a freely accessible critical care database. Scientific Data. 2016. [doi:10.1038/sdata.2016.35](https://doi.org/10.1038/sdata.2016.35).
- Knaus WA, et al. APACHE II: a severity of disease classification system. Critical Care Medicine. 1985;13(10):818–829. [PubMed](https://pubmed.ncbi.nlm.nih.gov/3928249/).
- Verburg IWM, et al. Which models can I use to predict adult ICU length of stay? A systematic review. Critical Care Medicine. 2017;45(2):e222–e231. [doi:10.1097/CCM.0000000000002054](https://doi.org/10.1097/CCM.0000000000002054).
