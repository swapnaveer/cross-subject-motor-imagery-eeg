# Cross-Subject Motor Imagery EEG Decoding

## Overview

This project explores whether EEG recordings from multiple participants can be used to classify imagined left- versus right-fist movements in a participant whose recordings were excluded from model training. The analysis uses the PhysioNet EEG Motor Movement/Imagery Database (EEGMMIDB).

This is an independent, exploratory analysis of offline EEG data. It is not a real-time brain-computer interface or a clinically validated decoder.

## Research question

Can a motor-imagery EEG decoder trained on other participants classify imagined left- versus right-fist movements in a participant excluded from training?

## Dataset and scope

- Dataset: [PhysioNet EEG Motor Movement/Imagery Database (EEGMMIDB)](https://physionet.org/content/eegmmidb/1.0.0/)
- Participants: S001–S015 (15 participants)
- Runs: 4, 8, and 12
- Trials: 675 total (45 per participant)
- Sampling frequency: 160 Hz
- Selected EEG channels: FC3, FC4, C3, Cz, C4, CP3, CP4
- Epoch: 0–4 seconds from the task annotation
- Preprocessing: 7–30 Hz band-pass filtering

## Methods

Two approaches were compared:

1. **CSP + LDA:** Common Spatial Patterns (four components, Ledoit-Wolf regularization) followed by shrinkage Linear Discriminant Analysis.
2. **Band-power + Logistic Regression:** Log-transformed mu (8–13 Hz) and beta (13–30 Hz) band-power features, followed by feature standardization and class-balanced Logistic Regression.

Models were evaluated using **leave-one-subject-out (LOSO) cross-validation**. In each fold, all trials from one participant were held out for testing, and model fitting was performed using the remaining participants. Balanced accuracy was also calculated.

## Results

| Model | Mean participant-level accuracy | Mean balanced accuracy |
|---|---:|---:|
| CSP + LDA | 59.1% | 58.5% |
| Band-power + Logistic Regression | 57.6% | 57.5% |

The difference between the approaches was small; the results do not establish a clear advantage for either model. Performance varied substantially across held-out participants. The highest CSP + LDA result was 91.1% for S007, while the mean accuracy across the other 14 participants was 56.8%.

These are exploratory results from one public dataset and a relatively small number of trials per participant. They should not be interpreted as evidence of robust generalization to new populations, recording systems, or real-time BCI use.

## Repository contents

- `notebooks/cross_subject_motor_imagery_analysis.ipynb` — complete analysis notebook, including preprocessing, model comparison, LOSO evaluation, plots, and interpretation.
- `results/cross_subject_summary.csv` — model-level summary metrics.
- `results/leave_one_subject_out_results.csv` — participant-level evaluation metrics.
- `results/subject_trial_counts.csv` — trial counts by participant.
- `results/cross_subject_accuracy.png` — participant-level CSP + LDA accuracy plot.

## Run the analysis

Use Python 3.10 or later.

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\\Scripts\\activate
pip install -r requirements.txt
jupyter lab
```

Open `notebooks/cross_subject_motor_imagery_analysis.ipynb` and run the cells in order. The notebook retrieves the EEG recordings through MNE-Python's EEGBCI dataset utilities and requires an internet connection for the initial download.

## References

- [PhysioNet EEGMMIDB](https://physionet.org/content/eegmmidb/1.0.0/)
- [MNE-Python EEGBCI dataset loader](https://mne.tools/stable/generated/mne.datasets.eegbci.load_data.html)
- [MNE-Python Common Spatial Patterns](https://mne.tools/stable/generated/mne.decoding.CSP.html)
- [scikit-learn Linear Discriminant Analysis](https://scikit-learn.org/stable/modules/lda_qda.html)
- [scikit-learn Logistic Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)

## Disclaimer

This repository contains an independent educational/research exploration using publicly available data. It is not intended for diagnosis, treatment, or clinical decision-making.
