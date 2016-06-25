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

