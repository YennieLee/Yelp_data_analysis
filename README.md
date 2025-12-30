# Data-Driven Insights into Restaurant Success in the U.S.

## **🔍** Overview
This project aims to identify **what drives success in the restaurant industry** in the United States. Running a restaurant is challenging — around 30% of restaurants in the U.S. close within their first three years.

To uncover key strategies for success, I analyzed the restaurants’ review data, which contains more than **2.7 million reviews** across the U.S.

The findings are designed to support **restaurant owners and operators** in making data-driven decisions to improve long-term performance.

The analysis followed three main steps:

1. **Defined three custom performance metrics** for restaurants — *stability score*, *loyalty score*, and *reliability score*.
2. **Identified key features** that positively or negatively influence each metric.
3. **Translated analytical insights into actionable recommendations** for restaurant operators.

---

## **🗂** About the Dataset
<img width="1917" height="1282" alt="image" src="https://github.com/user-attachments/assets/256b5ce7-829a-437e-8bd7-bb727e9c0afe" />

- **Source**: Yelp Open Dataset
- **Raw data**
    - ~150K business records
    - ~6.9M review records (2005–2022)
- **Filtering criteria**
    - Restaurant businesses only
    - Minimum of 25 reviews per restaurant
- **Final dataset**
    - ~20K restaurants
    - ~2.7M reviews

---

## 📚 Python Libraries

- **Basic:** `os`
- **Preprocessing:** `pandas`, `numpy`
- **Statistics:** `scipy`, `statsmodels`
- **Machine Learning:** `sklearn`, `catboost`, `optuna`
- **Visualization:** `matplotlib`, `seaborn`

---

## 🎯 Restaurant Performance Metrics
### 1. Stability Score
- The stability score measures a restaurant’s operational resilience — its ability to withstand and recover from negative events such as rating drops or critical reviews.
- Higher stability scores are associated with lower closure probabilities.
  <img width="691" height="468" alt="image" src="https://github.com/user-attachments/assets/36c2073c-bdd8-4f8f-94a2-a52f6b210af4" />

### 2. Loyalty Score
- The loyalty score reflects how beloved a restaurant is — how many regular customers it has and how often they visit.
- Restaurants with higher loyalty scores tend to maintain higher average ratings.
  <img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/17821324-e27f-49e6-bbf5-874528d840cc" />

### 3. Reliability Score
- The **Reliability Score** captures how credible a restaurant appears to potential customers by measuring the share of reviews written by influential reviewers.
- Restaurants with higher reliability scores attract more first-time visitors and accumulate more reviews.
  <img width="571" height="453" alt="image" src="https://github.com/user-attachments/assets/7da2ff35-b6ef-4e86-aca2-05d9681a6e1b" />

---

## 💡 Insights Deep Dive
### 1. Stability Score
  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/dae4d968-3748-4977-bdd9-369f267885f8" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/fe7dcf45-c07d-47d8-9510-719c11a3b4ca" alt="image2"></td>
      <td><img src="https://github.com/user-attachments/assets/09b80055-96d9-4719-93f5-049fe72496ca" alt="image3"></td>
    </tr>
  </table>

- **Sentiment of reviews** (positive vs. negative) has the strongest impact.
- **Excessively long operating hours** negatively affect stability.
- **Moderate category variety** improves stability, while excessive diversification hurts it.
- Restaurants **without alcohol service** tend to be more operationally stable.


### 2. Loyalty Score
  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/74b4d91f-a97e-4d30-972d-ff7e7623b835" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/84aec465-a310-4fa4-803a-0e2baf0313e9" alt="image2"></td>
      <td><img src="https://github.com/user-attachments/assets/bb8b1caf-f465-4926-a317-fdcc51f5745a" alt="image3"></td>
    </tr>
  </table>

- **Review volume** is the strongest predictor of loyalty.
- Unclear ambience correlates with lower loyalty.
- **Alcohol service increases loyalty**, despite reducing stability.

### 3. Reliability Score

  <table>
    <tr>
      <td><img src="https://github.com/user-attachments/assets/d064a08e-31cf-449f-9ec7-f1299eebeba9" alt="image1"></td>
      <td><img src="https://github.com/user-attachments/assets/abb18c8d-806c-4090-a1ce-f764d36c25ed" alt="image2"></td>
      <td><img src="https://github.com/user-attachments/assets/48412154-8a64-4e42-98c6-11e205deaf71" alt="image3"></td>
    </tr>
  </table>

- **Breadth of food categories** is the most influential factor.
- **Quality of reviews** matters more than sheer quantity.
- Restaurants serving **familiar, mainstream cuisines** achieve higher reliability than niche or experimental concepts.

## 🪄 Recommendations

### 1. Improving Stability

- Focus on **core menu offerings** rather than excessive variety.
- Monitor customer feedback closely and address recurring complaints.
- Consider **regular off-days** for more stable operations
- Be cautious with alcohol service depending on target customer base

### 2. Building Loyalty

- Develop a clear and consistent **brand atmosphere or concept**.
- Offer alcohol where it aligns with the restaurant’s concept and audience.

### 3. Enhancing Reliability

- Encourage **high-quality, thoughtful customer reviews**.
- Balance uniqueness with familiarity in menu design.
- Prioritize accessible cuisine before expanding into niche categories (e.g., fusion concepts).


