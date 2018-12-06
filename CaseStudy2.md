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

The purpose of this analysis is to explore what variables are most associated with attrition levels in Fortune 1000 companies. Exploratory analytics will be used to determine these variables as well as other general trends associated with specific jobs. Finally, we will create a model that will predict whether or not an employee will leave the company voluntarily.

# Analysis

### Exploratory Data Analysis


```r
# Read in training data
dfTrain <- read.csv("CaseStudy2-data.csv")

# Break numeric vars in categories
```

If a variable did not have a significant impact on turnover, we would expect that the attrition percentage within a group is the same as the attrition percentage in the entire dataset. As we see below, in the whole training set 83.9% of employees stayed while 16.1% left. So, as we view the relative percentages for turnover for each categorical variable, those groups with high attrition percentages appear to have a strong impact on turnover. 

TODO Explanation of our varaible selection and which ones we left out.


```r
# Percentage of retained/lost employees
kable(table(dfTrain$Attrition) / nrow(dfTrain), 
      col.names=c("Attrition", "Percent")) %>% 
      kable_styling(full_width=F)
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
dfTrain$Age <- cut(dfTrain$Age, breaks=c(-Inf,30,40,50,Inf),labels=c(1,2,3,4))
dfTrain$DailyRate <- cut(dfTrain$DailyRate, breaks=c(-Inf,500,1000,Inf),labels=c(1,2,3))
dfTrain$DistanceFromHome <- cut(dfTrain$DistanceFromHome, breaks=c(-Inf,10,Inf),labels=c(1,2))
dfTrain$HourlyRate <- cut(dfTrain$HourlyRate, breaks=c(-Inf,65,Inf),labels=c(1,2))
dfTrain$MonthlyIncome <- cut(dfTrain$MonthlyIncome, breaks=c(-Inf,5000,10000,15000,Inf),labels=c(1,2,3,4))
dfTrain$MonthlyRate <- cut(dfTrain$MonthlyRate, breaks=c(-Inf,5000,10000,15000,20000,Inf),labels=c(1,2,3,4,5))
dfTrain$NumCompaniesWorked <- cut(dfTrain$NumCompaniesWorked, breaks=c(-Inf,4,Inf),labels=c(1,2))
dfTrain$PercentSalaryHike <- cut(dfTrain$PercentSalaryHike, breaks=c(-Inf, 13, 17, Inf), labels=c(1, 2, 3))
dfTrain$TotalWorkingYears <- cut(dfTrain$TotalWorkingYears, breaks=c(-Inf, 10, 20, Inf), labels=c(1, 2, 3))
dfTrain$TrainingTimesLastYear <- cut(dfTrain$TrainingTimesLastYear, breaks=c(-Inf, 1, 4, Inf), labels=c(1, 2, 3))
dfTrain$YearsAtCompany <- cut(dfTrain$YearsAtCompany, breaks=c(-Inf, 5, 15, 25, Inf), labels=c(1, 2, 3, 4))
dfTrain$YearsInCurrentRole <- cut(dfTrain$YearsInCurrentRole, breaks=c(-Inf, 5, 10, Inf), labels=c(1, 2, 3))
dfTrain$YearsSinceLastPromotion <- cut(dfTrain$YearsSinceLastPromotion, breaks=c(-Inf, 2, 7, Inf), labels=c(1, 2, 3))
dfTrain$YearsWithCurrManager <- cut(dfTrain$YearsWithCurrManager, breaks=c(-Inf, 5, 10, Inf), labels=c(1, 2, 3))

# TODO make tables look nicer 

# Make relative frequency tables for categorical variables
AbsDiff <- data.frame(Variable=character(), AvgDistance=numeric())
for (var in variables) {
  print(var)
  freqtable <- table(dfTrain[[var]], dfTrain$Attrition)
  count <- plyr::count(dfTrain[[var]])
  RelFreq <- freqtable / count$freq
  print(RelFreq)
  Sum <- sum(abs(RelFreq[,2]-0.1606838))/nrow(RelFreq)
  print(Sum)
  AbsDiff <- rbind(AbsDiff, data.frame(Variable=var, AverageDistance=Sum))
}
```

