# 📊 Data Analytics Job Market Exploration

## 📝 Overview
This project was created out of a desire to navigate and understand the job market more effectively.  
It delves into the **top-paying** and **in-demand skills** to help identify optimal job opportunities for **Data Analysts**.

The data is sourced from [Luke Barousse's Python Lessons](https://lukebarousse.com/python), which provides a foundation for this analysis. It contains detailed information on **job titles, salaries, locations, and essential skills**.  

Through a series of Python scripts, I explore key questions such as:
- Which skills are most in demand?  
- What are the salary trends across data roles?  


---

## ❓ The Questions
Below are the main questions addressed in this project:

1. 📈 How's the **Data Analytics Job Market** in the US?  
2. 💡 What are the **most in-demand skills** for the top 3 most popular data roles?  
3. 💰 How well do **jobs and skills pay** for Data Analysts?  

---

## 🛠️ Tools I Used
For this deep dive into the data analyst job market, I used the following tools:

- **Python** – The backbone of the analysis, powering data wrangling and insights.  
  - `pandas` → Data cleaning and manipulation  
  - `matplotlib` → Data visualization  
  - `seaborn` → Advanced and polished visualizations  
- **Jupyter Notebooks** – To run scripts interactively and mix code with notes/visuals  
- **Visual Studio Code** – My go-to editor for Python development  
- **Git & GitHub** – For version control, collaboration, and sharing the project  

---

## 🧹 Data Preparation & Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring **accuracy and usability**:

- Removed duplicate entries  
- Handled missing values  
- Converted stringified lists into Python objects  
- Standardized column names for consistency  

---

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
## Filter US Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.

```python
df_US = df[df['job_country'] == 'United States']

```
# Analysis

## How's the **Data Analytics Job Market** in the US?
To find the number of jobs in US, I filtered out the positions based in US and then found number of jobs for each position and visualized with barplot. I also tried to narrow down the companies that offers more data analyst jobs and their job requirements

### Visualize Data
```python
df_plot = df_jobs['job_title_short'].value_counts().to_frame()

sns.set_theme(style='ticks')
sns.barplot(data=df_plot, x='count', y='job_title_short', hue='count', palette='dark:b_r', legend=False)
sns.despine()
plt.title('Number of Jobs in US per Job Title')
plt.xlabel('Number of Jobs in US')
plt.ylabel('')
plt.show()
```

##  What are the **most in-demand skills** for the top 3 most popular data roles? 
To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_count', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 45000) # make the scales the same

fig.suptitle('Counts of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()

```
### Results


## How well do **jobs and skills pay** for Data Analysts? 
To identify the highest-paying roles and skills, I only got jobs in the United States and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

#### Visualize Data 
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs in the US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results






