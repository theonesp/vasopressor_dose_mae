Table1 
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
