# Yelp_data_analysis

## 🔍 Overview

This project aims to identify what drives success in the restaurant industry in the United States. Running a restaurant is challenging — around 30% of restaurants in the U.S. close within their first three years.

To uncover key strategies for success, I analyzed the **Yelp Open Dataset**, which contains more than **2.7 million restaurant reviews** across the U.S.

I approached the analysis in three major steps:

1. **Defined three performance metrics** for restaurants — *stability score*, *loyalty score*, and *reliability score*.
2. **Validated the metrics** using statistical hypothesis testing.
3. **Built machine learning models** to determine what businesses should do to improve these scores and ultimately enhance their performance.

---

## 🗂 About the Dataset
- **[Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/)**
    - 150K business records
    - 6.9M review records (2005–2022)
- Filtered only restaurant businesses and selected restaurants with at least 25 reviews
    
    → Final dataset: **20K restaurants, 2.7M reviews**
- ERD (Entity Relationship Diagram) :
  
  <img width="1609" height="1319" alt="image" src="https://github.com/user-attachments/assets/86968ed3-146e-4bd7-aaf2-46fa81b78fd0" />

---

## 📚 Python Libraries

- **Basic:** `os`
- **Preprocessing:** `pandas`, `numpy`
- **Statistics:** `scipy`, `statsmodels`
- **Machine Learning:** `sklearn`, `catboost`, `optuna`
- **Visualization:** `matplotlib`, `seaborn`

---

## 🎯 Defining the Scores

I defined three metrics to evaluate restaurant performance.

### 1. Stability Score

The stability score measures a restaurant’s resilience — how stable it is and how effectively it recovers from negative events such as rating drops or critical reviews.

It consists of **two components: base score and recovery score**.

1. **Base Score**
    - Calculated using average star rating, rating standard deviation, review count, and the ratio of low-star reviews.
    - Represents the fundamental stability of the business.
    - Used only the 5th–95th percentiles to reduce outlier influence.
    - Normalized using min–max scaling.
2. **Recovery Score**
    - Measures how quickly and strongly the restaurant’s rating rebounds after a “low-rating shock.”
    - “A low-rating shock” refers to a rapid decline in ratings of more than **0.8 points**, caused by negative reviews.
    - Faster and stronger recovery → higher score.
    - Normalized using min–max scaling.

- **Formula:**
  
  ```
  (0.5 * base_score + 0.5 * recovery_score) * 100
  ```

  
- **Distribution:**
  
  <img width="990" height="590" alt="image" src="https://github.com/user-attachments/assets/353caf3f-3cb9-4455-8479-c0b45ed5f782" />

### 2. Loyalty Score

The loyalty score reflects how beloved a restaurant is — how many regular customers it has and how often they visit.

It is composed of **three components: regular customer score, check-in count, and check-in interval**.

1. **Regular Customer Score**
    - Represents the ratio of repeat customers and their ratings.
    - Defined regular customers as users who left multiple reviews for the same restaurant.
    - Base score = 0.5
        - up to +0.3 based on regular customer ratio
        - up to ±0.2 based on their ratings
2. **Check-in Count**
    - Measures how many times the restaurant has been visited.
    - Calculated from check-in date records.
    - Normalized using min–max scaling.
3. **Check-in Interval**
    - Indicates visit frequency.
    - Complements check-in count, which can be biased by long operating periods.
    - Shorter intervals → higher score.
    - Normalized using min–max scaling.

- **Formula:**

  ```
  (0.5 * regular_customer_score + 0.2 * checkin_count + 0.3 * checkin_interval) * 100
  ```

  
- **Distribution:**
  
  <img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/6bff8160-b47d-4a5c-9515-065d18da3c50" />


### 3. Reliability Score

The reliability score measures how many reviews come from influential users, reflecting the tendency of customers to trust restaurants recommended by well-known and credible reviewers.

It is composed of **expert review ratio and elite review ratio**.

1. **Expert Review Ratio**
    - “Experts” are users who focuses on reviewing specific food categories.
    - If the restaurant’s category matches one of a user’s top 3 categories, that review is considered an expert review.
    - The score reflects the ratio of expert reviews to total reviews.
2. **Elite Review Ratio**
    - Yelp selects “elite” users each year based on engagement and influence.
    - A review is considered “elite review” if the user held elite status at the time of writing.
    - The score reflects the ratio of elite reviews to total reviews.

- **Formula:**

  ```
  (0.4 * expert_review_ratio + 0.6 * elite_review_ratio) * 100
  ```

  
- **Distribution:**

  <img width="989" height="590" alt="image" src="https://github.com/user-attachments/assets/f63c6bc9-e22a-475b-b993-86f574119717" />

---

## 🧪 Statistical Hypothesis Testing

To validate the defined scores, I conducted several hypothesis tests.

### 1. Stability Score

<img width="691" height="468" alt="image" src="https://github.com/user-attachments/assets/c4785a86-a8cd-4b2e-82b4-ce9266d5cb3d" />

- **Result:**
  
  |Metric|Value|
  |------|---|
  |X|Stability score|
  |y|Business closure|
  |Method|Logistic Regression|
  |p-value|< 0.001|
  |Pseudo R²|0.012|

- Because restaurants can close for unrelated reasons (e.g., relocation, retirement), I identified more “true” closures by segmenting based on ratings and review counts.
- Overall, higher stability scores were associated with lower closure probabilities.
  
### 2. Loyalty Score

  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/8af91bf8-b519-4c7a-88a0-ef18d51a780f" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/a6d63eab-ddab-4897-95a3-76f1b6fa028a" alt="image2"></td>
    </tr>
  </table>

