---
title: "Case Study 2: Employee Data Analysis"
author: "Meredith Ludlow & Kristen Rollins"
date: "December 9, 2018"
output:
  html_document:
    keep_md: yes
---



# Introduction

The purpose of this analysis is to explore what variables are good predictors for attrition rates in Fortune 1000 companies. Exploratory analytics will be used to determine which variables are the best predictors of attrition, and we will create a model that will predict whether or not an employee will leave their company voluntarily. Finally, we will look at other trends associated with specific jobs and attrition rates.

# Analysis

### Exploratory Data Analysis



If a variable does not have a significant impact on turnover, we would expect that the attrition rate within a group is the same as the attrition rate of the entire dataset. As we see below, in the whole training set 83.9% of employees stayed while 16.1% left. So, as we view the relative rates for turnover for each categorical variable, we would expect variables with high attrition rates to be strong predictors of turnover. 

Note that we excluded a few variables from consideration at the start because they had the same value for every employee, and thus they wouldn't be any use as predictors. Then, we took all of the quantitative values and broke them up into different levels based on ranges of similar sizes.

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

The tables generated below are the attrition rates for each level within every variable.

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

In order to figure out which variables had attrition rates that were the most different from the attrition rate of the data set as a whole, we had to create a metric. The metric that we used was the average absolute difference in attrition rates. We took the attrition rates under a variable and found the average difference between them and the total attrition rate. The variables and their average differences are listed in the table below, with the highest metrics and thus most influential variables appearing first.

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

We used a k-nearest neighbors classification model to predict whether or not employees from a new data set left their company. First, we used the top 5 variables in the above table as our predictors in the model. Adding the 6th variable didn't improve the accuracy, but adding the 7th did. Our model below uses variables 1-5 and 7 as predictors. We ran the model looking at the three closest points and the five closest points to see which gave better results.


```
## Confusion Matrix and Statistics
## 
##      
##        No Yes
##   No  246   5
##   Yes  39  10
##                                           
##                Accuracy : 0.8533          
##                  95% CI : (0.8082, 0.8914)
##     No Information Rate : 0.95            
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.2555          
##  Mcnemar's Test P-Value : 6.527e-07       
##                                           
##             Sensitivity : 0.8632          
##             Specificity : 0.6667          
##          Pos Pred Value : 0.9801          
##          Neg Pred Value : 0.2041          
##              Prevalence : 0.9500          
##          Detection Rate : 0.8200          
##    Detection Prevalence : 0.8367          
##       Balanced Accuracy : 0.7649          
##                                           
##        'Positive' Class : No              
## 
```

```
## Confusion Matrix and Statistics
## 
##      
##        No Yes
##   No  247   4
##   Yes  43   6
##                                           
##                Accuracy : 0.8433          
##                  95% CI : (0.7972, 0.8826)
##     No Information Rate : 0.9667          
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.1567          
##  Mcnemar's Test P-Value : 2.976e-08       
##                                           
##             Sensitivity : 0.8517          
##             Specificity : 0.6000          
##          Pos Pred Value : 0.9841          
##          Neg Pred Value : 0.1224          
##              Prevalence : 0.9667          
##          Detection Rate : 0.8233          
##    Detection Prevalence : 0.8367          
##       Balanced Accuracy : 0.7259          
##                                           
##        'Positive' Class : No              
## 
```

The KNN model that looks at the three closest data points to the test data point has a higher accuracy than the model that looks at 5. We found that there is slight variation in accuracy between runs of the model, because ties are broken at random. So, the accuracy of the k=3 model ranges between 85% and 87%. In other words, our model is able to predict whether or not an employee will leave 85-87% of the time. The sensitivity of the model is around 86%, meaning that when an employee did not leave the company, the model correctly predicted that they stayed about 86% on the time. The specificity of the model was around 71%, meaning that when someone did leave, the model was able to predict it about 71% of the time.

### Trends by Job Role

Next, we will look at the variable Job Role and see what trends are present. 

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> JobRole </th>
   <th style="text-align:right;"> AverageSatisfaction </th>
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

<img src="CaseStudy2_files/figure-html/jobrole_jobsat-1.png" style="display: block; margin: auto;" />

The graph above shows the average job satisfaction for each job position. The lowest is Human Resources at 2.57 and the highest are Research Scientist and Healthcare Representative at 2.80. The next graph shows the distribution of monthly income by job position. Managers and research directors are paid the most. It appears that the lowest paying jobs, i.e lab technician, sales representative, and research scientist, also have the smallest standard deviations. 

<img src="CaseStudy2_files/figure-html/income_jobsat-1.png" style="display: block; margin: auto;" />

The graph below shows the distribution of age for each job position. Most of the median ages seem to be close together. However, management positions, like manager and research director, have higher median ages, while sales representatives have the lowest.

<img src="CaseStudy2_files/figure-html/jobrole_age-1.png" style="display: block; margin: auto;" />

The next six graphs will show the attrition rates for each of the levels of the 6 variables that were included in our KNN model. Note how the attrtion rates are very different for each variable. This is what makes them good indicator variables to use for predicting attrition.

<img src="CaseStudy2_files/figure-html/attrition_graphs-1.png" style="display: block; margin: auto;" /><img src="CaseStudy2_files/figure-html/attrition_graphs-2.png" style="display: block; margin: auto;" /><img src="CaseStudy2_files/figure-html/attrition_graphs-3.png" style="display: block; margin: auto;" /><img src="CaseStudy2_files/figure-html/attrition_graphs-4.png" style="display: block; margin: auto;" /><img src="CaseStudy2_files/figure-html/attrition_graphs-5.png" style="display: block; margin: auto;" /><img src="CaseStudy2_files/figure-html/attrition_graphs-6.png" style="display: block; margin: auto;" />

# Conclusion

Using the variables Overtime, JobRole, JobLevel, JobInvolvement, MaritalStatus, and WorkLifeBalance, we created a model that was able to predict whether or not an employee will leave their company with about 86% accuracy. At first, one would think that things like salary amount and job satisfacation would play a large role in whether or not people stay at a job. Based on the influential variables we found, qualities of the actual job, like what your role is and how involved you are, play a bigger role. Having overtime seems to be associated with high attrition rate and single people seem to leave jobs more than married or divorced people. Focusing on the variable Job Role, we saw that job types with higher pay tend to be held by older people and all of the variables in our model had a high variation in attrition rate among their levels. It can be concluded that this is what makes them good predictors of attrition.

# Presentation

This write-up is supplemented by video presentations from both Meredith and Kristen. The links are provided below.

Meredith: https://youtu.be/8XBRCE_n3Dw

Kristen: https://youtu.be/9L9gQXNmSG0
