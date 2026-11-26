# pandas_notes

# 🐼 Pandas Notes – Learning, Syntax & Quick Reference

A practical collection of Python Pandas concepts, exercises, syntax, and examples covering data creation, reading, manipulation, cleaning, analysis, and transformation.

This repository documents my learning journey through the **Kaggle Pandas Micro-Course**, along with additional Pandas operations that are useful in real-world data analytics and data science projects.

The objective is to understand not just what Pandas functions do, but also when and how to use them.

---

# 📚 Table of Contents

1. [Introduction](#-introduction)
2. [Repository Structure](#-repository-structure)
3. [Installation and Import](#-installation-and-import)
4. [Creating Series and DataFrames](#1-creating-series-and-dataframes)
5. [Reading and Writing Data](#2-reading-and-writing-data)
6. [Indexing, Selecting and Assigning](#3-indexing-selecting-and-assigning)
7. [Summary Functions and Maps](#4-summary-functions-and-maps)
8. [Grouping and Sorting](#5-grouping-and-sorting)
9. [Data Types and Missing Values](#6-data-types-and-missing-values)
10. [Renaming and Combining](#7-renaming-and-combining)
11. [Additional Important Pandas Syntax](#-additional-important-pandas-syntax)
12. [Important Pandas Cheat Sheet](#-important-pandas-cheat-sheet)
13. [Common Mistakes](#-common-mistakes)
14. [Real-World Data Analysis Workflow](#-real-world-data-analysis-workflow)
15. [Learning Roadmap](#-learning-roadmap)
16. [Resources](#-resources)

---

# 🐼 Introduction

**Pandas** is an open-source Python library used for data manipulation, cleaning, exploration, and analysis.

It provides two major data structures:

| Structure | Definition |
|---|---|
| Series | One-dimensional labeled array |
| DataFrame | Two-dimensional labeled tabular data |

A DataFrame consists of rows and columns, similar to a database table or Excel spreadsheet.

# 📁 Repository Structure

```text
pandas_notes/
│
├── README.md
│
├── exercise-creating-reading-and-writing.ipynb
├── exercise-indexing-selecting-assigning.ipynb
├── exercise-summary-functions-and-maps.ipynb
├── exercise-grouping-and-sorting.ipynb
├── exercise-data-types-and-missing-values.ipynb
└── exercise-renaming-and-combining.ipynb
```

## Notebook Overview

| Notebook | Main Concepts |
|---|---|
| Creating, Reading and Writing | Creating Series/DataFrames, CSV import/export |
| Indexing, Selecting and Assigning | Column selection, row selection, `loc`, `iloc`, Boolean filtering |
| Summary Functions and Maps | Median, unique, value counts, arithmetic, `map`, `apply` |
| Grouping and Sorting | `groupby`, `count`, `size`, `agg`, sorting |
| Data Types and Missing Values | `dtype`, `astype`, `isnull`, `fillna`, `value_counts` |
| Renaming and Combining | `rename`, `rename_axis`, `concat`, `join`, `set_index` |

---

# ⚙️ Installation and Import

Install Pandas:

```bash
pip install pandas
```

Import libraries:

```python
import pandas as pd
import numpy as np
```

Check Pandas version:

```python
pd.__version__
```

---

# 1. Creating Series and DataFrames

## 1.1 Creating a DataFrame

**Definition:** Creates a two-dimensional tabular structure containing rows and columns.

```python
fruits = pd.DataFrame({
    'Apples': [30, 41],
    'Bananas': [21, 34]
})
```

Create using nested lists:

```python
df = pd.DataFrame(
    [[30, 21], [41, 34]],
    columns=['Apples', 'Bananas']
)
```

Create with custom row indexes:

```python
df = pd.DataFrame(
    [[35, 21], [41, 34]],
    columns=['Apples', 'Bananas'],
    index=['2017 Sales', '2018 Sales']
)
```

## 1.2 Creating a Series

**Definition:** Creates a one-dimensional labeled array.

```python
ingredients = pd.Series(
    ['4 cups', '1 cup', '2 large', '1 can'],
    index=['Flour', 'Milk', 'Eggs', 'Spam'],
    name='Dinner'
)
```

## 1.3 Important Difference

| Series | DataFrame |
|---|---|
| One-dimensional | Two-dimensional |
| Single sequence of values | Multiple columns |
| `pd.Series()` | `pd.DataFrame()` |
| Single column selection usually returns a Series | Multiple column selection returns a DataFrame |

---

# 2. Reading and Writing Data

## 2.1 Reading CSV

**Definition:** Loads data from a CSV file into a DataFrame.

```python
reviews = pd.read_csv('data.csv')
```

Use the first column as the index:

```python
reviews = pd.read_csv('data.csv', index_col=0)
```

## 2.2 Exporting CSV

**Definition:** Saves a DataFrame into a CSV file.

```python
df.to_csv('output.csv')
```

Without saving the index:

```python
df.to_csv('output.csv', index=False)
```

## 2.3 Other File Formats

```python
df = pd.read_excel('data.xlsx')
df = pd.read_json('data.json')

df.to_excel('output.xlsx', index=False)
df.to_json('output.json', orient='records')
```

---

# 3. Indexing, Selecting and Assigning

This section is important because selecting the correct rows and columns is fundamental to data analysis.

## 3.1 Selecting a Column

**Definition:** Extracts one column from a DataFrame.

```python
reviews['description']
```

Shortcut:

```python
reviews.description
```

The result is a Series.

## 3.2 Selecting Multiple Columns

```python
reviews[['country', 'province', 'variety']]
```

Returns a DataFrame.

## 3.3 Selecting Using `iloc`

**Definition:** Integer-position-based selection.

```python
reviews.iloc[0]
```

First row.

```python
reviews.iloc[0:10]
```

First 10 rows.

```python
reviews.iloc[0, 2]
```

First row, third column.

```python
reviews.iloc[[1, 2, 3, 5, 8]]
```

Select specific row positions.

## 3.4 Selecting Using `loc`

**Definition:** Label-based selection.

```python
reviews.loc[0]
```

Select row with index label 0.

```python
reviews.loc[0:10]
```

Select index labels from 0 through 10, including 10.

Select particular rows and columns:

```python
reviews.loc[
    [0, 1, 10, 100],
    ['country', 'province', 'region_1']
]
```

## 3.5 Difference Between `loc` and `iloc`

| loc | iloc |
|---|---|
| Label-based | Position-based |
| Uses actual index labels | Uses integer positions |
| Slice endpoint included | Slice endpoint excluded |

Example:

```python
df.loc[0:5]
df.iloc[0:5]
```

With a default integer index, the first selects six labels (0–5), while the second selects five positions (0–4).

## 3.6 Boolean Filtering

**Definition:** Selects rows based on conditions.

```python
reviews.loc[reviews.country == 'Italy']
```

Greater than:

```python
reviews.loc[reviews.points >= 95]
```

Multiple conditions using AND:

```python
reviews.loc[
    (reviews.points >= 95) &
    (reviews.country == 'Australia')
]
```

Multiple conditions using OR:

```python
reviews.loc[
    (reviews.country == 'Australia') |
    (reviews.country == 'New Zealand')
]
```

## 3.7 Assigning Values

Create a new column:

```python
df['Bonus'] = 1000
```

Modify an existing value:

```python
df.loc[0, 'Age'] = 25
```

Create a column from an existing column:

```python
df['Discounted_Price'] = df['Price'] * 0.90
```

---

# 4. Summary Functions and Maps

These functions help understand datasets and transform values.

## 4.1 `median()`

**Definition:** Returns the middle value of a numerical Series.

```python
reviews.points.median()
```

## 4.2 `unique()`

**Definition:** Returns distinct values without duplicates.

```python
reviews.country.unique()
```

## 4.3 `value_counts()`

**Definition:** Counts the frequency of each unique value.

```python
reviews.country.value_counts()
```

## 4.4 `count()`

**Definition:** Counts non-null values.

```python
reviews.price.count()
```

## 4.5 `mean()`

**Definition:** Calculates the arithmetic average.

```python
reviews.price.mean()
```

## 4.6 `min()` and `max()`

**Definition:** Return the minimum and maximum values.

```python
reviews.points.min()
reviews.points.max()
```

## 4.7 `sum()`

**Definition:** Calculates the sum of values.

```python
reviews.points.sum()
```

## 4.8 `describe()`

**Definition:** Provides statistical summary information.

```python
reviews.points.describe()
```

Usually includes:

- Count
- Mean
- Standard deviation
- Minimum
- 25th percentile
- 50th percentile
- 75th percentile
- Maximum

## 4.9 `map()`

**Definition:** Applies a transformation or mapping to every value in a Series.

```python
reviews.points.map(lambda p: p - reviews.points.mean())
```

Using a dictionary:

```python
df['Gender'] = df['Gender'].map({
    'M': 'Male',
    'F': 'Female'
})
```

## 4.10 `apply()`

**Definition:** Applies a function to a Series or along an axis of a DataFrame.

For a Series:

```python
reviews.points.apply(lambda p: p + 1)
```

For a DataFrame, row-wise:

```python
def calculate_score(row):
    return row.points / row.price

reviews.apply(calculate_score, axis=1)
```

### Difference Between `map()` and `apply()`

| map() | apply() |
|---|---|
| Primarily used with Series | Used with Series and DataFrame |
| Transforms individual Series values | Applies a function to values or rows/columns |
| Useful for dictionary mapping | Useful for custom calculations |

## 4.11 Arithmetic Operations

Centering data by subtracting its mean:

```python
centered_price = reviews.price - reviews.price.mean()
```

Calculate a points-to-price ratio:

```python
reviews.points / reviews.price
```

Find the index of the maximum ratio:

```python
idx = (reviews.points / reviews.price).idxmax()
```

Retrieve the corresponding wine:

```python
reviews.loc[idx, 'title']
```

## 4.12 Custom Functions

Define a function using `def`:

```python
def stars(row):
    if row.country == 'Canada':
        return 3
    elif row.points >= 95:
        return 3
    elif row.points >= 85:
        return 2
    else:
        return 1
```

Apply it row-wise:

```python
star_ratings = reviews.apply(stars, axis=1)
```

---

# 5. Grouping and Sorting

`groupby()` is one of the most powerful Pandas functions for data analysis.

## 5.1 `groupby()`

**Definition:** Splits data into groups based on one or more columns.

```python
reviews.groupby('country')
```

## 5.2 Grouping and Counting

Count reviews by country:

```python
reviews.groupby('country').size()
```

Count non-null values in a specific column:

```python
reviews.groupby('taster_twitter_handle')['taster_twitter_handle'].count()
```

## 5.3 Grouping and Calculating Maximum

```python
reviews.groupby('price')['points'].max()
```

Returns the highest rating for each price.

## 5.4 Grouping and Calculating Mean

```python
reviews.groupby('taster_name')['points'].mean()
```

Returns average ratings for each reviewer.

## 5.5 Grouping and Aggregation Using `agg()`

**Definition:** Performs multiple aggregation operations on grouped data.

```python
reviews.groupby('variety')['price'].agg(['min', 'max'])
```

Calculate multiple statistics:

```python
reviews.groupby('country')['points'].agg(
    ['min', 'max', 'mean', 'median']
)
```

## 5.6 Grouping by Multiple Columns

```python
reviews.groupby(['country', 'variety']).size()
```

Creates a hierarchical index (MultiIndex).

## 5.7 Sorting Using `sort_values()`

**Definition:** Sorts DataFrame or Series values.

Ascending:

```python
reviews.sort_values(by='points')
```

Descending:

```python
reviews.sort_values(
    by='points',
    ascending=False
)
```

Sort using multiple columns:

```python
df.sort_values(
    by=['min', 'max'],
    ascending=False
)
```

## 5.8 Sorting Index Using `sort_index()`

```python
reviews.sort_index()
```

## 5.9 Important GroupBy Syntax

| Syntax | Definition |
|---|---|
| `groupby()` | Groups records |
| `count()` | Counts non-null values |
| `size()` | Counts rows, including rows with missing values |
| `mean()` | Average of each group |
| `max()` | Maximum of each group |
| `min()` | Minimum of each group |
| `sum()` | Sum of each group |
| `agg()` | Multiple aggregations |
| `sort_values()` | Sorts by values |
| `sort_index()` | Sorts by index |

---

# 6. Data Types and Missing Values

## 6.1 `dtype`

**Definition:** Returns the data type of a Series.

```python
reviews.points.dtype
```

## 6.2 `dtypes`

**Definition:** Returns the data types of all DataFrame columns.

```python
reviews.dtypes
```

## 6.3 `astype()`

**Definition:** Converts a Series or DataFrame column into another data type.

Convert to string:

```python
reviews.points.astype('str')
```

Convert to integer:

```python
df['Age'] = df['Age'].astype('int')
```

Convert to float:

```python
df['Price'] = df['Price'].astype('float')
```

## 6.4 `isnull()` and `isna()`

**Definition:** Identify missing values.

```python
reviews.price.isnull()
```

Count missing values:

```python
reviews.price.isnull().sum()
```

These two methods are equivalent:

```python
df.isnull()
df.isna()
```

## 6.5 `notnull()` and `notna()`

**Definition:** Identify values that are not missing.

```python
reviews.price.notnull()
```

## 6.6 `fillna()`

**Definition:** Replaces missing values with specified values.

```python
reviews.region_1.fillna('Unknown')
```

Fill with zero:

```python
df['Age'].fillna(0)
```

Fill with mean:

```python
df['Price'].fillna(df['Price'].mean())
```

## 6.7 `dropna()`

**Definition:** Removes rows or columns containing missing values.

Remove rows:

```python
df.dropna()
```

Remove columns:

```python
df.dropna(axis=1)
```

## 6.8 `sort_values()`

Used to arrange the results after replacing missing values.

```python
reviews.region_1.fillna('Unknown').value_counts().sort_values(ascending=False)
```

---

# 7. Renaming and Combining

## 7.1 `rename()`

**Definition:** Changes column names or index labels.

```python
reviews.rename(
    columns={
        'region_1': 'region',
        'region_2': 'locale'
    }
)
```

## 7.2 `rename_axis()`

**Definition:** Renames the index or column axis name.

```python
reviews.rename_axis('wines', axis='rows')
```

## 7.3 `set_index()`

**Definition:** Makes an existing column the DataFrame index.

```python
df.set_index('Employee_ID')
```

## 7.4 `reset_index()`

**Definition:** Restores the index to a regular column and creates a default index.

```python
df.reset_index()
```

## 7.5 `concat()`

**Definition:** Combines DataFrames along rows or columns.

Vertical concatenation:

```python
pd.concat([df1, df2])
```

Horizontal concatenation:

```python
pd.concat([df1, df2], axis=1)
```

Ignore original indexes:

```python
pd.concat(
    [df1, df2],
    ignore_index=True
)
```

## 7.6 `join()`

**Definition:** Combines DataFrames using their indexes by default.

```python
df1.join(df2)
```

Specify join type:

```python
df1.join(df2, how='left')
```

---

# ➕ Additional Important Pandas Syntax

The following operations are useful extensions to the six notebooks. They are included as additional reference material, not as claims that they were already completed in the exercises.

# 8. Data Exploration

| Syntax | Definition | Example |
|---|---|---|
| `head(n)` | First n rows | `df.head(10)` |
| `tail(n)` | Last n rows | `df.tail(10)` |
| `shape` | Rows and columns | `df.shape` |
| `info()` | Dataset structure | `df.info()` |
| `columns` | Column names | `df.columns` |
| `index` | Index information | `df.index` |
| `sample(n)` | Random n rows | `df.sample(5)` |
| `nunique()` | Number of distinct values | `df.nunique()` |
| `memory_usage()` | Memory used by columns | `df.memory_usage()` |

# 9. Advanced Filtering

## 9.1 `isin()`

**Definition:** Checks whether values belong to a specified list.

```python
df[df['City'].isin(['Delhi', 'Mumbai'])]
```

## 9.2 `between()`

**Definition:** Checks whether values fall within a given range (inclusive by default).

```python
df[df['Age'].between(18, 30)]
```

## 9.3 `query()`

**Definition:** Filters rows using a string expression.

```python
df.query('Age > 25')
```

Multiple conditions:

```python
df.query('Age > 25 and Salary > 30000')
```

## 9.4 `where()`

**Definition:** Keeps values satisfying a condition and replaces other values with missing values by default.

```python
df['Salary'].where(df['Salary'] > 30000)
```

## 9.5 `mask()`

**Definition:** Replaces values where a condition is true.

```python
df['Salary'].mask(df['Salary'] < 30000, 0)
```

## 9.6 `nlargest()` and `nsmallest()`

**Definition:** Returns the largest or smallest n values.

```python
df.nlargest(5, 'Salary')
```

```python
df.nsmallest(5, 'Salary')
```

# 10. Data Cleaning

## 10.1 `drop_duplicates()`

**Definition:** Removes duplicate rows.

```python
df.drop_duplicates()
```

Remove duplicates based on a column:

```python
df.drop_duplicates(subset=['Email'])
```

Keep the last occurrence:

```python
df.drop_duplicates(
    subset=['Email'],
    keep='last'
)
```

## 10.2 `replace()`

**Definition:** Replaces specific values with new values.

```python
df['City'] = df['City'].replace(
    'Bangalore',
    'Bengaluru'
)
```

Replace multiple values:

```python
df['Gender'] = df['Gender'].replace({
    'M': 'Male',
    'F': 'Female'
})
```

## 10.3 `drop()`

**Definition:** Removes specified rows or columns.

Remove a column:

```python
df.drop(columns=['Age'])
```

Remove multiple columns:

```python
df.drop(columns=['Age', 'City'])
```

Remove a row:

```python
df.drop(index=0)
```

## 10.4 `select_dtypes()`

**Definition:** Selects columns based on data type.

```python
df.select_dtypes(include='number')
```

Select text columns:

```python
df.select_dtypes(include='object')
```

# 11. String Operations

Pandas provides the `.str` accessor for string manipulation.

| Syntax | Definition | Example |
|---|---|---|
| `str.lower()` | Lowercase | `df['Name'].str.lower()` |
| `str.upper()` | Uppercase | `df['Name'].str.upper()` |
| `str.title()` | Title case | `df['Name'].str.title()` |
| `str.strip()` | Removes surrounding spaces | `df['Name'].str.strip()` |
| `str.len()` | String length | `df['Name'].str.len()` |
| `str.contains()` | Checks substring | `df['Name'].str.contains('A')` |
| `str.replace()` | Replaces text | `df['Name'].str.replace('Old', 'New')` |
| `str.split()` | Splits strings | `df['Name'].str.split(' ')` |
| `str.startswith()` | Checks starting text | `df['Name'].str.startswith('A')` |
| `str.endswith()` | Checks ending text | `df['Name'].str.endswith('a')` |

Example:

```python
df['Name'] = df['Name'].str.strip().str.title()
```

# 12. Date and Time Operations

## 12.1 `pd.to_datetime()`

**Definition:** Converts a column or values into datetime format.

```python
df['Date'] = pd.to_datetime(df['Date'])
```

## 12.2 Extract Date Components

```python
df['Date'].dt.year
df['Date'].dt.month
df['Date'].dt.day
df['Date'].dt.day_name()
df['Date'].dt.month_name()
df['Date'].dt.dayofweek
```

## 12.3 Date Filtering

```python
df[df['Date'] >= '2025-01-01']
```

## 12.4 `pd.date_range()`

**Definition:** Generates a sequence of dates.

```python
pd.date_range(
    start='2025-01-01',
    end='2025-01-10',
    freq='D'
)
```

# 13. Merging and Joining

These operations are especially important when working with multiple datasets.

## 13.1 `merge()`

**Definition:** Combines two DataFrames based on common columns or keys.

```python
pd.merge(
    df1,
    df2,
    on='ID',
    how='inner'
)
```

### Types of Joins

| Join | Definition |
|---|---|
| Inner | Only matching records |
| Left | All left records and matching right records |
| Right | All right records and matching left records |
| Outer | All records from both DataFrames |

Examples:

```python
pd.merge(df1, df2, on='ID', how='left')

pd.merge(df1, df2, on='ID', how='right')

pd.merge(df1, df2, on='ID', how='outer')
```

## 13.2 `merge()` vs `concat()` vs `join()`

| Function | Main Purpose |
|---|---|
| `merge()` | Combines using common keys |
| `concat()` | Stacks DataFrames along an axis |
| `join()` | Combines primarily using indexes |

# 14. Pivot Tables and Crosstab

## 14.1 `pivot_table()`

**Definition:** Summarizes data across rows and columns using an aggregation function.

```python
pd.pivot_table(
    df,
    values='Salary',
    index='Department',
    columns='City',
    aggfunc='mean'
)
```

## 14.2 `crosstab()`

**Definition:** Calculates frequency distributions between categorical variables.

```python
pd.crosstab(
    df['Department'],
    df['City']
)
```

Calculate proportions:

```python
pd.crosstab(
    df['Department'],
    df['City'],
    normalize='index'
)
```

# 15. Useful Additional Functions

| Syntax | Definition | Example |
|---|---|---|
| `copy()` | Creates a copy | `df.copy()` |
| `abs()` | Absolute values | `df['Profit'].abs()` |
| `round()` | Rounds values | `df['Price'].round(2)` |
| `clip()` | Restricts values to limits | `df['Age'].clip(18, 60)` |
| `shift()` | Moves values by periods | `df['Sales'].shift(1)` |
| `diff()` | Calculates differences | `df['Sales'].diff()` |
| `pct_change()` | Percentage change | `df['Sales'].pct_change()` |
| `rank()` | Assigns ranks | `df['Salary'].rank()` |
| `cumsum()` | Cumulative sum | `df['Sales'].cumsum()` |
| `cummax()` | Cumulative maximum | `df['Sales'].cummax()` |
| `cummin()` | Cumulative minimum | `df['Sales'].cummin()` |
| `explode()` | Expands list-like values into rows | `df.explode('Tags')` |
| `reset_index()` | Resets index | `df.reset_index()` |
| `to_dict()` | Converts to dictionary | `df.to_dict()` |

---

# 📌 Important Pandas Cheat Sheet

A quick revision table of frequently used syntax.

| Operation | Syntax |
|---|---|
| Import Pandas | `import pandas as pd` |
| Create Series | `pd.Series(data)` |
| Create DataFrame | `pd.DataFrame(data)` |
| Read CSV | `pd.read_csv('file.csv')` |
| Write CSV | `df.to_csv('file.csv', index=False)` |
| First rows | `df.head()` |
| Last rows | `df.tail()` |
| Dataset dimensions | `df.shape` |
| Dataset information | `df.info()` |
| Statistical summary | `df.describe()` |
| Column names | `df.columns` |
| Data types | `df.dtypes` |
| Select column | `df['column']` |
| Select multiple columns | `df[['col1', 'col2']]` |
| Label selection | `df.loc[]` |
| Position selection | `df.iloc[]` |
| Filter condition | `df[df['Age'] > 25]` |
| Unique values | `df['City'].unique()` |
| Frequency count | `df['City'].value_counts()` |
| Mean | `df['Salary'].mean()` |
| Median | `df['Salary'].median()` |
| Missing values | `df.isnull().sum()` |
| Fill missing values | `df.fillna(value)` |
| Drop missing values | `df.dropna()` |
| Convert datatype | `df.astype(type)` |
| Group data | `df.groupby('City')` |
| Aggregate | `df.agg()` |
| Sort values | `df.sort_values('Salary')` |
| Rename columns | `df.rename(columns={})` |
| Combine DataFrames | `pd.concat([df1, df2])` |
| Merge DataFrames | `pd.merge(df1, df2)` |
| Remove duplicates | `df.drop_duplicates()` |
| Apply a function | `df.apply(function)` |
| Map values | `df['col'].map(mapping)` |
| Filter by list | `df['col'].isin(values)` |
| Filter by range | `df['col'].between(a, b)` |
| Pivot table | `pd.pivot_table()` |
| Crosstab | `pd.crosstab()` |

---

# ⚠️ Common Mistakes to Avoid

### 1. Confusing `loc` and `iloc`

```python
df.loc[0:5]   # Label-based, includes endpoint
df.iloc[0:5]  # Position-based, excludes endpoint
```

### 2. Using `and` instead of `&`

Incorrect:

```python
df[(df['Age'] > 20) and (df['Age'] < 30)]
```

Correct:

```python
df[(df['Age'] > 20) & (df['Age'] < 30)]
```

### 3. Forgetting parentheses in conditions

Incorrect:

```python
df[df['Age'] > 20 & df['Salary'] > 30000]
```

Correct:

```python
df[(df['Age'] > 20) & (df['Salary'] > 30000)]
```

### 4. Confusing `count()` and `size()`

- `count()` counts non-null values.
- `size()` counts all rows in a group.

### 5. Forgetting to assign results

Many Pandas methods return a new object.

```python
df = df.dropna()
```

### 6. Dividing by zero or missing values

```python
df['Ratio'] = df['Points'] / df['Price']
```

Check zero and missing values before performing such calculations.

### 7. Using chained assignment

Prefer:

```python
df.loc[df['Age'] > 25, 'Category'] = 'Adult'
```

Avoid modifying a filtered intermediate DataFrame when you intend to update the original.

---

# 🔄 Real-World Data Analysis Workflow

A common workflow when working with a new dataset:

```python
# 1. Import libraries
import pandas as pd
import numpy as np

# 2. Load dataset
df = pd.read_csv('data.csv')

# 3. Explore data
print(df.head())
print(df.shape)
print(df.info())
print(df.describe())

# 4. Check missing values
print(df.isnull().sum())

# 5. Handle missing values
df['Age'] = df['Age'].fillna(df['Age'].median())

# 6. Remove duplicates
df = df.drop_duplicates()

# 7. Convert data types
df['Date'] = pd.to_datetime(df['Date'], errors='coerce')

# 8. Filter data
filtered_df = df.loc[df['Age'] >= 18]

# 9. Group and aggregate
result = filtered_df.groupby('Department').agg(
    avg_salary=('Salary', 'mean'),
    total_salary=('Salary', 'sum'),
    employee_count=('Salary', 'count')
)

# 10. Sort the result
result = result.sort_values(
    'avg_salary',
    ascending=False
)

# 11. Export result
result.to_csv('final_analysis.csv')
```

---

# 🎯 Learning Roadmap

### Completed – Kaggle Pandas Exercises

- [x] Creating Series and DataFrames
- [x] Reading and Writing Data
- [x] Indexing and Selecting
- [x] Assigning Values
- [x] Summary Functions
- [x] Maps and Apply
- [x] Grouping and Sorting
- [x] Data Types
- [x] Missing Values
- [x] Renaming and Combining

### Recommended Next Topics

- [ ] Advanced Boolean Filtering
- [ ] String Operations
- [ ] Date and Time Operations
- [ ] Duplicate Handling
- [ ] Merging and Joining
- [ ] Pivot Tables and Crosstab
- [ ] Window Functions and Rolling Statistics
- [ ] Advanced Data Cleaning
- [ ] Exploratory Data Analysis (EDA)
- [ ] Real-World Data Analysis Projects

---

# 📖 Learning Resources

1. [Kaggle – Learn Pandas](https://www.kaggle.com/learn/pandas)
2. [Official Pandas Documentation](https://pandas.pydata.org/docs/)
3. [Pandas User Guide](https://pandas.pydata.org/docs/user_guide/index.html)
4. [Pandas API Reference](https://pandas.pydata.org/docs/reference/index.html)

---

# 🚀 About This Repository

This repository is part of my continuous learning journey in Python, Data Analytics, and Data Science.

It serves as a personal reference for Pandas syntax, data manipulation techniques, data cleaning operations, and practical data analysis concepts.

The intention is to keep improving these notes while learning new concepts and applying them to real-world datasets.

**Learn → Practice → Analyze → Improve.**

⭐ If you find these notes useful, feel free to star the repository!

---

**Technologies:** Python | Pandas | NumPy | Jupyter Notebook

**Repository:** [pandas_notes](https://github.com/Surisetti-Adithya/pandas_notes)
