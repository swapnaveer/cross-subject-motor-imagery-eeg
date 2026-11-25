# Cross-Subject Motor Imagery EEG Decoding

**Research question:** Can a motor-imagery EEG decoder trained on other participants classify imagined left- versus right-fist movements in a participant excluded from training?

In this project, I explored whether EEG recordings from multiple participants could be used to classify imagined left- and right-hand movements in a participant whose recordings were not included during model training.

Two classification approaches are compared:

1. Common Spatial Patterns (CSP) followed by Linear Discriminant Analysis (LDA).
2. Log-transformed mu- and beta-band power features followed by standardized Logistic Regression.

Model performance is assessed using leave-one-subject-out (LOSO) validation. In each fold, all trials from one participant are held out for testing, while the remaining participants are used for model fitting.

The analysis is an exploratory evaluation of cross-subject decoding.

## 1. Data and preprocessing

The analysis uses runs 4, 8, and 12 from the [PhysioNet EEG Motor Movement/Imagery Database (EEGMMIDB)](https://physionet.org/content/eegmmidb/1.0.0/) for participants S001–S015.

EEG recordings are filtered between 7 and 30 Hz, and epochs are extracted from 0 to 4 seconds relative to the task annotations. Seven channels are selected from central and adjacent sensorimotor regions: FC3, FC4, C3, Cz, C4, CP3, and CP4.

The analysis includes 15 participants and 675 trials.

## 2. Classification models

**CSP + LDA.** Common Spatial Patterns (CSP) identifies spatial filters that distinguish the two motor-imagery classes. The resulting features are classified using shrinkage Linear Discriminant Analysis.

**Band-power + Logistic Regression.** Log-transformed power is calculated for the mu (8–13 Hz) and beta (13–30 Hz) frequency bands for each selected channel. Features are standardized before classification with Logistic Regression.

The feature extraction for the band-power model is performed independently for each epoch. Scaling and classifier fitting occur within the training data in each validation fold.

## 3. Evaluation

Both models are evaluated using identical participant-level folds. In each iteration, one participant is held out for testing. CSP, LDA, feature scaling, and Logistic Regression are fitted using only the remaining participants. Predictions for the held-out participant are used to calculate accuracy and balanced accuracy.

Performance is summarized using participant-level accuracy and balanced accuracy. Pooled trial-level predictions are also retained for confusion-matrix analysis.

## 4. Results and limitations

Across the 15 participants, CSP + LDA achieved an average accuracy of 59.1% and an average balanced accuracy of 58.5%. The band-power + Logistic Regression model achieved an average accuracy of 57.6% and balanced accuracy of 57.5%. The difference between the two approaches was small, so these results do not show a clear advantage for either model.

Performance varied across participants. The highest CSP + LDA accuracy was 91.1% for participant S007. When S007 was excluded, the average CSP + LDA accuracy across the remaining 14 participants was 56.8%.

The analysis used 15 participants from one publicly available dataset, with 45 trials per participant. It uses offline EEG recordings and does not demonstrate performance in a real-time brain-computer interface. The models were evaluated on one dataset only, so the results may not carry over to other datasets or groups of people.

This work is an independent exploratory analysis of a publicly available EEG dataset. It evaluates offline classification performance.

## 5. Files

- `notebooks/cross_subject_motor_imagery_analysis.ipynb` — analysis notebook.
- `results/cross_subject_summary.csv` — participant-level model summary.
- `results/leave_one_subject_out_results.csv` — results for each held-out participant.
- `results/subject_trial_counts.csv` — trial counts by participant.
- `results/cross_subject_accuracy.png` — participant-level CSP + LDA accuracy plot.

## 6. Data Sources and Citations

- Dataset: PhysioNet EEG Motor Movement/Imagery Dataset (EEGMMIDB). https://doi.org/10.13026/C28G6P
- Dataset reference: Schalk, G., McFarland, D. J., Hinterberger, T., Birbaumer, N., & Wolpaw, J. R. (2004). BCI2000: A general-purpose brain-computer interface system. IEEE Transactions on Biomedical Engineering, 51(6), 1034–1043.
