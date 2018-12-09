---
title: "Case Study 2: Employee Data Analysis"
author: "Meredith Ludlow & Kristen Rollins"
date: "December 9, 2018"
output:
  html_document:
    keep_md: yes
---



# Introduction

The purpose of this analysis is to explore what variables are good predictors for attrition rates in Fortune 1000 companies. Exploratory analytics will be used to determine which variables are the best predictors of attrition, as well as looking at other trends associated with specific jobs. Finally, we will create a model that will predict whether or not an employee will leave the company voluntarily.

# Analysis

### Exploratory Data Analysis


```r
# Read in training data
dfTrain <- read.csv("CaseStudy2-data.csv")
```

If a variable does not have a significant impact on turnover, we would expect that the attrition rate within a group is the same as the attrition rate of the entire dataset. As we see below, in the whole training set 83.9% of employees stayed while 16.1% left. So, as we view the relative rates for turnover for each categorical variable, we would expect variables with high attrition rates to be strong predictors of turnover. 

We excluded a few variables from consideration at the start because they had the same value for every employee and wouldn't be any use as predictors. Then, we took all of the numerical values and broke them up into different levels based on ranges that we chose.


```r
# Percentage of retained/lost employees
kable(table(dfTrain$Attrition) / nrow(dfTrain), 
      col.names=c("Attrition", "Percent")) %>% 
      kable_styling(full_width=FALSE)
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Attrition </th>
   <th style="text-align:right;"> Percent </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> No </td>
   <td style="text-align:right;"> 0.8393162 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yes </td>
   <td style="text-align:right;"> 0.1606838 </td>
  </tr>
</tbody>
</table>

```r
# Define variables for analysis
variables <- c("BusinessTravel", "Department", "Education", 
              "EducationField", "EnvironmentSatisfaction",
              "Gender", "JobInvolvement", "JobLevel",
              "JobRole", "JobSatisfaction", "MaritalStatus",
              "OverTime", "PerformanceRating", 
              "RelationshipSatisfaction", "StockOptionLevel", 
              "WorkLifeBalance", "Age", "DailyRate", "DistanceFromHome", 
              "HourlyRate", "MonthlyIncome", "MonthlyRate", 
              "NumCompaniesWorked", "PercentSalaryHike",
              "TotalWorkingYears", "TrainingTimesLastYear", 
              "YearsAtCompany", "YearsInCurrentRole", 
              "YearsSinceLastPromotion", "YearsWithCurrManager")

# Turn numerical values into categorical
dfTrain$Age <- cut(dfTrain$Age, breaks=c(-Inf,30,40,50,Inf),
                   labels=c("18-30","30-40","40-50","50-60"))
dfTrain$DailyRate <- cut(dfTrain$DailyRate, breaks=c(-Inf,500,1000,Inf),
                         labels=c("0-500","500-1000",">1000"))
dfTrain$DistanceFromHome <- cut(dfTrain$DistanceFromHome, breaks=c(-Inf,10,Inf),
                                labels=c("0-10",">10"))
dfTrain$HourlyRate <- cut(dfTrain$HourlyRate, breaks=c(-Inf,65,Inf),
                          labels=c("0-65",">65"))
dfTrain$MonthlyIncome <- cut(dfTrain$MonthlyIncome, breaks=c(-Inf,5000,10000,15000,Inf),
                             labels=c("0-5000","5000-10000","10000-15000",">15000"))
dfTrain$MonthlyRate <- cut(dfTrain$MonthlyRate, breaks=c(-Inf,5000,10000,15000,20000,Inf),
                           labels=c("0-5000","5000-10000","10000-15000","15000-20000",">20000"))
dfTrain$NumCompaniesWorked <- cut(dfTrain$NumCompaniesWorked, breaks=c(-Inf,4,Inf),
                                  labels=c("0-4",">4"))
dfTrain$PercentSalaryHike <- cut(dfTrain$PercentSalaryHike, breaks=c(-Inf, 13, 17, Inf),
                                 labels=c("0-13","14-17",">17"))
dfTrain$TotalWorkingYears <- cut(dfTrain$TotalWorkingYears, breaks=c(-Inf, 10, 20, Inf),
                                 labels=c("0-10","11-20",">20"))
dfTrain$TrainingTimesLastYear <- cut(dfTrain$TrainingTimesLastYear, breaks=c(-Inf, 1, 4, Inf),
                                     labels=c("0-1", "2-4", ">4"))
