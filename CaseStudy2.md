---
title: "Case Study 2: Employee Data Analysis"
author: "Meredith Ludlow & Kristen Rollins"
date: "December 9, 2018"
output:
  html_document:
    keep_md: yes
---



# Summary

TODO 

# Introduction

The purpose of this analysis is to explore what variables are most associated with attrition levels in Fotune 1000 companies. Exploratory analytics will be used to determine these variables as well as other general trends associated with specific jobs. Finally, we will create a model that will predict whether or not an employee will leave the company voluntarily.
TODO (Kristen)

# Analysis

### Exploratory Data Analysis


```r
# Read in training data
dfTrain <- read.csv("CaseStudy2-data.csv")

# TODO maybe make categories for numeric vars like age, income, etc
```

If a variable did not have a significant impact on turnover, we would expect that the attrition percentage within a group is the same as the attrition percentage in the entire dataset. As we see below, in the whole training set 83.9% of employees stayed while 16.1% left. So, as we view the relative percentages for turnover for each categorical variable, those groups with high attrition percentages appear to have a strong impact on turnover. 
Explanation of our varaible selection and which ones we left out.


```r
# Percentage of retained/lost employees
kable(table(dfTrain$Attrition) / nrow(dfTrain))
```



Var1         Freq
-----  ----------
No      0.8393162
Yes     0.1606838

```r
table(dfTrain$Attrition) / nrow(dfTrain)
```

```
## 
##        No       Yes 
## 0.8393162 0.1606838
```

```r
# Define categorical variables
cat_vars <- c("BusinessTravel", "Department", "Education", 
              "EducationField", "EnvironmentSatisfaction",
              "Gender", "JobInvolvement", "JobLevel",
              "JobRole", "JobSatisfaction", "MaritalStatus",
              "OverTime", "PerformanceRating", 
              "RelationshipSatisfaction", "StockOptionLevel", 
              "WorkLifeBalance")

# Define numerical variables
num_vars <- c("Age", "DailyRate", "DistanceFromHome", "HourlyRate", "MonthlyIncome", "MonthlyRate", "NumCompaniesWorked", "PercentSalaryHike", "TotalWorkingYears", "TrainingTimesLastYear", "YearsAtCompany", "YearsInCurrentRole", "YearsSinceLastPromotion", "YearsWithCurrManager")

# Turn numerical values into categorical
dfTrain$PercentSalaryHike <- cut(dfTrain$PercentSalaryHike, breaks=c(-Inf, 13, 17, Inf), labels=c(1, 2, 3))
dfTrain$TotalWorkingYears <- cut(dfTrain$TotalWorkingYears, breaks=c(-Inf, 10, 20, Inf), labels=c(1, 2, 3))
dfTrain$TrainingTimesLastYear <- cut(dfTrain$TrainingTimesLastYear, breaks=c(-Inf, 1, 4, Inf), labels=c(1, 2, 3))
dfTrain$YearsAtCompany <- cut(dfTrain$YearsAtCompany, breaks=c(-Inf, 5, 15, 25, Inf), labels=c(1, 2, 3, 4))
dfTrain$YearsInCurrentRole <- cut(dfTrain$YearsInCurrentRole, breaks=c(-Inf, 5, 10, Inf), labels=c(1, 2, 3))
dfTrain$YearsSinceLastPromotion <- cut(dfTrain$YearsSinceLastPromotion, breaks=c(-Inf, 2, 7, Inf), labels=c(1, 2, 3))
dfTrain$YearsWithCurrManager <- cut(dfTrain$YearsWithCurrManager, breaks=c(-Inf, 5, 10, Inf), labels=c(1, 2, 3))
# TODO make tables look nicer ## I was trying to use kable like above to make these look nice but it wasn't working. When I put it on the table function it errors out when you divide it by cout$freq

# Make relative frequency tables for categorical variables
for (var in cat_vars) {
  print(var)
  freqtable <- table(dfTrain[[var]], dfTrain$Attrition)
  count <- plyr::count(dfTrain[[var]])
  #print(freqtable / count$freq)
  relfreq <- freqtable / count$freq
  print(kable(relfreq))
}
```