```
## [1] "BusinessTravel"
##                    
##                             No        Yes
##   Non-Travel        0.90909091 0.09090909
##   Travel_Frequently 0.75111111 0.24888889
##   Travel_Rarely     0.85389222 0.14610778
## [1] 0.0575186
## [1] "Department"
##                         
##                                 No       Yes
##   Human Resources        0.8695652 0.1304348
##   Research & Development 0.8602846 0.1397154
##   Sales                  0.7891738 0.2108262
## [1] 0.03378661
## [1] "Education"
##    
##            No       Yes
##   1 0.8129496 0.1870504
##   2 0.8468085 0.1531915
##   3 0.8337079 0.1662921
##   4 0.8512658 0.1487342
##   5 0.8571429 0.1428571
## [1] 0.0138487
## [1] "EducationField"
##                   
##                           No       Yes
##   Human Resources  0.8750000 0.1250000
##   Life Sciences    0.8456914 0.1543086
##   Marketing        0.7851240 0.2148760
##   Medical          0.8633880 0.1366120
##   Other            0.8888889 0.1111111
##   Technical Degree 0.7523810 0.2476190
## [1] 0.04280516
## [1] "EnvironmentSatisfaction"
##    
##            No       Yes
##   1 0.7389381 0.2610619
##   2 0.8441558 0.1558442
##   3 0.8583106 0.1416894
##   4 0.8815029 0.1184971
## [1] 0.04159973
## [1] "Gender"
##         
##                 No       Yes
##   Female 0.8639309 0.1360691
##   Male   0.8231966 0.1768034
## [1] 0.02036714
## [1] "JobInvolvement"
##    
##             No        Yes
##   1 0.63076923 0.36923077
##   2 0.80479452 0.19520548
##   3 0.86086957 0.13913043
##   4 0.91056911 0.08943089
## [1] 0.08396873
## [1] "JobLevel"
##    
##             No        Yes
##   1 0.74885845 0.25114155
##   2 0.90307329 0.09692671
##   3 0.83040936 0.16959064
##   4 0.94252874 0.05747126
##   5 0.94117647 0.05882353
## [1] 0.0736389
## [1] "JobRole"
##                            
##                                     No        Yes
##   Healthcare Representative 0.91089109 0.08910891
##   Human Resources           0.83783784 0.16216216
##   Laboratory Technician     0.75586854 0.24413146
##   Manager                   0.94871795 0.05128205
##   Manufacturing Director    0.93103448 0.06896552
##   Research Director         0.98437500 0.01562500
##   Research Scientist        0.85294118 0.14705882
##   Sales Executive           0.81782946 0.18217054
##   Sales Representative      0.60000000 0.40000000
## [1] 0.0863453
## [1] "JobSatisfaction"
##    
##            No       Yes
##   1 0.7672414 0.2327586
##   2 0.8425926 0.1574074
##   3 0.8510029 0.1489971
##   4 0.8713137 0.1286863
## [1] 0.02975884
## [1] "MaritalStatus"
##           
##                    No        Yes
##   Divorced 0.92045455 0.07954545
##   Married  0.86629002 0.13370998
##   Single   0.74400000 0.25600000
## [1] 0.06780945
## [1] "OverTime"
##      
##              No       Yes
##   No  0.8946108 0.1053892
##   Yes 0.7014925 0.2985075
## [1] 0.09655912
## [1] "PerformanceRating"
##    
##            No       Yes
##   3 0.8405651 0.1594349
##   4 0.8324022 0.1675978
## [1] 0.004081426
## [1] "RelationshipSatisfaction"
##    
##            No       Yes
##   1 0.8090909 0.1909091
##   2 0.8410042 0.1589958
##   3 0.8457300 0.1542700
##   4 0.8505747 0.1494253
## [1] 0.0123964
## [1] "StockOptionLevel"
##    
##             No        Yes
##   0 0.75247525 0.24752475
##   1 0.91220557 0.08779443
##   2 0.92800000 0.07200000
##   3 0.82191781 0.17808219
## [1] 0.06645313
## [1] "WorkLifeBalance"
##    
##            No       Yes
##   1 0.6406250 0.3593750
##   2 0.8284672 0.1715328
##   3 0.8628006 0.1371994
##   4 0.8320000 0.1680000
## [1] 0.0600852
## [1] "Age"
##    
##            No       Yes
##   1 0.7631579 0.2368421
##   2 0.8568548 0.1431452
##   3 0.8893281 0.1106719
##   4 0.8547009 0.1452991
## [1] 0.03977337
## [1] "DailyRate"
##    
##            No       Yes
##   1 0.8069620 0.1930380
##   2 0.8360277 0.1639723
##   3 0.8669834 0.1330166
## [1] 0.02110328
## [1] "DistanceFromHome"
##    
##            No       Yes
##   1 0.8566176 0.1433824
##   2 0.7994350 0.2005650
## [1] 0.02859131
## [1] "HourlyRate"
##    
##            No       Yes
##   1 0.8489583 0.1510417
##   2 0.8299663 0.1700337
## [1] 0.009496002
## [1] "MonthlyIncome"
##    
##             No        Yes
##   1 0.79124579 0.20875421
##   2 0.88088643 0.11911357
##   3 0.83783784 0.16216216
##   4 0.97115385 0.02884615
## [1] 0.05573916
## [1] "MonthlyRate"
##    
##            No       Yes
##   1 0.8057554 0.1942446
##   2 0.8638132 0.1361868
##   3 0.8097345 0.1902655
##   4 0.8512397 0.1487603
##   5 0.8464052 0.1535948
## [1] 0.0213304
## [1] "NumCompaniesWorked"
##    
##            No       Yes
##   1 0.8572973 0.1427027
##   2 0.7714286 0.2285714
## [1] 0.04293436
## [1] "PercentSalaryHike"
##    
##            No       Yes
##   1 0.8247012 0.1752988
##   2 0.8491620 0.1508380
##   3 0.8516129 0.1483871
## [1] 0.01225251
## [1] "TotalWorkingYears"
##    
##             No        Yes
##   1 0.80990629 0.19009371
##   2 0.88014981 0.11985019
##   3 0.91025641 0.08974359
## [1] 0.04706124
## [1] "TrainingTimesLastYear"
##    
##            No       Yes
##   1 0.8076923 0.1923077
##   2 0.8322511 0.1677489
##   3 0.9084507 0.0915493
## [1] 0.03594117
## [1] "YearsAtCompany"
##    
##             No        Yes
##   1 0.79769737 0.20230263
##   2 0.88026608 0.11973392
##   3 0.91304348 0.08695652
##   4 0.84210526 0.15789474
## [1] 0.03977126
## [1] "YearsInCurrentRole"
##    
##             No        Yes
##   1 0.81106613 0.18893387
##   2 0.87978142 0.12021858
##   3 0.93650794 0.06349206
## [1] 0.05530234
## [1] "YearsSinceLastPromotion"
##    
##            No       Yes
##   1 0.8369441 0.1630559
##   2 0.8443396 0.1556604
##   3 0.8518519 0.1481481
## [1] 0.006643716
## [1] "YearsWithCurrManager"
##    
##             No        Yes
##   1 0.81600000 0.18400000
##   2 0.86776860 0.13223140
##   3 0.96491228 0.03508772
## [1] 0.05912156
```