dfTrain$YearsAtCompany <- cut(dfTrain$YearsAtCompany, breaks=c(-Inf, 5, 15, 25, Inf), 
                              labels=c("0-5", "6-15", "16-25", ">25"))
dfTrain$YearsInCurrentRole <- cut(dfTrain$YearsInCurrentRole, breaks=c(-Inf, 5, 10, Inf),
                                  labels=c("0-5", "6-10", ">10"))
dfTrain$YearsSinceLastPromotion <- cut(dfTrain$YearsSinceLastPromotion, breaks=c(-Inf, 2, 7, Inf),
                                       labels=c("0-2", "3-7", ">7"))
dfTrain$YearsWithCurrManager <- cut(dfTrain$YearsWithCurrManager, breaks=c(-Inf, 5, 10, Inf),
                                    labels=c("0-5", "6-10", ">10"))
```

The tables generated below are the attrition rate for each level within every variable.


```r
# Make relative frequency tables for categorical variables
AbsDiff <- data.frame(Variable=character(), AvgDistance=numeric())
variables <- sort(variables)
for (var in variables) {
  freqtable <- table(dfTrain[[var]], dfTrain$Attrition)
  count <- plyr::count(dfTrain[[var]])
  RelFreq <- freqtable / count$freq
  RelFreq <- as.data.frame(cbind(var=rownames(RelFreq), RelFreq))
  RelFreq[,2] <- as.numeric(levels(RelFreq[,2]))
  RelFreq[,3] <- as.numeric(levels(RelFreq[,3]))
  colnames(RelFreq)[1] <- paste(var,sep="")
  print(kable(RelFreq,row.names=FALSE) %>% kable_styling(full_width=FALSE))
  Sum <- sum(abs(RelFreq[,3]-0.1606838))/nrow(RelFreq)
  AbsDiff <- rbind(AbsDiff, data.frame(Variable=var, AverageDistance=Sum))
}
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Age </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 18-30 </td>
   <td style="text-align:right;"> 0.7631579 </td>
   <td style="text-align:right;"> 0.1106719 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 30-40 </td>
   <td style="text-align:right;"> 0.8547009 </td>
   <td style="text-align:right;"> 0.1431452 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 40-50 </td>
   <td style="text-align:right;"> 0.8568548 </td>
   <td style="text-align:right;"> 0.1452991 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 50-60 </td>
   <td style="text-align:right;"> 0.8893281 </td>
   <td style="text-align:right;"> 0.2368421 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> BusinessTravel </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Non-Travel </td>
   <td style="text-align:right;"> 0.7511111 </td>
   <td style="text-align:right;"> 0.0909091 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Travel_Frequently </td>
   <td style="text-align:right;"> 0.8538922 </td>
   <td style="text-align:right;"> 0.1461078 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Travel_Rarely </td>
   <td style="text-align:right;"> 0.9090909 </td>
   <td style="text-align:right;"> 0.2488889 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> DailyRate </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-500 </td>
   <td style="text-align:right;"> 0.8069620 </td>
   <td style="text-align:right;"> 0.1330166 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 500-1000 </td>
   <td style="text-align:right;"> 0.8360277 </td>
   <td style="text-align:right;"> 0.1639723 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;1000 </td>
   <td style="text-align:right;"> 0.8669834 </td>
   <td style="text-align:right;"> 0.1930380 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Department </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Human Resources </td>
   <td style="text-align:right;"> 0.7891738 </td>
   <td style="text-align:right;"> 0.1304348 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Research &amp; Development </td>
   <td style="text-align:right;"> 0.8602846 </td>
   <td style="text-align:right;"> 0.1397154 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sales </td>
   <td style="text-align:right;"> 0.8695652 </td>
   <td style="text-align:right;"> 0.2108262 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> DistanceFromHome </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-10 </td>
   <td style="text-align:right;"> 0.7994350 </td>
   <td style="text-align:right;"> 0.1433824 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;10 </td>
   <td style="text-align:right;"> 0.8566176 </td>
   <td style="text-align:right;"> 0.2005650 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Education </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.8129496 </td>
   <td style="text-align:right;"> 0.1428571 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8337079 </td>
   <td style="text-align:right;"> 0.1487342 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8468085 </td>
   <td style="text-align:right;"> 0.1531915 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.8512658 </td>
   <td style="text-align:right;"> 0.1662921 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:right;"> 0.8571429 </td>
   <td style="text-align:right;"> 0.1870504 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> EducationField </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Human Resources </td>
   <td style="text-align:right;"> 0.7523810 </td>
   <td style="text-align:right;"> 0.1111111 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Life Sciences </td>
   <td style="text-align:right;"> 0.7851240 </td>
   <td style="text-align:right;"> 0.1250000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Marketing </td>
   <td style="text-align:right;"> 0.8456914 </td>
   <td style="text-align:right;"> 0.1366120 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Medical </td>
   <td style="text-align:right;"> 0.8633880 </td>
   <td style="text-align:right;"> 0.1543086 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Other </td>
   <td style="text-align:right;"> 0.8750000 </td>
   <td style="text-align:right;"> 0.2148760 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Technical Degree </td>
   <td style="text-align:right;"> 0.8888889 </td>
   <td style="text-align:right;"> 0.2476190 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> EnvironmentSatisfaction </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.7389381 </td>
   <td style="text-align:right;"> 0.1184971 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8441558 </td>
   <td style="text-align:right;"> 0.1416894 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8583106 </td>
   <td style="text-align:right;"> 0.1558442 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.8815029 </td>
   <td style="text-align:right;"> 0.2610619 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Gender </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Female </td>
   <td style="text-align:right;"> 0.8231966 </td>
   <td style="text-align:right;"> 0.1360691 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Male </td>
   <td style="text-align:right;"> 0.8639309 </td>
   <td style="text-align:right;"> 0.1768034 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> HourlyRate </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-65 </td>
   <td style="text-align:right;"> 0.8299663 </td>
   <td style="text-align:right;"> 0.1510417 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;65 </td>
   <td style="text-align:right;"> 0.8489583 </td>
   <td style="text-align:right;"> 0.1700337 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> JobInvolvement </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.6307692 </td>
   <td style="text-align:right;"> 0.0894309 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8047945 </td>
   <td style="text-align:right;"> 0.1391304 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8608696 </td>
   <td style="text-align:right;"> 0.1952055 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.9105691 </td>
   <td style="text-align:right;"> 0.3692308 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> JobLevel </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.7488584 </td>
   <td style="text-align:right;"> 0.0574713 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8304094 </td>
   <td style="text-align:right;"> 0.0588235 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.9030733 </td>
   <td style="text-align:right;"> 0.0969267 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.9411765 </td>
   <td style="text-align:right;"> 0.1695906 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:right;"> 0.9425287 </td>
   <td style="text-align:right;"> 0.2511416 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> JobRole </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Healthcare Representative </td>
   <td style="text-align:right;"> 0.6000000 </td>
   <td style="text-align:right;"> 0.0156250 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Human Resources </td>
   <td style="text-align:right;"> 0.7558685 </td>
   <td style="text-align:right;"> 0.0512821 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laboratory Technician </td>
   <td style="text-align:right;"> 0.8178295 </td>
   <td style="text-align:right;"> 0.0689655 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Manager </td>
   <td style="text-align:right;"> 0.8378378 </td>
   <td style="text-align:right;"> 0.0891089 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Manufacturing Director </td>
   <td style="text-align:right;"> 0.8529412 </td>
   <td style="text-align:right;"> 0.1470588 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Research Director </td>
   <td style="text-align:right;"> 0.9108911 </td>
   <td style="text-align:right;"> 0.1621622 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Research Scientist </td>
   <td style="text-align:right;"> 0.9310345 </td>
   <td style="text-align:right;"> 0.1821705 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sales Executive </td>
   <td style="text-align:right;"> 0.9487179 </td>
   <td style="text-align:right;"> 0.2441315 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sales Representative </td>
   <td style="text-align:right;"> 0.9843750 </td>
   <td style="text-align:right;"> 0.4000000 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> JobSatisfaction </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.7672414 </td>
   <td style="text-align:right;"> 0.1286863 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8425926 </td>
   <td style="text-align:right;"> 0.1489971 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8510029 </td>
   <td style="text-align:right;"> 0.1574074 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.8713137 </td>
   <td style="text-align:right;"> 0.2327586 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> MaritalStatus </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Divorced </td>
   <td style="text-align:right;"> 0.7440000 </td>
   <td style="text-align:right;"> 0.0795455 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Married </td>
   <td style="text-align:right;"> 0.8662900 </td>
   <td style="text-align:right;"> 0.1337100 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Single </td>
   <td style="text-align:right;"> 0.9204545 </td>
   <td style="text-align:right;"> 0.2560000 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> MonthlyIncome </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-5000 </td>
   <td style="text-align:right;"> 0.7912458 </td>
   <td style="text-align:right;"> 0.0288462 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5000-10000 </td>
   <td style="text-align:right;"> 0.8378378 </td>
   <td style="text-align:right;"> 0.1191136 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 10000-15000 </td>
   <td style="text-align:right;"> 0.8808864 </td>
   <td style="text-align:right;"> 0.1621622 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;15000 </td>
   <td style="text-align:right;"> 0.9711538 </td>
   <td style="text-align:right;"> 0.2087542 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> MonthlyRate </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-5000 </td>
   <td style="text-align:right;"> 0.8057554 </td>
   <td style="text-align:right;"> 0.1361868 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5000-10000 </td>
   <td style="text-align:right;"> 0.8097345 </td>
   <td style="text-align:right;"> 0.1487603 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 10000-15000 </td>
   <td style="text-align:right;"> 0.8464052 </td>
   <td style="text-align:right;"> 0.1535948 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 15000-20000 </td>
   <td style="text-align:right;"> 0.8512397 </td>
   <td style="text-align:right;"> 0.1902655 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;20000 </td>
   <td style="text-align:right;"> 0.8638132 </td>
   <td style="text-align:right;"> 0.1942446 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> NumCompaniesWorked </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-4 </td>
   <td style="text-align:right;"> 0.7714286 </td>
   <td style="text-align:right;"> 0.1427027 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;4 </td>
   <td style="text-align:right;"> 0.8572973 </td>
   <td style="text-align:right;"> 0.2285714 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> OverTime </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> No </td>
   <td style="text-align:right;"> 0.7014925 </td>
   <td style="text-align:right;"> 0.1053892 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Yes </td>
   <td style="text-align:right;"> 0.8946108 </td>
   <td style="text-align:right;"> 0.2985075 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> PercentSalaryHike </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-13 </td>
   <td style="text-align:right;"> 0.8247012 </td>
   <td style="text-align:right;"> 0.1483871 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 14-17 </td>
   <td style="text-align:right;"> 0.8491620 </td>
   <td style="text-align:right;"> 0.1508380 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;17 </td>
   <td style="text-align:right;"> 0.8516129 </td>
   <td style="text-align:right;"> 0.1752988 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> PerformanceRating </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8324022 </td>
   <td style="text-align:right;"> 0.1594349 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.8405651 </td>
   <td style="text-align:right;"> 0.1675978 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> RelationshipSatisfaction </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.8090909 </td>
   <td style="text-align:right;"> 0.1494253 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8410042 </td>
   <td style="text-align:right;"> 0.1542700 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8457300 </td>
   <td style="text-align:right;"> 0.1589958 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.8505747 </td>
   <td style="text-align:right;"> 0.1909091 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> StockOptionLevel </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:right;"> 0.7524752 </td>
   <td style="text-align:right;"> 0.0720000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.8219178 </td>
   <td style="text-align:right;"> 0.0877944 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.9122056 </td>
   <td style="text-align:right;"> 0.1780822 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.9280000 </td>
   <td style="text-align:right;"> 0.2475248 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> TotalWorkingYears </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-10 </td>
   <td style="text-align:right;"> 0.8099063 </td>
   <td style="text-align:right;"> 0.0897436 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 11-20 </td>
   <td style="text-align:right;"> 0.8801498 </td>
   <td style="text-align:right;"> 0.1198502 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;20 </td>
   <td style="text-align:right;"> 0.9102564 </td>
   <td style="text-align:right;"> 0.1900937 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> TrainingTimesLastYear </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-1 </td>
   <td style="text-align:right;"> 0.8076923 </td>
   <td style="text-align:right;"> 0.0915493 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2-4 </td>
   <td style="text-align:right;"> 0.8322511 </td>
   <td style="text-align:right;"> 0.1677489 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;4 </td>
   <td style="text-align:right;"> 0.9084507 </td>
   <td style="text-align:right;"> 0.1923077 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> WorkLifeBalance </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 0.6406250 </td>
   <td style="text-align:right;"> 0.1371994 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 0.8284672 </td>
   <td style="text-align:right;"> 0.1680000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 0.8320000 </td>
   <td style="text-align:right;"> 0.1715328 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 0.8628006 </td>
   <td style="text-align:right;"> 0.3593750 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> YearsAtCompany </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-5 </td>
   <td style="text-align:right;"> 0.7976974 </td>
   <td style="text-align:right;"> 0.0869565 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6-15 </td>
   <td style="text-align:right;"> 0.8421053 </td>
   <td style="text-align:right;"> 0.1197339 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 16-25 </td>
   <td style="text-align:right;"> 0.8802661 </td>
   <td style="text-align:right;"> 0.1578947 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;25 </td>
   <td style="text-align:right;"> 0.9130435 </td>
   <td style="text-align:right;"> 0.2023026 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> YearsInCurrentRole </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-5 </td>
   <td style="text-align:right;"> 0.8110661 </td>
   <td style="text-align:right;"> 0.0634921 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6-10 </td>
   <td style="text-align:right;"> 0.8797814 </td>
   <td style="text-align:right;"> 0.1202186 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;10 </td>
   <td style="text-align:right;"> 0.9365079 </td>
   <td style="text-align:right;"> 0.1889339 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> YearsSinceLastPromotion </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-2 </td>
   <td style="text-align:right;"> 0.8369441 </td>
   <td style="text-align:right;"> 0.1481481 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3-7 </td>
   <td style="text-align:right;"> 0.8443396 </td>
   <td style="text-align:right;"> 0.1556604 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;7 </td>
   <td style="text-align:right;"> 0.8518519 </td>
   <td style="text-align:right;"> 0.1630559 </td>
  </tr>
