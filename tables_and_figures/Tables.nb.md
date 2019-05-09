Code

-   [Show All Code](#)
-   [Hide All Code](#)
-   -   [Download Rmd](#)

Table1 {.title .toc-ignore}
======

#### *Miguel Angel Armengol de la Hoz* {.author}

Load libs
=========

``` {.r}
library(tableone)
library(magrittr)
library(officer)
```

    Attaching package: ‘officer’

    The following object is masked from ‘package:readxl’:

        read_xlsx

``` {.r}
library(xlsx)
library(dplyr)
```

Including fluid data only for table1 (not for modeling)

``` {.r}
FinalHemodynamicsDataset_for_table1<-FinalHemodynamicsDataset
FinalHemodynamicsDataset_for_table1<-left_join(FinalHemodynamicsDataset_for_table1,IO_data_Fluids_summarized)
```

    Joining, by = "Case_Name"

Table 1
=======

``` {.r}
listVars
```

     [1] "Gender"                         "Age"                            "Category"                      
     [4] "Diabetes"                       "RFDyslipidemia"                 "Hypertension_final"            
     [7] "Smoking"                        "Congestive_Heart_Failure"       "Previous_myocardial_infarction"
    [10] "ChronicLungDisease"             "Dialysis"                       "PreOpMedBetaBlockers"          
    [13] "PreOpMedACEI_or_ARBs"           "PreOpMedInotropes"              "PreOpMedSteroids"              
    [16] "PreOpMedAspirin"                "PreOpMedLipidLowering"         

``` {.r}
listVarstab1<-c(listVars,"STS_RiskAlgorithm_median_imp","delta_HCT_pct_median_imp","mean_ef_median_imp","Colloids_ml","Crystalloids_ml","Cross_Clamp_Time","duration_CPB_min","AUT65MAP_CPB_min")


catVars<-c("Gender",
  "Category",
  #Background diseases
  "Diabetes",
  "RFDyslipidemia",
  "Hypertension_final",
  "Smoking",
  "Congestive_Heart_Failure",
  "Previous_myocardial_infarction",
  "ChronicLungDisease",
  "Dialysis",
  #Preoperative medications
  "PreOpMedBetaBlockers",
  "PreOpMedACEI_or_ARBs",
  "PreOpMedInotropes",
  "PreOpMedSteroids",
  "PreOpMedAspirin",
  "PreOpMedLipidLowering") #AKA statins

table1 <- CreateTableOne(vars = listVarstab1, data = FinalHemodynamicsDataset_for_table1, factorVars = catVars,testNormal=oneway.test 
                        #strata is usefull in case we want several columns to stratify by
                         )

table1strat <- CreateTableOne(vars = listVarstab1, data = FinalHemodynamicsDataset_for_table1, factorVars = catVars,testNormal=oneway.test 
                        ,strata = c("Any_MAE")
                        #strata is usefull in case we want several columns to stratify by
                         )
## nonnormal specifies variables to be shown as median [IQR]
# 
# testApprox A function used to perform the large sample approximation based tests. The
# default is chisq.test. This is not recommended when some of the cell have
# small counts like fewer than 5.

# As an aside, the following code may help for your projects, as it improves the presentation of the tables above.  You will still need to update the column and row names manually, but this should paste nicely into Word or LateX!

write.xlsx(as.data.frame(print(table1)), "/Users/marmengol/MEGA/Boston/BIDMC-Harvard/Phenotyping/Tables and Figures/table1.xlsx")
```

                                              
                                               Overall          
      n                                           5158          
      Gender = Male (%)                           3594 (69.7)   
      Age (mean (sd))                            66.87 (11.80)  
      Category (%)                                              
         Aortic Surgery                             17 ( 0.3)   
         CABG                                     2550 (49.4)   
         CABG + Valve                              946 (18.3)   
         Other                                     157 ( 3.0)   
         Valve                                    1488 (28.8)   
      Diabetes = 1 (%)                            1183 (22.9)   
      RFDyslipidemia = 1 (%)                      3880 (75.2)   
      Hypertension_final = 1 (%)                  4026 (78.1)   
      Smoking = 1 (%)                             1489 (28.9)   
      Congestive_Heart_Failure = 1 (%)            1766 (34.2)   
      Previous_myocardial_infarction = 1 (%)      1578 (30.6)   
      ChronicLungDisease = 1 (%)                   623 (12.1)   
      Dialysis = 1 (%)                              85 ( 1.6)   
      PreOpMedBetaBlockers = 1 (%)                3840 (74.4)   
      PreOpMedACEI_or_ARBs = 1 (%)                2249 (43.6)   
      PreOpMedInotropes = 1 (%)                     47 ( 0.9)   
      PreOpMedSteroids = 1 (%)                     175 ( 3.4)   
      PreOpMedAspirin = 1 (%)                     4237 (82.1)   
      PreOpMedLipidLowering = 1 (%)               3959 (76.8)   
      STS_RiskAlgorithm_median_imp (mean (sd))    0.02 (0.03)   
      delta_HCT_pct_median_imp (mean (sd))        2.81 (8.25)   
      mean_ef_median_imp (mean (sd))             54.15 (13.64)  
      Colloids_ml (mean (sd))                   475.81 (175.06) 
      Crystalloids_ml (mean (sd))              2967.08 (3270.12)
      Cross_Clamp_Time (mean (sd))               73.16 (29.99)  
      duration_CPB_min (mean (sd))               94.63 (38.70)  
      AUT65MAP_CPB_min (mean (sd))               66.98 (32.93)  

``` {.r}
write.xlsx(as.data.frame(print(table1strat)), "/Users/marmengol/MEGA/Boston/BIDMC-Harvard/Phenotyping/Tables and Figures/table1strat.xlsx")
```

                                              Stratified by Any_MAE
                                               0                 1                 p      test
      n                                           4640               518                      
      Gender = Male (%)                           3255 (70.2)        339 (65.4)     0.031     
      Age (mean (sd))                            66.65 (11.76)     68.85 (11.99)   <0.001     
      Category (%)                                                                 <0.001     
         Aortic Surgery                             12 ( 0.3)          5 ( 1.0)               
         CABG                                     2355 (50.8)        195 (37.6)               
         CABG + Valve                              792 (17.1)        154 (29.7)               
         Other                                     130 ( 2.8)         27 ( 5.2)               
         Valve                                    1351 (29.1)        137 (26.4)               
      Diabetes = 1 (%)                            1044 (22.5)        139 (26.8)     0.030     
      RFDyslipidemia = 1 (%)                      3491 (75.2)        389 (75.1)     0.987     
      Hypertension_final = 1 (%)                  3607 (77.7)        419 (80.9)     0.112     
      Smoking = 1 (%)                             1311 (28.3)        178 (34.4)     0.004     
      Congestive_Heart_Failure = 1 (%)            1526 (32.9)        240 (46.3)    <0.001     
      Previous_myocardial_infarction = 1 (%)      1382 (29.8)        196 (37.8)    <0.001     
      ChronicLungDisease = 1 (%)                   531 (11.4)         92 (17.8)    <0.001     
      Dialysis = 1 (%)                              68 ( 1.5)         17 ( 3.3)     0.004     
      PreOpMedBetaBlockers = 1 (%)                3464 (74.7)        376 (72.6)     0.332     
      PreOpMedACEI_or_ARBs = 1 (%)                2032 (43.8)        217 (41.9)     0.435     
      PreOpMedInotropes = 1 (%)                     30 ( 0.6)         17 ( 3.3)    <0.001     
      PreOpMedSteroids = 1 (%)                     148 ( 3.2)         27 ( 5.2)     0.022     
      PreOpMedAspirin = 1 (%)                     3824 (82.4)        413 (79.7)     0.146     
      PreOpMedLipidLowering = 1 (%)               3567 (76.9)        392 (75.7)     0.577     
      STS_RiskAlgorithm_median_imp (mean (sd))    0.02 (0.03)       0.04 (0.06)    <0.001     
      delta_HCT_pct_median_imp (mean (sd))        2.89 (8.25)       2.02 (8.17)     0.022     
      mean_ef_median_imp (mean (sd))             54.34 (13.58)     52.45 (14.07)    0.003     
      Colloids_ml (mean (sd))                   474.14 (181.08)   500.00 (0.00)     0.844     
      Crystalloids_ml (mean (sd))              2946.21 (3420.78) 3155.82 (1242.28)  0.170     
      Cross_Clamp_Time (mean (sd))               71.78 (28.59)     85.81 (38.48)   <0.001     
      duration_CPB_min (mean (sd))               92.24 (35.64)    116.06 (55.06)   <0.001     
      AUT65MAP_CPB_min (mean (sd))               65.47 (30.62)     80.62 (46.95)   <0.001     

``` {.r}
#  if(!("dplyr" %in% installed.packages()[,1])) {
#  install.packages("dplyr")
#  }
# library(dplyr)
# test<-table1 %>% print(
#   printToggle      = FALSE,
#   showAllLevels    = TRUE,
#   cramVars         = "kon"
# ) %>% 
# {data.frame(
#   variable_name             = gsub(" ", "&nbsp;", rownames(.), fixed = TRUE), ., 
#   row.names        = NULL, 
#   check.names      = FALSE, 
#   stringsAsFactors = FALSE)} %>% 
# knitr::kable()
```

Table 2
=======

``` {.r}

library(tableone)
library(magrittr)

listExposures<-c('AOT30DBP_postCPB_min','AOT30DBP_preCPB_min','AOT30DBP_total_min','AOT65MAP_CPB_min','AOT65MAP_postCPB_min','AOT65MAP_preCPB_min','AOT65MAP_total_min','AOT95SBP_postCPB_min','AOT95SBP_preCPB_min','AOT95SBP_total_min','AUT30DBP_postCPB_min','AUT30DBP_preCPB_min','AUT30DBP_total_min','AUT50MAP_CPB_min','AUT50MAP_postCPB_min','AUT50MAP_preCPB_min','AUT50MAP_total_min','AUT65MAP_CPB_min','AUT65MAP_postCPB_min','AUT65MAP_preCPB_min','AUT65MAP_total_min','AUT95SBP_postCPB_min','AUT95SBP_preCPB_min','AUT95SBP_total_min','AWI51_60MAP_CPB_min','AWI51_60MAP_postCPB_min','AWI51_60MAP_preCPB_min','AWI51_60MAP_total_min','AWI61_65MAP_CPB_min','AWI61_65MAP_postCPB_min','AWI61_65MAP_preCPB_min','AWI61_65MAP_total_min','duration_CPB_min','duration_postCPB_min','duration_preCPB_min','duration_Surgery_min')

table2all <- CreateTableOne(vars = listExposures, data = FinalHemodynamicsDataset_for_table1, testNormal=oneway.test)
```

    The data frame does not have: AOT95SBP_postCPB_min AOT95SBP_preCPB_min AOT95SBP_total_min AUT95SBP_postCPB_min AUT95SBP_preCPB_min AUT95SBP_total_min  Dropped

``` {.r}
write.xlsx(as.data.frame(print(table2all)), "table2all.xlsx")
```

                                         
                                          Overall       
      n                                     5158        
      AOT30DBP_postCPB_min (mean (sd))     72.41 (23.04)
      AOT30DBP_preCPB_min (mean (sd))     119.97 (33.31)
      AOT30DBP_total_min (mean (sd))      206.22 (51.52)
      AOT65MAP_CPB_min (mean (sd))         25.92 (24.04)
      AOT65MAP_postCPB_min (mean (sd))     53.07 (22.90)
      AOT65MAP_preCPB_min (mean (sd))     103.85 (33.71)
      AOT65MAP_total_min (mean (sd))      182.21 (55.78)
      AUT30DBP_postCPB_min (mean (sd))      2.31 (6.17) 
      AUT30DBP_preCPB_min (mean (sd))       2.46 (10.16)
      AUT30DBP_total_min (mean (sd))        1.30 (7.16) 
      AUT50MAP_CPB_min (mean (sd))         20.99 (19.37)
      AUT50MAP_postCPB_min (mean (sd))      3.48 (6.42) 
      AUT50MAP_preCPB_min (mean (sd))       2.30 (4.56) 
      AUT50MAP_total_min (mean (sd))       25.17 (22.12)
      AUT65MAP_CPB_min (mean (sd))         66.98 (32.93)
      AUT65MAP_postCPB_min (mean (sd))     20.97 (18.76)
      AUT65MAP_preCPB_min (mean (sd))      18.41 (18.00)
      AUT65MAP_total_min (mean (sd))      105.73 (50.69)
      AWI51_60MAP_CPB_min (mean (sd))      34.71 (19.60)
      AWI51_60MAP_postCPB_min (mean (sd))  10.47 (10.95)
      AWI51_60MAP_preCPB_min (mean (sd))    8.97 (10.46)
      AWI51_60MAP_total_min (mean (sd))    53.10 (28.04)
      AWI61_65MAP_CPB_min (mean (sd))      11.38 (8.18) 
      AWI61_65MAP_postCPB_min (mean (sd))   8.45 (6.69) 
      AWI61_65MAP_preCPB_min (mean (sd))    8.06 (7.29) 
      AWI61_65MAP_total_min (mean (sd))    27.46 (13.96)
      duration_CPB_min (mean (sd))         94.63 (38.70)
      duration_postCPB_min (mean (sd))    114.51 (37.81)
      duration_preCPB_min (mean (sd))      85.77 (45.29)
      duration_Surgery_min (mean (sd))    294.91 (68.61)

``` {.r}
table2strat <- CreateTableOne(vars = listExposures, data = FinalHemodynamicsDataset_for_table1, factorVars = listOutstable2,testNormal=oneway.test 
                        ,strata = c("Any_MAE") )
```

    The data frame does not have: AOT95SBP_postCPB_min AOT95SBP_preCPB_min AOT95SBP_total_min AUT95SBP_postCPB_min AUT95SBP_preCPB_min AUT95SBP_total_min  Dropped

``` {.r}
                        
write.xlsx(as.data.frame(print(table2strat)), "table2strat.xlsx")
```

                                         Stratified by Any_MAE
                                          0              1               p      test
      n                                     4640            518                     
      AOT30DBP_postCPB_min (mean (sd))     71.43 (20.96)  81.26 (35.60)  <0.001     
      AOT30DBP_preCPB_min (mean (sd))     119.09 (31.97) 127.91 (42.74)  <0.001     
      AOT30DBP_total_min (mean (sd))      203.42 (45.36) 231.30 (85.52)  <0.001     
      AOT65MAP_CPB_min (mean (sd))         25.16 (22.57)  32.82 (33.86)  <0.001     
      AOT65MAP_postCPB_min (mean (sd))     52.84 (21.32)  55.20 (33.89)   0.026     
      AOT65MAP_preCPB_min (mean (sd))     103.35 (32.40) 108.32 (43.53)   0.001     
      AOT65MAP_total_min (mean (sd))      180.75 (51.47) 195.29 (84.09)  <0.001     
      AUT30DBP_postCPB_min (mean (sd))      1.92 (5.00)    4.73 (10.57)  <0.001     
      AUT30DBP_preCPB_min (mean (sd))       2.13 (9.79)    4.60 (12.14)   0.004     
      AUT30DBP_total_min (mean (sd))        1.08 (6.43)    3.21 (11.72)  <0.001     
      AUT50MAP_CPB_min (mean (sd))         20.43 (17.82)  25.96 (29.47)  <0.001     
      AUT50MAP_postCPB_min (mean (sd))      3.21 (5.85)    5.73 (9.71)   <0.001     
      AUT50MAP_preCPB_min (mean (sd))       2.17 (4.17)    3.37 (6.95)   <0.001     
      AUT50MAP_total_min (mean (sd))       24.32 (20.28)  32.80 (33.51)  <0.001     
      AUT65MAP_CPB_min (mean (sd))         65.47 (30.62)  80.62 (46.95)  <0.001     
      AUT65MAP_postCPB_min (mean (sd))     20.05 (17.53)  29.13 (26.00)  <0.001     
      AUT65MAP_preCPB_min (mean (sd))      17.88 (17.09)  23.15 (24.16)  <0.001     
      AUT65MAP_total_min (mean (sd))      102.81 (47.00) 131.92 (71.05)  <0.001     
      AWI51_60MAP_CPB_min (mean (sd))      33.95 (18.62)  41.63 (25.87)  <0.001     
      AWI51_60MAP_postCPB_min (mean (sd))   9.99 (10.20)  14.69 (15.54)  <0.001     
      AWI51_60MAP_preCPB_min (mean (sd))    8.69 (9.80)   11.44 (14.84)  <0.001     
      AWI51_60MAP_total_min (mean (sd))    51.60 (26.13)  66.52 (38.96)  <0.001     
      AWI61_65MAP_CPB_min (mean (sd))      11.17 (7.83)   13.23 (10.63)  <0.001     
      AWI61_65MAP_postCPB_min (mean (sd))   8.22 (6.49)   10.56 (7.94)   <0.001     
      AWI61_65MAP_preCPB_min (mean (sd))    7.92 (7.09)    9.26 (8.78)   <0.001     
      AWI61_65MAP_total_min (mean (sd))    26.89 (13.41)  32.61 (17.31)  <0.001     
      duration_CPB_min (mean (sd))         92.24 (35.64) 116.06 (55.06)  <0.001     
      duration_postCPB_min (mean (sd))    113.55 (36.27) 123.14 (48.78)  <0.001     
      duration_preCPB_min (mean (sd))      84.61 (43.72)  96.10 (56.48)  <0.001     
      duration_Surgery_min (mean (sd))    290.40 (62.11) 335.30 (102.60) <0.001     

``` {.r}
                        
#strata is usefull in case we want several columns to stratify by
# nonnormal specifies variables to be shown as median [IQR]
# testApprox A function used to perform the large sample approximation based tests. The
# default is chisq.test. This is not recommended when some of the cell have
# small counts like fewer than 5.
```

Table 3
=======

``` {.r}

library(tableone)
library(magrittr)

listOutstable2<-c('Any_MAE',listOuts)

table2all <- CreateTableOne(vars = listOutstable2, data = FinalHemodynamicsDataset, factorVars = listOutstable2,testNormal=oneway.test 
                         )

table2strat <- CreateTableOne(vars = listOutstable2, data = FinalHemodynamicsDataset, factorVars = listOutstable2,testNormal=oneway.test 
                        ,strata = c("Any_MAE"))
                        
write.xlsx(as.data.frame(print(table2strat)), "table2strat.xlsx")
```

                            Stratified by Any_MAE
                             0           1            p      test
      n                      4640        518                     
      Any_MAE = 1 (%)           0 (0.0)  518 (100.0)  <0.001     
      Reop_Bleeding = 1 (%)     0 (0.0)  113 ( 21.8)  <0.001     
      Infection = 1 (%)         0 (0.0)   88 ( 17.0)  <0.001     
      Stroke = 1 (%)            0 (0.0)   69 ( 13.3)  <0.001     
      Pulm_Pneumonia = 1 (%)    0 (0.0)  144 ( 27.8)  <0.001     
      Renal_Failure = 1 (%)     0 (0.0)  134 ( 25.9)  <0.001     
      Oth_Tamponade = 1 (%)     0 (0.0)    9 (  1.7)  <0.001     
      Death = 1 (%)             0 (0.0)  117 ( 22.6)  <0.001     

``` {.r}
                        
                        #strata is usefull in case we want several columns to stratify by
## nonnormal specifies variables to be shown as median [IQR]
# 
# testApprox A function used to perform the large sample approximation based tests. The
# default is chisq.test. This is not recommended when some of the cell have
# small counts like fewer than 5.

# table2<-print(table2,estApprox=listVars, exact = "stage", quote = TRUE,showAllLevels  = TRUE)
# 
# docx( ) %>%
#     addFlexTable(table2 %>%
#     FlexTable(header.cell.props = cellProperties( background.color = "#003366"),
#               header.text.props = textBold( color = "white" ),
#               add.rownames = TRUE ) %>%
#               setZebraStyle( odd = "#DDDDDD", even = "#FFFFFF" ) ) %>%
#     writeDoc(file = "table2.docx")
# 
# browseURL("table2.docx") #wordfile
```

LS0tCnRpdGxlOiAiVGFibGUxIgphdXRob3I6ICJNaWd1ZWwgQW5nZWwgQXJtZW5nb2wgZGUgbGEgSG96IgpvdXRwdXQ6IGh0bWxfbm90ZWJvb2sKLS0tCgoKYGBge3Igc2V0dXAsIGluY2x1ZGU9RkFMU0V9CiNsb2FkIHRoZSBkYXRhCiNsb2FkKCJDOi9CSUcgRklMRVMvUGhlbm90eXBpbmcvSGVtb2R5bmFtaWNQcm9qZWN0c0RhdGFzZXRGZWIxOC5SRGF0YSIpCmBgYAoKIyBMb2FkIGxpYnMKYGBge3J9CmxpYnJhcnkodGFibGVvbmUpCmxpYnJhcnkobWFncml0dHIpCmxpYnJhcnkob2ZmaWNlcikKbGlicmFyeSh4bHN4KQpsaWJyYXJ5KGRwbHlyKQpgYGAKCkluY2x1ZGluZyBmbHVpZCBkYXRhIG9ubHkgZm9yIHRhYmxlMSAobm90IGZvciBtb2RlbGluZykKCmBgYHtyfQpGaW5hbEhlbW9keW5hbWljc0RhdGFzZXRfZm9yX3RhYmxlMTwtRmluYWxIZW1vZHluYW1pY3NEYXRhc2V0CkZpbmFsSGVtb2R5bmFtaWNzRGF0YXNldF9mb3JfdGFibGUxPC1sZWZ0X2pvaW4oRmluYWxIZW1vZHluYW1pY3NEYXRhc2V0X2Zvcl90YWJsZTEsSU9fZGF0YV9GbHVpZHNfc3VtbWFyaXplZCkKYGBgCgoKIyBUYWJsZSAxCgpgYGB7cn0KbGlzdFZhcnMKbGlzdFZhcnN0YWIxPC1jKGxpc3RWYXJzLCJTVFNfUmlza0FsZ29yaXRobV9tZWRpYW5faW1wIiwiZGVsdGFfSENUX3BjdF9tZWRpYW5faW1wIiwibWVhbl9lZl9tZWRpYW5faW1wIiwiQ29sbG9pZHNfbWwiLCJDcnlzdGFsbG9pZHNfbWwiLCJDcm9zc19DbGFtcF9UaW1lIiwiZHVyYXRpb25fQ1BCX21pbiIsIkFVVDY1TUFQX0NQQl9taW4iKQoKCmNhdFZhcnM8LWMoIkdlbmRlciIsCiAgIkNhdGVnb3J5IiwKICAjQmFja2dyb3VuZCBkaXNlYXNlcwogICJEaWFiZXRlcyIsCiAgIlJGRHlzbGlwaWRlbWlhIiwKICAiSHlwZXJ0ZW5zaW9uX2ZpbmFsIiwKICAiU21va2luZyIsCiAgIkNvbmdlc3RpdmVfSGVhcnRfRmFpbHVyZSIsCiAgIlByZXZpb3VzX215b2NhcmRpYWxfaW5mYXJjdGlvbiIsCiAgIkNocm9uaWNMdW5nRGlzZWFzZSIsCiAgIkRpYWx5c2lzIiwKICAjUHJlb3BlcmF0aXZlIG1lZGljYXRpb25zCiAgIlByZU9wTWVkQmV0YUJsb2NrZXJzIiwKICAiUHJlT3BNZWRBQ0VJX29yX0FSQnMiLAogICJQcmVPcE1lZElub3Ryb3BlcyIsCiAgIlByZU9wTWVkU3Rlcm9pZHMiLAogICJQcmVPcE1lZEFzcGlyaW4iLAogICJQcmVPcE1lZExpcGlkTG93ZXJpbmciKSAjQUtBIHN0YXRpbnMKCnRhYmxlMSA8LSBDcmVhdGVUYWJsZU9uZSh2YXJzID0gbGlzdFZhcnN0YWIxLCBkYXRhID0gRmluYWxIZW1vZHluYW1pY3NEYXRhc2V0X2Zvcl90YWJsZTEsIGZhY3RvclZhcnMgPSBjYXRWYXJzLHRlc3ROb3JtYWw9b25ld2F5LnRlc3QgCiAgICAgICAgICAgICAgICAgICAgICAgICNzdHJhdGEgaXMgdXNlZnVsbCBpbiBjYXNlIHdlIHdhbnQgc2V2ZXJhbCBjb2x1bW5zIHRvIHN0cmF0aWZ5IGJ5CiAgICAgICAgICAgICAgICAgICAgICAgICApCgp0YWJsZTFzdHJhdCA8LSBDcmVhdGVUYWJsZU9uZSh2YXJzID0gbGlzdFZhcnN0YWIxLCBkYXRhID0gRmluYWxIZW1vZHluYW1pY3NEYXRhc2V0X2Zvcl90YWJsZTEsIGZhY3RvclZhcnMgPSBjYXRWYXJzLHRlc3ROb3JtYWw9b25ld2F5LnRlc3QgCiAgICAgICAgICAgICAgICAgICAgICAgICxzdHJhdGEgPSBjKCJBbnlfTUFFIikKICAgICAgICAgICAgICAgICAgICAgICAgI3N0cmF0YSBpcyB1c2VmdWxsIGluIGNhc2Ugd2Ugd2FudCBzZXZlcmFsIGNvbHVtbnMgdG8gc3RyYXRpZnkgYnkKICAgICAgICAgICAgICAgICAgICAgICAgICkKIyMgbm9ubm9ybWFsIHNwZWNpZmllcyB2YXJpYWJsZXMgdG8gYmUgc2hvd24gYXMgbWVkaWFuIFtJUVJdCiMgCiMgdGVzdEFwcHJveCBBIGZ1bmN0aW9uIHVzZWQgdG8gcGVyZm9ybSB0aGUgbGFyZ2Ugc2FtcGxlIGFwcHJveGltYXRpb24gYmFzZWQgdGVzdHMuIFRoZQojIGRlZmF1bHQgaXMgY2hpc3EudGVzdC4gVGhpcyBpcyBub3QgcmVjb21tZW5kZWQgd2hlbiBzb21lIG9mIHRoZSBjZWxsIGhhdmUKIyBzbWFsbCBjb3VudHMgbGlrZSBmZXdlciB0aGFuIDUuCgojIEFzIGFuIGFzaWRlLCB0aGUgZm9sbG93aW5nIGNvZGUgbWF5IGhlbHAgZm9yIHlvdXIgcHJvamVjdHMsIGFzIGl0IGltcHJvdmVzIHRoZSBwcmVzZW50YXRpb24gb2YgdGhlIHRhYmxlcyBhYm92ZS4gIFlvdSB3aWxsIHN0aWxsIG5lZWQgdG8gdXBkYXRlIHRoZSBjb2x1bW4gYW5kIHJvdyBuYW1lcyBtYW51YWxseSwgYnV0IHRoaXMgc2hvdWxkIHBhc3RlIG5pY2VseSBpbnRvIFdvcmQgb3IgTGF0ZVghCgp3cml0ZS54bHN4KGFzLmRhdGEuZnJhbWUocHJpbnQodGFibGUxKSksICIvVXNlcnMvbWFybWVuZ29sL01FR0EvQm9zdG9uL0JJRE1DLUhhcnZhcmQvUGhlbm90eXBpbmcvVGFibGVzIGFuZCBGaWd1cmVzL3RhYmxlMS54bHN4IikKd3JpdGUueGxzeChhcy5kYXRhLmZyYW1lKHByaW50KHRhYmxlMXN0cmF0KSksICIvVXNlcnMvbWFybWVuZ29sL01FR0EvQm9zdG9uL0JJRE1DLUhhcnZhcmQvUGhlbm90eXBpbmcvVGFibGVzIGFuZCBGaWd1cmVzL3RhYmxlMXN0cmF0Lnhsc3giKQoKCiMgIGlmKCEoImRwbHlyIiAlaW4lIGluc3RhbGxlZC5wYWNrYWdlcygpWywxXSkpIHsKIyAgaW5zdGFsbC5wYWNrYWdlcygiZHBseXIiKQojICB9CiMgbGlicmFyeShkcGx5cikKIyB0ZXN0PC10YWJsZTEgJT4lIHByaW50KAojICAgcHJpbnRUb2dnbGUgICAgICA9IEZBTFNFLAojICAgc2hvd0FsbExldmVscyAgICA9IFRSVUUsCiMgICBjcmFtVmFycyAgICAgICAgID0gImtvbiIKIyApICU+JSAKIyB7ZGF0YS5mcmFtZSgKIyAgIHZhcmlhYmxlX25hbWUgICAgICAgICAgICAgPSBnc3ViKCIgIiwgIiZuYnNwOyIsIHJvd25hbWVzKC4pLCBmaXhlZCA9IFRSVUUpLCAuLCAKIyAgIHJvdy5uYW1lcyAgICAgICAgPSBOVUxMLCAKIyAgIGNoZWNrLm5hbWVzICAgICAgPSBGQUxTRSwgCiMgICBzdHJpbmdzQXNGYWN0b3JzID0gRkFMU0UpfSAlPiUgCiMga25pdHI6OmthYmxlKCkKCmBgYAoKIyBUYWJsZSAyCgpgYGB7cn0KCgpsaWJyYXJ5KHRhYmxlb25lKQpsaWJyYXJ5KG1hZ3JpdHRyKQoKbGlzdEV4cG9zdXJlczwtYygnQU9UMzBEQlBfcG9zdENQQl9taW4nLCdBT1QzMERCUF9wcmVDUEJfbWluJywnQU9UMzBEQlBfdG90YWxfbWluJywnQU9UNjVNQVBfQ1BCX21pbicsJ0FPVDY1TUFQX3Bvc3RDUEJfbWluJywnQU9UNjVNQVBfcHJlQ1BCX21pbicsJ0FPVDY1TUFQX3RvdGFsX21pbicsJ0FPVDk1U0JQX3Bvc3RDUEJfbWluJywnQU9UOTVTQlBfcHJlQ1BCX21pbicsJ0FPVDk1U0JQX3RvdGFsX21pbicsJ0FVVDMwREJQX3Bvc3RDUEJfbWluJywnQVVUMzBEQlBfcHJlQ1BCX21pbicsJ0FVVDMwREJQX3RvdGFsX21pbicsJ0FVVDUwTUFQX0NQQl9taW4nLCdBVVQ1ME1BUF9wb3N0Q1BCX21pbicsJ0FVVDUwTUFQX3ByZUNQQl9taW4nLCdBVVQ1ME1BUF90b3RhbF9taW4nLCdBVVQ2NU1BUF9DUEJfbWluJywnQVVUNjVNQVBfcG9zdENQQl9taW4nLCdBVVQ2NU1BUF9wcmVDUEJfbWluJywnQVVUNjVNQVBfdG90YWxfbWluJywnQVVUOTVTQlBfcG9zdENQQl9taW4nLCdBVVQ5NVNCUF9wcmVDUEJfbWluJywnQVVUOTVTQlBfdG90YWxfbWluJywnQVdJNTFfNjBNQVBfQ1BCX21pbicsJ0FXSTUxXzYwTUFQX3Bvc3RDUEJfbWluJywnQVdJNTFfNjBNQVBfcHJlQ1BCX21pbicsJ0FXSTUxXzYwTUFQX3RvdGFsX21pbicsJ0FXSTYxXzY1TUFQX0NQQl9taW4nLCdBV0k2MV82NU1BUF9wb3N0Q1BCX21pbicsJ0FXSTYxXzY1TUFQX3ByZUNQQl9taW4nLCdBV0k2MV82NU1BUF90b3RhbF9taW4nLCdkdXJhdGlvbl9DUEJfbWluJywnZHVyYXRpb25fcG9zdENQQl9taW4nLCdkdXJhdGlvbl9wcmVDUEJfbWluJywnZHVyYXRpb25fU3VyZ2VyeV9taW4nKQoKdGFibGUyYWxsIDwtIENyZWF0ZVRhYmxlT25lKHZhcnMgPSBsaXN0RXhwb3N1cmVzLCBkYXRhID0gRmluYWxIZW1vZHluYW1pY3NEYXRhc2V0X2Zvcl90YWJsZTEsIHRlc3ROb3JtYWw9b25ld2F5LnRlc3QpCgp3cml0ZS54bHN4KGFzLmRhdGEuZnJhbWUocHJpbnQodGFibGUyYWxsKSksICJ0YWJsZTJhbGwueGxzeCIpCgoKdGFibGUyc3RyYXQgPC0gQ3JlYXRlVGFibGVPbmUodmFycyA9IGxpc3RFeHBvc3VyZXMsIGRhdGEgPSBGaW5hbEhlbW9keW5hbWljc0RhdGFzZXRfZm9yX3RhYmxlMSwgZmFjdG9yVmFycyA9IGxpc3RPdXRzdGFibGUyLHRlc3ROb3JtYWw9b25ld2F5LnRlc3QgCiAgICAgICAgICAgICAgICAgICAgICAgICxzdHJhdGEgPSBjKCJBbnlfTUFFIikgKQogICAgICAgICAgICAgICAgICAgICAgICAKd3JpdGUueGxzeChhcy5kYXRhLmZyYW1lKHByaW50KHRhYmxlMnN0cmF0KSksICJ0YWJsZTJzdHJhdC54bHN4IikKICAgICAgICAgICAgICAgICAgICAgICAgCiNzdHJhdGEgaXMgdXNlZnVsbCBpbiBjYXNlIHdlIHdhbnQgc2V2ZXJhbCBjb2x1bW5zIHRvIHN0cmF0aWZ5IGJ5CiMgbm9ubm9ybWFsIHNwZWNpZmllcyB2YXJpYWJsZXMgdG8gYmUgc2hvd24gYXMgbWVkaWFuIFtJUVJdCiMgdGVzdEFwcHJveCBBIGZ1bmN0aW9uIHVzZWQgdG8gcGVyZm9ybSB0aGUgbGFyZ2Ugc2FtcGxlIGFwcHJveGltYXRpb24gYmFzZWQgdGVzdHMuIFRoZQojIGRlZmF1bHQgaXMgY2hpc3EudGVzdC4gVGhpcyBpcyBub3QgcmVjb21tZW5kZWQgd2hlbiBzb21lIG9mIHRoZSBjZWxsIGhhdmUKIyBzbWFsbCBjb3VudHMgbGlrZSBmZXdlciB0aGFuIDUuCgpgYGAKCiMgVGFibGUgMwoKYGBge3J9CgoKbGlicmFyeSh0YWJsZW9uZSkKbGlicmFyeShtYWdyaXR0cikKCmxpc3RPdXRzdGFibGUyPC1jKCdBbnlfTUFFJyxsaXN0T3V0cykKCnRhYmxlMmFsbCA8LSBDcmVhdGVUYWJsZU9uZSh2YXJzID0gbGlzdE91dHN0YWJsZTIsIGRhdGEgPSBGaW5hbEhlbW9keW5hbWljc0RhdGFzZXQsIGZhY3RvclZhcnMgPSBsaXN0T3V0c3RhYmxlMix0ZXN0Tm9ybWFsPW9uZXdheS50ZXN0IAogICAgICAgICAgICAgICAgICAgICAgICAgKQoKdGFibGUyc3RyYXQgPC0gQ3JlYXRlVGFibGVPbmUodmFycyA9IGxpc3RPdXRzdGFibGUyLCBkYXRhID0gRmluYWxIZW1vZHluYW1pY3NEYXRhc2V0LCBmYWN0b3JWYXJzID0gbGlzdE91dHN0YWJsZTIsdGVzdE5vcm1hbD1vbmV3YXkudGVzdCAKICAgICAgICAgICAgICAgICAgICAgICAgLHN0cmF0YSA9IGMoIkFueV9NQUUiKSkKICAgICAgICAgICAgICAgICAgICAgICAgCndyaXRlLnhsc3goYXMuZGF0YS5mcmFtZShwcmludCh0YWJsZTJzdHJhdCkpLCAidGFibGUyc3RyYXQueGxzeCIpCiAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAjc3RyYXRhIGlzIHVzZWZ1bGwgaW4gY2FzZSB3ZSB3YW50IHNldmVyYWwgY29sdW1ucyB0byBzdHJhdGlmeSBieQojIyBub25ub3JtYWwgc3BlY2lmaWVzIHZhcmlhYmxlcyB0byBiZSBzaG93biBhcyBtZWRpYW4gW0lRUl0KIyAKIyB0ZXN0QXBwcm94IEEgZnVuY3Rpb24gdXNlZCB0byBwZXJmb3JtIHRoZSBsYXJnZSBzYW1wbGUgYXBwcm94aW1hdGlvbiBiYXNlZCB0ZXN0cy4gVGhlCiMgZGVmYXVsdCBpcyBjaGlzcS50ZXN0LiBUaGlzIGlzIG5vdCByZWNvbW1lbmRlZCB3aGVuIHNvbWUgb2YgdGhlIGNlbGwgaGF2ZQojIHNtYWxsIGNvdW50cyBsaWtlIGZld2VyIHRoYW4gNS4KCiMgdGFibGUyPC1wcmludCh0YWJsZTIsZXN0QXBwcm94PWxpc3RWYXJzLCBleGFjdCA9ICJzdGFnZSIsIHF1b3RlID0gVFJVRSxzaG93QWxsTGV2ZWxzICA9IFRSVUUpCiMgCiMgZG9jeCggKSAlPiUKIyAgICAgYWRkRmxleFRhYmxlKHRhYmxlMiAlPiUKIyAgICAgRmxleFRhYmxlKGhlYWRlci5jZWxsLnByb3BzID0gY2VsbFByb3BlcnRpZXMoIGJhY2tncm91bmQuY29sb3IgPSAiIzAwMzM2NiIpLAojICAgICAgICAgICAgICAgaGVhZGVyLnRleHQucHJvcHMgPSB0ZXh0Qm9sZCggY29sb3IgPSAid2hpdGUiICksCiMgICAgICAgICAgICAgICBhZGQucm93bmFtZXMgPSBUUlVFICkgJT4lCiMgICAgICAgICAgICAgICBzZXRaZWJyYVN0eWxlKCBvZGQgPSAiI0RERERERCIsIGV2ZW4gPSAiI0ZGRkZGRiIgKSApICU+JQojICAgICB3cml0ZURvYyhmaWxlID0gInRhYmxlMi5kb2N4IikKIyAKIyBicm93c2VVUkwoInRhYmxlMi5kb2N4IikgI3dvcmRmaWxlCmBgYAoK
