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

![image](https://user-images.githubusercontent.com/78123049/113039354-6a2fed80-914c-11eb-90cc-5053db3b435a.png)

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

![image](https://user-images.githubusercontent.com/78123049/113039542-a1060380-914c-11eb-91d3-a9da3bb73dc3.png)


## Frequency Distribution of Age

Let us see how old most of the workers are that have left the company.

```
Age_Dist = sns.distplot(df_filtered['Age']).set_title("Frequency Distribution of Age")
Age_Dist
```

![image](https://user-images.githubusercontent.com/78123049/113039621-ba0eb480-914c-11eb-8b2b-2e9dbac817a5.png)

According to the frequency distribution, there is one peak around age 30.  This suggests that most people who left the company were around the age of 30.  Then a factor such as retirement plays a minuscule role towards most employees who have left the position since many people do not retire at the age of 30. 

## Exploring Other Variables

### Department

What department has the most attrition?

```
Dept = df_filtered['Dept'].value_counts()[:20].plot(kind='barh').set_title("Department")
Dept
```

![image](https://user-images.githubusercontent.com/78123049/113040810-14f4db80-914e-11eb-9552-02b635018c66.png)

The department with the most attrition is Research and Development (R&D) with Sales coming in as a close second.  The last department is Human Resources (HR), which is not even close to R&D or Sales.  We can further isolate the data into Sales and R&D.

```
df_Sales = df_filtered[ df_filtered['Department'] == 'Sales' ]
df_Sales.rename(columns = {'BusinessTravel':'Travel', 'Department':'Dept', 'DistanceFromHome':'DistHome', 'Education':'EduLvl', 'EducationField':'EduField', 'EnvironmentSatisfaction':'EnviroSat', 'JobInvolvement':'JobInvolv', 'JobSatisfaction':'JobSat', 'NumCompaniesWorked':'#CoWorked', 'PercentSalaryHike':'%SalHike', 'PerformanceRating':'Performance', 'RelationshipSatisfaction':'RelationSat', 'StandardHours':'StdHrs', 'StockOptionLevel':'StockOpLvl', 'TotalWorkingYears':'#YrsWorked', 'TrainingTimesLastYear':'HrsTrainedLstYr', 'YearsAtCompany':'YrsCo', 'YearsInCurrentRole':'YrsCurrRole', 'YearsSinceLastPromotion':'YrsSinceLstPromo', 'YearsWithCurrManager':'YrsWCurrManag'}, inplace = True)
df_RD = df_filtered[df_filtered['Department'] == 'Research & Development']
df_RD.rename(columns = {'BusinessTravel':'Travel', 'Department':'Dept', 'DistanceFromHome':'DistHome', 'Education':'EduLvl', 'EducationField':'EduField', 'EnvironmentSatisfaction':'EnviroSat', 'JobInvolvement':'JobInvolv', 'JobSatisfaction':'JobSat', 'NumCompaniesWorked':'#CoWorked', 'PercentSalaryHike':'%SalHike', 'PerformanceRating':'Performance', 'RelationshipSatisfaction':'RelationSat', 'StandardHours':'StdHrs', 'StockOptionLevel':'StockOpLvl', 'TotalWorkingYears':'#YrsWorked', 'TrainingTimesLastYear':'HrsTrainedLstYr', 'YearsAtCompany':'YrsCo', 'YearsInCurrentRole':'YrsCurrRole', 'YearsSinceLastPromotion':'YrsSinceLstPromo', 'YearsWithCurrManager':'YrsWCurrManag'}, inplace = True)
```

We will be exploring the Sales and R&D data later after we try to pinpoint any major differences between those who have stayed at or left the company.

That is why I have created another set of data for those who have stayed at the company.

```
df_no1 = df[df['Attrition'] == 'No']
df_no = df_no1.drop(["Attrition", "DailyRate", "EmployeeCount", "EmployeeNumber", "MonthlyIncome", "MonthlyRate", "Over18"], axis = 1)
df_no.rename(columns = {'BusinessTravel':'Travel', 'Department':'Dept', 'DistanceFromHome':'DistHome', 'Education':'EduLvl', 'EducationField':'EduField', 'EnvironmentSatisfaction':'EnviroSat', 'JobInvolvement':'JobInvolv', 'JobSatisfaction':'JobSat', 'NumCompaniesWorked':'#CoWorked', 'PercentSalaryHike':'%SalHike', 'PerformanceRating':'Performance', 'RelationshipSatisfaction':'RelationSat', 'StandardHours':'StdHrs', 'StockOptionLevel':'StockOpLvl', 'TotalWorkingYears':'#YrsWorked', 'TrainingTimesLastYear':'HrsTrainedLstYr', 'YearsAtCompany':'YrsCo', 'YearsInCurrentRole':'YrsCurrRole', 'YearsSinceLastPromotion':'YrsSinceLstPromo', 'YearsWithCurrManager':'YrsWCurrManag'}, inplace = True)
```

### Distance Traveled to Work

Would distance traveled be a reason for attrition? We can compare the distance of those who are still at the job to those who have left in comparison to distance traveled for work.

```
Dist = sns.distplot(df_filtered['DistHome'])
Dist
All = sns.distplot(df_no['DistHome']).set_title("All Employee Distance From Home")
All
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113041612-0529c700-914f-11eb-93a1-7930658b9116.png)

By looking at the graph, we can see that there is a double peak for those who are still employed well before the 10-mile mark.  There is a single peak before the 10-mile mark for those who have been terminated but not nearly as much as those who are still employed by IBM.  There is a second peak well past the 20-mile mark, suggesting that those who have been terminated could have been due to the drive.  However, this may explain some of the numbers since most live less than 10 miles away. 

### Education Level and Field

Does the education level of the employees determine their rate of attrition?  My hypothesis is that education level may show that those who have a higher degree are more likely to stay at the job than those who do not.  

```
EduLvl_Yes = sns.distplot(df_filtered['EduLvl'])
EduLvl_Yes
EduLvl_No = sns.distplot(df_no['EduLvl']).set_title("All Employee Education Level")
EduLvl_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113042469-16bf9e80-9150-11eb-977d-d590271441b4.png)

My hypothesis was incorrect, and the education level plays no role in the rate of attrition amongst the employees at IBM.  Maybe those who have a degree that are not utilized at IBM leave in order to use their degree professionally.

```
EduFld_Yes = df_filtered['EduField'].value_counts()[:20].plot(kind='barh').set_title("Education Field of Former Employees")
EduFld_Yes
```

![image](https://user-images.githubusercontent.com/78123049/113043202-f3e1ba00-9150-11eb-9752-6690e1abdaac.png)

```
EduFld_No = df_no['EduField'].value_counts()[:20].plot(kind='barh').set_title("Education Field of Current Employees")
EduFld_No
```

![image](https://user-images.githubusercontent.com/78123049/113043284-14aa0f80-9151-11eb-84a5-1bfdb80f62f8.png)

This assumption was also unfounded since the rate of education field is very similar to the current employees and those who have left IBM.

### Workplace Environment

Did the workplace environment lead to employee attrition?  This can be easily determined by graphing both former and current employees to see if the workplace environment plays any role in attrition.   

```
EnviroSat_Yes = sns.distplot(df_filtered['EnviroSat'])
EnviroSat_Yes
EnviroSat_No = sns.distplot(df_no['EnviroSat']).set_title("All Employee Environmental Satisfaction")
EnviroSat_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113043502-63f04000-9151-11eb-8e26-bf4f00006457.png)

By looking at the graph, there seems to be a trend that the workplace environment plays an important role for those who currently work at IBM.  A lot of the ones who left IBM gave the workplace environment a 1 rating.  This could suggest that some of the workers left due to the workplace environment.  The workplace environment could have been a small reason since many of those who left IBM also gave a 3 and a 4 rating.  

### Gender

Would gender play a role in the attrition occurring at IBM?

```
Gender_Yes = df_filtered['Gender'].value_counts()[:20].plot(kind='barh').set_title("Gender of Former Employees")
Gender_Yes
```

![image](https://user-images.githubusercontent.com/78123049/113043689-a6b21800-9151-11eb-9c39-21cec1f691cd.png)

There are more men than females that have left IBM but not enough to suggest that men are leaving at a higher rate than women.

### Hourly Rate

Does the hourly rate of pay for both current and former employees create attrition in the workplace?

```
HrlyRate_Yes = sns.distplot(df_filtered['HourlyRate'])
HrlyRate_Yes
HrlyRate_No = sns.distplot(df_no['HourlyRate']).set_title("All Employee Hourly Rate")
HrlyRate_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113044079-217b3300-9152-11eb-860f-07143ddfe41b.png)

So the current employees have a slight edge than the former employess since they have a higher peak at the $80 and hour mark while the former employees peak at a little over $50 an hour peak.  This might suggest that employees left due to the wage gap between employees.  However, more specialized work demands higher pay.

### Job Involvement, Level, and Role

Would how involved workers are in their job decide whether they would leave the company?

```
JobInvolv_Yes = sns.distplot(df_filtered['JobInvolv'])
JobInvolv_Yes
JobInvolv_No = sns.distplot(df_no['JobInvolv']).set_title("All Employee Job Involvement")
JobInvolv_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113044709-d31a6400-9152-11eb-8cab-851b2b70aeea.png)

Current employees might have an edge in being more involved in their jobs but this could also prove to be a motivation issue.  The reason that the former employees aren't as involved as the current employees could be any number of reasons.

The job level might show what level employees were at when leaving IBM.

```
JobLvl_Yes = sns.distplot(df_filtered['JobLevel'])
JobLvl_Yes
JobLvl_No = sns.distplot(df_no['JobLevel']).set_title("All Employees Job Levels")
JobLvl_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113045162-5936aa80-9153-11eb-9ad4-e7e841c04dfa.png)

The job level showed that most left IBM at an entry level position which is very important.  This can be due to a number of factors such as the hiring process, motivating these employees in the long run, or even having them buy into the company culture.  I would like to see specific survey questions in relation to job level.  Why did they leave an entry level position at IBM?

Are their specific Jobs that see more attrition than others?

```
JobRole_Yes = df_filtered['JobRole'].value_counts()[:20].plot(kind='barh').set_title("Job Role of Former Employees")
JobRole_Yes
```

![image](https://user-images.githubusercontent.com/78123049/113045581-dfeb8780-9153-11eb-83fc-39c63192bf9d.png)

```
JobRole_No = df_no['JobRole'].value_counts()[:20].plot(kind='barh').set_title("Job Role of Current Employees")
JobRole_No
```

![image](https://user-images.githubusercontent.com/78123049/113045643-f0036700-9153-11eb-800b-95df836fcd1a.png)

By looking at the job roles for the former and current employees, that the top 3 for both categories are laboratory technicians, research scientists, and sales executives.  These are specialized jobs that require time and training to find a replacement.  However, they alos have the highest numbers of those who have stayed at the company.

### Marital Status

I think marital status will play an important role in attrition at IBM?

```
MaritalStatus_Yes = df_filtered['MaritalStatus'].value_counts()[:20].plot(kind='barh').set_title("Marital Status of Former Employees")
MaritalStatus_Yes
```

![image](https://user-images.githubusercontent.com/78123049/113052391-e978ed80-915b-11eb-8b44-4d17b6a40819.png)

```
MaritalStatus_No = df_no['MaritalStatus'].value_counts()[:20].plot(kind='barh').set_title("Marital Status of Current Employees")
MaritalStatus_No
```

![image](https://user-images.githubusercontent.com/78123049/113052759-5ee4be00-915c-11eb-9700-f61794e35056.png)

There are more single people leaving the job in comparison to those who are married.  Those who are married have the highest number of people who stayed with IBM.  This makes sense since married people need something stable and leaving the job for any reason isn't very stable.  It would be interesting to add another column such as married with kids.

### Number of Companies Worked

Those who have a tendency to work for a lot of companies might leave after a certaine amount of time.  Does this hold true for IBM?

```
NumCo_Yes = sns.distplot(df_filtered['#CoWorked'])
NumCo_Yes
NumCo_No = sns.distplot(df_no['#CoWorked']).set_title("All Employee Number of Companies Worked")
NumCo_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113053187-dc103300-915c-11eb-8006-71605487d1da.png)

The results have surprised me since both the former and current employees are almost identical in the number of companies they have worked.  The number of companies worked plays a nonexistant role in attrition.

### Over Time

Can over time be a factor of attrition at IBM?

```
OT_Yes = df_filtered['OverTime'].value_counts()[:20].plot(kind='barh').set_title("Former Employee Over Time")
OT_Yes
```

![image](https://user-images.githubusercontent.com/78123049/113054310-1deda900-915e-11eb-93e1-64daaf4dfd7d.png)

```
OT_No = df_no['OverTime'].value_counts()[:20].plot(kind='barh').set_title("Current Employee Over Time")
OT_No
```

![image](https://user-images.githubusercontent.com/78123049/113054367-2d6cf200-915e-11eb-8a55-6f356f17873e.png)

There are more former employees who took over time in compairson to the current employees.  I would want to know if over time was forced upon those employees.

### Percent Salary Hike

Does the percent salary hike give an employees a reason to leave the job?

```
SalHike_Yes = sns.distplot(df_filtered['%SalHike'])
SalHike_Yes
SalHike_No = sns.distplot(df_no['%SalHike']).set_title("All Employee Percent Salary Hike")
SalHike_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113054787-ab30fd80-915e-11eb-86b3-144d0b694724.png)

Surprisingly, both graphs are equal which would suggest an even salary hike amongst employees.

### Performance Rating

```
Perform_Yes = df_filtered['Performance'].value_counts()[:20].plot(kind='barh').set_title("Former Employee Performance Rating")
Perform_Yes
```

![image](https://user-images.githubusercontent.com/78123049/113054988-eaf7e500-915e-11eb-9988-c1fab34b5218.png)

```
Perform_No = df_no['Performance'].value_counts()[:20].plot(kind='barh').set_title("Current Employee Performance Rating")
Perform_No
```

![image](https://user-images.githubusercontent.com/78123049/113055032-fd721e80-915e-11eb-81c7-fc1cb4929ed7.png)

Both, current and former employees, showed equal levels of performance.  This scale isn't in depth enough for any genuine data.  I would suggest a scale out of 10 for more specific results.  This would allow managers to give more specific performance rating.  Many people would fall into 3 for above average work but if the scale was out of 10, those employees could be given a score from 6-8.

### Manager Relationship Satisfaction

The relationship an employee has with their manager is very important for productivity but is it also important when discussing attrition.

```
RelationSat_Yes = sns.distplot(df_filtered['RelationSat'])
RelationSat_Yes
RelationSat_No = sns.distplot(df_no['RelationSat']).set_title("All Employee Manager Relatioship Satisfaction")
RelationSat_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113056115-3d85d100-9160-11eb-9093-b86a11735f6f.png)

The graph shows increasing numbers of employee satisfaction with current employees while former employees shows a neutral trend towards manager relationship satisifaction.  This could mean that manager relationship satisfaction is important for some but not others.

### Employee Stock Option Level

Giving employees an opportunity to own a part of the company is great compensation for many employees and can increase productivity. Can employees leave if they aren't given that option.

```
StockOp_Yes = sns.distplot(df_filtered['StockOpLvl'])
StockOp_Yes
StockOp_No = sns.distplot(df_no['StockOpLvl']).set_title("All Employee Stock Option Level")
StockOp_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113056706-fc41f100-9160-11eb-90f2-eaea1f0f6a9a.png)

The graph shows that many employees are willing to stay at IBM if they are given at least some stock options.  There are many employees who are currently working for the company given at least the first level in stock options while many who have left the company have been given 0 stock options.

### Total Working Years

Which kind of employees are leaving at IBM, are they the first time career workers or have they worked for years?

```
YrsWorked_Yes = sns.distplot(df_filtered['#YrsWorked'])
YrsWorked_Yes
YrsWorked_No = sns.distplot(df_no['#YrsWorked']).set_title("All Employee Total Working Years")
YrsWorked_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113057185-96099e00-9161-11eb-8d88-4b06bc83ec3b.png)

The graph shows that many who leave IBM are first time workers but a lot of workers who have stayed at IBM have worked for less than 10 years in total.

### Employee Hours Trained

The number of hours trained for employees could show prepare employees better for the job.  The ones who are given more hours might be more likely to stay.

```
TrainTime_Yes = sns.distplot(df_filtered['HrsTrainedLstYr'])
TrainTime_Yes
TrainTime_No = sns.distplot(df_no['HrsTrainedLstYr']).set_title("All Employee Hours Trained Last Year")
TrainTime_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113057611-1d571180-9162-11eb-889b-7454b8dd1ad0.png)

Surprisingly, employees hours trained is uniform between both current and former employees.  There appears to be no underlying reason as to why hours trained might play a role in employee attrition.

### Work Life Balance

Work life balance is important in the workplace and those who feel that they aren't given appropriate balance, such as taking work home, might leave their current job.

```
WrkLife_Yes = sns.distplot(df_filtered['WorkLifeBalance'])
WrkLife_Yes
WrkLife_No = sns.distplot(df_no['WorkLifeBalance']).set_title("All Employee Work Life Balance Rating")
WrkLife_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113057928-83439900-9162-11eb-955c-ffae873d6701.png)

Many more current employees would rank work life balance at a 3 out of 4 than former employees suggesting that there is some validity to my earlier hypothesis.  

### Years Worked at Current Company

The longer an employee stays with a company, the less likely that employee will leave that company.  Does this prove to be true at IBM?

```
YrsCo_Yes = sns.distplot(df_filtered['YrsCo'])
YrsCo_Yes
YrsCo_No = sns.distplot(df_no['YrsCo']).set_title("All Employee Years Worked at Company")
YrsCo_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113058397-eb927a80-9162-11eb-9316-3c447fbebee5.png)

Those who have just started working are more likely to leave but once they hit the 5 year mark, they will most likely stay at IBM.  Employee retention in the first two years is very low in comparison to those who have stayed.

### Years in Current Role

Does the amount of years worked in their current role play a role in employee attrition?

```
YrsCurrRole_Yes = sns.distplot(df_filtered['YrsCurrRole'])
YrsCurrRole_Yes
YrsCurrRole_No = sns.distplot(df_no['YrsCurrRole']).set_title("All Employee Years in Current Role")
YrsCurrRole_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113059101-e4b83780-9163-11eb-93ae-334a18fc7b60.png)

The only interesting data is the peak at the 7 year mark.  The 0-2 year mark was also shown in the years worked at company graph.  This shows that those who don't move into a different role by this period might leave but IBM has a lot more employees staying in their current role than those leaving.  However, years in current role might not play a part in employee attrition since both current and former employee graphs are almost idetntical.

### Years Since Last Promotion

Those who do not get promotions as regularly as others might find work at another company.

```
YrsPromo_Yes = sns.distplot(df_filtered['YrsSinceLstPromo'])
YrsPromo_Yes
YrsPromo_No = sns.distplot(df_no['YrsSinceLstPromo']).set_title("All Employee Years Since Last Promotion")
YrsPromo_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113059578-77f16d00-9164-11eb-81e4-345d3da6cd17.png)

So both current and former employees graphs are almost identical suggesting that years since last promotion plays no role in employee attrition.

### Years With Current Manager

Do the years spent with their current manager play a role in attrition?

```
YrsCurrManag_Yes = sns.distplot(df_filtered['YrsWCurrManag'])
YrsCurrManag_Yes
YrsCurrManag_No = sns.distplot(df_no['YrsWCurrManag']).set_title("All Employee Years With Current Maanger")
YrsCurrManag_No
plt.legend(labels=['Former','Current'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113059977-ed5d3d80-9164-11eb-8a5e-05235862002c.png)

Once we get past the five year mark, employees are more likely to stay with their current manger than leave them.  However, a lot of employees have left the job at that point as well.  What happened between the 0-2 year mark makes sense when looking at past data.  Years with current manager seems to not play a role in employee attrition since the graphs for both current and former employees are similar.

## Analysis of Former Sales and R&D Employees

### Environmental Satisfaction

The environmental satisfaction for former employees was much lower than current employees but are there any disparities between former sales and R&D employees.

```
EnviroSat_Sales = sns.distplot(df_Sales['EnviroSat'])
EnviroSat_Sales
EnviroSat_RD = sns.distplot(df_RD['EnviroSat']).set_title("Environmental Satisfaction of Former Sales and R&D Employees")
EnviroSat_RD
plt.legend(labels=['Sales','RD'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113062618-e33d3e00-9168-11eb-8d78-ffb6040f321a.png)

The only stark difference between the two departments is the level 2 rating.  More sales employees rated their environmental satisfaction at a level 2 while R&D had less uniformity.  Those who did not rate environmental satisfaction at a level 2, placed that rating at 3 and 4.  This suggest a little more people in the R&D department enjoyed their workplace environment in comparison to Sales employees.

### Job Levels

A lot of employees at the level 1 job level left IBM in comparison to those who have stayed.  More employees have stayed at IBM when they are at the level 2 job level.  Are there any differences amongst former sales and R&D employees.

```
JobLvl_Sales = sns.distplot(df_Sales['JobLevel'])
JobLvl_Sales
JobLvl_RD = sns.distplot(df_RD['JobLevel']).set_title("Sales and R&D Employees Job Levels")
JobLvl_RD
plt.legend(labels=['Sales','R&D'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113063260-dec55500-9169-11eb-935f-5c4fe56a82d8.png)

A lot more R&D employees left IBM at the first job level while more sales employees left IBM at the second job level.  Something at these levels could be giving employees from both R&D and sales a reason to leave IBM.

### Over Time

Since there was a disparity between current and former employees regarding over time where more former employees worked more over time; is there a disparity between former sales and R&D employees?

```
OT_Sales = df_Sales['OverTime'].value_counts()[:20].plot(kind='barh').set_title("Former Sales Employee Over Time")
OT_Sales
```

![image](https://user-images.githubusercontent.com/78123049/113063854-cf92d700-916a-11eb-9192-35703768abd7.png)


```
OT_RD = df_RD['OverTime'].value_counts()[:20].plot(kind='barh').set_title("Former R&D Employee Over Time")
OT_RD
```

![image](https://user-images.githubusercontent.com/78123049/113063899-e1747a00-916a-11eb-8d71-637a8a8cd632.png)

There isn't a disparity in the use of over time between former sales and R&D employees.

### Stock Option Level

What differences are their between former sales and R&D employees regarding stock options?

```
StockOp_Sales = sns.distplot(df_filtered['StockOpLvl'])
StockOp_Sales
StockOp_RD = sns.distplot(df_no['StockOpLvl']).set_title("Sales and R&D Employee Stock Option Level")
StockOp_RD
plt.legend(labels=['Sales','R&D'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113064129-462fd480-916b-11eb-8f27-90c0d9f2edc3.png)

The R&D department was given more stock options at level 1 and 2 in comparison to sales.

### Work Life Balance

Are there differences in work life balance between former sales and R&D departments?

```
WrkLife_Sales = sns.distplot(df_Sales['WorkLifeBalance'])
WrkLife_Sales
WrkLife_RD = sns.distplot(df_RD['WorkLifeBalance']).set_title("Sales and R&D Employee Work Life Balance Rating")
WrkLife_RD
plt.legend(labels=['Sales','R&D'],fontsize = 12)
```

![image](https://user-images.githubusercontent.com/78123049/113064344-aa529880-916b-11eb-9bb4-cd5923d1aa0a.png)

There aren't any disparities between sales and R&D departments regarding work life balance.  It also matches the former employees graph where a lot of employees ranked their work life balance at a 3 out of 4.