```
## [1] "BusinessTravel"
## 
## 
##                             No         Yes
## ------------------  ----------  ----------
## Non-Travel           0.9090909   0.0909091
## Travel_Frequently    0.7511111   0.2488889
## Travel_Rarely        0.8538922   0.1461078
## [1] "Department"
## 
## 
##                                  No         Yes
## -----------------------  ----------  ----------
## Human Resources           0.8695652   0.1304348
## Research & Development    0.8602846   0.1397154
## Sales                     0.7891738   0.2108262
## [1] "Education"
## 
## 
##         No         Yes
## ----------  ----------
##  0.8129496   0.1870504
##  0.8468085   0.1531915
##  0.8337079   0.1662921
##  0.8512658   0.1487342
##  0.8571429   0.1428571
## [1] "EducationField"
## 
## 
##                            No         Yes
## -----------------  ----------  ----------
## Human Resources     0.8750000   0.1250000
## Life Sciences       0.8456914   0.1543086
## Marketing           0.7851240   0.2148760
## Medical             0.8633880   0.1366120
## Other               0.8888889   0.1111111
## Technical Degree    0.7523810   0.2476190
## [1] "EnvironmentSatisfaction"
## 
## 
##         No         Yes
## ----------  ----------
##  0.7389381   0.2610619
##  0.8441558   0.1558442
##  0.8583106   0.1416894
##  0.8815029   0.1184971
## [1] "Gender"
## 
## 
##                  No         Yes
## -------  ----------  ----------
## Female    0.8639309   0.1360691
## Male      0.8231966   0.1768034
## [1] "JobInvolvement"
## 
## 
##         No         Yes
## ----------  ----------
##  0.6307692   0.3692308
##  0.8047945   0.1952055
##  0.8608696   0.1391304
##  0.9105691   0.0894309
## [1] "JobLevel"
## 
## 
##         No         Yes
## ----------  ----------
##  0.7488584   0.2511416
##  0.9030733   0.0969267
##  0.8304094   0.1695906
##  0.9425287   0.0574713
##  0.9411765   0.0588235
## [1] "JobRole"
## 
## 
##                                     No         Yes
## --------------------------  ----------  ----------
## Healthcare Representative    0.9108911   0.0891089
## Human Resources              0.8378378   0.1621622
## Laboratory Technician        0.7558685   0.2441315
## Manager                      0.9487179   0.0512821
## Manufacturing Director       0.9310345   0.0689655
## Research Director            0.9843750   0.0156250
## Research Scientist           0.8529412   0.1470588
## Sales Executive              0.8178295   0.1821705
## Sales Representative         0.6000000   0.4000000
## [1] "JobSatisfaction"
## 
## 
##         No         Yes
## ----------  ----------
##  0.7672414   0.2327586
##  0.8425926   0.1574074
##  0.8510029   0.1489971
##  0.8713137   0.1286863
## [1] "MaritalStatus"
<<<<<<< HEAD
##           
##                    No        Yes
##   Divorced 0.92045455 0.07954545
##   Married  0.86629002 0.13370998
##   Single   0.74400000 0.25600000
=======
## 
## 
##                    No         Yes
## ---------  ----------  ----------
## Divorced    0.9204545   0.0795455
## Married     0.8662900   0.1337100
## Single      0.7440000   0.2560000
## [1] "Over18"
## 
## 
##              No         Yes
## ---  ----------  ----------
## Y     0.8393162   0.1606838
>>>>>>> 54505103367bbfad5e30e758c72fde31e5e28ba3
## [1] "OverTime"
## 
## 
##               No         Yes
## ----  ----------  ----------
## No     0.8946108   0.1053892
## Yes    0.7014925   0.2985075
## [1] "PerformanceRating"
## 
## 
##              No         Yes
## ---  ----------  ----------
## 3     0.8405651   0.1594349
## 4     0.8324022   0.1675978
## [1] "RelationshipSatisfaction"
## 
## 
##         No         Yes
## ----------  ----------
##  0.8090909   0.1909091
##  0.8410042   0.1589958
##  0.8457300   0.1542700
##  0.8505747   0.1494253
## [1] "StockOptionLevel"
## 
## 
##              No         Yes
## ---  ----------  ----------
## 0     0.7524752   0.2475248
## 1     0.9122056   0.0877944
## 2     0.9280000   0.0720000
## 3     0.8219178   0.1780822
## [1] "WorkLifeBalance"
## 
## 
##         No         Yes
## ----------  ----------
##  0.6406250   0.3593750
##  0.8284672   0.1715328
##  0.8628006   0.1371994
##  0.8320000   0.1680000
```


