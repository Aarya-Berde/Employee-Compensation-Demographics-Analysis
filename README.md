# Employee-Compensation-Demographics-Analysis

## **Overview**

This project provides a comprehensive exploratory data analysis (EDA) of employee demographics, job-related variables, and compensation. The dataset contains **10,000 employee records** across **16 columns**, including age, gender, education, working hours, occupation, field, company type, salary, package, overtime, bonuses, and allowances.

The goal is to uncover trends, patterns, and insights that help organizations and HR professionals make **data-driven decisions** related to workforce planning, salary management, and employee benefits.

---

## **Dataset**

The dataset includes the following columns:

| Column           | Description                                                     |
| ---------------- | --------------------------------------------------------------- |
| Age              | Employee age in years                                           |
| Gender           | Employee gender (Male, Female, Other)                           |
| Education Level  | Highest qualification (High School, Bachelor’s, Master’s, etc.) |
| Working Hours    | Average weekly working hours                                    |
| Occupation       | Employee role/title                                             |
| Field            | Department or professional field (IT, Finance, CA, etc.)        |
| Company Type     | MNC, Startup, or Other                                          |
| Salary Per Month | Monthly salary in INR                                           |
| Package          | Total annual package in INR                                     |
| Overtime Pay     | Additional pay for overtime                                     |
| Bonus            | Annual bonus amount                                             |
| Allowances       | Other allowances included in total compensation                 |
| Location         | City of employment                                              |
| Marital Status   | Married, Single, etc.                                           |
| Experience       | Years of experience (optional)                                  |
| Job Type         | On-site, Remote, Hybrid                                         |

---

## **EDA Questions**

### **1. Univariate Analysis** (Individual columns)

1. Age distribution of employees
2. Gender ratio in the dataset
3. Distribution of education levels
4. Working hours distribution
5. Salary per month & package distribution

**Visualization Types:** Histogram, KDE, Bar chart, Boxplot, Pie chart

---

### **2. Bivariate Analysis** (Two variables)

6. Does age affect salary/package?
7. Does gender affect salary/package?
8. Does education level influence salary/package?
9. Do working hours correlate with salary/package?
10. How does occupation affect salary?
11. Do people in different fields earn differently?
12. Does company type (MNC vs Startup) affect package?
13. How does location influence salary?

**Visualization Types:** Scatterplot, Boxplot, Grouped bar chart, Correlation heatmap

---

### **3. Multivariate Analysis** (3+ variables combined)

14. Does education + gender together impact salary?
15. How does experience + education level together influence package?
16. Does field + occupation + company type together affect salary?
17. How do working hours + overtime pay together affect salary?
18. Does bonus + allowances together significantly increase total package?

**Visualization Types:** Grouped bar plot, Interaction plot, Heatmap, Bubble chart, Correlation heatmap

---

## **Visualizations**

Below are sample visualizations used in the analysis (replace placeholders with your actual plots):

### **Univariate Examples**

* Age distribution: `histogram + KDE`
* Gender ratio: `bar chart`
* Education levels: `horizontal bar chart`

### **Bivariate Examples**

* Age vs Salary: `scatterplot + correlation heatmap`
* Gender vs Salary: `boxplot + pie chart`
* Education vs Package: `boxplot`

### **Multivariate Examples**

* Education + Gender vs Package: `grouped bar plot`
* Working hours + Overtime Pay vs Salary: `heatmap`
* Bonus + Allowances vs Package: `correlation heatmap`


## **1. Libraries & Dataset Loading**

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import warnings
warnings.filterwarnings('ignore')