</tbody>
</table>
<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> YearsWithCurrManager </th>
   <th style="text-align:right;"> No </th>
   <th style="text-align:right;"> Yes </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 0-5 </td>
   <td style="text-align:right;"> 0.8160000 </td>
   <td style="text-align:right;"> 0.0350877 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6-10 </td>
   <td style="text-align:right;"> 0.8677686 </td>
   <td style="text-align:right;"> 0.1322314 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> &gt;10 </td>
   <td style="text-align:right;"> 0.9649123 </td>
   <td style="text-align:right;"> 0.1840000 </td>
  </tr>
</tbody>
</table>

In order to figure out which variables had attrition rates that were the most different from the attirtion rate of the data set as a whole, we had to create a metric. The metric that we used was the average absolute difference in attrition rates. We took the attrition rates under a variable and found the average difference between them and the total attrition rate. The varaibles with the higest average difference are listed in the table below.


```r
# Get average distance metric for all variables
AbsDiff <- AbsDiff[order(-AbsDiff$AverageDistance),]
kable(AbsDiff,row.names=FALSE) %>% kable_styling(full_width=FALSE)
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:right;"> AverageDistance </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> OverTime </td>
   <td style="text-align:right;"> 0.0965591 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> JobRole </td>
   <td style="text-align:right;"> 0.0863453 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> JobInvolvement </td>
   <td style="text-align:right;"> 0.0839687 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> JobLevel </td>
   <td style="text-align:right;"> 0.0736389 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MaritalStatus </td>
   <td style="text-align:right;"> 0.0678095 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> StockOptionLevel </td>
   <td style="text-align:right;"> 0.0664531 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> WorkLifeBalance </td>
   <td style="text-align:right;"> 0.0600852 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> YearsWithCurrManager </td>
   <td style="text-align:right;"> 0.0591216 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> BusinessTravel </td>
   <td style="text-align:right;"> 0.0575186 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MonthlyIncome </td>
   <td style="text-align:right;"> 0.0557392 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> YearsInCurrentRole </td>
   <td style="text-align:right;"> 0.0553023 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> TotalWorkingYears </td>
   <td style="text-align:right;"> 0.0470612 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NumCompaniesWorked </td>
   <td style="text-align:right;"> 0.0429344 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EducationField </td>
   <td style="text-align:right;"> 0.0428052 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> EnvironmentSatisfaction </td>
   <td style="text-align:right;"> 0.0415997 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Age </td>
   <td style="text-align:right;"> 0.0397734 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> YearsAtCompany </td>
   <td style="text-align:right;"> 0.0397713 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> TrainingTimesLastYear </td>
   <td style="text-align:right;"> 0.0359412 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Department </td>
   <td style="text-align:right;"> 0.0337866 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> JobSatisfaction </td>
   <td style="text-align:right;"> 0.0297588 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> DistanceFromHome </td>
   <td style="text-align:right;"> 0.0285913 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> MonthlyRate </td>
   <td style="text-align:right;"> 0.0213304 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> DailyRate </td>
   <td style="text-align:right;"> 0.0211033 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Gender </td>
   <td style="text-align:right;"> 0.0203671 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Education </td>
   <td style="text-align:right;"> 0.0138487 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> RelationshipSatisfaction </td>
   <td style="text-align:right;"> 0.0123964 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PercentSalaryHike </td>
   <td style="text-align:right;"> 0.0122525 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> HourlyRate </td>
   <td style="text-align:right;"> 0.0094960 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> YearsSinceLastPromotion </td>
   <td style="text-align:right;"> 0.0066437 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> PerformanceRating </td>
   <td style="text-align:right;"> 0.0040814 </td>
  </tr>
</tbody>
</table>

### KNN Classification

We used a KNN in model to predict whether or not an employee will leave for employees in a new data set. First, we used the top 5 variables in the above table as our predictors in the model. Adding the 6th variable didn't improve the accuracy, but adding the 7th did. Our model below is using varaibles 1-5 and 7 as predictors. We ran the model looking at the three closest points and the five closest points to see which gave better results.


```r
# Read in validation data
dfVal <- read.csv("CaseStudy2Validation.csv")

