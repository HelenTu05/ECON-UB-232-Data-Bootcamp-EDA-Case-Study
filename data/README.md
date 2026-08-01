# Data

This case study uses the **Home Credit Default Risk** dataset, originally released
for the [Kaggle competition of the same name](https://www.kaggle.com/competitions/home-credit-default-risk/data).

| File | Contents | Tracked in git? |
| --- | --- | --- |
| `application_data.csv` | One row per loan application, 122 columns | No — download it |
| `previous_application.csv` | Prior applications for the same clients | No — download it |
| `columns_description.xlsx` | Data dictionary for every column | Yes |

The two CSVs are several hundred MB and are excluded via `.gitignore`. The data
dictionary is small and is committed so the column semantics can be read without
downloading anything.

## Download

With the Kaggle CLI:

```bash
kaggle competitions download -c home-credit-default-risk -p data/ --unzip
```

Or manually from the
[competition data page](https://www.kaggle.com/competitions/home-credit-default-risk/data)
(a free Kaggle account and acceptance of the competition rules are required).

The expected layout is:

```
data
├── application_data.csv        # downloaded
├── previous_application.csv    # downloaded
└── columns_description.xlsx
```

## Pointing the notebook elsewhere

`notebooks/01_loan_default_eda.ipynb` reads `../data` by default. To use a
different location — a Google Drive mount on Colab, for instance — set an
environment variable before launching Jupyter:

```bash
export LOAN_DATA_DIR=/path/to/data   # Windows: set LOAN_DATA_DIR=...
```

On Colab:

```python
from google.colab import drive
drive.mount('/content/drive')
import os
os.environ['LOAN_DATA_DIR'] = '/content/drive/MyDrive/your-folder'
```
