# AI-vs-Real-Images-ML-Project
> **Academic Project Notice**  
> This project was developed as part of an academic Machine Learning assignment focused on supervised learning, image classification, feature engineering, hyperparameter tuning, and model evaluation.

# AI vs Real Images Classification 🧠🖼️

A Machine Learning project for classifying images as either **AI-generated** or **Real** using a full supervised learning pipeline.

The project focuses on building the pipeline manually and explaining each step clearly:

- Dataset loading
- Data balancing
- Train/Test spliting
- Feature engineering
- KNN implementation from scratch
- Hyperparameter experiments
- Grid Search with 5-Fold Cross Validation
- Final model evaluation

The learning algorithm used in this project is **K-Nearest Neighbors (KNN)**, implemented from scratch without using ready-made machine learning implementations such as `scikit-learn`.

---

## Shortcuts 🛣️

--> **Main notebook:**  
[AI_VS_Real_Images.ipynb](./AI_VS_Real_Images.ipynb)

--> **Dataset source:**  
[AI vs Real Images Dataset on Kaggle](https://www.kaggle.com/datasets/rhythmghai/ai-vs-real-images-dataset)

--> **Final model section:**  
Look for: `Final Model Evaluation on Test Set`

--> **Grid Search CV section:**  
Look for: `Grid Search Cross Validation`

---

## Table of Contents 📌

- [About the Project](#about-the-project-)
- [Dataset](#dataset-)
- [Dataset Download](#dataset-download-)
- [Project Workflow](#project-workflow-)
- [Feature Engineering Methods](#feature-engineering-methods-)
- [KNN From Scratch](#knn-from-scratch-%EF%B8%8F)
- [Model Evaluation](#model-evaluation-)
- [Grid Search Cross Validation](#grid-search-cross-validation-)
- [Final Results](#final-results-)
- [Project Structure](#project-structure-)
- [How to Run](#how-to-run-)
- [Use of AI Tools](#use-of-ai-tools-disclosure-%EF%B8%8F)
- [Final Conclusion](#final-conclusion-)

---

## About the Project 📌

The goal of this project is to classify images into two classes:

- `AI` — AI-generated images
- `Real` — real images

The project uses a Kaggle dataset containing AI-generated and real images from several categories, such as:

- animals
- city
- food
- nature
- people

Since the original dataset was imbalanced, we applied undersampling to create a balanced dataset before training.

---

## Dataset 📦

The dataset was taken from Kaggle:

**AI vs Real Images Dataset**  
https://www.kaggle.com/datasets/rhythmghai/ai-vs-real-images-dataset

After balancing, the dataset contains:

| Class | Number of Images |
| --- | ---: |
| AI-generated | 250 |
| Real | 250 |

Final split:

| Set | Number of Images |
| --- | ---: |
| Train | 400 |
| Test | 100 |

---

## Dataset Download 📦

The original dataset is available on Kaggle:

[AI vs Real Images Dataset on Kaggle](https://www.kaggle.com/datasets/rhythmghai/ai-vs-real-images-dataset)

For convenience, the fixed ZIP file used in this project is also available as a GitHub Release asset:

[Download dataset_fixed.zip](https://github.com/ShlomiOnTheBeast/AI-vs-Real-Images-ML-Project/releases/download/v1.0/dataset_fixed.zip)

The notebook automatically downloads and extracts the dataset during execution.

---

## Project Workflow 🔄

The project follows a complete supervised machine learning workflow:

1. Load the dataset
2. Convert image paths and labels into a DataFrame
3. Analyze class distribution
4. Handle data imbalance using undersampling
5. Split the dataset into train and test sets
6. Extract image features
7. Implement KNN from scratch
8. Run baseline experiment
9. Run hyperparameter experiments
10. Perform Grid Search with 5-Fold Cross Validation
11. Train the final selected model on the full training set
12. Evaluate the final model once on the test set

---

## Feature Engineering Methods 🧪

A major part of the project was testing different ways to represent images numerically.

We implemented and compared four feature engineering methods:

| Method | Description | Feature Size |
| --- | --- | ---: |
| RGB Flatten | Resize image, normalize RGB pixels, flatten into a vector | 3072 |
| Grayscale Flatten | Convert to grayscale, resize, normalize, flatten | 1024 |
| Color Histogram | Represent RGB color distribution using histograms | depends on bins |
| Grayscale Histogram | Represent brightness distribution using grayscale histogram | depends on bins |

### Why Feature Engineering Matters

KNN compares samples using distances.

Therefore, the way an image is represented has a major impact on model performance.

Raw flattened pixels keep exact pixel positions, but they may not capture useful global patterns.  
Histogram-based features summarize color or brightness distributions, which worked much better for this dataset.

---

## KNN From Scratch ⚙️

The K-Nearest Neighbors algorithm was implemented manually.

The implementation includes:

- `fit()` function
- `predict_one()` function
- `predict()` function
- Euclidean distance
- Manhattan distance
- configurable `k`
- majority voting

The project does not use `scikit-learn` for the KNN implementation.

### KNN Logic

For each test image:

1. Calculate the distance between the test image and all training images.
2. Select the `k` nearest neighbors.
3. Count the labels of those neighbors.
4. Predict the majority label.

---

## Model Evaluation 📊

The project evaluates the models using several metrics:

- Accuracy
- Precision
- Recall
- F1-score
- Macro F1-score
- Confusion Matrix

### Main Metric

The main evaluation metric is:

**Macro F1-score**

Macro F1 was selected because both classes are equally important:

- AI-generated images
- Real images

Accuracy alone can be misleading if the model predicts mostly one class.

---

## Hyperparameter Experiments 🧮

We tested several hyperparameter combinations.

For KNN:

| Hyperparameter | Values |
| --- | --- |
| `k` | 1, 3, 5, 7, 9, 11 |
| `distance_metric` | Euclidean, Manhattan |

For histogram-based feature engineering:

| Hyperparameter | Values |
| --- | --- |
| `bins` | 8, 16, 32 |

These experiments helped compare feature engineering methods and understand how different parameters affect performance.

---

## Grid Search Cross Validation 🔍

To select the final model correctly, we used **Grid Search with 5-Fold Cross Validation**.

The Grid Search included:

- 4 feature engineering methods
- 6 values of `k`
- 2 distance metrics
- 3 histogram bin values for histogram methods

In total, the Grid Search evaluated:

```text
96 configurations
```

Each configuration was evaluated using:
`5-Fold Cross Validation`

For each configuration:
1. The training set was split into 5 folds.
2. The model was trained on 4 folds.
3. The model was validated on the remaining fold.
4. This process was repeated 5 times.
5. The average validation Macro F1-score was calculated.

The final model was selected according to the highest **mean validation Macro F1-score**.
This approach prevents choosing the final model directly according to the test set.

---

## Final Selected Model 🏆
The best configuration selected by Grid Search CV was:

| Parameter | Value |
| :--- | :--- |
| Feature Method | Color Histogram |
| Image Size | 64x64 |
| Bins | 32 |
| k | 9 |
| Distance Metric | Manhattan |

After selecting this configuration, the final KNN model was trained on the full training set and evaluated once on the test set.

---

## Final Results ✅
The final selected model achieved:

| Metric | Score |
| :--- | :--- |
| Test Accuracy | 0.84 |
| Test Macro F1 | 0.8399 |

### Final Confusion Matrix Summary
| Class | Correctly Classified |
| :--- | :--- |
| AI-generated | 43 / 50 |
| Real | 41 / 50 |

### Per-Class Performance
| Class | F1-score |
| :--- | :--- |
| AI | 0.8431 |
| Real | 0.8367 |

---

## Comparison to Baseline
Compared to the baseline model, the final selected model improved significantly:

| Model | Accuracy | Macro F1 |
| :--- | :--- | :--- |
| Baseline RGB Flatten KNN | 0.48 | 0.3404 |
| Final Selected Model | 0.84 | 0.8399 |

---

## Important Note About the Results ⚠️
The project includes both:
* Exploratory test-set experiments
* The final selected model chosen by Grid Search CV

Some exploratory experiments may show slightly different or even slightly higher test-set results. 

However, the **Final Selected Model** is considered the main result of the project because it was selected using the correct machine learning process:
1. Model selection using cross-validation on the training set.
2. Training the final model on the full training set.
3. Evaluating the final model once on the test set.

This prevents choosing the final model directly based on the test set.

---

## Project Structure 🧌
| File | Short Summary |
| :--- | :--- |
| `AI_VS_Real_Images.ipynb` | Full project notebook with code, explanations, outputs, visualizations, experiments, Grid Search CV, and final evaluation |
| `README.md` | Project overview and documentation |

---

## How to Run 🏃
This project was developed and tested in **Google Colab**.

### Recommended Way
1. Open the notebook in Google Colab.
2. Run all cells from top to bottom.
3. The dataset is downloaded inside the notebook.
4. Outputs, visualizations, experiment tables, and final results are displayed inside the notebook.

### Requirements
The notebook uses common Python libraries:
```python
numpy
pandas
matplotlib
PIL
zipfile
pathlib
gdown
hashlib
```

**Note:** No ready-made KNN implementation is used. The KNN algorithm is implemented manually in the notebook.

---

## How to Use the Notebook 👨‍💻

The notebook is organized as a full machine learning pipeline.

Recommended reading order:

1. Start with the project introduction and dataset explanation.
2. Review the dataset loading and balancing steps.
3. Continue to the feature engineering methods.
4. Review the KNN implementation from scratch.
5. Examine the baseline experiment.
6. Review the hyperparameter experiments.
7. Focus on the Grid Search Cross Validation section.
8. Review the final model evaluation and conclusion.

The most important sections are:

| Section | Purpose |
| :--- | :--- |
| Dataset Loading | Loads and organizes the images |
| Feature Engineering | Converts images into numerical vectors |
| KNN From Scratch | Implements the learning algorithm |
| Hyperparameter Experiments | Compares different configurations |
| Grid Search CV | Selects the final model correctly |
| Final Evaluation | Evaluates the final model on the test set |


---

## Key Takeaways 🧠

* Raw flattened pixels were not strong enough for this task.
* Histogram-based features performed much better.
* Color Histogram was the strongest feature representation.
* Manhattan distance performed well with histogram features.
* Macro F1 was more informative than Accuracy because both classes are important.
* Grid Search CV allowed us to select the final model without using the test set for model selection.

---

## Use of AI Tools (Disclosure) 🖨️

This project made limited use of AI-based tools as a productivity aid during development, debugging, explanation writing, and documentation.

### Purpose of Use

AI tools were used for:
* Planning the notebook structure
* Improving explanations and markdown sections
* Reviewing the KNN implementation logic
* Explaining evaluation metrics
* Helping document feature engineering methods
* Improving README wording

All code, outputs, and explanations were reviewed and understood by the project members.

---

## Final Conclusion 🧠

The main conclusion of this project is that feature representation has a major impact on model performance.

The baseline RGB Flatten representation performed poorly because raw pixel positions were not enough to separate AI-generated and real images.

Histogram-based features performed much better.

The final selected model used **Color Histogram** features with KNN and achieved strong balanced performance on both classes.

This project demonstrates the importance of:
* proper preprocessing
* feature engineering
* hyperparameter tuning
* cross validation
* correct final test evaluation