# Identify variables used to make predictions, based on avg dist metric
pred_vars <- c("OverTime", "JobRole", "JobInvolvement", "JobLevel", "MaritalStatus","WorkLifeBalance")

# Convert wanted factors into integers
dfTrain$OverTime <- as.integer(dfTrain$OverTime)
dfTrain$JobRole <- as.integer(dfTrain$JobRole)
dfTrain$MaritalStatus <- as.integer(dfTrain$MaritalStatus)
dfVal$OverTime <- as.integer(dfVal$OverTime)
dfVal$JobRole <- as.integer(dfVal$JobRole)
dfVal$MaritalStatus <- as.integer(dfVal$MaritalStatus)

# Generate attrition predictions based on training data
dfVal$dfPreds3 <- class::knn(dfTrain[,pred_vars], dfVal[,pred_vars], 
                             dfTrain$Attrition, k=3)
dfVal$dfPreds5 <- class::knn(dfTrain[,pred_vars], dfVal[,pred_vars], 
                             dfTrain$Attrition, k=5)

# Get accuracy of predictions
confusionMatrix(table(dfVal$Attrition, dfVal$dfPreds3))
```

```
## Confusion Matrix and Statistics
## 
##      
##        No Yes
##   No  246   5
##   Yes  37  12
##                                           
##                Accuracy : 0.86            
##                  95% CI : (0.8155, 0.8972)
##     No Information Rate : 0.9433          
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.3052          
##  Mcnemar's Test P-Value : 1.724e-06       
##                                           
##             Sensitivity : 0.8693          
##             Specificity : 0.7059          
##          Pos Pred Value : 0.9801          
##          Neg Pred Value : 0.2449          
##              Prevalence : 0.9433          
##          Detection Rate : 0.8200          
##    Detection Prevalence : 0.8367          
##       Balanced Accuracy : 0.7876          
##                                           
##        'Positive' Class : No              
## 
```

```r
confusionMatrix(table(dfVal$Attrition, dfVal$dfPreds5))
```

```
## Confusion Matrix and Statistics
## 
##      
##        No Yes
##   No  249   2
##   Yes  41   8
##                                           
##                Accuracy : 0.8567          
##                  95% CI : (0.8118, 0.8943)
##     No Information Rate : 0.9667          
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.2285          
##  Mcnemar's Test P-Value : 6.834e-09       
##                                           
##             Sensitivity : 0.8586          
##             Specificity : 0.8000          
##          Pos Pred Value : 0.9920          
##          Neg Pred Value : 0.1633          
##              Prevalence : 0.9667          
##          Detection Rate : 0.8300          
##    Detection Prevalence : 0.8367          
##       Balanced Accuracy : 0.8293          
##                                           
##        'Positive' Class : No              
## 
```

```r
dfPreds <- select(dfVal, ID, dfPreds3)

