# Celiac Disease Lab Data – EDA and Modeling

This project explores laboratory and clinical data related to celiac disease using basic data analysis and machine learning in R.

The goal is to understand the data, look for patterns, and build simple models that can help predict whether a patient is diagnosed with celiac disease based on lab measurements and symptoms.  
Special attention is given to false negatives, since missing a diagnosis can delay treatment and cause long-term complications.

## Data

The dataset was obtained from Kaggle:

Celiac Disease Lab Data  
https://www.kaggle.com/datasets/jackwin07/celiac-disease-coeliac-disease

The original dataset was shuffled and balanced to include the same number of diagnosed and non-diagnosed patients.  
This processed version was saved and reused to keep results consistent.

The dataset itself is not included in this repository.

## What was done

The analysis includes:

- Basic exploratory data analysis (EDA)  
- Data cleaning and feature encoding  
- Visualization of numeric feature distributions  
- Correlation analysis  
- Dimensionality reduction using PCA  
- Supervised classification using:
  - Decision tree (C5.0)  
  - k-nearest neighbors (KNN)  
  - Random forest  

Model performance is evaluated using confusion matrices, accuracy, and ROC curves.

## Main results

- The decision tree model is easy to interpret but underperforms compared to the other methods.  
- KNN gives moderate results and is sensitive to feature scaling.  
- The random forest model performs best overall and substantially reduces false negatives. 

---

## Files

- `celiac_project.Rmd` – main analysis notebook  
- `celiac_project.html` – knitted output  
- `README.md` – project description  

---

## Notes

This project is intended as a learning and exploration exercise.  
The models are simple and not intended for clinical use.

---

## Next steps

- Use cross-validation instead of a single train/test split  
- Tune model hyperparameters  
- Add feature importance analysis  
- Explore multi-class classification and symptom-based subtypes  