- **Result:**

  |Metric|Value|
  |------|---|
  |X|Loyalty score group|
  |y|Star rating|
  |Method|Welch’s ANOVA|
  |p-value|< 0.001|
  |F-statistic|1875|

- Restaurants with more loyal customers tend to maintain higher ratings.
- Higher loyalty groups showed higher average ratings and lower variance.

### 3. Reliability Score

  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/641b7672-1025-4ddd-b567-40da6808d15d" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/d360f44a-75b4-4086-a6a4-caee42c8a2e5" alt="image2"></td>
    </tr>
  </table>

- **Result:**
  
  |Metric|Value|
  |------|---|
  |X|Reliability score group|
  |y|Review count|
  |Method|Welch’s ANOVA|
  |p-value|< 0.001|
  |F-statistic|284|

- Restaurants with more expert/elite reviews tend to attract more visitors and accumulate more reviews.
- Higher reliability groups had significantly more reviews on average.

---

## 🤖 Machine Learning

I built machine learning regression models to predict each of the three scores.

From the results, I identified key features that increase or decrease the scores and used them to develop actionable business recommendations.

### 🔧 ML Preprocessing

1. **Feature Engineering**
    - Added derived features such as:
        - service types offered
        - nearby restaurant density
        - review-based metrics
        - VADER sentiment score
2. **Preprocessing**
    - **Missing value imputation**
        - `alcohol`: “full_bar” for bar/nightlife categories; otherwise “none”
        - `ambience`: filled missing values with “unknown”
    - **Outlier removal**
        - Removed restaurants with `total_weekly_operating_hours` == 0
    - **Log transformation**
        - Applied to right-skewed columns (e.g., `neighbor_density`, `review_count`, `tip_count`)

### ⚙ Machine Learning Modeling

1. Evaluated 9 baseline models using default parameters
    - Example: Model Performance for Loyalty Score
      
      | Model            | RMSE | MAE | R²    |
      |------------------|------|-----|-------|
      | CatBoost         | 4.66 | 3.56 | 0.6953 |
      | LightGBM         | 4.65 | 3.57 | 0.6962 |
      | Gradient Boost   | 4.66 | 3.58 | 0.6956 |
      | Random Forest    | 4.72 | 3.63 | 0.6870 |
      | SVM              | 4.85 | 3.73 | 0.6697 |
      | XGBoost          | 4.87 | 3.74 | 0.6673 |
      | AdaBoost         | 5.15 | 4.11 | 0.6283 |
      | KNN              | 6.00 | 4.72 | 0.4943 |
      | Decision Tree    | 6.94 | 5.31 | 0.3239 |

2. Selected **CatBoost** for its stable performance across all scores
3. Used **Optuna** to optimize CatBoost hyperparameters (minimizing MAE)
4. Final model performance
   
      |Metrics|MAE|R²|
      |------|---|---|
      |Stability Score|8.68|0.7035|
      |Loyalty Score|3.56|0.6968|
      |Reliability Score|3.09|0.6547|

---

## 💡 Model Insights

### 1. Stability Score

<img width="771" height="740" alt="image" src="https://github.com/user-attachments/assets/05cab047-2e26-4951-a07a-747a25e451d2" />

- **Top features:** `avg_sentiment`, `loyalty_score`, `tip_count`, `avg_review_length`, `store_status`
- **Negative influences**
    - Long, detailed negative reviews (`avg_review_length`)
    - Excessively long operating hours
- **Detailed Insights**
  
  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/6a59cb7a-3af9-4978-bba4-dfb6c13a43eb" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/91a12ed6-3d2d-4514-b037-ca8a53ea10de" alt="image2"></td>
    </tr>
  </table>

    - Moderate category variety improves stability
    - Restaurants without alcohol tend to have higher stability
- **Actionable Ideas**
    - Consider regular off-days for more stable operations
    - Be cautious with alcohol service depending on target customer base

### 2. Loyalty Score

<img width="768" height="740" alt="image" src="https://github.com/user-attachments/assets/bb110fe5-91eb-4b42-8482-a16f51ea1183" />

- **Top features:** `tip_count`, `review_count`, `stability_score`, `avg_sentiment`, `reliability_score`
- **Positive influences**
    - Outdoor seating, group-friendly environment
    - Operation in high-density restaurant areas
- **Detailed Insights**

  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/19052d95-6e0a-4940-8e36-2208e328fa5e" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/574f187f-da65-4ba3-9a0f-a2ce32620f37" alt="image2"></td>
    </tr>
  </table>
  
    - Unknown ambience correlates with lower loyalty
    - Alcohol service increases loyalty (opposite of stability score)
- **Actionable Ideas**
    - Create outdoor/group seating areas
    - Build a strong atmosphere or concept, and offer alcohol if appropriate

### 3. Reliability Score

<img width="768" height="740" alt="image" src="https://github.com/user-attachments/assets/e5eb8924-fdbd-47e7-a8d1-aec057ed2573" />

- **Top features:** `category_variety`, `state`, `avg_review_length`, `c_nightlife`, `neighbor_density`
- **Negative influences**
    - High rating variability (`stars_std`)
    - Some unfamiliar cuisines (e.g., Asian fusion)
- **Detailed Insights**
  
  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/cfe3cf5b-39fb-4fc7-87c6-c9848edf7d5b" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/472105e0-4b3b-415c-84f9-a7ce7df808d3" alt="image2"></td>
    </tr>
  </table>

    - Quality of reviews matters more than quantity  
 
      
    <img width="763" height="299" alt="image" src="https://github.com/user-attachments/assets/51177848-b0a4-4d32-a505-1a2f4eb258a0" />

    - Reliability increases when serving familiar, mainstream cuisines
- **Actionable Ideas**
    - Encourage high-quality reviews from customers
    - Consider balancing unique menu concepts with familiar items