# Write predictions to csv file
write.csv(dfPreds, "CaseStudy2Predictions_Ludlow_Rollins.csv")
```

The KNN model that looks at the three closest data points to the test data point has a higher accuracy than the model that looks at 5. The accuracy is 85.67%. Our model is able to predict whether or not an employee will leave 85.67% of the time. The sensitivity of the model is 86.36% meaning that 86.36% of the time when the model labeled a person as not leaving, it was correct. The specificity of the model was 71.43% meaning that when someone did leave, the model was able to predict it 71.43% of the time. 

### Trends by Job Role

Next, we will look at the variable Job Role an see what trends con be seen. 


```r
# Re-load original data
dfTrain <- read.csv("CaseStudy2-data.csv")
# Job satisfaction by group
Jobs <- group_by(dfTrain, JobRole) %>% summarise(Avg=mean(JobSatisfaction, na.rm=TRUE))
kable(Jobs) %>% kable_styling(full_width = FALSE)
```

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> JobRole </th>
   <th style="text-align:right;"> Avg </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Healthcare Representative </td>
   <td style="text-align:right;"> 2.801980 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Human Resources </td>
   <td style="text-align:right;"> 2.567568 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Laboratory Technician </td>
   <td style="text-align:right;"> 2.685446 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Manager </td>
   <td style="text-align:right;"> 2.692308 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Manufacturing Director </td>
   <td style="text-align:right;"> 2.750000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Research Director </td>
   <td style="text-align:right;"> 2.625000 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Research Scientist </td>
   <td style="text-align:right;"> 2.802521 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sales Executive </td>
   <td style="text-align:right;"> 2.755814 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Sales Representative </td>
   <td style="text-align:right;"> 2.738462 </td>
  </tr>
</tbody>
</table>

```r
ggplot(data=Jobs, aes(x=JobRole, y=Avg, fill=JobRole)) + 
  geom_bar(stat='identity', colour = 'black') + 
  labs(title="Mean Job Satisfaction by Role", x="Job Role", y="Mean Satisfaction") +
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5)) +
  theme(axis.text.x = element_text(angle = 50, hjust = 1))
