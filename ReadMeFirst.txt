## READ ME FIRST

# DTSA 5301 – DATA SCIENCE AS A FIELD.-
# WEEK 5.- FINAL PROJECT 1.-
# "NYPD Shooting Incident Data Report"


# Files:
ReadMeFirst.txt: this file.

Week5NYPDRev0.rmd : This is the un-knitted .RMD. You may download and knit. I used RStudio code 2023.06.1 Build 524.

Week5NYPDRev0.HTML: This is the knitted output of the .RMD file above.



# Brief description of the Project:
This document presents the analysis and results of the Shooting Incidents (SI) data obtained from the City of New York between 2006 and 2022. The goals and questions I would like to answer are presented at
the beginning of the document, followed by the answers to the questions which are also the conclusions. This section looks rather long since it is here where the visualizations and comments are presented.
After importing and tiding the data, I follow the iterative "Transform - Visualize - Model process". The results are presented clean, with the code in Attachments. 
The attachments provide the details, logic, and commented code to support all computations and graphs for all sections. There have been sharp variations in SI in recent years. I often use long-run averages, and
compare them to recent years (2022 mostly) to test current relevance.

# Data Source:
The data contains Shooting Incidents in New York city between 2006 and 2022. It was sourced from the official Government website <https://catalog.data.gov/datase>, in csv format from the following
link:
<https://data.cityofnewyork.us/api/views/833y-fsy8/rows.csv?accessType=DOWNLOAD>

The code is written to read off the link and download, without saving the data locally.
You could, if you chose to, save the data in the same directory of the .RMD file, and read locally. 
This needs a few minor modifications to the code. I have purposely left this to be a manual operation for those who choose to do it.
I you choose to download the file manually, it must be saved downloaded in the directory in which "Week5NYPDRev0.Rmd" is located. 
You need to add a line of code to read the file, and comment out the following 2 lines of code that read from the url:
urlNYPD <- "https://data.cityofnewyork.us/api/views/833y-fsy8/rows.csv?accessType=DOWNLOAD"
origNypdDF <- read.csv(urlNYPD) %>% as.data.frame()


# Libraries:
This document requires the following libraries: 
library( "tidyverse")
library("dplyr") 
library("ggplot2") 
library("lubridate")
library("viridis")

# System Info:
# R version 4.3.0 (2023-04-21 ucrt)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 10 x64 (build 19045)
## 
## Matrix products: default
## 
## 
## locale:
## [1] LC_COLLATE=English_United States.utf8 
## [2] LC_CTYPE=English_United States.utf8   
## [3] LC_MONETARY=English_United States.utf8
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.utf8    
## 
## time zone: America/New_York
## tzcode source: internal
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] viridis_0.6.3     viridisLite_0.4.2 lubridate_1.9.2   forcats_1.0.0    
##  [5] stringr_1.5.0     dplyr_1.1.2       purrr_1.0.1       readr_2.1.4      
##  [9] tidyr_1.3.0       tibble_3.2.1      ggplot2_3.4.2     tidyverse_2.0.0  
## 
## loaded via a namespace (and not attached):
##  [1] gtable_0.3.3      jsonlite_1.8.7    highr_0.10        compiler_4.3.0   
##  [5] tidyselect_1.2.0  gridExtra_2.3     jquerylib_0.1.4   scales_1.2.1     
##  [9] yaml_2.3.7        fastmap_1.1.1     R6_2.5.1          labeling_0.4.2   
## [13] generics_0.1.3    knitr_1.43        munsell_0.5.0     bslib_0.5.0      
## [17] pillar_1.9.0      tzdb_0.4.0        rlang_1.1.1       utf8_1.2.3       
## [21] stringi_1.7.12    cachem_1.0.8      xfun_0.39         sass_0.4.6       
## [25] timechange_0.2.0  cli_3.6.1         withr_2.5.0       magrittr_2.0.3   
## [29] digest_0.6.33     grid_4.3.0        rstudioapi_0.15.0 hms_1.1.3        
## [33] lifecycle_1.0.3   vctrs_0.6.3       evaluate_0.21     glue_1.6.2       
## [37] farver_2.1.1      fansi_1.0.4       colorspace_2.1-0  rmarkdown_2.23   
## [41] tools_4.3.0       pkgconfig_2.0.3   htmltools_0.5.5