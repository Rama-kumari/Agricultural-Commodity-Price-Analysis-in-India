# Agricultural-Commodity-Price-Analysis-in-India
Agricultural commodities play a crucial role in the economy, and their prices are vital indicators for farmers, traders, and policymakers. Understanding how these prices vary by region and commodity can assist in better market planning, policy-making, and investment decisions
1.	Abstract -
This project focuses on analyzing the wholesale prices of various agricultural commodities across different states and markets in India. The dataset includes Minimum, Maximum, and Modal prices along with commodity types, states, and districts. The aim is to uncover pricing patterns, identify outliers, and derive insights about price distribution and commodity trends. Through data preprocessing, statistical analysis, and visualizations, this project enables data- driven insights for stakeholders such as farmers, traders, and policymakers.

2.	Introduction -
Agricultural commodities play a crucial role in the economy, and their prices are vital indicators for farmers, traders, and policymakers. Understanding how these prices vary by region and commodity can assist in better market planning, policy-making, and investment decisions. This analysis aims to explore the price distribution, detect trends, and highlight potential areas for further investigation in agricultural pricing dynamics.

3.	Methodology -
•	Tools Used: Python, Pandas, Matplotlib, Seaborn.
•	Data Cleaning:
o	Handled missing data using .dropna().
o	Removed duplicate entries.
o	Renamed columns for ease of access.
•	Statistical Summaries:
o	Used.describe() to obtain measures like mean, median, min, and max.
•	Data Visualization:
o	Plotted various graphs to understand distribution and correlation.
•	Aggregation:
o	Grouped data using groupby() to compute average prices by State and Commodity.
•	Insight Retrieval:
o	Used filtering techniques to locate the highest modal price.
 
4.	Objectives -
The primary objectives of this analysis are:
1.	Data Cleaning & Preparation:
o	Remove missing values and duplicates.
o	Rename complex column names for readability.
2.	Descriptive Analysis:
o	Generate statistical summaries of price columns.
3.	Visual Analysis:
o	Explore data patterns using boxplots, histograms, bar charts, pie charts, and heatmaps.
4.	Grouping & Aggregation:
o	Calculate average modal prices grouped by State and Commodity.
5.	Insight Extraction:
o	Identify the commodity with the highest modal price and its location.


import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
df = pd.read_csv('D:/PYTHON/pythonproo.csv')

# Rename price columns for easier access
df.rename(columns={
    'Min_x0020_Price': 'Min_Price',
    'Max_x0020_Price': 'Max_Price',
    'Modal_x0020_Price': 'Modal_Price'
}, inplace=True)

# Objective 1: Data Cleaning and Preprocessing
print("▶ Checking for missing values:\n", df.isnull().sum())
df.dropna(inplace=True)

print("\n▶ Checking for duplicates...")
df.drop_duplicates(inplace=True)

# Objective 2: Descriptive Statistics
print("\n▶ Basic Statistical Description:\n", df[['Min_Price', 'Max_Price', 'Modal_Price']].describe())

# Objective 3: Visualizations

# Boxplot of Price Distributions
plt.figure(figsize=(8, 6))
sns.boxplot(data=df[['Min_Price', 'Max_Price', 'Modal_Price']])
plt.title("Boxplot of Prices")
plt.ylabel("Price")
plt.show()

# Histogram of Modal Prices
plt.figure(figsize=(8, 6))
sns.histplot(df['Modal_Price'], kde=True, color='skyblue', bins=20)
plt.title("Histogram of Modal Prices")
plt.xlabel("Modal Price")
plt.ylabel("Frequency")
plt.show()

# Bar Chart: Average Modal Price per Commodity
avg_price = df.groupby('Commodity')['Modal_Price'].mean().sort_values(ascending=False).head(10)
plt.figure(figsize=(10, 6))
avg_price.plot(kind='bar', color='coral')
plt.title("Top 10 Commodities by Average Modal Price")
plt.ylabel("Average Modal Price")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Pie Chart: Distribution of Commodities
commodity_counts = df['Commodity'].value_counts().head(6)
plt.figure(figsize=(8, 8))
plt.pie(commodity_counts, labels=commodity_counts.index, autopct='%1.1f%%', startangle=140)
plt.title("Top 6 Commodity Distribution")
plt.axis('equal')
plt.show()

# Heatmap: Correlation between Price Columns
plt.figure(figsize=(6, 4))
sns.heatmap(df[['Min_Price', 'Max_Price', 'Modal_Price']].corr(), annot=True, cmap='YlGnBu')
plt.title("Price Correlation Heatmap")
plt.show()

# Objective 4: Grouping and Aggregation
grouped = df.groupby(['State', 'Commodity'])[['Modal_Price']].mean().reset_index()
print("\n▶ Average Modal Price by State and Commodity:\n", grouped.head())

# Objective 5: Insight - Commodity with Highest Modal Price
top_price = df[df['Modal_Price'] == df['Modal_Price'].max()]
print("\n▶ Commodity with Highest Modal Price:\n", top_price[['State', 'District', 'Commodity', 'Modal_Price']])
