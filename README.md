# Resting-State EEG and MCMI Analysis

Nine Google Colab notebooks for the exploratory analysis of 19-channel resting-state EEG and Millon Clinical Multiaxial Inventory (MCMI) scores. The original cohort supplied 30 EDFs. The main analyses use 19 adults after quality control and age screening. Saved cell outputs document the original runs.

## Data access

**You need access to the project Google Drive or a compatible dataset of your own.** Ask the project owner to share the relevant files with the Google account you use in Colab. Opening the link does not grant access.

- [Project Drive folder](https://drive.google.com/drive/folders/1trcxi6l5z6bq7qCurZBCTrEDqbOh3-fU)
- [Original EEG recordings](https://drive.google.com/drive/folders/1XgQQpL-JhxdQmkpTW7ZaaMSIglC80lXM)
- [MCMI score workbook](https://drive.google.com/open?id=1EOvKbEfvLAQPTjizNE9DEUR9Ik8xBsiy)

The repository contains notebooks and their saved outputs. The original recordings, score workbook, embedding files, and full report are stored separately.

### Use a dataset you already have

In each Colab runtime, mount Drive first if your paths are on Drive, then configure the paths before running the notebook setup cell.

```python
from google.colab import drive
import os

drive.mount('/content/drive')
os.environ['EEG_WORK'] = '/content/drive/MyDrive/eeg-mcmi-work'
os.environ['EEG_RAW'] = '/content/drive/MyDrive/eeg-mcmi-data/edf'
os.environ['EEG_REFERENCE_DIR'] = '/content/drive/MyDrive/eeg-mcmi-data/inputs'
```

Place the EDFs in `EEG_RAW` and the score workbook at `EEG_REFERENCE_DIR/scores.xlsx`. The workbook uses participant IDs in its first column (named `نام مراجع`), an age column named `سن`, and the original MCMI score column names (for example, `BR_8B`, `RAW_8B`, and `W_Score`). EDF filenames are of the form `Subject_01.edf` and must match the workbook IDs. The notebooks contain cohort-specific count checks, EDF layout assumptions, and electrode labels. For a different cohort, adapt those checks and the data loaders before interpreting results.

Notebook 02 contains a historical summary cell that reads two archived workbooks, `EEG_MCMI_Comprehensive_FA.xlsx` and `EEG_MCMI_advanced_FA.xlsx`. When using your own data, skip that summary cell and enable the full recomputation cells. Use extract mode in notebooks 05 and 06 to generate embeddings for your own data. Notebook 07 fetches a pinned public Brant checkpoint.

## Run order

Open the notebooks in Colab, then run the cells in order. Use the same working directory across notebooks. Enable the full QC run in notebook 01 for a fresh dataset.

| Notebook | Purpose |
|---|---|
| [01 · Signal QC](01_signal_qc.ipynb) | Preprocess EDFs, screen channels and recordings, and generate the analysis manifest. |
| [02 · Direct spectrum](02_direct_spectrum.ipynb) | Spectral power, occipital alpha, connectivity, and profile prediction. |
| [03 · Temporal variability](03_temporal_variability.ipynb) | Band-power variability and alpha step size. |
| [04 · Delta specificity](04_delta_specificity.ipynb) | Delta and 8B follow-up. Run before combining delta with LaBraM. |
| [05 · LaBraM](05_labram.ipynb) | Channel embeddings, associations, and profile prediction. |
| [06 · CBraMod](06_cbramod.ipynb) | Channel embeddings and associations. |
| [07 · Brant](07_brant.ipynb) | Embeddings, 4B prediction, and BR_8B prediction with and without delta variability. |
| [08 · Model convergence](08_model_convergence.ipynb) | T6 agreement across models and F8 overlap with delta. Requires notebooks 04–07. |
| [09 · Literature checks](09_literature_checks.ipynb) | Targeted comparisons with published findings. The prediction summary requires notebook 07. |

Use a recent Colab runtime with NumPy 2.x, SciPy, pandas, scikit-learn, Matplotlib, and openpyxl. Model cells document their checkpoint and additional dependencies. LaBraM and CBraMod extraction use PyTorch and Braindecode. Store regenerated data and results outside this repository.
