# Student Performance Factors Analysis

### Basic Information

* **Members:** Dana Ortiz, [dana.ortiz@gwmail.gwu.edu](mailto:dana.ortiz@gwmail.gwu.edu)
* **Date:** December 2025
* **Model Version:** 1.3
* **License:** MIT
* **Model Implementation Code:** [SLP_CODE_FINAL.ipynb](SLP_CODE_FINAL.ipynb)

### Intended Use

* **Intended Uses:** This model is an educational example of a supervised learning system that predicts a student’s final exam score based on academic, behavioral, family, and environmental factors. The use case mirrors how an educational analytics company might explore factors associated with student performance for academic purposes.
* **Out-of-Scope Use Cases:** Any real-world use for grading, admissions, discipline, student tracking, diagnosis of learning disabilities, or automated intervention decisions. This model should not be used to make official decisions about individual students. It is strictly for educational demonstrations and exploratory analysis.

### Training Data

* **Data Dictionary:**

| Name                           | Modeling Role | Measurement Level   | Description                                                                           |
| ------------------------------ | ------------- | ------------------- | ------------------------------------------------------------------------------------- |
| **Hours_Studied**              | input         | int                 | Number of hours spent studying per week                                               |
| **Attendance**                 | input         | int                 | Percentage of classes attended                                                        |
| **Parental_Involvement**       | input         | ordinal categorical | Level of parental involvement, mapped from Low/Medium/High to 1/2/3                   |
| **Access_to_Resources**        | input         | ordinal categorical | Availability of educational resources, mapped from Low/Medium/High to 1/2/3           |
| **Extracurricular_Activities** | input         | binary categorical  | Whether the student participates in extracurricular activities, mapped Yes/No to 1/0  |
| **Sleep_Hours**                | input         | int                 | Average number of hours of sleep per night                                            |
| **Previous_Scores**            | input         | int                 | Scores from previous exams                                                            |
| **Motivation_Level**           | input         | ordinal categorical | Student motivation level, mapped from Low/Medium/High to 1/2/3                        |
| **Internet_Access**            | input         | binary categorical  | Whether the student has internet access, mapped Yes/No to 1/0                         |
| **Tutoring_Sessions**          | input         | int                 | Number of tutoring sessions attended per month                                        |
| **Family_Income**              | input         | ordinal categorical | Family income level, mapped from Low/Medium/High to 1/2/3                             |
| **Teacher_Quality**            | input         | ordinal categorical | Quality of teachers, mapped from Low/Medium/High to 1/2/3                             |
| **School_Type**                | input         | binary categorical  | Type of school attended, mapped Public/Private to 0/1                                 |
| **Peer_Influence**             | input         | ordinal categorical | Influence of peers on academic performance, mapped Negative/Neutral/Positive to 1/2/3 |
| **Physical_Activity**          | input         | int                 | Average number of hours of physical activity per week                                 |
| **Learning_Disabilities**      | input         | binary categorical  | Whether the student has a learning disability, mapped Yes/No to 1/0                   |
| **Parental_Education_Level**   | input         | ordinal categorical | Highest education level of parents, mapped High School/College/Postgraduate to 1/2/3  |
| **Distance_from_Home**         | input         | ordinal categorical | Distance from home to school, mapped Near/Moderate/Far to 1/2/3                       |
| **Gender**                     | input         | binary categorical  | Gender of the student, mapped Male/Female to 0/1                                      |
| **Exam_Score**                 | target        | int                 | Final exam score                                                                      |

* **Source of Training Data:** StudentPerformanceFactors.csv, the uploaded student performance factors dataset used for the project.
* **How training data was divided into training and validation data:** The final linear regression model used a random 80/20 train-test split with `random_state=42`. Rows with missing values were dropped before fitting the final linear regression model.
* **Number of rows in training and validation data:**

  * Original dataset rows: 6,607
  * Rows after dropping missing values for final linear regression: 6,378
  * Training rows: 5,102
  * Validation/test rows: 1,276

### Test Data

