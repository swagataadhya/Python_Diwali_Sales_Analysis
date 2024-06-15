# Python_Diwali_Sales_Analysis
EDA (Exploratory Data Analysis) using Python's Numpy, Pandas, Matplotlib and Seaborn libraries, from initial data loading to insightful visualizations and conclusions.

Setup and Data Loading

import numpy as np 
import pandas as pd 
import matplotlib.pyplot as plt
get_ipython().run_line_magic('matplotlib', 'inline')
import seaborn as sns

df = pd.read_csv('Diwali Sales Data.csv', encoding= 'unicode_escape')

- This section sets up the environment and imports the necessary libraries: NumPy, Pandas, Matplotlib, and Seaborn.
- The line %matplotlib inline ensures that plots will be displayed inline within Jupyter notebooks.
- The Diwali sales data is loaded into a DataFrame df.

Initial Data Inspection

df.shape
df.head(10)
df.info()

- df.shape shows the dimensions of the DataFrame (rows and columns).
- df.head(10) displays the first 10 rows of the DataFrame.
- df.info() provides a summary of the DataFrame, including data types and non-null values.

Data Cleaning

df.drop(['Status', 'unnamed1'], axis=1, inplace=True)
df.info()
pd.isnull(df).sum()
df.dropna(inplace=True)
df.shape
df['Amount'] = df['Amount'].astype('int')
df['Amount'].dtypes
df.columns
df.rename(columns= {'Marital_Status':'Shaadi'})
df.describe()
df[['Age', 'Orders', 'Amount']].describe()

- Drops unnecessary columns 'Status' and 'unnamed1'.
- Checks for null values and drops rows with any null values.
- Converts the 'Amount' column to integer type.
- Renames the 'Marital_Status' column to 'Shaadi'.
- Provides a statistical summary of the DataFrame and specific columns.

Exploratory Data Analysis (EDA)
Gender Analysis

ax = sns.countplot(x = 'Gender',data = df)
for bars in ax.containers:
    ax.bar_label(bars)

sales_gen = df.groupby(['Gender'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)
sns.barplot(x = 'Gender',y= 'Amount' ,data = sales_gen)

- The count plot shows the distribution of buyers by gender.
- The bar plot shows the total amount spent by each gender.

Age Group Analysis

ax = sns.countplot(data = df, x = 'Age Group', hue = 'Gender')
for bars in ax.containers:
    ax.bar_label(bars)

sales_age = df.groupby(['Age Group'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)
sns.barplot(x = 'Age Group',y= 'Amount' ,data = sales_age)

- The count plot shows the distribution of buyers by age group and gender.
- The bar plot shows the total amount spent by each age group.

State Analysis

sales_state = df.groupby(['State'], as_index=False)['Orders'].sum().sort_values(by='Orders', ascending=False).head(10)
sns.set(rc={'figure.figsize':(15,5)})
sns.barplot(data = sales_state, x = 'State',y= 'Orders')

sales_state = df.groupby(['State'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False).head(10)
sns.set(rc={'figure.figsize':(15,5)})
sns.barplot(data = sales_state, x = 'State',y= 'Amount')

- The bar plots show the top 10 states by the number of orders and total sales amount.

Marital Status Analysis

ax = sns.countplot(data = df, x = 'Marital_Status')
sns.set(rc={'figure.figsize':(7,5)})
for bars in ax.containers:
    ax.bar_label(bars)

sales_state = df.groupby(['Marital_Status', 'Gender'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)
sns.set(rc={'figure.figsize':(6,5)})
sns.barplot(data = sales_state, x = 'Marital_Status',y= 'Amount', hue='Gender')

- The count plot shows the distribution of buyers by marital status.
- The bar plot shows the total amount spent by each marital status and gender.

Occupation Analysis

sns.set(rc={'figure.figsize':(20,5)})
ax = sns.countplot(data = df, x = 'Occupation')
for bars in ax.containers:
    ax.bar_label(bars)

sales_state = df.groupby(['Occupation'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False)
sns.set(rc={'figure.figsize':(20,5)})
sns.barplot(data = sales_state, x = 'Occupation',y= 'Amount')

- The count plot shows the distribution of buyers by occupation.
- The bar plot shows the total amount spent by each occupation.

Product Category Analysis

sns.set(rc={'figure.figsize':(20,5)})
ax = sns.countplot(data = df, x = 'Product_Category')
for bars in ax.containers:
    ax.bar_label(bars)

sales_state = df.groupby(['Product_Category'], as_index=False)['Amount'].sum().sort_values(by='Amount', ascending=False).head(10)
sns.set(rc={'figure.figsize':(20,5)})
sns.barplot(data = sales_state, x = 'Product_Category',y= 'Amount')

- The count plot shows the distribution of sold products by category.
- The bar plot shows the total amount spent on each product category.

Top Products by Orders

sales_state = df.groupby(['Product_ID'], as_index=False)['Orders'].sum().sort_values(by='Orders', ascending=False).head(10)
sns.set(rc={'figure.figsize':(20,5)})
sns.barplot(data = sales_state, x = 'Product_ID',y= 'Orders')

fig1, ax1 = plt.subplots(figsize=(12,7))
df.groupby('Product_ID')['Orders'].sum().nlargest(10).sort_values(ascending=False).plot(kind='bar')

- The bar plots show the top 10 products by the number of orders.

Summary of Findings:

Demographics: The most frequent buyers are married women aged 26-35.
Location: The top buying states are Uttar Pradesh, Maharashtra, and Karnataka.
Occupation: Buyers are primarily from the IT, Healthcare, and Aviation sectors.
Product Preference: Popular product categories are Food, Clothing, and Electronics.
