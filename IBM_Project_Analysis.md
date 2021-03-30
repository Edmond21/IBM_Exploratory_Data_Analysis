# IBM Eploratroy Data Analysis

## Introduction to Dataset

The company I chose was due to the availability of the dataset.  The IBM community have a lot of data sets but ultimately, I found the data set from the HR department through Kaggle from the user pavansubhash.  The csv file contained 35 columns and 1469 rows.  The 35 variables included:

1.	Age
2.	Attrition
3.	Business Travel
4.	Daily Rate
5.	Department 
6.	Distance From Home
7.	Education (1-5)
    1.	Below College
    2.	College
    3.	Bachelor
    4.	Master
    5.	Doctorate
8.	Education Field
9.	Employee Count
10.	Employee Number
11.	Environment Satisfaction (1-4)
    1.	Low
    2.	Medium
    3.	High
    4.	Very High
12.	Gender
13.	Hourly Rate
14.	Job Involvement (1-4)
    1.	Low
    2.	Medium
    3.	High
    4.	Very High
15.	Job Level
16.	Job Role
17.	Job Satisfaction (1-4)
    1.	Low
    2.	Medium
    3.	High
    4.	Very High
18.	Marital Status
19.	Monthly Income
20.	Monthly Rate
21.	Number Companies Worked
22.	Over 18
23.	Overtime
24.	Percent Salary Hike
25.	Performance Rating (1-4)
    1.	Low
    2.	Good
    3.	Excellent
    4.	Outstanding
26.	Relationship Satisfaction (1-4)
    1.	Low
    2.	Medium
    3.	High
    4.	Very High
27.	Standard Hours
28.	Stock Option Level
29.	Total Working Years
30.	Training Time Last Year
31.	Work Life Balance (1-4)
    1.	Bad
    2.	Good
    3.	Better
    4.	Best
32.	Years at Company
33.	Years in Current Role
34.	Years Since Last Promotion
35.	Years with Current Manager

## Data Organization and Cleaning

The first thing I want to do is eliminate all the rows with ‘No’ in Attrition.  Those who are still at IBM are not the ones I want to analyze. 

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('C:/Users/jacob/OneDrive/Desktop/IBM HR Analytics/archive/HR_Employee_Attrition.csv')

df_filtered1 = df[ df['Attrition'] == 'Yes' ]
```

Then I want to eliminate columns such as Attrition, Daily Rate, Employee Count, Employee Number, Monthly Income, Monthly Rate, and Over 18.  These columns are either redundant or not necessary for the analysis I want to conduct.  I am now left with 237 rows and 28 columns.

```
df_filtered = df_filtered1.drop(["Attrition", "DailyRate", "EmployeeCount", "EmployeeNumber", "MonthlyIncome", "MonthlyRate", "Over18"], axis = 1)
```

I have done the same for those who are still working at IBM

```
df_no1 = df[df['Attrition'] == 'No']
df_no = df_no1.drop(["Attrition", "DailyRate", "EmployeeCount", "EmployeeNumber", "MonthlyIncome", "MonthlyRate", "Over18"], axis = 1)
```

By looking at the column names, I need to change the names of a few columns to make it easier on myself when addressing the data.

```
for col in df_filtered.columns:
    print(col)
```

![](C:/Users/jacob/OneDrive/Desktop/IBM_HR_Analytics/Pictures/Unchanged_Columns.png)

The changes I made are:
* BusinessTravel -> Travel
* Department -> Dept
* DistanceFromHome -> DistHome
* Education -> EduLvl (Edu would have been too broad)
* EducationField -> EduField
* EnvironmentSatisfaction -> EnviroSat
* JobInvolvement -> JobInvolv
* JobSatisfaction -> JobSat
* NumCompaniesWorked -> #CoWorked
* PercentSalaryHike -> %SalHike
* PerformanceRating -> Performance
* RelationshipSatisfaction -> RelationSat
* StandardHours -> StdHrs
* StockOptionLevel -> StockOpLvl
* TotalWorkingYears -> #YrsWorked
* TrainingTimesLastYear -> HrsTrainedLstYr
* YearsAtCompany -> YrsCo
* YearsInCurrentRole -> YrsCurrRole
* YearsSinceLastPromotion -> YrsSinceLstPromo
* YearsWithCurrManager -> YrsWCurrManag

```
df_filtered.rename(columns = {'BusinessTravel':'Travel', 'Department':'Dept', 'DistanceFromHome':'DistHome', 'Education':'EduLvl', 'EducationField':'EduField', 'EnvironmentSatisfaction':'EnviroSat', 'JobInvolvement':'JobInvolv', 'JobSatisfaction':'JobSat', 'NumCompaniesWorked':'#CoWorked', 'PercentSalaryHike':'%SalHike', 'PerformanceRating':'Performance', 'RelationshipSatisfaction':'RelationSat', 'StandardHours':'StdHrs', 'StockOptionLevel':'StockOpLvl', 'TotalWorkingYears':'#YrsWorked', 'TrainingTimesLastYear':'HrsTrainedLstYr', 'YearsAtCompany':'YrsCo', 'YearsInCurrentRole':'YrsCurrRole', 'YearsSinceLastPromotion':'YrsSinceLstPromo', 'YearsWithCurrManager':'YrsWCurrManag'}, inplace = True)
for col in df_filtered.columns:
    print(col)
```

![](C:/Users/jacob/OneDrive/Desktop/IBM_HR_Analytics/Pictures/Changed_Columns.png)

## Frequency Distribution of Age

Let us see how old most of the workers are that have left the company.

```
Age_Dist = sns.distplot(df_filtered['Age']).set_title("Frequency Distribution of Age")
Age_Dist
```

![](C:/Users/jacob/OneDrive/Desktop/IBM_HR_Analytics/Pictures/Frequency_of_Age.png)
