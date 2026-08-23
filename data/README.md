# Data

## Source

**Celiac Disease Lab Data** — <https://www.kaggle.com/datasets/jackwin07/celiac-disease-coeliac-disease>

> ### ⚠️ The data is not included in this repository
>
> The Kaggle source is published under an **"Unknown"** license — the uploader granted no redistribution
> rights, so neither the raw file nor any derivative of it is redistributed here. `data/*.csv` is
> gitignored for this reason.
>
> **To run the analysis yourself**, download the dataset from the link above and follow
> [Preparing the data](#preparing-the-data) to produce `data/shuffled_data.csv`.
>
> To simply *read* the results, no download is needed — [`celiac_project.md`](../celiac_project.md)
> contains every figure, table, and confusion matrix.

## `shuffled_data.csv`

726 rows × 15 columns. Class-balanced by construction: 363 diagnosed (`yes`) and 363 non-diagnosed (`no`).

| Column | Description |
|---|---|
| `Age` | Patient age in years |
| `Gender` | `Male` / `Female` |
| `Diabetes` | Comorbid diabetes, `Yes` / `no` |
| `Diabetes.Type` | `1` / `2`, `NA` when non-diabetic |
| `Diarrhoea` | `fatty` / `watery` / `inflammatory` |
| `Abdominal` | Abdominal pain, `yes` / `no` |
| `Short_Stature` | `PSS` (proportionate) / `DSS` (disproportionate) / `Variant` |
| `Sticky_Stool` | `yes` / `no` |
| `Weight_loss` | `yes` / `no` |
| `IgA`, `IgG`, `IgM` | Serum immunoglobulin levels |
| `Marsh` | Marsh histological score (`0`, `1`, `2`, `3a`, `3b`, `3c`) |
| `cd_type` | Clinical subtype, `none` when not diagnosed |
| `Disease_Diagnose` | **Target.** `yes` / `no` |

### Two columns that leak the target

- **`cd_type`** names the celiac subtype and is `none` for every non-diagnosed patient, so it encodes the
  label directly. It is dropped before modeling.
- **`Marsh`** is the histological score used to *make* the diagnosis clinically. It is retained in the main
  models — as the strongest available predictor — but the report also fits a random forest without it, since
  a model leaning on `Marsh` is close to predicting the label from the label. See the "Limitations" section
  of the main README.

## Preparing the data

Download `celiac_disease_lab_data.csv` from the Kaggle link above, place it in this directory, then run:

```r
set.seed(123)

data <- read.csv("celiac_disease_lab_data.csv")

# Shuffle, then take an equal number of each class
shuffled_data <- data[sample(nrow(data)), ]
filtered_no  <- shuffled_data[shuffled_data$Disease_Diagnose == "no",  ][1:363, ]
filtered_yes <- shuffled_data[shuffled_data$Disease_Diagnose == "yes", ][1:363, ]
combined_data <- rbind(filtered_no, filtered_yes)

# Standardise categorical fields
combined_data$Marsh <- gsub("marsh type", "", combined_data$Marsh)
combined_data$Marsh <- gsub("none", "0", combined_data$Marsh)
combined_data$Diabetes.Type <- gsub("Type ", "", combined_data$Diabetes.Type)
combined_data$Diabetes.Type <- gsub("None", NA, combined_data$Diabetes.Type)

write.csv(combined_data, "shuffled_data.csv", row.names = FALSE)
```

This yields 726 rows: 363 diagnosed and 363 not. Note that the file used to produce the committed report was
generated before this script was written down, so re-running it will not reproduce that exact file
row-for-row — the sampled subset differs. Results should be very close but will not match to the decimal.
