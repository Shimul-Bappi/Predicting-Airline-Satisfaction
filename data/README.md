# Data

The competition data is **not** stored in this repository.

Download it from the competition page:
<https://www.kaggle.com/competitions/playground-series-s6e10/data>

Required files: `train.csv`, `test.csv`, `sample_submission.csv`.

```bash
# with the Kaggle CLI (requires ~/.kaggle/kaggle.json and accepting the competition rules)
kaggle competitions download -c playground-series-s6e10 -p data
cd data && unzip playground-series-s6e10.zip
```

On Kaggle the files are already mounted at `/kaggle/input/competitions/playground-series-s6e10`.