* **Source of test data:** The project did not use a separate external test dataset. The test data came from the 20% holdout split of the uploaded StudentPerformanceFactors.csv file.
* **Number of rows in test data:** 1,276 rows for the final linear regression model.
* **State any differences in columns between training and test data:** There were no differences in feature columns between the training and test split. Both used the same encoded predictor variables. The target column, `Exam_Score`, was removed from the input features and used only for evaluation.

### Model Details

* **Columns used as inputs in the final model:** `Hours_Studied`, `Attendance`, `Parental_Involvement`, `Access_to_Resources`, `Extracurricular_Activities`, `Sleep_Hours`, `Previous_Scores`, `Motivation_Level`, `Internet_Access`, `Tutoring_Sessions`, `Family_Income`, `Teacher_Quality`, `School_Type`, `Peer_Influence`, `Physical_Activity`, `Learning_Disabilities`, `Parental_Education_Level`, `Distance_from_Home`, `Gender`
* **Column(s) used as target(s) in the final model:** `Exam_Score`
* **Type of model:** Multiple Linear Regression
* **Comparison model tested:** Decision Tree Regressor
* **Software used to implement the model:** Python, pandas, NumPy, scikit-learn, Matplotlib, Seaborn, Google Colab
* **Version of the modeling software:** Python 3.14
* **Hyperparameters or other settings of your model:**

Final model:

```python
LinearRegression()
```

Train-test split:

```python
train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

Comparison model:

```python
DecisionTreeRegressor(
    max_depth=5,
    random_state=42
)
```

### Quantitative Analysis

* **Metrics Used to Evaluate:** Mean Squared Error, Root Mean Squared Error, and R².

| Model                       | Training MSE | Training RMSE | Training R² | Validation MSE | Validation RMSE | Validation R² |
| --------------------------- | ------------ | ------------- | ----------- | -------------- | --------------- | ------------- |
| **Linear Regression**       | **4.30**     | **2.07**      | **0.72**    | **4.15**       | **2.04**        | **0.73**      |
| **Decision Tree Regressor** | **6.96**     | **2.64**      | **0.55**    | **6.44**       | **2.54**        | **0.54**      |

The linear regression model was selected as the final model because it had a lower validation MSE and higher validation R² than the decision tree model. The linear regression model also provides clearer coefficient-based interpretation, making it more useful for explaining which factors are associated with higher or lower exam scores.

### Ethical Considerations

* **Potential negative impacts of using this model:**

  * *Math or Software Problems:* The model uses simple numeric encodings for categorical variables, which may impose artificial ordering or spacing between categories. The final linear regression model also drops rows with missing values, which may remove certain groups of students from the analysis. Because the model uses a random split from one dataset, its performance may not generalize to students from other schools, districts, years, or educational systems.
  * *Real World Risks:* If misused, this model could encourage schools or organizations to label students as “low-performing” based on personal, family, or demographic characteristics. Variables such as learning disabilities, family income, parental education, internet access, and distance from home may reflect structural inequalities rather than individual effort. Using predictions without human review could stigmatize students, reinforce bias, or lead to unfair resource allocation.

* **Uncertainties relating to the impacts of using the model:**

  * *Math or Software Uncertainties:* The model assumes mostly linear and additive relationships between student factors and exam scores. It does not fully capture complex interactions, school-level effects, teaching differences, curriculum quality, or changes over time. The dataset source and collection process are not fully documented in the uploaded files, so representativeness is uncertain.
  * *Real World Uncertainties:* It is unclear whether the relationships in this dataset would hold for other schools or student populations. A factor that appears predictive in the model may not be causal. For example, internet access, parental involvement, and tutoring may be connected to broader socioeconomic conditions rather than directly causing exam score changes.

* **Unexpected Results:** The linear regression model performed better than the decision tree model, with a validation MSE of 4.15 and R² of 0.73 compared with the decision tree’s validation MSE of 6.44 and R² of 0.54. This suggests that the relationships in this dataset may be captured well by a simpler linear model. One important issue is that the printed regression equation in the project materials includes a `Passed` term, but `Passed` is not present in the uploaded CSV and is not created in the uploaded notebook. This should be corrected before final submission by either removing `Passed` from the equation or adding clear code that creates and documents the variable.

### AI Use Disclosure

AI tools were used during this project for README drafting. All final code, outputs, interpretations, and conclusions should be reviewed by the project author before submission.
