Data source: https://www.kaggle.com/datasets/vishakhdapat/customer-segmentation-clustering

 Customer Segmentation Project
 Objective

The goal of this project is to perform Customer Segmentation — analyzing customer data to understand their behavior, spending patterns, and demographic profiles. This helps businesses identify distinct customer groups, improve marketing strategies, and enhance customer satisfaction.

 Project Overview

This dataset contains information about 2,240 customers, including:

Personal details (age, education, marital status, income)

Spending on different product categories

Web and store purchase behavior

Response to marketing campaigns

Through data cleaning, feature engineering, and visual analysis, we uncover patterns and correlations to understand different types of customers.

 1. Data Loading and Cleaning

We started by importing essential libraries:

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


The dataset customer_segmentation.csv was loaded using pd.read_csv() and contained 2,240 rows and 29 columns.

 Missing Values

Found 24 missing values in the Income column.

These rows were dropped using:

df.dropna(inplace=True)


After this, the dataset contained 2,216 rows.

 Data Types

We converted Dt_Customer to a proper datetime format:

df["Dt_Customer"] = pd.to_datetime(df["Dt_Customer"], dayfirst=True)

 2. Feature Engineering

To enhance our analysis, we created new columns:

 Age

Calculated using the year of birth:

df["Age"] = 2025 - df["Year_Birth"]

 Total Children

Sum of kids and teens in the household:

df["Total_Children"] = df["Kidhome"] + df["Teenhome"]

 Total Spending

Combined total of all product-related spendings:

spend_cols = ["MntWines", "MntFruits", "MntMeatProducts", "MntFishProducts", "MntGoldProds"]
df["Total_Spending"] = df[spend_cols].sum(axis=1)

 Customer Since

Calculated the number of days since a customer joined:

df["Customer_Since"] = (pd.Timestamp("today") - df["Dt_Customer"]).dt.days


These new features help in understanding customer value and engagement over time.

 3. Exploratory Data Analysis (EDA)
🔹 Age Distribution

A histogram shows that most customers are aged between 40–70 years.

sns.histplot(df["Age"], bins=30, kde=True)

🔹 Income Distribution

Most customers earn between 30,000–70,000, with a few high-income outliers.

sns.histplot(df["Income"], bins=30, kde=True)

🔹 Total Spending

The spending varies widely, with most customers spending below 1,000.

sns.histplot(df["Total_Spending"], bins=30, kde=True)

 4. Categorical Analysis
 Income by Education

Graduates and PhD holders have the highest income levels.

sns.boxplot(x="Education", y="Income", data=df)

 Spending by Marital Status

Single and married customers show slightly different spending behaviors.

sns.boxplot(x="Marital_Status", y="Total_Spending", data=df)

 5. Correlation Analysis

We explored relationships between key numerical variables:

corr = df[["Income","Age","Recency","Total_Spending","NumWebPurchases","NumStorePurchases"]].corr()
sns.heatmap(corr, annot=True, cmap="coolwarm")

Insights:

Income is strongly correlated with Total Spending (0.66).

Store purchases and Total Spending have a strong correlation (0.67).

Age has a weak correlation with spending, showing that age doesn’t strongly influence purchase behavior.

 6. Pivot Analysis

We used pivot tables to explore deeper insights:

pivot_income = df.pivot_table(values="Income", index="Education", columns="Marital_Status", aggfunc="mean")

Key Findings:

Graduates and PhD holders generally earn more.

Married and together customers have similar income levels.

“Basic” education level customers have the lowest average income.

  7. Summary of Insights
Insight	Description
 Age Group	Most customers are middle-aged (40–70 years).
 Income	Majority earn between 30K–70K, with a few high-income outliers.
 Spending	Total spending is positively related to income.
 Education	Graduates and PhD holders spend and earn more.
 Marital Status	Married and “Together” customers dominate the dataset.
 Customer Since	Many customers have been with the company for over 4,000 days (~11 years).
 8. Conclusion

This project gives a clear view of customer demographics and purchasing behavior.
We learned that:

Income and total spending are key drivers of customer segmentation.

Education level significantly affects spending patterns.

Long-term customers show consistent engagement.

With further steps like clustering (K-Means) or RFM segmentation, we can group customers into actionable segments such as:

High-value loyal customers

New low-spending customers

Inactive high-income customers

These insights can guide marketing campaigns, retention strategies, and personalized promotions.