```

<img src="CaseStudy2_files/figure-html/jobrole_jobsat-1.png" style="display: block; margin: auto;" />

The graph above shows the average job satisfaction for each job position. The lowest is Human Resources at 2.57 and the highest is Research Scientist and Healthcare Representative at 2.80. The next graph shows the distribution of monthly income by job position. Managers and research directors are paid the most. IT seems like the lowest paying jobs, lab technician, sales representative, and research scientist, also have the smallest standard deviations. 


```r
# Distribution of Monthly Income by Job Role
ggplot(dfTrain, aes(x=JobRole, y=MonthlyIncome, group=JobRole)) + 
  ggtitle("Income Distribution by Job Role") + 
  xlab("Job Role") + 
  ylab("Monthly Income") + 
  geom_boxplot() +
  stat_summary(fun.y=mean, geom="point", colour="blue") +
  theme(axis.text.x = element_text(angle = 50, hjust = 1))
```

<img src="CaseStudy2_files/figure-html/income_jobsat-1.png" style="display: block; margin: auto;" />

The graph below shows the distribution of age for each job position. Most of the median ages seem to be close together, however management positions, like manager and research director, have higher median ages, while sales representatives have the lowest.


```r
# Distribution of Age by Job Role
ggplot(dfTrain, aes(x=JobRole, y=Age, group=JobRole)) + 
  ggtitle("Age Distribution by Job Type") + 
  xlab("Job Role") + 
  ylab("Age") + 
  geom_boxplot() +
  stat_summary(fun.y=mean, geom="point", colour="red") +
  theme(axis.text.x = element_text(angle = 50, hjust = 1))
