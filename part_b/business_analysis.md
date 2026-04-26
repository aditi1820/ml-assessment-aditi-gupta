B1. Problem Formulation
(a) Problem Definition

The goal is to predict the number of items sold for each store under different promotional strategies.

Target Variable:
items_sold
Input Features:
Store attributes (store size, location type), promotion type, competition density, temporal features (month, day of week, festivals, weekends), and customer behavior patterns.
Type of Problem:
This is a supervised regression problem because we are predicting a continuous numerical value (items sold) based on historical data.
(b) Why Items Sold Instead of Revenue

Using total revenue can be misleading because it is influenced by price variations, discounts, and product mix.

In contrast, items sold directly measures customer response to promotions, making it a more reliable indicator of promotion effectiveness.

This illustrates the broader principle that:

 The target variable should align closely with the business objective and should not be affected by external or confounding factors.

(c) Alternative Modelling Strategy

Instead of using a single global model, a better approach would be:

 Segmented or hierarchical modeling

Build separate models for different store types (urban, semi-urban, rural), OR
Include store-specific features or embeddings

This is because customer behavior and promotion effectiveness vary significantly across different locations.

 B2. Data and EDA Strategy
(a) Data Joining Strategy

The data comes from multiple tables:

Transactions
Store attributes
Promotion details
Calendar

These should be joined using common keys such as store_id and transaction_date.

Final Dataset Grain:
One row = one store per day (or per transaction period)
Aggregations:
Total items sold per store per day
Average basket size
Number of transactions
Promotion applied

This creates a structured dataset suitable for modeling.

(b) EDA Approach
Distribution of Items Sold
Identify skewness and outliers
Helps decide transformation if needed
Promotion Type vs Sales
Compare effectiveness of different promotions
Helps in feature importance understanding
Time-based Trends (Monthly/Weekly)
Identify seasonality patterns
Helps in feature engineering
Correlation Analysis
Understand relationships between variables
Helps in feature selection
(c) Handling Imbalance

Since 80% of transactions have no promotion:

The model may become biased toward predicting “no promotion effect”
It may fail to learn the impact of promotions

To address this:

Use sampling techniques (oversampling promotion cases)
Add promotion indicator features
Evaluate performance separately for promotion vs non-promotion cases

B3. Model Evaluation and Deployment
(a) Train-Test Strategy

Since the data is time-based:

Use a temporal split
Train on earlier data
Test on recent data

 Random split is inappropriate because it causes data leakage

Future data may influence past predictions

Evaluation Metrics:

RMSE: Measures overall prediction error
MAE: Measures average absolute error

Interpretation:

Lower values indicate better performance
MAE is easier to interpret in business terms
(b) Explaining Model Recommendations

Feature importance helps understand why the model recommends different promotions.

For example:

December → high demand due to festivals → loyalty points may work better
March → lower demand → discounts may drive sales

We can communicate:

Which features (month, promotion type, competition) influenced the decision
How seasonal patterns affect promotion effectiveness
(c) Deployment Strategy
Model Saving:
Save the trained pipeline using joblib or pickle
Monthly Prediction Process:
Collect new data
Apply same preprocessing pipeline
Generate predictions for all stores
Monitoring:
Track RMSE/MAE over time
Detect performance degradation
Retraining Trigger:
If performance drops significantly
Or new patterns emerge in data