```r
# TODO look at other variables
```

### Employee Trends


```r
# Job satisfaction by group
Jobs <- group_by(dfTrain, JobRole) %>% summarise(Avg=mean(JobSatisfaction, na.rm=TRUE))
print(Jobs)
```

```
## # A tibble: 9 x 2
##   JobRole                     Avg
##   <fct>                     <dbl>
## 1 Healthcare Representative  2.80
## 2 Human Resources            2.57
## 3 Laboratory Technician      2.69
## 4 Manager                    2.69
## 5 Manufacturing Director     2.75
## 6 Research Director          2.62
## 7 Research Scientist         2.80
## 8 Sales Executive            2.76
## 9 Sales Representative       2.74
```

```r
ggplot(data=Jobs, aes(x=JobRole, y=Avg, fill=JobRole)) + geom_bar(stat='identity', colour = 'black') + coord_flip() + theme(legend.position="none") + theme(plot.title = element_text(hjust = 0.5))
```

![](CaseStudy2_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

We wanted to see how various factors would effect job satisfaction. The average job satisfaction for each posistion is fairly close together. The lowest is Human Resources at 2.57 and the highest is Research Scientist and Healthcare Representative at 2.80. 


```r
ggplot(dfTrain, aes(x=JobSatisfaction, y=MonthlyIncome)) + 
  geom_point(pch = 21, size = 2, color="green") +
  theme(plot.title = element_text(hjust = 0.5))
```

![](CaseStudy2_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

Income does not appear to have an effect on job satisfaction, as every level of satisfaction has a similar distribution of income.


### KNN Classification


```r
# Read in validation data
dfVal <- read.csv("CaseStudy2Validation.csv")

# I selected Business Travel, JobInvolvement, JobLevel, JobRole, and MAritalStatus because they looked the most different from the attrition percentages of the group as a whole. We can come up with a better justificcation later?

# Convert wanted factors into integers.
dfTrain$BusinessTravel <- as.integer(dfTrain$BusinessTravel)
dfTrain$JobRole <- as.integer(dfTrain$JobRole)
dfTrain$MaritalStatus <- as.integer(dfTrain$MaritalStatus)
dfVal$MaritalStatus <- as.integer(dfVal$MaritalStatus)
dfVal$JobRole <- as.integer(dfVal$JobRole)
dfVal$BusinessTravel <- as.integer(dfVal$BusinessTravel)

# Generate attrition predictions based on training data
dfPreds <- class::knn(dfTrain[,c(4,15,16,17,21)], dfVal[,c(4,15,16,17,21)], dfTrain$Attrition, k=5)

# Get accuracy of predictions
confusionMatrix(table(dfVal$Attrition, dfPreds))
```

```
## Confusion Matrix and Statistics
## 
##      dfPreds
##        No Yes
##   No  247   4
##   Yes  47   2
##                                           
##                Accuracy : 0.83            
##                  95% CI : (0.7826, 0.8707)
##     No Information Rate : 0.98            
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.0385          
##  Mcnemar's Test P-Value : 4.074e-09       
##                                           
##             Sensitivity : 0.84014         
##             Specificity : 0.33333         
##          Pos Pred Value : 0.98406         
##          Neg Pred Value : 0.04082         
##              Prevalence : 0.98000         
##          Detection Rate : 0.82333         
##    Detection Prevalence : 0.83667         
##       Balanced Accuracy : 0.58673         
##                                           
##        'Positive' Class : No              
## 
```

```r
# Write predictions to csv file
#write.csv(dfPreds, "CaseStudy2Predictions_Ludlow_Rollins.csv")
```

# Conclusion

TODO

# Presentation

This write-up is supplemented by video presentations from both Meredith and Kristen. The links are provided below.

Meredith: 

Kristen: 