```

<img src="CaseStudy2_files/figure-html/jobrole_age-1.png" style="display: block; margin: auto;" />

The next six graphs will show the attrition rates for each of the levels of the 6 variables that were included in our KNN model. Note how the attrtion rates are very different for each variable. This is what makes them good indicator variables to use for predicting attrition.


```r
# Attrition rate by Job Role
freqtable <- table(dfTrain$JobRole, dfTrain$Attrition)
count <- plyr::count(dfTrain$JobRole)
RelFreq <- freqtable / count$freq
dfRel <- data.frame(RelFreq)
dfRel2 <- dfRel[10:18,]

ggplot(data=dfRel2, aes(x=Var1, y=Freq, fill=Var1)) + 
  geom_bar(stat='identity', colour = 'black') + 
  ggtitle("Attrition Rate per Job Type") + 
  ylab("Attrition Rate") + 
  xlab("Job Type") + 
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5)) +
  theme(axis.text.x = element_text(angle = 50, hjust = 1))
```

<img src="CaseStudy2_files/figure-html/attrition_graphs-1.png" style="display: block; margin: auto;" />

```r
# Attrition rate by job involvement
freqtable <- table(dfTrain$JobInvolvement, dfTrain$Attrition)
count <- plyr::count(dfTrain$JobInvolvement)
RelFreq <- freqtable / count$freq
dfRel <- data.frame(RelFreq)
dfRel2 <- dfRel[5:8,]

