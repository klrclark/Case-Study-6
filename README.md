# Case-Study-6
Karen Clark
2016-0509 MSDS 6306
MSDS 6306 DOING DATA SCIENCE - 404

The case study included two datasets that could have been downloaded from the following URLs or downloaded directly from the website and saved on your personal computer.  I chose to download the dataset from the URL directly (which were considered the original datasets) and then pull it from the file saved on my computer

Load the Gross Domestic Product data for the 190 ranked countries in this data set: 
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv 

Load the educational data from this data set: 
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv 

Original data sources (if the links above don’t work): 
http://data.worldbank.org/data-catalog/GDP-ranking-table 
http://data.worldbank.org/data-catalog/ed-stats 

Original data files can be found in the Data folder, with the following names:
GDA - GDP ranking table
CountryData -  Country Data - ed-stats data set

Since I chose to download the most current file there were 195 ranked countries in the dataset.

There are two data sets that are used to complete this case study:

1.	Country data (234 observations) – Contains data by country, Region, , income population classification, and various other variables that were not used in this case study

2.	Gross Domestic Product Ranking Table - FGDP (243 observations) – contains a view of a countries economy over a course of a year.
The data supplied is the country, the ranking of the country, and the economy for that country.

A Data set was created by combining the Country Data with the Gross Domestic Product Ranking table on the country code.  Out of the 228 countries in the FGDP data set only 195 records that match to the country data dataset.

The goal of this case study was to cleanse the data and then perform some simple analytics against the cleansed data.

All Datasets created are stored in the Data Folder
The Rmarkdown, .md, and html files are in the RMarkdown folder

R Session Information

R version 3.3.0 (2016-05-03)
Platform: x86_64-w64-mingw32/x64 (64-bit)
Running under: Windows 7 x64 (build 7601) Service Pack 1

locale:
[1] LC_COLLATE=English_United States.1252  LC_CTYPE=English_United States.1252    LC_MONETARY=English_United States.1252
[4] LC_NUMERIC=C                           LC_TIME=English_United States.1252    

attached base packages:
[1] tcltk     stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
[1] sqldf_0.4-10  RSQLite_1.0.0 DBI_0.4       gsubfn_0.6-6  proto_0.3-10  dplyr_0.4.3   ggplot2_2.1.0

loaded via a namespace (and not attached):
 [1] Rcpp_0.12.5        knitr_1.12.3       magrittr_1.5       munsell_0.4.3      colorspace_1.2-6   R6_2.1.2          
 [7] stringr_1.0.0      httr_1.1.0         plyr_1.8.3         tools_3.3.0        parallel_3.3.0     grid_3.3.0        
[13] gtable_0.2.0       htmltools_0.3.5    yaml_2.1.13        digest_0.6.9       assertthat_0.1     countrycode_0.18  
[19] crayon_1.3.1       RColorBrewer_1.1-2 bitops_1.0-6       RCurl_1.95-4.8     testthat_1.0.2     memoise_1.0.0     
[25] rmarkdown_0.9.6    labeling_0.3       stringi_1.0-1      scales_0.4.0       swirl_2.4.1        chron_2.3-47      
