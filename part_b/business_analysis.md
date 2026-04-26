## Business Case Analysis

---

# B1. Problem Formulation

### (a) Machine Learning Formulation

The goal of this problem is to determine the most effective promotion strategy for each store in order to maximize sales performance.

* **Target Variable:**
  `items_sold` (number of items sold per store per month)

* **Input Features:**

  * Store characteristics: store size, location type (urban, semi-urban, rural)
  * Customer behavior: footfall, basket size
  * Promotion type: Flat Discount, BOGO, Free Gift, Category Offer, Loyalty Points
  * External factors: competition density, festivals, weekends, seasonality

* **Type of Problem:**
  This is a **supervised regression problem**, as the target variable is continuous.

* **Justification:**
  The objective is to predict a numerical outcome (sales volume), which makes regression the most suitable approach. This allows comparison of expected performance across different promotions.

---

### (b) Why Items Sold is Better than Revenue

Using **items sold** as the target variable is more reliable than total revenue because:

* Revenue is influenced by pricing and discounts, which may distort true performance
* Promotions like heavy discounts may increase volume but reduce profit margins
* Items sold directly reflects customer demand and engagement

**Broader Principle:**
The target variable should align closely with the actual business objective. Choosing an inappropriate target can lead to misleading insights and poor decision-making.

---

### (c) Alternative Modelling Strategy

Instead of using a single global model, a better approach is:

* **Segmented Modeling (by store type or region)**
* Or a **hierarchical model with store-level variations**

**Justification:**

* Different locations have different customer behavior patterns
* Promotions may perform differently across regions
* A single model may fail to capture these variations

Segmented models allow more accurate and context-aware predictions.

---

# B2. Data and EDA Strategy

### (a) Data Joining and Dataset Design

The data is spread across four tables:

* Transactions
* Store attributes
* Promotion details
* Calendar data

**Joining Strategy:**

* Join transactions with store attributes using `store_id`
* Join promotions using promotion identifiers or date
* Join calendar data using `transaction_date`

**Grain of Dataset:**
One row represents **one store for one month**

**Aggregations:**

* Total items sold per store per month
* Average basket size
* Total visits/footfall
* Promotion applied in that month

This ensures a structured dataset suitable for modeling.

---

### (b) Exploratory Data Analysis (EDA)

Key analyses include:

1. **Promotion vs Sales (Bar Plot):**
   Compare average items sold across different promotions to identify high-performing strategies

2. **Time Series Analysis (Line Plot):**
   Observe sales trends over time to detect seasonality and patterns

3. **Correlation Heatmap:**
   Identify relationships between numerical variables and remove redundant features

4. **Sales Distribution by Store Type (Box Plot):**
   Compare performance across urban, semi-urban, and rural stores

**Impact:**

* Helps identify important features
* Detects patterns and outliers
* Guides feature engineering and model selection

---

### (c) Handling Promotion Imbalance

Since 80% of transactions occur without promotions:

**Impact:**

* Model may become biased toward “no promotion”
* Reduced sensitivity to promotional effects

**Solutions:**

* Balance the dataset using sampling techniques
* Introduce feature engineering to highlight promotion effects
* Use appropriate evaluation strategies

---

# B3. Model Evaluation and Deployment

### (a) Train-Test Split and Metrics

**Split Strategy:**

* Use a **time-based split**
* Train on earlier data and test on the most recent data

**Why not random split?**

* Causes data leakage
* Uses future data to predict past outcomes
* Not realistic for business scenarios

**Evaluation Metrics:**

* **RMSE:** Penalizes large errors, useful for risk-sensitive decisions
* **MAE:** Provides average prediction error, easy to interpret

**Interpretation:**
Lower RMSE and MAE indicate better predictive performance.

---

### (b) Explaining Model Recommendations

Feature importance can be used to understand model decisions.

For example:

* In December, Loyalty Points Bonus may perform better due to festive demand and customer loyalty behavior
* In March, Flat Discounts may be more effective due to lower demand or higher competition

**Communication Approach:**

* Present top influencing features
* Explain seasonal and behavioral patterns
* Use simple visualizations for clarity

This helps build trust with business stakeholders.

---

### (c) Deployment Strategy

**Step 1: Model Saving**

* Save the trained model using joblib or pickle

**Step 2: Monthly Prediction Pipeline**

* Collect new monthly data
* Apply the same preprocessing pipeline
* Generate predictions for all stores

**Step 3: Recommendation System**

* Select the promotion with the highest predicted sales for each store

**Monitoring:**

* Track prediction errors over time
* Compare predicted vs actual performance
* Detect data drift or performance degradation

**Retraining:**

* Periodically retrain the model using updated data
* Ensure the model adapts to changing patterns

This ensures a scalable and reliable deployment system.