ggplot(data=dfRel2, aes(x=Var1, y=Freq, fill=Var1)) + 
  geom_bar(stat='identity', colour = 'black') + 
  ggtitle("Attrition Rate by Job Involvement") + 
  ylab("Attrition Rate") + 
  xlab("Job Involvement") + 
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5))
```

<img src="CaseStudy2_files/figure-html/attrition_graphs-2.png" style="display: block; margin: auto;" />

```r
# Attrition rate by job level
freqtable <- table(dfTrain$JobLevel, dfTrain$Attrition)
count <- plyr::count(dfTrain$JobLevel)
RelFreq <- freqtable / count$freq
dfRel <- data.frame(RelFreq)
dfRel2 <- dfRel[6:10,]

ggplot(data=dfRel2, aes(x=Var1, y=Freq, fill=Var1)) + 
  geom_bar(stat='identity', colour = 'black') + 
  ggtitle("Attrition Rate by Job Level") + 
  ylab("Attrition Rate") + 
  xlab("Job Level") + 
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5))
```

<img src="CaseStudy2_files/figure-html/attrition_graphs-3.png" style="display: block; margin: auto;" />

```r
# Attrition rate by overtime
freqtable <- table(dfTrain$OverTime, dfTrain$Attrition)
count <- plyr::count(dfTrain$OverTime)
RelFreq <- freqtable / count$freq
dfRel <- data.frame(RelFreq)
dfRel2 <- dfRel[3:4,]

ggplot(data=dfRel2, aes(x=Var1, y=Freq, fill=Var1)) + 
  geom_bar(stat='identity', colour = 'black') + 
  ggtitle("Attrition Rate by Overtime") + 
  ylab("Attrition Rate") + 
  xlab("Overtime") + 
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5))
```

<img src="CaseStudy2_files/figure-html/attrition_graphs-4.png" style="display: block; margin: auto;" />

```r
# Attrition rate by marital status
freqtable <- table(dfTrain$MaritalStatus, dfTrain$Attrition)
count <- plyr::count(dfTrain$MaritalStatus)
RelFreq <- freqtable / count$freq
dfRel <- data.frame(RelFreq)
dfRel2 <- dfRel[4:6,]

ggplot(data=dfRel2, aes(x=Var1, y=Freq, fill=Var1)) + 
  geom_bar(stat='identity', colour = 'black') + 
  ggtitle("Attrition Rate by Marital Status") + 
  ylab("Attrition Rate") + 
  xlab("Marital Status") + 
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5))
```

<img src="CaseStudy2_files/figure-html/attrition_graphs-5.png" style="display: block; margin: auto;" />

```r
# Attrition rate by Work-Life Balance
freqtable <- table(dfTrain$WorkLifeBalance, dfTrain$Attrition)
count <- plyr::count(dfTrain$WorkLifeBalance)
RelFreq <- freqtable / count$freq
dfRel <- data.frame(RelFreq)
dfRel2 <- dfRel[5:8,]

ggplot(data=dfRel2, aes(x=Var1, y=Freq, fill=Var1)) + 
  geom_bar(stat='identity', colour = 'black') + 
  ggtitle("Attrition Rate by Work-Life Balance") + 
  ylab("Attrition Rate") + 
  xlab("Work-Life Balance") + 
  theme(legend.position="none") + 
  theme(plot.title = element_text(hjust = 0.5))
```

<img src="CaseStudy2_files/figure-html/attrition_graphs-6.png" style="display: block; margin: auto;" />

# Conclusion

Using the variables Overtime, JobRole, JobLevel, JobInvolvement, MaritalStatus, and WorkLifeBalance, we created a model that was able to predict whether or not an employee will leave with 85.67% accuracy. At first one would think that things like salary amount and jab satisfacation would play a large role in whther or not people stay at a job. Based on the influential variables we found, that qualities of the actual job, like what your role is and how invloved you are, play a bigger role. Having overtime seems to be associated with high attrition rate and single people seem to leave jobs more than married or divorced people. Focusing on the varaible Job Role, we saw that job types with higher pay tend to be held by older people and all of the varaible sin our model had a high varaition in attrition rate among thier levels. It can be concluded that this is what makes them could predictors of attrition.

# Presentation

This write-up is supplemented by video presentations from both Meredith and Kristen. The links are provided below.

Meredith: 

Kristen: 