df = pd.read_csv("employees_dataset.csv")  # Load the dataset
```
* `pandas` is used to manipulate and analyze tabular data.
* `matplotlib.pyplot` and `seaborn` are used for data visualization.
* `numpy` is used for numeric operations like logarithms or calculations.
* `warnings.filterwarnings('ignore')` suppresses unnecessary warnings to keep output clean.
* `pd.read_csv()` loads the CSV dataset into a DataFrame called `df`.

**Purpose:** Prepares the environment for data analysis.

---

## **2. Univariate Analysis**

### 2.1 Age distribution

```python
plt.figure(figsize=(8,5))
sns.histplot(df['Age'], kde=True, bins=30, color='skyblue')
plt.title("Age Distribution of Employees")
plt.xlabel("Age")
plt.ylabel("Count")
plt.show()
```
* `sns.histplot()` creates a histogram of `Age` to show frequency distribution.
* `kde=True` overlays a smooth density curve for better visualization.
* `bins=30` defines how many intervals to divide the age data into.

**Purpose:** Understand age demographics of employees.

---

### 2.2 Gender ratio

```python
plt.figure(figsize=(6,4))
sns.countplot(x='Gender', data=df, palette='Set2')
plt.title("Gender Ratio")
plt.show()
```

* `sns.countplot()` counts occurrences of each category in `Gender`.
* `palette` sets the colors of bars.

**Purpose:** Show the proportion of male, female, and other employees.

---

### 2.3 Education levels

```python
plt.figure(figsize=(8,5))
sns.countplot(y='Education Level', data=df, order=df['Education Level'].value_counts().index, palette='Set3')
plt.title("Distribution of Education Levels")
plt.show()
```
* `y='Education Level'` plots bars horizontally for readability.
* `order` ensures bars are sorted by frequency.

**Purpose:** Identify the most common education qualifications.

---

### 2.4 Working hours distribution

```python
plt.figure(figsize=(8,5))
sns.histplot(df['Working Hours'], kde=True, bins=30, color='salmon')
plt.title("Working Hours Distribution")
plt.xlabel("Hours per Week")
plt.ylabel("Count")
plt.show()
```
* Histogram shows how many employees work specific hours.
* `kde=True` adds a smooth curve.

```python
plt.figure(figsize=(6,4))
sns.boxplot(x=df['Working Hours'], color='lightgreen')
plt.title("Working Hours Boxplot")
plt.show()
```
* Boxplot shows **median, quartiles, and outliers** of working hours.

**Purpose:** Identify overtime trends or extreme working patterns.

---

### 2.5 Salary / Package distribution

```python
plt.figure(figsize=(8,5))
sns.histplot(df['Salary Per Month'], kde=True, bins=30, color='purple')
plt.title("Salary per Month Distribution")
plt.xlabel("Salary")
plt.ylabel("Count")
plt.show()
```
* Shows frequency of different salary ranges.

```python
plt.figure(figsize=(8,5))
sns.histplot(np.log1p(df['Package']), kde=True, bins=30, color='orange')
plt.title("Package Distribution (Log Scale)")
plt.xlabel("Log(Package)")
plt.ylabel("Count")
plt.show()
```
* `np.log1p()` is used to handle skewed data; compresses extreme values.

**Purpose:** Detect outliers and understand typical salary ranges.

---

## **3. Bivariate Analysis**

### 3.1 Age vs Salary / Package

```python
sns.scatterplot(x='Age', y='Salary Per Month', data=df, hue='Gender', alpha=0.6)
plt.title("Age vs Salary")
plt.show()
```
* Scatter plot shows relationship between age and salary.
* `hue='Gender'` differentiates points by gender.

```python
num_cols = ['Age','Salary Per Month','Package']
corr = df[num_cols].corr()
sns.heatmap(corr, annot=True, cmap='YlGnBu')
plt.title("Correlation Heatmap")
plt.show()
```
* Correlation matrix quantifies linear relationship between numeric columns.
* Heatmap visualizes it with color intensity.

**Purpose:** Check if salary increases with age.

---

### 3.2 Gender vs Salary / Package

```python
plt.figure(figsize=(6,4))
sns.boxplot(x='Gender', y='Salary Per Month', data=df, palette='Set2')
plt.title("Salary by Gender")
plt.show()
```
* Boxplot compares salary distributions between genders.
* Highlights median, quartiles, and outliers.

**Optional Pie Chart:**

```python
avg_package = df.groupby('Gender')['Package'].mean()
plt.pie(avg_package, labels=avg_package.index, autopct='%1.1f%%', startangle=90, colors=['#66b3ff','#ff9999','#99ff99'])
plt.title("Average Package by Gender")
plt.show()
```
* Pie chart shows share of average package among gender categories.

**Purpose:** Visualize gender impact on compensation.

---

### 3.3 Education level vs Salary / Package

```python
plt.figure(figsize=(10,5))
sns.boxplot(x='Education Level', y='Package', data=df, palette='Set3')
plt.title("Package by Education Level")
plt.xticks(rotation=45)
plt.show()
```
* Boxplot shows salary distribution across different education levels.

**Purpose:** Analyze whether higher degrees earn more.

---

### 3.4 Working hours vs Salary

```python
sns.scatterplot(x='Working Hours', y='Salary Per Month', data=df, hue='Gender', alpha=0.6)
plt.title("Working Hours vs Salary")
plt.show()
```
* Scatter plot shows correlation between work hours and salary.

**Purpose:** Understand if overtime affects salary.

---

### 3.5 Occupation vs Salary

```python
plt.figure(figsize=(10,5))
sns.boxplot(x='Occupation', y='Package', data=df, palette='Set2')
plt.title("Salary by Occupation")
plt.xticks(rotation=45)
plt.show()
```

**Purpose:** Compare salaries among different roles.

---

### 3.6 Field vs Salary (Grouped bar)

```python
plt.figure(figsize=(10,5))
sns.barplot(x='Field', y='Package', hue='Gender', data=df, ci=None)
plt.title("Package by Field & Gender")
plt.xticks(rotation=45)
plt.show()
```
* Barplot grouped by gender for each field.

**Purpose:** See how field and gender jointly affect package.

---

### 3.7 Company type vs Package

```python
plt.figure(figsize=(6,4))
sns.boxplot(x='Company Type', y='Package', data=df, palette='Pastel1')
plt.title("Package by Company Type")
plt.show()
```

**Purpose:** Compare salary distribution between MNC and Startup employees.

---

### 3.8 Location vs Salary

```python
top_cities = df.groupby('Location')['Salary Per Month'].mean().sort_values(ascending=False).head(10)
plt.figure(figsize=(10,5))
sns.barplot(x=top_cities.index, y=top_cities.values, palette='Set3')
plt.title("Top 10 Cities by Average Salary")
plt.xticks(rotation=45)
plt.show()
```

**Purpose:** Identify high-paying cities.

---

## **4. Multivariate Analysis**

### 4.1 Education + Gender vs Salary

```python
sns.catplot(x='Education Level', y='Package', hue='Gender', kind='bar', data=df, height=5, aspect=2)
plt.title("Package by Education Level & Gender")
plt.xticks(rotation=45)
plt.show()
```
* Grouped bar plot visualizes the interaction effect of education and gender.

---

### 4.2 Age + Education vs Package

```python
sns.pointplot(x='Education Level', y='Package', hue=pd.qcut(df['Age'], 3, labels=['Young','Mid','Senior']), data=df)
plt.title("Package by Education & Age Group")
plt.show()
```
* Point plot shows salary trend across education levels for different age groups.

---

### 4.3 Field + Company Type vs Package (Heatmap)

```python
pivot = df.pivot_table(values='Package', index='Field', columns='Company Type', aggfunc='mean')
sns.heatmap(pivot, annot=True, fmt=".0f", cmap='YlGnBu')
plt.title("Average Package by Field & Company Type")
plt.show()
```
* Heatmap visualizes average package for combinations of Field and Company Type.

---

### 4.4 Working hours + Overtime vs Salary (Heatmap)

```python
pivot = df.pivot_table(values='Salary Per Month',
                       index=pd.qcut(df['Working Hours'], 6),
                       columns=pd.qcut(df['Overtime Pay'], 6),
                       aggfunc='mean')
plt.figure(figsize=(8,6))
sns.heatmap(pivot, annot=True, fmt=".0f", cmap='YlGnBu')
plt.title("Average Salary by Working Hours & Overtime Pay")
plt.show()
```
* Heatmap shows combined effect of working hours and overtime pay on salary.

---

### 4.5 Bonus + Allowances vs Package (Correlation Heatmap)

```python
num_cols2 = ['Bonus','Allowances','Package']
corr2 = df[num_cols2].corr()
sns.heatmap(corr2, annot=True, cmap='coolwarm')
plt.title("Correlation: Bonus & Allowances vs Package")
plt.show()
```

**Purpose:** Check whether bonus and allowances jointly increase the total package.


## **Conclusion**

This EDA project provides a **comprehensive overview of employee demographics, work patterns, and compensation trends**. It helps HR professionals and organizations:

* Identify **salary gaps by gender, field, and role**
* Optimize **compensation and benefits strategy**
* Understand **workload vs reward patterns**
* Make **data-driven workforce planning decisions**