```r
print(AbsDiff)
```

```
##                    Variable AverageDistance
## 1            BusinessTravel     0.057518605
## 2                Department     0.033786611
## 3                 Education     0.013848697
## 4            EducationField     0.042805155
## 5   EnvironmentSatisfaction     0.041599727
## 6                    Gender     0.020367140
## 7            JobInvolvement     0.083968730
## 8                  JobLevel     0.073638898
## 9                   JobRole     0.086345295
## 10          JobSatisfaction     0.029758838
## 11            MaritalStatus     0.067809455
## 12                 OverTime     0.096559121
## 13        PerformanceRating     0.004081426
## 14 RelationshipSatisfaction     0.012396404
## 15         StockOptionLevel     0.066453128
## 16          WorkLifeBalance     0.060085203
## 17                      Age     0.039773365
## 18                DailyRate     0.021103278
## 19         DistanceFromHome     0.028591309
## 20               HourlyRate     0.009496002
## 21            MonthlyIncome     0.055739161
## 22              MonthlyRate     0.021330404
## 23       NumCompaniesWorked     0.042934363
## 24        PercentSalaryHike     0.012252506
## 25        TotalWorkingYears     0.047061244
## 26    TrainingTimesLastYear     0.035941171
## 27           YearsAtCompany     0.039771262
## 28       YearsInCurrentRole     0.055302343
## 29  YearsSinceLastPromotion     0.006643716
## 30     YearsWithCurrManager     0.059121559
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

![](CaseStudy2_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

We wanted to see how various factors would effect job satisfaction. The average job satisfaction for each position is fairly close together. The lowest is Human Resources at 2.57 and the highest is Research Scientist and Healthcare Representative at 2.80. 


```r
ggplot(dfTrain, aes(x=JobSatisfaction, y=MonthlyIncome)) + 
  geom_point(pch = 21, size = 2, color="green") +
  theme(plot.title = element_text(hjust = 0.5))
```

![](CaseStudy2_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

Income does not appear to have an effect on job satisfaction, as every level of satisfaction has a similar distribution of income.

### KNN Classification


```r
# Read in validation data
dfVal <- read.csv("CaseStudy2Validation.csv")

# I selected Business Travel, JobInvolvement, JobLevel, JobRole, and MAritalStatus because they looked the most different from the attrition percentages of the group as a whole. We can come up with a better justification later?

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
##   No  251   0
##   Yes  48   1
##                                           
##                Accuracy : 0.84            
##                  95% CI : (0.7935, 0.8796)
##     No Information Rate : 0.9967          
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.0337          
##  Mcnemar's Test P-Value : 1.17e-11        
##                                           
##             Sensitivity : 0.83946         
##             Specificity : 1.00000         
##          Pos Pred Value : 1.00000         
##          Neg Pred Value : 0.02041         
##              Prevalence : 0.99667         
##          Detection Rate : 0.83667         
##    Detection Prevalence : 0.83667         
##       Balanced Accuracy : 0.91973         
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
