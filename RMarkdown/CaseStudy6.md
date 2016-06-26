Introduction
============

The goal of this case study was to cleanse the selected data detailed
below and then perform some simple analytics against the cleansed data.

There are two data sets that are used to complete this case study:

1.  Country data (234 observations) - Contains data by country, Region,
    , income population classification, and various other variables that
    were not used in this case study.
    (<http://data.worldbank.org/data-catalog/ed-stats>)

2.  Gross Domestic Product Ranking Table - FGDP (243 observations) -
    contains a view of a countries economy over a course of a year. The
    data supplied is the country, the ranking of the country, and the
    economy for that country.
    (<http://data.worldbank.org/data-catalog/GDP-ranking-table>)

A Dataset was created by combining the Country Data with the Gross
Domestic Product (GDP) Ranking table on the country code. Out of the 228
countries in the GDP data set only 195 records that match to the country
data dataset.

Data Cleansing
==============

Both datasets needs to have columns and rows removed in order to tidy
the data.

Exploring the data set: Gross Domestic Product Ranking Table - GDP
------------------------------------------------------------------

    #Opening csv file in R and storing data set in a data frame.
    GDA <- read.csv("C:\\Users\\kclark\\Personal\\SMU\\Doing Data Science MSDS 6306\\Unit 6\\GDP.csv", stringsAsFactors = FALSE, header = FALSE)

    #Validating that GDA is a data frame
    class(GDA)

    ## [1] "data.frame"

    #Checking the structure of the data frame and data types
    str(GDA)

    ## 'data.frame':    243 obs. of  6 variables:
    ##  $ V1: chr  "" "" "" "" ...
    ##  $ V2: chr  "Gross domestic product 2014" "" "" "Ranking" ...
    ##  $ V3: logi  NA NA NA NA NA NA ...
    ##  $ V4: chr  "" "" "" "Economy" ...
    ##  $ V5: chr  "" "" "(millions of" "US dollars)" ...
    ##  $ V6: chr  "" "" "" "" ...

    #View the first 10 records for the GDA dataset
    head(GDA)

    ##    V1                          V2 V3            V4           V5 V6
    ## 1     Gross domestic product 2014 NA                              
    ## 2                                 NA                              
    ## 3                                 NA               (millions of   
    ## 4                         Ranking NA       Economy  US dollars)   
    ## 5                                 NA                              
    ## 6 USA                           1 NA United States   17,419,000

    #view the last 10 records of the GDA dataset
    tail(GDA)

    ##     V1
    ## 238   
    ## 239   
    ## 240   
    ## 241   
    ## 242   
    ## 243   
    ##                                                                                                                                                V2
    ## 238                                                                                                                           .. Not available.  
    ## 239                            Note: Rankings include only those economies with confirmed GDP estimates. Figures in italics are for 2013 or 2012.
    ## 240      a. Based on data from official statistics of Ukraine and Russian Federation; by relying on these data, the World Bank does not intend to
    ## 241 make any judgment on the legal or other status of the territories concerned or to prejudice the final determination of the parties' claims.  
    ## 242        b. Includes Former Spanish Sahara.  c. Covers mainland Tanzania only. d. Data are for the area controlled by the government of Cyprus.
    ## 243                                                                            e. Excludes Abkhazia and South Ossetia.  f. Excludes Transnistria.
    ##     V3 V4 V5 V6
    ## 238 NA         
    ## 239 NA         
    ## 240 NA         
    ## 241 NA         
    ## 242 NA         
    ## 243 NA

### Row and Column Selections for GDP-Ranking Dataset

The first 5 rows of the GDA data set includes the variable names and
blanks. The last 7 rows include information about the data set. The
first 5 rows and the last 7 rows are removed because they add no value
to the data to be analyzed.

    #This statement will only keep rows 6 to 236 and a new version of the dataset - GDA1 is created.  
    GDA1 <- GDA[6:236,]

The current GDA1 data set had 6 columns, but only 4 actually contain the
data for the case study. The code below will remove the extra columns
that are not needed.

    #Remove columns 3 and 6 from the dataset by only selecting 1, 2, 4, and 5
    GDA1 <- GDA1[c(1,2,4,5)]

### Adding Variable Names to the Dataset

Names to each variable are added to the data set. The industry standard
of capitalizing each word was applied. The variable names are human
readable and self-explanatory.

    #Add Names to the variables in the data set
    names(GDA1) <- c("CountryCode","Ranking","Country", "Economy")

### Removing Blank Records and Bad data in the Dataset

There are a couple of blank records still within the data set. Set the
blank variables to NA for blank rows and then remove them from the data
set and create a new data set GDA2

    #sets the blank variables to NA where entire rows are blank if GDA - Economy variable is blank
    GDA1[GDA1==""] <- NA

    #Remove rows where all columns are NAs
    GDA2 <- GDA1[apply(GDA1,1,function(x)any(!is.na(x))),]

The data set GDA2 still contains records where the Economy field does
not have an applicable value stored in it - " .. ". The identified rows
below will be removed from the GDA2 data set.

    #Find the rows where GDA2$Economy = " .. "
    fslines <- which(GDA2$Economy == " .. ")

    # view the record numbers for the rows that were identified
    fslines

    ##  [1] 196 197 198 199 200 201 202 203 204 205 206 207 208 209 210 211 212
    ## [18] 213 214

    #remove identified rows and create a new data set GDA3
    GDA3 <- GDA2[-which(GDA2$Economy == " .. "), ]

### Final Dataset for GDP-Ranking

The final dataset for GDP-Ranking is cleansed and ready to use

    #view dataset GDA3
    GDA3

    ##     CountryCode Ranking                        Country    Economy
    ## 6           USA       1                  United States 17,419,000
    ## 7           CHN       2                          China 10,354,832
    ## 8           JPN       3                          Japan  4,601,461
    ## 9           DEU       4                        Germany  3,868,291
    ## 10          GBR       5                 United Kingdom  2,988,893
    ## 11          FRA       6                         France  2,829,192
    ## 12          BRA       7                         Brazil  2,416,636
    ## 13          ITA       8                          Italy  2,141,161
    ## 14          IND       9                          India  2,048,517
    ## 15          RUS      10             Russian Federation  1,860,598
    ## 16          CAN      11                         Canada  1,785,387
    ## 17          AUS      12                      Australia  1,454,675
    ## 18          KOR      13                    Korea, Rep.  1,410,383
    ## 19          ESP      14                          Spain  1,381,342
    ## 20          MEX      15                         Mexico  1,294,690
    ## 21          IDN      16                      Indonesia    888,538
    ## 22          NLD      17                    Netherlands    879,319
    ## 23          TUR      18                         Turkey    798,429
    ## 24          SAU      19                   Saudi Arabia    753,832
    ## 25          CHE      20                    Switzerland    701,037
    ## 26          SWE      21                         Sweden    571,090
    ## 27          NGA      22                        Nigeria    568,508
    ## 28          POL      23                         Poland    544,967
    ## 29          ARG      24                      Argentina    537,660
    ## 30          BEL      25                        Belgium    531,547
    ## 31          NOR      26                         Norway    499,817
    ## 32          AUT      27                        Austria    436,888
    ## 33          IRN      28             Iran, Islamic Rep.    425,326
    ## 34          THA      29                       Thailand    404,824
    ## 35          ARE      30           United Arab Emirates    399,451
    ## 36          VEN      31                  Venezuela, RB    381,286
    ## 37          COL      32                       Colombia    377,740
    ## 38          ZAF      33                   South Africa    350,141
    ## 39          DNK      34                        Denmark    342,362
    ## 40          MYS      35                       Malaysia    338,104
    ## 41          SGP      36                      Singapore    307,860
    ## 42          ISR      37                         Israel    305,675
    ## 43          EGY      38               Egypt, Arab Rep.    301,499
    ## 44          HKG      39           Hong Kong SAR, China    290,896
    ## 45          PHL      40                    Philippines    284,777
    ## 46          FIN      41                        Finland    272,217
    ## 47          CHL      42                          Chile    258,062
    ## 48          IRL      43                        Ireland    250,814
    ## 49          PAK      44                       Pakistan    243,632
    ## 50          GRC      45                         Greece    235,574
    ## 51          PRT      46                       Portugal    230,117
    ## 52          IRQ      47                           Iraq    223,500
    ## 53          KAZ      48                     Kazakhstan    217,872
    ## 54          DZA      49                        Algeria    213,518
    ## 55          QAT      50                          Qatar    210,109
    ## 56          CZE      51                 Czech Republic    205,270
    ## 57          PER      52                           Peru    202,596
    ## 58          NZL      53                    New Zealand    199,970
    ## 59          ROM      54                        Romania    199,044
    ## 60          VNM      55                        Vietnam    186,205
    ## 61          BGD      56                     Bangladesh    172,887
    ## 62          KWT      57                         Kuwait    163,612
    ## 63          AGO      58                         Angola    138,357
    ## 64          HUN      59                        Hungary    138,347
    ## 65          UKR      60                        Ukraine    131,805
    ## 66          MAR      61                        Morocco    110,009
    ## 67          PRI      62                    Puerto Rico    103,135
    ## 68          ECU      63                        Ecuador    100,917
    ## 69          SVK      64                Slovak Republic    100,249
    ## 70          OMN      65                           Oman     81,797
    ## 71          LKA      66                      Sri Lanka     78,824
    ## 72          CUB      67                           Cuba     77,150
    ## 73          BLR      68                        Belarus     76,139
    ## 74          AZE      69                     Azerbaijan     75,198
    ## 75          SDN      70                          Sudan     73,815
    ## 76          LUX      71                     Luxembourg     64,874
    ## 77          MMR      72                        Myanmar     64,330
    ## 78          DOM      73             Dominican Republic     64,138
    ## 79          UZB      74                     Uzbekistan     62,644
    ## 80          KEN      75                          Kenya     60,937
    ## 81          GTM      76                      Guatemala     58,827
    ## 82          URY      77                        Uruguay     57,471
    ## 83          HRV      78                        Croatia     57,113
    ## 84          BGR      79                       Bulgaria     56,717
    ## 85          ETH      80                       Ethiopia     55,612
    ## 86          MAC      81               Macao SAR, China     55,502
    ## 87          CRI      82                     Costa Rica     49,553
    ## 88          SVN      83                       Slovenia     49,491
    ## 89          TUN      84                        Tunisia     48,613
    ## 90          LTU      85                      Lithuania     48,354
    ## 91          TZA      86                       Tanzania     48,057
    ## 92          TKM      87                   Turkmenistan     47,932
    ## 93          PAN      88                         Panama     46,213
    ## 94          LBN      89                        Lebanon     45,731
    ## 95          SRB      90                         Serbia     43,866
    ## 96          LBY      91                          Libya     41,143
    ## 97          GHA      92                          Ghana     38,617
    ## 98          YEM      93                    Yemen, Rep.     35,955
    ## 99          JOR      94                         Jordan     35,827
    ## 100         CIV      95                  Côte d'Ivoire     34,254
    ## 101         BHR      96                        Bahrain     33,851
    ## 102         ZAR      97               Congo, Dem. Rep.     33,121
    ## 103         BOL      98                        Bolivia     32,996
    ## 104         CMR      99                       Cameroon     32,051
    ## 105         LVA     100                         Latvia     31,287
    ## 106         PRY     101                       Paraguay     30,881
    ## 107         TTO     102            Trinidad and Tobago     28,883
    ## 108         ZMB     103                         Zambia     27,066
    ## 109         UGA     104                         Uganda     26,998
    ## 110         EST     105                        Estonia     26,485
    ## 111         SLV     106                    El Salvador     25,164
    ## 112         CYP     107                         Cyprus     23,226
    ## 113         AFG     108                    Afghanistan     20,038
    ## 114         NPL     109                          Nepal     19,770
    ## 115         HND     110                       Honduras     19,385
    ## 116         BIH     111         Bosnia and Herzegovina     18,521
    ## 117         GAB     112                          Gabon     18,180
    ## 118         BRN     113              Brunei Darussalam     17,105
    ## 119         ISL     114                        Iceland     17,036
    ## 120         PNG     115               Papua New Guinea     16,929
    ## 121         KHM     116                       Cambodia     16,778
    ## 122         GEO     117                        Georgia     16,530
    ## 123         MOZ     118                     Mozambique     15,938
    ## 124         BWA     119                       Botswana     15,813
    ## 125         SEN     120                        Senegal     15,658
    ## 126         GNQ     121              Equatorial Guinea     15,530
    ## 127         ZWE     122                       Zimbabwe     14,197
    ## 128         COG     123                    Congo, Rep.     14,177
    ## 129         TCD     124                           Chad     13,922
    ## 130         JAM     125                        Jamaica     13,891
    ## 131         SSD     126                    South Sudan     13,282
    ## 132         ALB     127                        Albania     13,212
    ## 133         NAM     128                        Namibia     12,995
    ## 134         WBG     129             West Bank and Gaza     12,738
    ## 135         MUS     130                      Mauritius     12,630
    ## 136         BFA     131                   Burkina Faso     12,542
    ## 137         MLI     132                           Mali     12,037
    ## 138         MNG     133                       Mongolia     12,016
    ## 139         LAO     134                        Lao PDR     11,997
    ## 140         NIC     135                      Nicaragua     11,806
    ## 141         ARM     136                        Armenia     11,644
    ## 142         MKD     137                 Macedonia, FYR     11,324
    ## 143         MDG     138                     Madagascar     10,593
    ## 144         MLT     139                          Malta      9,643
    ## 145         BEN     140                          Benin      9,575
    ## 146         TJK     141                     Tajikistan      9,242
    ## 147         HTI     142                          Haiti      8,713
    ## 148         BHS     143                   Bahamas, The      8,511
    ## 149         NER     144                          Niger      8,169
    ## 150         MDA     145                        Moldova      7,962
    ## 151         RWA     146                         Rwanda      7,890
    ## 152         KGZ     147                Kyrgyz Republic      7,404
    ## 153         KSV     148                         Kosovo      7,387
    ## 154         GIN     149                         Guinea      6,624
    ## 155         SOM     150                        Somalia      5,707
    ## 156         BMU     151                        Bermuda      5,574
    ## 157         LIE     152                  Liechtenstein      5,488
    ## 158         SUR     153                       Suriname      5,210
    ## 159         MRT     154                     Mauritania      5,061
    ## 160         SLE     155                   Sierra Leone      4,838
    ## 161         MNE     156                     Montenegro      4,588
    ## 162         FJI     157                           Fiji      4,532
    ## 163         TGO     158                           Togo      4,518
    ## 164         SWZ     159                      Swaziland      4,413
    ## 165         BRB     160                       Barbados      4,355
    ## 166         MWI     161                         Malawi      4,258
    ## 167         ADO     162                        Andorra      3,249
    ## 168         GUY     163                         Guyana      3,097
    ## 169         BDI     164                        Burundi      3,094
    ## 170         MDV     165                       Maldives      3,062
    ## 171         FRO     166                  Faroe Islands      2,613
    ## 172         GRL     167                      Greenland      2,441
    ## 173         LSO     168                        Lesotho      2,181
    ## 174         LBR     169                        Liberia      2,013
    ## 175         BTN     170                         Bhutan      1,959
    ## 176         CPV     171                     Cabo Verde      1,871
    ## 177         CAF     172       Central African Republic      1,723
    ## 178         BLZ     173                         Belize      1,699
    ## 179         DJI     174                       Djibouti      1,589
    ## 180         SYC     175                     Seychelles      1,423
    ## 181         TMP     176                    Timor-Leste      1,417
    ## 182         LCA     177                      St. Lucia      1,404
    ## 183         ATG     178            Antigua and Barbuda      1,221
    ## 184         SLB     179                Solomon Islands      1,158
    ## 185         GNB     180                  Guinea-Bissau      1,022
    ## 186         GRD     181                        Grenada        912
    ## 187         KNA     182            St. Kitts and Nevis        852
    ## 188         GMB     183                    Gambia, The        851
    ## 189         VUT     184                        Vanuatu        815
    ## 190         WSM     185                          Samoa        800
    ## 191         VCT     186 St. Vincent and the Grenadines        729
    ## 192         COM     187                        Comoros        624
    ## 193         DMA     188                       Dominica        524
    ## 194         TON     189                          Tonga        434
    ## 195         STP     190          São Tomé and Principe        337
    ## 196         FSM     191          Micronesia, Fed. Sts.        318
    ## 197         PLW     192                          Palau        251
    ## 198         MHL     193               Marshall Islands        187
    ## 199         KIR     194                       Kiribati        167
    ## 200         TUV     195                         Tuvalu         38
    ## 222         WLD    <NA>                          World 77,960,607
    ## 224         LIC    <NA>                     Low income    397,849
    ## 225         MIC    <NA>                  Middle income 24,748,448
    ## 226         LMC    <NA>            Lower middle income  5,781,069
    ## 227         UMC    <NA>            Upper middle income 18,958,149
    ## 228         LMY    <NA>            Low & middle income 25,148,400
    ## 229         EAP    <NA>            East Asia & Pacific 12,609,716
    ## 230         ECA    <NA>          Europe & Central Asia  1,817,461
    ## 231         LAC    <NA>      Latin America & Caribbean  4,845,035
    ## 232         MNA    <NA>     Middle East & North Africa  1,556,768
    ## 233         SAS    <NA>                     South Asia  2,588,688
    ## 234         SSA    <NA>             Sub-Saharan Africa  1,728,322
    ## 235         HIC    <NA>                    High income 52,850,488
    ## 236         EMU    <NA>                      Euro area 13,410,232

Exploring the Dataset: Country Data
-----------------------------------

    #Read the csv file into R and storing the data in a data frame named CountryData
    CountryData <- read.csv("C:\\Users\\kclark\\Personal\\SMU\\Doing Data Science MSDS 6306\\Unit 6\\country.csv", stringsAsFactors = FALSE, header = FALSE)

    #Validating that CountryData is a data frame
    class(CountryData) 

    ## [1] "data.frame"

    #Checking the structure of the data frame and data types
    str(CountryData)

    ## 'data.frame':    242 obs. of  31 variables:
    ##  $ V1 : chr  "Country Code" "ABW" "ADO" "AFG" ...
    ##  $ V2 : chr  "Short Name" "Aruba" "Andorra" "Afghanistan" ...
    ##  $ V3 : chr  "Table Name" "Aruba" "Andorra" "Afghanistan" ...
    ##  $ V4 : chr  "Long Name" "Aruba" "Principality of Andorra" "Islamic State of Afghanistan" ...
    ##  $ V5 : chr  "2-alpha code" "AW" "AD" "AF" ...
    ##  $ V6 : chr  "Currency Unit" "Aruban florin" "Euro" "Afghan afghani" ...
    ##  $ V7 : chr  "Special Notes" "SNA data for 2000-2011 are updated from official government statistics; 1994-1999 from UN databases. Base year has changed from"| __truncated__ "" "Fiscal year end: March 20; reporting period for national accounts data: FY (from 2013 are CY). National accounts data are sourc"| __truncated__ ...
    ##  $ V8 : chr  "Region" "Latin America & Caribbean" "Europe & Central Asia" "South Asia" ...
    ##  $ V9 : chr  "Income Group" "High income: nonOECD" "High income: nonOECD" "Low income" ...
    ##  $ V10: chr  "WB-2 code" "AW" "AD" "AF" ...
    ##  $ V11: chr  "National accounts base year" "2000" "1990" "2002/03" ...
    ##  $ V12: chr  "National accounts reference year" "" "" "" ...
    ##  $ V13: chr  "SNA price valuation" "Value added at basic prices (VAB)" "" "Value added at basic prices (VAB)" ...
    ##  $ V14: chr  "Lending category" "" "" "IDA" ...
    ##  $ V15: chr  "Other groups" "" "" "HIPC" ...
    ##  $ V16: chr  "System of National Accounts" "Country uses the 1993 System of National Accounts methodology." "Country uses the 1968 System of National Accounts methodology." "Country uses the 1993 System of National Accounts methodology." ...
    ##  $ V17: chr  "Alternative conversion factor" "" "" "" ...
    ##  $ V18: chr  "PPP survey year" "" "" "" ...
    ##  $ V19: chr  "Balance of Payments Manual in use" "IMF Balance of Payments Manual, 6th edition." "" "" ...
    ##  $ V20: chr  "External debt Reporting status" "" "" "Actual" ...
    ##  $ V21: chr  "System of trade" "Special trade system" "Special trade system" "General trade system" ...
    ##  $ V22: chr  "Government Accounting concept" "" "" "Consolidated central government" ...
    ##  $ V23: chr  "IMF data dissemination standard" "" "" "General Data Dissemination System (GDDS)" ...
    ##  $ V24: chr  "Latest population census" "2010" "2011. Population figures compiled from administrative registers." "1979" ...
    ##  $ V25: chr  "Latest household survey" "" "" "Multiple Indicator Cluster Survey (MICS), 2010/11" ...
    ##  $ V26: chr  "Source of most recent Income and expenditure data" "" "" "Integrated household survey (IHS), 2008" ...
    ##  $ V27: chr  "Vital registration complete" "Yes" "Yes" "" ...
    ##  $ V28: chr  "Latest agricultural census" "" "" "2013/14" ...
    ##  $ V29: chr  "Latest industrial data" "" "" "" ...
    ##  $ V30: chr  "Latest trade data" "2012" "2006" "2012" ...
    ##  $ V31: chr  "Latest water withdrawal data" "" "" "2000" ...

    #View the first 10 rows of data
    head(CountryData)

    ##             V1          V2          V3                           V4
    ## 1 Country Code  Short Name  Table Name                    Long Name
    ## 2          ABW       Aruba       Aruba                        Aruba
    ## 3          ADO     Andorra     Andorra      Principality of Andorra
    ## 4          AFG Afghanistan Afghanistan Islamic State of Afghanistan
    ## 5          AGO      Angola      Angola  People's Republic of Angola
    ## 6          ALB     Albania     Albania          Republic of Albania
    ##             V5             V6
    ## 1 2-alpha code  Currency Unit
    ## 2           AW  Aruban florin
    ## 3           AD           Euro
    ## 4           AF Afghan afghani
    ## 5           AO Angolan kwanza
    ## 6           AL   Albanian lek
    ##                                                                                                                                                                                                                                                  V7
    ## 1                                                                                                                                                                                                                                     Special Notes
    ## 2                                                                                                     SNA data for 2000-2011 are updated from official government statistics; 1994-1999 from UN databases. Base year has changed from 1995 to 2000.
    ## 3                                                                                                                                                                                                                                                  
    ## 4 Fiscal year end: March 20; reporting period for national accounts data: FY (from 2013 are CY). National accounts data are sourced from the IMF and differ from the Central Statistics Organization numbers due to exclusion of the opium economy.
    ## 5                                                                                                                April 2013 database update: Based on IMF data, national accounts data were revised for 2000 onward; the base year changed to 2002.
    ## 6                                                                                                                                                                                                                                                  
    ##                          V8                   V9       V10
    ## 1                    Region         Income Group WB-2 code
    ## 2 Latin America & Caribbean High income: nonOECD        AW
    ## 3     Europe & Central Asia High income: nonOECD        AD
    ## 4                South Asia           Low income        AF
    ## 5        Sub-Saharan Africa  Upper middle income        AO
    ## 6     Europe & Central Asia  Upper middle income        AL
    ##                                                  V11
    ## 1                        National accounts base year
    ## 2                                               2000
    ## 3                                               1990
    ## 4                                            2002/03
    ## 5                                               2002
    ## 6 Original chained constant price data are rescaled.
    ##                                V12                                  V13
    ## 1 National accounts reference year                  SNA price valuation
    ## 2                                     Value added at basic prices (VAB)
    ## 3                                                                      
    ## 4                                     Value added at basic prices (VAB)
    ## 5                                  Value added at producer prices (VAP)
    ## 6                             1996    Value added at basic prices (VAB)
    ##                V14          V15
    ## 1 Lending category Other groups
    ## 2                              
    ## 3                              
    ## 4              IDA         HIPC
    ## 5             IBRD             
    ## 6             IBRD             
    ##                                                              V16
    ## 1                                    System of National Accounts
    ## 2 Country uses the 1993 System of National Accounts methodology.
    ## 3 Country uses the 1968 System of National Accounts methodology.
    ## 4 Country uses the 1993 System of National Accounts methodology.
    ## 5 Country uses the 1993 System of National Accounts methodology.
    ## 6 Country uses the 1993 System of National Accounts methodology.
    ##                             V17             V18
    ## 1 Alternative conversion factor PPP survey year
    ## 2                                              
    ## 3                                              
    ## 4                                              
    ## 5                       1991<U+0096>96            2005
    ## 6                                       Rolling
    ##                                            V19
    ## 1            Balance of Payments Manual in use
    ## 2 IMF Balance of Payments Manual, 6th edition.
    ## 3                                             
    ## 4                                             
    ## 5 IMF Balance of Payments Manual, 6th edition.
    ## 6 IMF Balance of Payments Manual, 6th edition.
    ##                              V20                  V21
    ## 1 External debt Reporting status      System of trade
    ## 2                                Special trade system
    ## 3                                Special trade system
    ## 4                         Actual General trade system
    ## 5                         Actual Special trade system
    ## 6                         Actual General trade system
    ##                               V22                                      V23
    ## 1   Government Accounting concept          IMF data dissemination standard
    ## 2                                                                         
    ## 3                                                                         
    ## 4 Consolidated central government General Data Dissemination System (GDDS)
    ## 5    Budgetary central government General Data Dissemination System (GDDS)
    ## 6    Budgetary central government General Data Dissemination System (GDDS)
    ##                                                                V24
    ## 1                                         Latest population census
    ## 2                                                             2010
    ## 3 2011. Population figures compiled from administrative registers.
    ## 4                                                             1979
    ## 5                                                             1970
    ## 6                                                             2011
    ##                                                 V25
    ## 1                           Latest household survey
    ## 2                                                  
    ## 3                                                  
    ## 4 Multiple Indicator Cluster Survey (MICS), 2010/11
    ## 5              Malaria Indicator Survey (MIS), 2011
    ## 6      Demographic and Health Survey (DHS), 2008/09
    ##                                                      V26
    ## 1      Source of most recent Income and expenditure data
    ## 2                                                       
    ## 3                                                       
    ## 4                Integrated household survey (IHS), 2008
    ## 5                Integrated household survey (IHS), 2008
    ## 6 Living Standards Measurement Study Survey (LSMS), 2012
    ##                           V27                        V28
    ## 1 Vital registration complete Latest agricultural census
    ## 2                         Yes                           
    ## 3                         Yes                           
    ## 4                                                2013/14
    ## 5                                                   2015
    ## 6                         Yes                       2012
    ##                      V29               V30                          V31
    ## 1 Latest industrial data Latest trade data Latest water withdrawal data
    ## 2                                     2012                             
    ## 3                                     2006                             
    ## 4                                     2012                         2000
    ## 5                                                                  2005
    ## 6                   2010              2012                         2006

    #View last 10 rows of data
    tail(CountryData)

    ##      V1              V2               V3                               V4
    ## 237 WSM           Samoa            Samoa                            Samoa
    ## 238 YEM           Yemen      Yemen, Rep.                Republic of Yemen
    ## 239 ZAF    South Africa     South Africa         Republic of South Africa
    ## 240 ZAR Dem. Rep. Congo Congo, Dem. Rep. Democratic Republic of the Congo
    ## 241 ZMB          Zambia           Zambia               Republic of Zambia
    ## 242 ZWE        Zimbabwe         Zimbabwe             Republic of Zimbabwe
    ##     V5                 V6
    ## 237 WS        Samoan tala
    ## 238 YE        Yemeni rial
    ## 239 ZA South African rand
    ## 240 CD    Congolese franc
    ## 241 ZM New Zambian kwacha
    ## 242 ZW        U.S. dollar
    ##                                                                                                                                                                                                                                                     V7
    ## 237                                                                                          Fiscal year ends on June 30; reporting period for national accounts data: FY. Data are revised from Samoa Bureau of Statistics and Central Bank of Samoa.
    ## 238                                                                                                            Based on official government statistics and International Monetary Fund data, national accounts data have been revised for 1990 onward.
    ## 239                                                                                                                                                                        Fiscal year end: March 31; reporting period for national accounts data: CY.
    ## 240                                                                                                                 Based on INS (2000-09) and IMF (2010-13) data, national accounts data were revised for 2000 onward; the base year changed to 2005.
    ## 241                                                                                           National accounts data have rebased to reflect the January 1, 2013, introduction of the new Zambian kwacha at a rate of 1,000 old kwacha = 1 new kwacha.
    ## 242 Fiscal year end: June 30; reporting period for national accounts data: CY. As of January 2009, multiple hard currencies, such as rand, pound sterling, euro and U.S. dollar are in use. Data are reported in U.S. dollars, the most-used currency.
    ##                             V8                  V9 V10  V11 V12
    ## 237        East Asia & Pacific Lower middle income  WS 2002    
    ## 238 Middle East & North Africa Lower middle income  RY 2007    
    ## 239         Sub-Saharan Africa Upper middle income  ZA 2005    
    ## 240         Sub-Saharan Africa          Low income  ZR 2005    
    ## 241         Sub-Saharan Africa Lower middle income  ZM 1994    
    ## 242         Sub-Saharan Africa          Low income  ZW 2009    
    ##                                      V13   V14  V15
    ## 237    Value added at basic prices (VAB)   IDA     
    ## 238 Value added at producer prices (VAP)   IDA     
    ## 239    Value added at basic prices (VAB)  IBRD     
    ## 240    Value added at basic prices (VAB)   IDA HIPC
    ## 241    Value added at basic prices (VAB)   IDA HIPC
    ## 242    Value added at basic prices (VAB) Blend     
    ##                                                                V16
    ## 237 Country uses the 1993 System of National Accounts methodology.
    ## 238 Country uses the 1993 System of National Accounts methodology.
    ## 239 Country uses the 1993 System of National Accounts methodology.
    ## 240 Country uses the 1993 System of National Accounts methodology.
    ## 241 Country uses the 1968 System of National Accounts methodology.
    ## 242 Country uses the 1993 System of National Accounts methodology.
    ##            V17  V18                                          V19
    ## 237                 IMF Balance of Payments Manual, 6th edition.
    ## 238    1990<U+0096>96 2005 IMF Balance of Payments Manual, 6th edition.
    ## 239            2005 IMF Balance of Payments Manual, 6th edition.
    ## 240  1999<U+0096>2001 2005 IMF Balance of Payments Manual, 6th edition.
    ## 241    1990<U+0096>92 2005 IMF Balance of Payments Manual, 6th edition.
    ## 242 1991, 1998 2005 IMF Balance of Payments Manual, 6th edition.
    ##             V20                  V21                             V22
    ## 237      Actual Special trade system    Budgetary central government
    ## 238      Actual Special trade system    Budgetary central government
    ## 239 Preliminary General trade system Consolidated central government
    ## 240      Actual Special trade system Consolidated central government
    ## 241      Actual Special trade system    Budgetary central government
    ## 242      Actual General trade system Consolidated central government
    ##                                            V23  V24
    ## 237   General Data Dissemination System (GDDS) 2011
    ## 238   General Data Dissemination System (GDDS) 2004
    ## 239 Special Data Dissemination Standard (SDDS) 2011
    ## 240   General Data Dissemination System (GDDS) 1984
    ## 241   General Data Dissemination System (GDDS) 2010
    ## 242   General Data Dissemination System (GDDS) 2012
    ##                                                                            V25
    ## 237                                  Demographic and Health Survey (DHS), 2009
    ## 238                                  Demographic and Health Survey (DHS), 2013
    ## 239 Demographic and Health Survey (DHS), 2003; World Health Survey (WHS), 2003
    ## 240                                  Demographic and Health Survey (DHS), 2013
    ## 241                                  Demographic and Health Survey (DHS), 2013
    ## 242                               Demographic and Health Survey (DHS), 2010/11
    ##                                                V26 V27
    ## 237                                                   
    ## 238 Expenditure survey/budget survey (ES/BS), 2005    
    ## 239 Expenditure survey/budget survey (ES/BS), 2010    
    ## 240                  1-2-3 survey (1-2-3), 2004/05    
    ## 241        Integrated household survey (IHS), 2010    
    ## 242     Integrated household survey (IHS), 2011/12    
    ##                                      V28  V29  V30  V31
    ## 237                                 2009      2012     
    ## 238                                      2006 2012 2005
    ## 239                                 2007 2010 2012 2000
    ## 240                                                2005
    ## 241 2010. Population and Housing Census.      2011 2002
    ## 242                                           2012 2002

### Selecting the Right Columns and Rows for Country Data Dataset

There are 31 columns in the data set CountryData. Most do not have
significant value to the data being analyzed. Therefore, the code below
will select the pertinent columns to keep and create a new data set
CData.

    #This statement will only keep columns 1, 2, and 9 which are the Country Code, Short Name, Income Group.  New Data set create - CData
    CData <- CountryData[c(1,2,9)]

    #View dataset
    CData

    ##               V1                                             V2
    ## 1   Country Code                                     Short Name
    ## 2            ABW                                          Aruba
    ## 3            ADO                                        Andorra
    ## 4            AFG                                    Afghanistan
    ## 5            AGO                                         Angola
    ## 6            ALB                                        Albania
    ## 7            ARB                                     Arab World
    ## 8            ARE                           United Arab Emirates
    ## 9            ARG                                      Argentina
    ## 10           ARM                                        Armenia
    ## 11           ASM                                 American Samoa
    ## 12           ATG                            Antigua and Barbuda
    ## 13           AUS                                      Australia
    ## 14           AUT                                        Austria
    ## 15           AZE                                     Azerbaijan
    ## 16           BDI                                        Burundi
    ## 17           BEL                                        Belgium
    ## 18           BEN                                          Benin
    ## 19           BFA                                   Burkina Faso
    ## 20           BGD                                     Bangladesh
    ## 21           BGR                                       Bulgaria
    ## 22           BHR                                        Bahrain
    ## 23           BHS                                    The Bahamas
    ## 24           BIH                         Bosnia and Herzegovina
    ## 25           BLR                                        Belarus
    ## 26           BLZ                                         Belize
    ## 27           BMU                                        Bermuda
    ## 28           BOL                                        Bolivia
    ## 29           BRA                                         Brazil
    ## 30           BRB                                       Barbados
    ## 31           BRN                                         Brunei
    ## 32           BTN                                         Bhutan
    ## 33           BWA                                       Botswana
    ## 34           CAF                       Central African Republic
    ## 35           CAN                                         Canada
    ## 36           CHE                                    Switzerland
    ## 37           CHI                                Channel Islands
    ## 38           CHL                                          Chile
    ## 39           CHN                                          China
    ## 40           CIV                                  Côte d'Ivoire
    ## 41           CMR                                       Cameroon
    ## 42           COG                                          Congo
    ## 43           COL                                       Colombia
    ## 44           COM                                        Comoros
    ## 45           CPV                                     Cabo Verde
    ## 46           CRI                                     Costa Rica
    ## 47           CUB                                           Cuba
    ## 48           CUW                                        Curaçao
    ## 49           CYM                                 Cayman Islands
    ## 50           CYP                                         Cyprus
    ## 51           CZE                                 Czech Republic
    ## 52           DEU                                        Germany
    ## 53           DJI                                       Djibouti
    ## 54           DMA                                       Dominica
    ## 55           DNK                                        Denmark
    ## 56           DOM                             Dominican Republic
    ## 57           DZA                                        Algeria
    ## 58           EAP          East Asia & Pacific (developing only)
    ## 59           EAS        East Asia & Pacific (all income levels)
    ## 60           ECA        Europe & Central Asia (developing only)
    ## 61           ECS      Europe & Central Asia (all income levels)
    ## 62           ECU                                        Ecuador
    ## 63           EGY                                          Egypt
    ## 64           EMU                                      Euro area
    ## 65           ERI                                        Eritrea
    ## 66           ESP                                          Spain
    ## 67           EST                                        Estonia
    ## 68           ETH                                       Ethiopia
    ## 69           EUU                                 European Union
    ## 70           FIN                                        Finland
    ## 71           FJI                                           Fiji
    ## 72           FRA                                         France
    ## 73           FRO                                 Faeroe Islands
    ## 74           FSM                                     Micronesia
    ## 75           GAB                                          Gabon
    ## 76           GBR                                 United Kingdom
    ## 77           GEO                                        Georgia
    ## 78           GHA                                          Ghana
    ## 79           GIN                                         Guinea
    ## 80           GMB                                     The Gambia
    ## 81           GNB                                  Guinea-Bissau
    ## 82           GNQ                              Equatorial Guinea
    ## 83           GRC                                         Greece
    ## 84           GRD                                        Grenada
    ## 85           GRL                                      Greenland
    ## 86           GTM                                      Guatemala
    ## 87           GUM                                           Guam
    ## 88           GUY                                         Guyana
    ## 89           HIC                                    High income
    ## 90           HKG                           Hong Kong SAR, China
    ## 91           HND                                       Honduras
    ## 92           HPC         Heavily indebted poor countries (HIPC)
    ## 93           HRV                                        Croatia
    ## 94           HTI                                          Haiti
    ## 95           HUN                                        Hungary
    ## 96           IDN                                      Indonesia
    ## 97           IMY                                    Isle of Man
    ## 98           IND                                          India
    ## 99           IRL                                        Ireland
    ## 100          IRN                                           Iran
    ## 101          IRQ                                           Iraq
    ## 102          ISL                                        Iceland
    ## 103          ISR                                         Israel
    ## 104          ITA                                          Italy
    ## 105          JAM                                        Jamaica
    ## 106          JOR                                         Jordan
    ## 107          JPN                                          Japan
    ## 108          KAZ                                     Kazakhstan
    ## 109          KEN                                          Kenya
    ## 110          KGZ                                Kyrgyz Republic
    ## 111          KHM                                       Cambodia
    ## 112          KIR                                       Kiribati
    ## 113          KNA                            St. Kitts and Nevis
    ## 114          KOR                                          Korea
    ## 115          KSV                                         Kosovo
    ## 116          KWT                                         Kuwait
    ## 117          LAC    Latin America & Caribbean (developing only)
    ## 118          LAO                                        Lao PDR
    ## 119          LBN                                        Lebanon
    ## 120          LBR                                        Liberia
    ## 121          LBY                                          Libya
    ## 122          LCA                                      St. Lucia
    ## 123          LCN  Latin America & Caribbean (all income levels)
    ## 124          LDC   Least developed countries: UN classification
    ## 125          LIC                                     Low income
    ## 126          LIE                                  Liechtenstein
    ## 127          LKA                                      Sri Lanka
    ## 128          LMC                            Lower middle income
    ## 129          LMY                            Low & middle income
    ## 130          LSO                                        Lesotho
    ## 131          LTU                                      Lithuania
    ## 132          LUX                                     Luxembourg
    ## 133          LVA                                         Latvia
    ## 134          MAC                               Macao SAR, China
    ## 135          MAF                       St. Martin (French part)
    ## 136          MAR                                        Morocco
    ## 137          MCO                                         Monaco
    ## 138          MDA                                        Moldova
    ## 139          MDG                                     Madagascar
    ## 140          MDV                                       Maldives
    ## 141          MEA Middle East & North Africa (all income levels)
    ## 142          MEX                                         Mexico
    ## 143          MHL                               Marshall Islands
    ## 144          MIC                                  Middle income
    ## 145          MKD                                      Macedonia
    ## 146          MLI                                           Mali
    ## 147          MLT                                          Malta
    ## 148          MMR                                        Myanmar
    ## 149          MNA   Middle East & North Africa (developing only)
    ## 150          MNE                                     Montenegro
    ## 151          MNG                                       Mongolia
    ## 152          MNP                       Northern Mariana Islands
    ## 153          MOZ                                     Mozambique
    ## 154          MRT                                     Mauritania
    ## 155          MUS                                      Mauritius
    ## 156          MWI                                         Malawi
    ## 157          MYS                                       Malaysia
    ## 158          NAC                                  North America
    ## 159          NAM                                        Namibia
    ## 160          NCL                                  New Caledonia
    ## 161          NER                                          Niger
    ## 162          NGA                                        Nigeria
    ## 163          NIC                                      Nicaragua
    ## 164          NLD                                    Netherlands
    ## 165          NOC                           High income: nonOECD
    ## 166          NOR                                         Norway
    ## 167          NPL                                          Nepal
    ## 168          NZL                                    New Zealand
    ## 169          OEC                              High income: OECD
    ## 170          OED                                   OECD members
    ## 171          OMN                                           Oman
    ## 172          PAK                                       Pakistan
    ## 173          PAN                                         Panama
    ## 174          PER                                           Peru
    ## 175          PHL                                    Philippines
    ## 176          PLW                                          Palau
    ## 177          PNG                               Papua New Guinea
    ## 178          POL                                         Poland
    ## 179          PRI                                    Puerto Rico
    ## 180          PRK                       Dem. People's Rep. Korea
    ## 181          PRT                                       Portugal
    ## 182          PRY                                       Paraguay
    ## 183          PYF                               French Polynesia
    ## 184          QAT                                          Qatar
    ## 185          ROM                                        Romania
    ## 186          RUS                                         Russia
    ## 187          RWA                                         Rwanda
    ## 188          SAS                                     South Asia
    ## 189          SAU                                   Saudi Arabia
    ## 190          SDN                                          Sudan
    ## 191          SEN                                        Senegal
    ## 192          SGP                                      Singapore
    ## 193          SLB                                Solomon Islands
    ## 194          SLE                                   Sierra Leone
    ## 195          SLV                                    El Salvador
    ## 196          SMR                                     San Marino
    ## 197          SOM                                        Somalia
    ## 198          SRB                                         Serbia
    ## 199          SSA           Sub-Saharan Africa (developing only)
    ## 200          SSD                                    South Sudan
    ## 201          SSF         Sub-Saharan Africa (all income levels)
    ## 202          STP                          São Tomé and Principe
    ## 203          SUR                                       Suriname
    ## 204          SVK                                Slovak Republic
    ## 205          SVN                                       Slovenia
    ## 206          SWE                                         Sweden
    ## 207          SWZ                                      Swaziland
    ## 208          SXM                      Sint Maarten (Dutch part)
    ## 209          SYC                                     Seychelles
    ## 210          SYR                           Syrian Arab Republic
    ## 211          TCA                       Turks and Caicos Islands
    ## 212          TCD                                           Chad
    ## 213          TGO                                           Togo
    ## 214          THA                                       Thailand
    ## 215          TJK                                     Tajikistan
    ## 216          TKM                                   Turkmenistan
    ## 217          TMP                                    Timor-Leste
    ## 218          TON                                          Tonga
    ## 219          TTO                            Trinidad and Tobago
    ## 220          TUN                                        Tunisia
    ## 221          TUR                                         Turkey
    ## 222          TUV                                         Tuvalu
    ## 223          TZA                                       Tanzania
    ## 224          UGA                                         Uganda
    ## 225          UKR                                        Ukraine
    ## 226          UMC                            Upper middle income
    ## 227          URY                                        Uruguay
    ## 228          USA                                  United States
    ## 229          UZB                                     Uzbekistan
    ## 230          VCT                 St. Vincent and the Grenadines
    ## 231          VEN                                      Venezuela
    ## 232          VIR                                 Virgin Islands
    ## 233          VNM                                        Vietnam
    ## 234          VUT                                        Vanuatu
    ## 235          WBG                             West Bank and Gaza
    ## 236          WLD                                          World
    ## 237          WSM                                          Samoa
    ## 238          YEM                                          Yemen
    ## 239          ZAF                                   South Africa
    ## 240          ZAR                                Dem. Rep. Congo
    ## 241          ZMB                                         Zambia
    ## 242          ZWE                                       Zimbabwe
    ##                       V9
    ## 1           Income Group
    ## 2   High income: nonOECD
    ## 3   High income: nonOECD
    ## 4             Low income
    ## 5    Upper middle income
    ## 6    Upper middle income
    ## 7                       
    ## 8   High income: nonOECD
    ## 9    Upper middle income
    ## 10   Lower middle income
    ## 11   Upper middle income
    ## 12  High income: nonOECD
    ## 13     High income: OECD
    ## 14     High income: OECD
    ## 15   Upper middle income
    ## 16            Low income
    ## 17     High income: OECD
    ## 18            Low income
    ## 19            Low income
    ## 20            Low income
    ## 21   Upper middle income
    ## 22  High income: nonOECD
    ## 23  High income: nonOECD
    ## 24   Upper middle income
    ## 25   Upper middle income
    ## 26   Upper middle income
    ## 27  High income: nonOECD
    ## 28   Lower middle income
    ## 29   Upper middle income
    ## 30  High income: nonOECD
    ## 31  High income: nonOECD
    ## 32   Lower middle income
    ## 33   Upper middle income
    ## 34            Low income
    ## 35     High income: OECD
    ## 36     High income: OECD
    ## 37  High income: nonOECD
    ## 38     High income: OECD
    ## 39   Upper middle income
    ## 40   Lower middle income
    ## 41   Lower middle income
    ## 42   Lower middle income
    ## 43   Upper middle income
    ## 44            Low income
    ## 45   Lower middle income
    ## 46   Upper middle income
    ## 47   Upper middle income
    ## 48  High income: nonOECD
    ## 49  High income: nonOECD
    ## 50  High income: nonOECD
    ## 51     High income: OECD
    ## 52     High income: OECD
    ## 53   Lower middle income
    ## 54   Upper middle income
    ## 55     High income: OECD
    ## 56   Upper middle income
    ## 57   Upper middle income
    ## 58                      
    ## 59                      
    ## 60                      
    ## 61                      
    ## 62   Upper middle income
    ## 63   Lower middle income
    ## 64                      
    ## 65            Low income
    ## 66     High income: OECD
    ## 67     High income: OECD
    ## 68            Low income
    ## 69                      
    ## 70     High income: OECD
    ## 71   Upper middle income
    ## 72     High income: OECD
    ## 73  High income: nonOECD
    ## 74   Lower middle income
    ## 75   Upper middle income
    ## 76     High income: OECD
    ## 77   Lower middle income
    ## 78   Lower middle income
    ## 79            Low income
    ## 80            Low income
    ## 81            Low income
    ## 82  High income: nonOECD
    ## 83     High income: OECD
    ## 84   Upper middle income
    ## 85  High income: nonOECD
    ## 86   Lower middle income
    ## 87  High income: nonOECD
    ## 88   Lower middle income
    ## 89                      
    ## 90  High income: nonOECD
    ## 91   Lower middle income
    ## 92                      
    ## 93  High income: nonOECD
    ## 94            Low income
    ## 95   Upper middle income
    ## 96   Lower middle income
    ## 97  High income: nonOECD
    ## 98   Lower middle income
    ## 99     High income: OECD
    ## 100  Upper middle income
    ## 101  Upper middle income
    ## 102    High income: OECD
    ## 103    High income: OECD
    ## 104    High income: OECD
    ## 105  Upper middle income
    ## 106  Upper middle income
    ## 107    High income: OECD
    ## 108  Upper middle income
    ## 109           Low income
    ## 110  Lower middle income
    ## 111           Low income
    ## 112  Lower middle income
    ## 113 High income: nonOECD
    ## 114    High income: OECD
    ## 115  Lower middle income
    ## 116 High income: nonOECD
    ## 117                     
    ## 118  Lower middle income
    ## 119  Upper middle income
    ## 120           Low income
    ## 121  Upper middle income
    ## 122  Upper middle income
    ## 123                     
    ## 124                     
    ## 125                     
    ## 126 High income: nonOECD
    ## 127  Lower middle income
    ## 128                     
    ## 129                     
    ## 130  Lower middle income
    ## 131 High income: nonOECD
    ## 132    High income: OECD
    ## 133 High income: nonOECD
    ## 134 High income: nonOECD
    ## 135 High income: nonOECD
    ## 136  Lower middle income
    ## 137 High income: nonOECD
    ## 138  Lower middle income
    ## 139           Low income
    ## 140  Upper middle income
    ## 141                     
    ## 142  Upper middle income
    ## 143  Upper middle income
    ## 144                     
    ## 145  Upper middle income
    ## 146           Low income
    ## 147 High income: nonOECD
    ## 148           Low income
    ## 149                     
    ## 150  Upper middle income
    ## 151  Lower middle income
    ## 152 High income: nonOECD
    ## 153           Low income
    ## 154  Lower middle income
    ## 155  Upper middle income
    ## 156           Low income
    ## 157  Upper middle income
    ## 158                     
    ## 159  Upper middle income
    ## 160 High income: nonOECD
    ## 161           Low income
    ## 162  Lower middle income
    ## 163  Lower middle income
    ## 164    High income: OECD
    ## 165                     
    ## 166    High income: OECD
    ## 167           Low income
    ## 168    High income: OECD
    ## 169                     
    ## 170                     
    ## 171 High income: nonOECD
    ## 172  Lower middle income
    ## 173  Upper middle income
    ## 174  Upper middle income
    ## 175  Lower middle income
    ## 176  Upper middle income
    ## 177  Lower middle income
    ## 178    High income: OECD
    ## 179 High income: nonOECD
    ## 180           Low income
    ## 181    High income: OECD
    ## 182  Lower middle income
    ## 183 High income: nonOECD
    ## 184 High income: nonOECD
    ## 185  Upper middle income
    ## 186 High income: nonOECD
    ## 187           Low income
    ## 188                     
    ## 189 High income: nonOECD
    ## 190  Lower middle income
    ## 191  Lower middle income
    ## 192 High income: nonOECD
    ## 193  Lower middle income
    ## 194           Low income
    ## 195  Lower middle income
    ## 196 High income: nonOECD
    ## 197           Low income
    ## 198  Upper middle income
    ## 199                     
    ## 200  Lower middle income
    ## 201                     
    ## 202  Lower middle income
    ## 203  Upper middle income
    ## 204    High income: OECD
    ## 205    High income: OECD
    ## 206    High income: OECD
    ## 207  Lower middle income
    ## 208 High income: nonOECD
    ## 209  Upper middle income
    ## 210  Lower middle income
    ## 211 High income: nonOECD
    ## 212           Low income
    ## 213           Low income
    ## 214  Upper middle income
    ## 215           Low income
    ## 216  Upper middle income
    ## 217  Lower middle income
    ## 218  Upper middle income
    ## 219 High income: nonOECD
    ## 220  Upper middle income
    ## 221  Upper middle income
    ## 222  Upper middle income
    ## 223           Low income
    ## 224           Low income
    ## 225  Lower middle income
    ## 226                     
    ## 227 High income: nonOECD
    ## 228    High income: OECD
    ## 229  Lower middle income
    ## 230  Upper middle income
    ## 231  Upper middle income
    ## 232 High income: nonOECD
    ## 233  Lower middle income
    ## 234  Lower middle income
    ## 235  Lower middle income
    ## 236                     
    ## 237  Lower middle income
    ## 238  Lower middle income
    ## 239  Upper middle income
    ## 240           Low income
    ## 241  Lower middle income
    ## 242           Low income

The first row of the data set CData contains the variable names. The
code below will remove them from the data set.

    #Remove row 1 from the dataset CData
    CData <- CData[-c(1), ]

    #View Dataset
    CData

    ##      V1                                             V2
    ## 2   ABW                                          Aruba
    ## 3   ADO                                        Andorra
    ## 4   AFG                                    Afghanistan
    ## 5   AGO                                         Angola
    ## 6   ALB                                        Albania
    ## 7   ARB                                     Arab World
    ## 8   ARE                           United Arab Emirates
    ## 9   ARG                                      Argentina
    ## 10  ARM                                        Armenia
    ## 11  ASM                                 American Samoa
    ## 12  ATG                            Antigua and Barbuda
    ## 13  AUS                                      Australia
    ## 14  AUT                                        Austria
    ## 15  AZE                                     Azerbaijan
    ## 16  BDI                                        Burundi
    ## 17  BEL                                        Belgium
    ## 18  BEN                                          Benin
    ## 19  BFA                                   Burkina Faso
    ## 20  BGD                                     Bangladesh
    ## 21  BGR                                       Bulgaria
    ## 22  BHR                                        Bahrain
    ## 23  BHS                                    The Bahamas
    ## 24  BIH                         Bosnia and Herzegovina
    ## 25  BLR                                        Belarus
    ## 26  BLZ                                         Belize
    ## 27  BMU                                        Bermuda
    ## 28  BOL                                        Bolivia
    ## 29  BRA                                         Brazil
    ## 30  BRB                                       Barbados
    ## 31  BRN                                         Brunei
    ## 32  BTN                                         Bhutan
    ## 33  BWA                                       Botswana
    ## 34  CAF                       Central African Republic
    ## 35  CAN                                         Canada
    ## 36  CHE                                    Switzerland
    ## 37  CHI                                Channel Islands
    ## 38  CHL                                          Chile
    ## 39  CHN                                          China
    ## 40  CIV                                  Côte d'Ivoire
    ## 41  CMR                                       Cameroon
    ## 42  COG                                          Congo
    ## 43  COL                                       Colombia
    ## 44  COM                                        Comoros
    ## 45  CPV                                     Cabo Verde
    ## 46  CRI                                     Costa Rica
    ## 47  CUB                                           Cuba
    ## 48  CUW                                        Curaçao
    ## 49  CYM                                 Cayman Islands
    ## 50  CYP                                         Cyprus
    ## 51  CZE                                 Czech Republic
    ## 52  DEU                                        Germany
    ## 53  DJI                                       Djibouti
    ## 54  DMA                                       Dominica
    ## 55  DNK                                        Denmark
    ## 56  DOM                             Dominican Republic
    ## 57  DZA                                        Algeria
    ## 58  EAP          East Asia & Pacific (developing only)
    ## 59  EAS        East Asia & Pacific (all income levels)
    ## 60  ECA        Europe & Central Asia (developing only)
    ## 61  ECS      Europe & Central Asia (all income levels)
    ## 62  ECU                                        Ecuador
    ## 63  EGY                                          Egypt
    ## 64  EMU                                      Euro area
    ## 65  ERI                                        Eritrea
    ## 66  ESP                                          Spain
    ## 67  EST                                        Estonia
    ## 68  ETH                                       Ethiopia
    ## 69  EUU                                 European Union
    ## 70  FIN                                        Finland
    ## 71  FJI                                           Fiji
    ## 72  FRA                                         France
    ## 73  FRO                                 Faeroe Islands
    ## 74  FSM                                     Micronesia
    ## 75  GAB                                          Gabon
    ## 76  GBR                                 United Kingdom
    ## 77  GEO                                        Georgia
    ## 78  GHA                                          Ghana
    ## 79  GIN                                         Guinea
    ## 80  GMB                                     The Gambia
    ## 81  GNB                                  Guinea-Bissau
    ## 82  GNQ                              Equatorial Guinea
    ## 83  GRC                                         Greece
    ## 84  GRD                                        Grenada
    ## 85  GRL                                      Greenland
    ## 86  GTM                                      Guatemala
    ## 87  GUM                                           Guam
    ## 88  GUY                                         Guyana
    ## 89  HIC                                    High income
    ## 90  HKG                           Hong Kong SAR, China
    ## 91  HND                                       Honduras
    ## 92  HPC         Heavily indebted poor countries (HIPC)
    ## 93  HRV                                        Croatia
    ## 94  HTI                                          Haiti
    ## 95  HUN                                        Hungary
    ## 96  IDN                                      Indonesia
    ## 97  IMY                                    Isle of Man
    ## 98  IND                                          India
    ## 99  IRL                                        Ireland
    ## 100 IRN                                           Iran
    ## 101 IRQ                                           Iraq
    ## 102 ISL                                        Iceland
    ## 103 ISR                                         Israel
    ## 104 ITA                                          Italy
    ## 105 JAM                                        Jamaica
    ## 106 JOR                                         Jordan
    ## 107 JPN                                          Japan
    ## 108 KAZ                                     Kazakhstan
    ## 109 KEN                                          Kenya
    ## 110 KGZ                                Kyrgyz Republic
    ## 111 KHM                                       Cambodia
    ## 112 KIR                                       Kiribati
    ## 113 KNA                            St. Kitts and Nevis
    ## 114 KOR                                          Korea
    ## 115 KSV                                         Kosovo
    ## 116 KWT                                         Kuwait
    ## 117 LAC    Latin America & Caribbean (developing only)
    ## 118 LAO                                        Lao PDR
    ## 119 LBN                                        Lebanon
    ## 120 LBR                                        Liberia
    ## 121 LBY                                          Libya
    ## 122 LCA                                      St. Lucia
    ## 123 LCN  Latin America & Caribbean (all income levels)
    ## 124 LDC   Least developed countries: UN classification
    ## 125 LIC                                     Low income
    ## 126 LIE                                  Liechtenstein
    ## 127 LKA                                      Sri Lanka
    ## 128 LMC                            Lower middle income
    ## 129 LMY                            Low & middle income
    ## 130 LSO                                        Lesotho
    ## 131 LTU                                      Lithuania
    ## 132 LUX                                     Luxembourg
    ## 133 LVA                                         Latvia
    ## 134 MAC                               Macao SAR, China
    ## 135 MAF                       St. Martin (French part)
    ## 136 MAR                                        Morocco
    ## 137 MCO                                         Monaco
    ## 138 MDA                                        Moldova
    ## 139 MDG                                     Madagascar
    ## 140 MDV                                       Maldives
    ## 141 MEA Middle East & North Africa (all income levels)
    ## 142 MEX                                         Mexico
    ## 143 MHL                               Marshall Islands
    ## 144 MIC                                  Middle income
    ## 145 MKD                                      Macedonia
    ## 146 MLI                                           Mali
    ## 147 MLT                                          Malta
    ## 148 MMR                                        Myanmar
    ## 149 MNA   Middle East & North Africa (developing only)
    ## 150 MNE                                     Montenegro
    ## 151 MNG                                       Mongolia
    ## 152 MNP                       Northern Mariana Islands
    ## 153 MOZ                                     Mozambique
    ## 154 MRT                                     Mauritania
    ## 155 MUS                                      Mauritius
    ## 156 MWI                                         Malawi
    ## 157 MYS                                       Malaysia
    ## 158 NAC                                  North America
    ## 159 NAM                                        Namibia
    ## 160 NCL                                  New Caledonia
    ## 161 NER                                          Niger
    ## 162 NGA                                        Nigeria
    ## 163 NIC                                      Nicaragua
    ## 164 NLD                                    Netherlands
    ## 165 NOC                           High income: nonOECD
    ## 166 NOR                                         Norway
    ## 167 NPL                                          Nepal
    ## 168 NZL                                    New Zealand
    ## 169 OEC                              High income: OECD
    ## 170 OED                                   OECD members
    ## 171 OMN                                           Oman
    ## 172 PAK                                       Pakistan
    ## 173 PAN                                         Panama
    ## 174 PER                                           Peru
    ## 175 PHL                                    Philippines
    ## 176 PLW                                          Palau
    ## 177 PNG                               Papua New Guinea
    ## 178 POL                                         Poland
    ## 179 PRI                                    Puerto Rico
    ## 180 PRK                       Dem. People's Rep. Korea
    ## 181 PRT                                       Portugal
    ## 182 PRY                                       Paraguay
    ## 183 PYF                               French Polynesia
    ## 184 QAT                                          Qatar
    ## 185 ROM                                        Romania
    ## 186 RUS                                         Russia
    ## 187 RWA                                         Rwanda
    ## 188 SAS                                     South Asia
    ## 189 SAU                                   Saudi Arabia
    ## 190 SDN                                          Sudan
    ## 191 SEN                                        Senegal
    ## 192 SGP                                      Singapore
    ## 193 SLB                                Solomon Islands
    ## 194 SLE                                   Sierra Leone
    ## 195 SLV                                    El Salvador
    ## 196 SMR                                     San Marino
    ## 197 SOM                                        Somalia
    ## 198 SRB                                         Serbia
    ## 199 SSA           Sub-Saharan Africa (developing only)
    ## 200 SSD                                    South Sudan
    ## 201 SSF         Sub-Saharan Africa (all income levels)
    ## 202 STP                          São Tomé and Principe
    ## 203 SUR                                       Suriname
    ## 204 SVK                                Slovak Republic
    ## 205 SVN                                       Slovenia
    ## 206 SWE                                         Sweden
    ## 207 SWZ                                      Swaziland
    ## 208 SXM                      Sint Maarten (Dutch part)
    ## 209 SYC                                     Seychelles
    ## 210 SYR                           Syrian Arab Republic
    ## 211 TCA                       Turks and Caicos Islands
    ## 212 TCD                                           Chad
    ## 213 TGO                                           Togo
    ## 214 THA                                       Thailand
    ## 215 TJK                                     Tajikistan
    ## 216 TKM                                   Turkmenistan
    ## 217 TMP                                    Timor-Leste
    ## 218 TON                                          Tonga
    ## 219 TTO                            Trinidad and Tobago
    ## 220 TUN                                        Tunisia
    ## 221 TUR                                         Turkey
    ## 222 TUV                                         Tuvalu
    ## 223 TZA                                       Tanzania
    ## 224 UGA                                         Uganda
    ## 225 UKR                                        Ukraine
    ## 226 UMC                            Upper middle income
    ## 227 URY                                        Uruguay
    ## 228 USA                                  United States
    ## 229 UZB                                     Uzbekistan
    ## 230 VCT                 St. Vincent and the Grenadines
    ## 231 VEN                                      Venezuela
    ## 232 VIR                                 Virgin Islands
    ## 233 VNM                                        Vietnam
    ## 234 VUT                                        Vanuatu
    ## 235 WBG                             West Bank and Gaza
    ## 236 WLD                                          World
    ## 237 WSM                                          Samoa
    ## 238 YEM                                          Yemen
    ## 239 ZAF                                   South Africa
    ## 240 ZAR                                Dem. Rep. Congo
    ## 241 ZMB                                         Zambia
    ## 242 ZWE                                       Zimbabwe
    ##                       V9
    ## 2   High income: nonOECD
    ## 3   High income: nonOECD
    ## 4             Low income
    ## 5    Upper middle income
    ## 6    Upper middle income
    ## 7                       
    ## 8   High income: nonOECD
    ## 9    Upper middle income
    ## 10   Lower middle income
    ## 11   Upper middle income
    ## 12  High income: nonOECD
    ## 13     High income: OECD
    ## 14     High income: OECD
    ## 15   Upper middle income
    ## 16            Low income
    ## 17     High income: OECD
    ## 18            Low income
    ## 19            Low income
    ## 20            Low income
    ## 21   Upper middle income
    ## 22  High income: nonOECD
    ## 23  High income: nonOECD
    ## 24   Upper middle income
    ## 25   Upper middle income
    ## 26   Upper middle income
    ## 27  High income: nonOECD
    ## 28   Lower middle income
    ## 29   Upper middle income
    ## 30  High income: nonOECD
    ## 31  High income: nonOECD
    ## 32   Lower middle income
    ## 33   Upper middle income
    ## 34            Low income
    ## 35     High income: OECD
    ## 36     High income: OECD
    ## 37  High income: nonOECD
    ## 38     High income: OECD
    ## 39   Upper middle income
    ## 40   Lower middle income
    ## 41   Lower middle income
    ## 42   Lower middle income
    ## 43   Upper middle income
    ## 44            Low income
    ## 45   Lower middle income
    ## 46   Upper middle income
    ## 47   Upper middle income
    ## 48  High income: nonOECD
    ## 49  High income: nonOECD
    ## 50  High income: nonOECD
    ## 51     High income: OECD
    ## 52     High income: OECD
    ## 53   Lower middle income
    ## 54   Upper middle income
    ## 55     High income: OECD
    ## 56   Upper middle income
    ## 57   Upper middle income
    ## 58                      
    ## 59                      
    ## 60                      
    ## 61                      
    ## 62   Upper middle income
    ## 63   Lower middle income
    ## 64                      
    ## 65            Low income
    ## 66     High income: OECD
    ## 67     High income: OECD
    ## 68            Low income
    ## 69                      
    ## 70     High income: OECD
    ## 71   Upper middle income
    ## 72     High income: OECD
    ## 73  High income: nonOECD
    ## 74   Lower middle income
    ## 75   Upper middle income
    ## 76     High income: OECD
    ## 77   Lower middle income
    ## 78   Lower middle income
    ## 79            Low income
    ## 80            Low income
    ## 81            Low income
    ## 82  High income: nonOECD
    ## 83     High income: OECD
    ## 84   Upper middle income
    ## 85  High income: nonOECD
    ## 86   Lower middle income
    ## 87  High income: nonOECD
    ## 88   Lower middle income
    ## 89                      
    ## 90  High income: nonOECD
    ## 91   Lower middle income
    ## 92                      
    ## 93  High income: nonOECD
    ## 94            Low income
    ## 95   Upper middle income
    ## 96   Lower middle income
    ## 97  High income: nonOECD
    ## 98   Lower middle income
    ## 99     High income: OECD
    ## 100  Upper middle income
    ## 101  Upper middle income
    ## 102    High income: OECD
    ## 103    High income: OECD
    ## 104    High income: OECD
    ## 105  Upper middle income
    ## 106  Upper middle income
    ## 107    High income: OECD
    ## 108  Upper middle income
    ## 109           Low income
    ## 110  Lower middle income
    ## 111           Low income
    ## 112  Lower middle income
    ## 113 High income: nonOECD
    ## 114    High income: OECD
    ## 115  Lower middle income
    ## 116 High income: nonOECD
    ## 117                     
    ## 118  Lower middle income
    ## 119  Upper middle income
    ## 120           Low income
    ## 121  Upper middle income
    ## 122  Upper middle income
    ## 123                     
    ## 124                     
    ## 125                     
    ## 126 High income: nonOECD
    ## 127  Lower middle income
    ## 128                     
    ## 129                     
    ## 130  Lower middle income
    ## 131 High income: nonOECD
    ## 132    High income: OECD
    ## 133 High income: nonOECD
    ## 134 High income: nonOECD
    ## 135 High income: nonOECD
    ## 136  Lower middle income
    ## 137 High income: nonOECD
    ## 138  Lower middle income
    ## 139           Low income
    ## 140  Upper middle income
    ## 141                     
    ## 142  Upper middle income
    ## 143  Upper middle income
    ## 144                     
    ## 145  Upper middle income
    ## 146           Low income
    ## 147 High income: nonOECD
    ## 148           Low income
    ## 149                     
    ## 150  Upper middle income
    ## 151  Lower middle income
    ## 152 High income: nonOECD
    ## 153           Low income
    ## 154  Lower middle income
    ## 155  Upper middle income
    ## 156           Low income
    ## 157  Upper middle income
    ## 158                     
    ## 159  Upper middle income
    ## 160 High income: nonOECD
    ## 161           Low income
    ## 162  Lower middle income
    ## 163  Lower middle income
    ## 164    High income: OECD
    ## 165                     
    ## 166    High income: OECD
    ## 167           Low income
    ## 168    High income: OECD
    ## 169                     
    ## 170                     
    ## 171 High income: nonOECD
    ## 172  Lower middle income
    ## 173  Upper middle income
    ## 174  Upper middle income
    ## 175  Lower middle income
    ## 176  Upper middle income
    ## 177  Lower middle income
    ## 178    High income: OECD
    ## 179 High income: nonOECD
    ## 180           Low income
    ## 181    High income: OECD
    ## 182  Lower middle income
    ## 183 High income: nonOECD
    ## 184 High income: nonOECD
    ## 185  Upper middle income
    ## 186 High income: nonOECD
    ## 187           Low income
    ## 188                     
    ## 189 High income: nonOECD
    ## 190  Lower middle income
    ## 191  Lower middle income
    ## 192 High income: nonOECD
    ## 193  Lower middle income
    ## 194           Low income
    ## 195  Lower middle income
    ## 196 High income: nonOECD
    ## 197           Low income
    ## 198  Upper middle income
    ## 199                     
    ## 200  Lower middle income
    ## 201                     
    ## 202  Lower middle income
    ## 203  Upper middle income
    ## 204    High income: OECD
    ## 205    High income: OECD
    ## 206    High income: OECD
    ## 207  Lower middle income
    ## 208 High income: nonOECD
    ## 209  Upper middle income
    ## 210  Lower middle income
    ## 211 High income: nonOECD
    ## 212           Low income
    ## 213           Low income
    ## 214  Upper middle income
    ## 215           Low income
    ## 216  Upper middle income
    ## 217  Lower middle income
    ## 218  Upper middle income
    ## 219 High income: nonOECD
    ## 220  Upper middle income
    ## 221  Upper middle income
    ## 222  Upper middle income
    ## 223           Low income
    ## 224           Low income
    ## 225  Lower middle income
    ## 226                     
    ## 227 High income: nonOECD
    ## 228    High income: OECD
    ## 229  Lower middle income
    ## 230  Upper middle income
    ## 231  Upper middle income
    ## 232 High income: nonOECD
    ## 233  Lower middle income
    ## 234  Lower middle income
    ## 235  Lower middle income
    ## 236                     
    ## 237  Lower middle income
    ## 238  Lower middle income
    ## 239  Upper middle income
    ## 240           Low income
    ## 241  Lower middle income
    ## 242           Low income

Names are added to each variable are added to the data set. The industry
standard of capitalizing each word was applied. The variable names are
human readable and self-explanatory.

    #Add Names to the variables in the data set
    names(CData) <- c("CountryCode","Country", "IncomeGroup")

    #view dataset
    CData

    ##     CountryCode                                        Country
    ## 2           ABW                                          Aruba
    ## 3           ADO                                        Andorra
    ## 4           AFG                                    Afghanistan
    ## 5           AGO                                         Angola
    ## 6           ALB                                        Albania
    ## 7           ARB                                     Arab World
    ## 8           ARE                           United Arab Emirates
    ## 9           ARG                                      Argentina
    ## 10          ARM                                        Armenia
    ## 11          ASM                                 American Samoa
    ## 12          ATG                            Antigua and Barbuda
    ## 13          AUS                                      Australia
    ## 14          AUT                                        Austria
    ## 15          AZE                                     Azerbaijan
    ## 16          BDI                                        Burundi
    ## 17          BEL                                        Belgium
    ## 18          BEN                                          Benin
    ## 19          BFA                                   Burkina Faso
    ## 20          BGD                                     Bangladesh
    ## 21          BGR                                       Bulgaria
    ## 22          BHR                                        Bahrain
    ## 23          BHS                                    The Bahamas
    ## 24          BIH                         Bosnia and Herzegovina
    ## 25          BLR                                        Belarus
    ## 26          BLZ                                         Belize
    ## 27          BMU                                        Bermuda
    ## 28          BOL                                        Bolivia
    ## 29          BRA                                         Brazil
    ## 30          BRB                                       Barbados
    ## 31          BRN                                         Brunei
    ## 32          BTN                                         Bhutan
    ## 33          BWA                                       Botswana
    ## 34          CAF                       Central African Republic
    ## 35          CAN                                         Canada
    ## 36          CHE                                    Switzerland
    ## 37          CHI                                Channel Islands
    ## 38          CHL                                          Chile
    ## 39          CHN                                          China
    ## 40          CIV                                  Côte d'Ivoire
    ## 41          CMR                                       Cameroon
    ## 42          COG                                          Congo
    ## 43          COL                                       Colombia
    ## 44          COM                                        Comoros
    ## 45          CPV                                     Cabo Verde
    ## 46          CRI                                     Costa Rica
    ## 47          CUB                                           Cuba
    ## 48          CUW                                        Curaçao
    ## 49          CYM                                 Cayman Islands
    ## 50          CYP                                         Cyprus
    ## 51          CZE                                 Czech Republic
    ## 52          DEU                                        Germany
    ## 53          DJI                                       Djibouti
    ## 54          DMA                                       Dominica
    ## 55          DNK                                        Denmark
    ## 56          DOM                             Dominican Republic
    ## 57          DZA                                        Algeria
    ## 58          EAP          East Asia & Pacific (developing only)
    ## 59          EAS        East Asia & Pacific (all income levels)
    ## 60          ECA        Europe & Central Asia (developing only)
    ## 61          ECS      Europe & Central Asia (all income levels)
    ## 62          ECU                                        Ecuador
    ## 63          EGY                                          Egypt
    ## 64          EMU                                      Euro area
    ## 65          ERI                                        Eritrea
    ## 66          ESP                                          Spain
    ## 67          EST                                        Estonia
    ## 68          ETH                                       Ethiopia
    ## 69          EUU                                 European Union
    ## 70          FIN                                        Finland
    ## 71          FJI                                           Fiji
    ## 72          FRA                                         France
    ## 73          FRO                                 Faeroe Islands
    ## 74          FSM                                     Micronesia
    ## 75          GAB                                          Gabon
    ## 76          GBR                                 United Kingdom
    ## 77          GEO                                        Georgia
    ## 78          GHA                                          Ghana
    ## 79          GIN                                         Guinea
    ## 80          GMB                                     The Gambia
    ## 81          GNB                                  Guinea-Bissau
    ## 82          GNQ                              Equatorial Guinea
    ## 83          GRC                                         Greece
    ## 84          GRD                                        Grenada
    ## 85          GRL                                      Greenland
    ## 86          GTM                                      Guatemala
    ## 87          GUM                                           Guam
    ## 88          GUY                                         Guyana
    ## 89          HIC                                    High income
    ## 90          HKG                           Hong Kong SAR, China
    ## 91          HND                                       Honduras
    ## 92          HPC         Heavily indebted poor countries (HIPC)
    ## 93          HRV                                        Croatia
    ## 94          HTI                                          Haiti
    ## 95          HUN                                        Hungary
    ## 96          IDN                                      Indonesia
    ## 97          IMY                                    Isle of Man
    ## 98          IND                                          India
    ## 99          IRL                                        Ireland
    ## 100         IRN                                           Iran
    ## 101         IRQ                                           Iraq
    ## 102         ISL                                        Iceland
    ## 103         ISR                                         Israel
    ## 104         ITA                                          Italy
    ## 105         JAM                                        Jamaica
    ## 106         JOR                                         Jordan
    ## 107         JPN                                          Japan
    ## 108         KAZ                                     Kazakhstan
    ## 109         KEN                                          Kenya
    ## 110         KGZ                                Kyrgyz Republic
    ## 111         KHM                                       Cambodia
    ## 112         KIR                                       Kiribati
    ## 113         KNA                            St. Kitts and Nevis
    ## 114         KOR                                          Korea
    ## 115         KSV                                         Kosovo
    ## 116         KWT                                         Kuwait
    ## 117         LAC    Latin America & Caribbean (developing only)
    ## 118         LAO                                        Lao PDR
    ## 119         LBN                                        Lebanon
    ## 120         LBR                                        Liberia
    ## 121         LBY                                          Libya
    ## 122         LCA                                      St. Lucia
    ## 123         LCN  Latin America & Caribbean (all income levels)
    ## 124         LDC   Least developed countries: UN classification
    ## 125         LIC                                     Low income
    ## 126         LIE                                  Liechtenstein
    ## 127         LKA                                      Sri Lanka
    ## 128         LMC                            Lower middle income
    ## 129         LMY                            Low & middle income
    ## 130         LSO                                        Lesotho
    ## 131         LTU                                      Lithuania
    ## 132         LUX                                     Luxembourg
    ## 133         LVA                                         Latvia
    ## 134         MAC                               Macao SAR, China
    ## 135         MAF                       St. Martin (French part)
    ## 136         MAR                                        Morocco
    ## 137         MCO                                         Monaco
    ## 138         MDA                                        Moldova
    ## 139         MDG                                     Madagascar
    ## 140         MDV                                       Maldives
    ## 141         MEA Middle East & North Africa (all income levels)
    ## 142         MEX                                         Mexico
    ## 143         MHL                               Marshall Islands
    ## 144         MIC                                  Middle income
    ## 145         MKD                                      Macedonia
    ## 146         MLI                                           Mali
    ## 147         MLT                                          Malta
    ## 148         MMR                                        Myanmar
    ## 149         MNA   Middle East & North Africa (developing only)
    ## 150         MNE                                     Montenegro
    ## 151         MNG                                       Mongolia
    ## 152         MNP                       Northern Mariana Islands
    ## 153         MOZ                                     Mozambique
    ## 154         MRT                                     Mauritania
    ## 155         MUS                                      Mauritius
    ## 156         MWI                                         Malawi
    ## 157         MYS                                       Malaysia
    ## 158         NAC                                  North America
    ## 159         NAM                                        Namibia
    ## 160         NCL                                  New Caledonia
    ## 161         NER                                          Niger
    ## 162         NGA                                        Nigeria
    ## 163         NIC                                      Nicaragua
    ## 164         NLD                                    Netherlands
    ## 165         NOC                           High income: nonOECD
    ## 166         NOR                                         Norway
    ## 167         NPL                                          Nepal
    ## 168         NZL                                    New Zealand
    ## 169         OEC                              High income: OECD
    ## 170         OED                                   OECD members
    ## 171         OMN                                           Oman
    ## 172         PAK                                       Pakistan
    ## 173         PAN                                         Panama
    ## 174         PER                                           Peru
    ## 175         PHL                                    Philippines
    ## 176         PLW                                          Palau
    ## 177         PNG                               Papua New Guinea
    ## 178         POL                                         Poland
    ## 179         PRI                                    Puerto Rico
    ## 180         PRK                       Dem. People's Rep. Korea
    ## 181         PRT                                       Portugal
    ## 182         PRY                                       Paraguay
    ## 183         PYF                               French Polynesia
    ## 184         QAT                                          Qatar
    ## 185         ROM                                        Romania
    ## 186         RUS                                         Russia
    ## 187         RWA                                         Rwanda
    ## 188         SAS                                     South Asia
    ## 189         SAU                                   Saudi Arabia
    ## 190         SDN                                          Sudan
    ## 191         SEN                                        Senegal
    ## 192         SGP                                      Singapore
    ## 193         SLB                                Solomon Islands
    ## 194         SLE                                   Sierra Leone
    ## 195         SLV                                    El Salvador
    ## 196         SMR                                     San Marino
    ## 197         SOM                                        Somalia
    ## 198         SRB                                         Serbia
    ## 199         SSA           Sub-Saharan Africa (developing only)
    ## 200         SSD                                    South Sudan
    ## 201         SSF         Sub-Saharan Africa (all income levels)
    ## 202         STP                          São Tomé and Principe
    ## 203         SUR                                       Suriname
    ## 204         SVK                                Slovak Republic
    ## 205         SVN                                       Slovenia
    ## 206         SWE                                         Sweden
    ## 207         SWZ                                      Swaziland
    ## 208         SXM                      Sint Maarten (Dutch part)
    ## 209         SYC                                     Seychelles
    ## 210         SYR                           Syrian Arab Republic
    ## 211         TCA                       Turks and Caicos Islands
    ## 212         TCD                                           Chad
    ## 213         TGO                                           Togo
    ## 214         THA                                       Thailand
    ## 215         TJK                                     Tajikistan
    ## 216         TKM                                   Turkmenistan
    ## 217         TMP                                    Timor-Leste
    ## 218         TON                                          Tonga
    ## 219         TTO                            Trinidad and Tobago
    ## 220         TUN                                        Tunisia
    ## 221         TUR                                         Turkey
    ## 222         TUV                                         Tuvalu
    ## 223         TZA                                       Tanzania
    ## 224         UGA                                         Uganda
    ## 225         UKR                                        Ukraine
    ## 226         UMC                            Upper middle income
    ## 227         URY                                        Uruguay
    ## 228         USA                                  United States
    ## 229         UZB                                     Uzbekistan
    ## 230         VCT                 St. Vincent and the Grenadines
    ## 231         VEN                                      Venezuela
    ## 232         VIR                                 Virgin Islands
    ## 233         VNM                                        Vietnam
    ## 234         VUT                                        Vanuatu
    ## 235         WBG                             West Bank and Gaza
    ## 236         WLD                                          World
    ## 237         WSM                                          Samoa
    ## 238         YEM                                          Yemen
    ## 239         ZAF                                   South Africa
    ## 240         ZAR                                Dem. Rep. Congo
    ## 241         ZMB                                         Zambia
    ## 242         ZWE                                       Zimbabwe
    ##              IncomeGroup
    ## 2   High income: nonOECD
    ## 3   High income: nonOECD
    ## 4             Low income
    ## 5    Upper middle income
    ## 6    Upper middle income
    ## 7                       
    ## 8   High income: nonOECD
    ## 9    Upper middle income
    ## 10   Lower middle income
    ## 11   Upper middle income
    ## 12  High income: nonOECD
    ## 13     High income: OECD
    ## 14     High income: OECD
    ## 15   Upper middle income
    ## 16            Low income
    ## 17     High income: OECD
    ## 18            Low income
    ## 19            Low income
    ## 20            Low income
    ## 21   Upper middle income
    ## 22  High income: nonOECD
    ## 23  High income: nonOECD
    ## 24   Upper middle income
    ## 25   Upper middle income
    ## 26   Upper middle income
    ## 27  High income: nonOECD
    ## 28   Lower middle income
    ## 29   Upper middle income
    ## 30  High income: nonOECD
    ## 31  High income: nonOECD
    ## 32   Lower middle income
    ## 33   Upper middle income
    ## 34            Low income
    ## 35     High income: OECD
    ## 36     High income: OECD
    ## 37  High income: nonOECD
    ## 38     High income: OECD
    ## 39   Upper middle income
    ## 40   Lower middle income
    ## 41   Lower middle income
    ## 42   Lower middle income
    ## 43   Upper middle income
    ## 44            Low income
    ## 45   Lower middle income
    ## 46   Upper middle income
    ## 47   Upper middle income
    ## 48  High income: nonOECD
    ## 49  High income: nonOECD
    ## 50  High income: nonOECD
    ## 51     High income: OECD
    ## 52     High income: OECD
    ## 53   Lower middle income
    ## 54   Upper middle income
    ## 55     High income: OECD
    ## 56   Upper middle income
    ## 57   Upper middle income
    ## 58                      
    ## 59                      
    ## 60                      
    ## 61                      
    ## 62   Upper middle income
    ## 63   Lower middle income
    ## 64                      
    ## 65            Low income
    ## 66     High income: OECD
    ## 67     High income: OECD
    ## 68            Low income
    ## 69                      
    ## 70     High income: OECD
    ## 71   Upper middle income
    ## 72     High income: OECD
    ## 73  High income: nonOECD
    ## 74   Lower middle income
    ## 75   Upper middle income
    ## 76     High income: OECD
    ## 77   Lower middle income
    ## 78   Lower middle income
    ## 79            Low income
    ## 80            Low income
    ## 81            Low income
    ## 82  High income: nonOECD
    ## 83     High income: OECD
    ## 84   Upper middle income
    ## 85  High income: nonOECD
    ## 86   Lower middle income
    ## 87  High income: nonOECD
    ## 88   Lower middle income
    ## 89                      
    ## 90  High income: nonOECD
    ## 91   Lower middle income
    ## 92                      
    ## 93  High income: nonOECD
    ## 94            Low income
    ## 95   Upper middle income
    ## 96   Lower middle income
    ## 97  High income: nonOECD
    ## 98   Lower middle income
    ## 99     High income: OECD
    ## 100  Upper middle income
    ## 101  Upper middle income
    ## 102    High income: OECD
    ## 103    High income: OECD
    ## 104    High income: OECD
    ## 105  Upper middle income
    ## 106  Upper middle income
    ## 107    High income: OECD
    ## 108  Upper middle income
    ## 109           Low income
    ## 110  Lower middle income
    ## 111           Low income
    ## 112  Lower middle income
    ## 113 High income: nonOECD
    ## 114    High income: OECD
    ## 115  Lower middle income
    ## 116 High income: nonOECD
    ## 117                     
    ## 118  Lower middle income
    ## 119  Upper middle income
    ## 120           Low income
    ## 121  Upper middle income
    ## 122  Upper middle income
    ## 123                     
    ## 124                     
    ## 125                     
    ## 126 High income: nonOECD
    ## 127  Lower middle income
    ## 128                     
    ## 129                     
    ## 130  Lower middle income
    ## 131 High income: nonOECD
    ## 132    High income: OECD
    ## 133 High income: nonOECD
    ## 134 High income: nonOECD
    ## 135 High income: nonOECD
    ## 136  Lower middle income
    ## 137 High income: nonOECD
    ## 138  Lower middle income
    ## 139           Low income
    ## 140  Upper middle income
    ## 141                     
    ## 142  Upper middle income
    ## 143  Upper middle income
    ## 144                     
    ## 145  Upper middle income
    ## 146           Low income
    ## 147 High income: nonOECD
    ## 148           Low income
    ## 149                     
    ## 150  Upper middle income
    ## 151  Lower middle income
    ## 152 High income: nonOECD
    ## 153           Low income
    ## 154  Lower middle income
    ## 155  Upper middle income
    ## 156           Low income
    ## 157  Upper middle income
    ## 158                     
    ## 159  Upper middle income
    ## 160 High income: nonOECD
    ## 161           Low income
    ## 162  Lower middle income
    ## 163  Lower middle income
    ## 164    High income: OECD
    ## 165                     
    ## 166    High income: OECD
    ## 167           Low income
    ## 168    High income: OECD
    ## 169                     
    ## 170                     
    ## 171 High income: nonOECD
    ## 172  Lower middle income
    ## 173  Upper middle income
    ## 174  Upper middle income
    ## 175  Lower middle income
    ## 176  Upper middle income
    ## 177  Lower middle income
    ## 178    High income: OECD
    ## 179 High income: nonOECD
    ## 180           Low income
    ## 181    High income: OECD
    ## 182  Lower middle income
    ## 183 High income: nonOECD
    ## 184 High income: nonOECD
    ## 185  Upper middle income
    ## 186 High income: nonOECD
    ## 187           Low income
    ## 188                     
    ## 189 High income: nonOECD
    ## 190  Lower middle income
    ## 191  Lower middle income
    ## 192 High income: nonOECD
    ## 193  Lower middle income
    ## 194           Low income
    ## 195  Lower middle income
    ## 196 High income: nonOECD
    ## 197           Low income
    ## 198  Upper middle income
    ## 199                     
    ## 200  Lower middle income
    ## 201                     
    ## 202  Lower middle income
    ## 203  Upper middle income
    ## 204    High income: OECD
    ## 205    High income: OECD
    ## 206    High income: OECD
    ## 207  Lower middle income
    ## 208 High income: nonOECD
    ## 209  Upper middle income
    ## 210  Lower middle income
    ## 211 High income: nonOECD
    ## 212           Low income
    ## 213           Low income
    ## 214  Upper middle income
    ## 215           Low income
    ## 216  Upper middle income
    ## 217  Lower middle income
    ## 218  Upper middle income
    ## 219 High income: nonOECD
    ## 220  Upper middle income
    ## 221  Upper middle income
    ## 222  Upper middle income
    ## 223           Low income
    ## 224           Low income
    ## 225  Lower middle income
    ## 226                     
    ## 227 High income: nonOECD
    ## 228    High income: OECD
    ## 229  Lower middle income
    ## 230  Upper middle income
    ## 231  Upper middle income
    ## 232 High income: nonOECD
    ## 233  Lower middle income
    ## 234  Lower middle income
    ## 235  Lower middle income
    ## 236                     
    ## 237  Lower middle income
    ## 238  Lower middle income
    ## 239  Upper middle income
    ## 240           Low income
    ## 241  Lower middle income
    ## 242           Low income

The data set CData still contains records where the CData$IncomeGroup
field is empty. The identified rows below will be removed from the CData
data set

    #Find the rows where CData$IncomeGroup = " " and update to NA
    CData <- apply(CData, 2, function(x) gsub("^$|^ $", NA, x))

    #view dataset
    CData

    ##     CountryCode Country                                         
    ## 2   "ABW"       "Aruba"                                         
    ## 3   "ADO"       "Andorra"                                       
    ## 4   "AFG"       "Afghanistan"                                   
    ## 5   "AGO"       "Angola"                                        
    ## 6   "ALB"       "Albania"                                       
    ## 7   "ARB"       "Arab World"                                    
    ## 8   "ARE"       "United Arab Emirates"                          
    ## 9   "ARG"       "Argentina"                                     
    ## 10  "ARM"       "Armenia"                                       
    ## 11  "ASM"       "American Samoa"                                
    ## 12  "ATG"       "Antigua and Barbuda"                           
    ## 13  "AUS"       "Australia"                                     
    ## 14  "AUT"       "Austria"                                       
    ## 15  "AZE"       "Azerbaijan"                                    
    ## 16  "BDI"       "Burundi"                                       
    ## 17  "BEL"       "Belgium"                                       
    ## 18  "BEN"       "Benin"                                         
    ## 19  "BFA"       "Burkina Faso"                                  
    ## 20  "BGD"       "Bangladesh"                                    
    ## 21  "BGR"       "Bulgaria"                                      
    ## 22  "BHR"       "Bahrain"                                       
    ## 23  "BHS"       "The Bahamas"                                   
    ## 24  "BIH"       "Bosnia and Herzegovina"                        
    ## 25  "BLR"       "Belarus"                                       
    ## 26  "BLZ"       "Belize"                                        
    ## 27  "BMU"       "Bermuda"                                       
    ## 28  "BOL"       "Bolivia"                                       
    ## 29  "BRA"       "Brazil"                                        
    ## 30  "BRB"       "Barbados"                                      
    ## 31  "BRN"       "Brunei"                                        
    ## 32  "BTN"       "Bhutan"                                        
    ## 33  "BWA"       "Botswana"                                      
    ## 34  "CAF"       "Central African Republic"                      
    ## 35  "CAN"       "Canada"                                        
    ## 36  "CHE"       "Switzerland"                                   
    ## 37  "CHI"       "Channel Islands"                               
    ## 38  "CHL"       "Chile"                                         
    ## 39  "CHN"       "China"                                         
    ## 40  "CIV"       "Côte d'Ivoire"                                 
    ## 41  "CMR"       "Cameroon"                                      
    ## 42  "COG"       "Congo"                                         
    ## 43  "COL"       "Colombia"                                      
    ## 44  "COM"       "Comoros"                                       
    ## 45  "CPV"       "Cabo Verde"                                    
    ## 46  "CRI"       "Costa Rica"                                    
    ## 47  "CUB"       "Cuba"                                          
    ## 48  "CUW"       "Curaçao"                                       
    ## 49  "CYM"       "Cayman Islands"                                
    ## 50  "CYP"       "Cyprus"                                        
    ## 51  "CZE"       "Czech Republic"                                
    ## 52  "DEU"       "Germany"                                       
    ## 53  "DJI"       "Djibouti"                                      
    ## 54  "DMA"       "Dominica"                                      
    ## 55  "DNK"       "Denmark"                                       
    ## 56  "DOM"       "Dominican Republic"                            
    ## 57  "DZA"       "Algeria"                                       
    ## 58  "EAP"       "East Asia & Pacific (developing only)"         
    ## 59  "EAS"       "East Asia & Pacific (all income levels)"       
    ## 60  "ECA"       "Europe & Central Asia (developing only)"       
    ## 61  "ECS"       "Europe & Central Asia (all income levels)"     
    ## 62  "ECU"       "Ecuador"                                       
    ## 63  "EGY"       "Egypt"                                         
    ## 64  "EMU"       "Euro area"                                     
    ## 65  "ERI"       "Eritrea"                                       
    ## 66  "ESP"       "Spain"                                         
    ## 67  "EST"       "Estonia"                                       
    ## 68  "ETH"       "Ethiopia"                                      
    ## 69  "EUU"       "European Union"                                
    ## 70  "FIN"       "Finland"                                       
    ## 71  "FJI"       "Fiji"                                          
    ## 72  "FRA"       "France"                                        
    ## 73  "FRO"       "Faeroe Islands"                                
    ## 74  "FSM"       "Micronesia"                                    
    ## 75  "GAB"       "Gabon"                                         
    ## 76  "GBR"       "United Kingdom"                                
    ## 77  "GEO"       "Georgia"                                       
    ## 78  "GHA"       "Ghana"                                         
    ## 79  "GIN"       "Guinea"                                        
    ## 80  "GMB"       "The Gambia"                                    
    ## 81  "GNB"       "Guinea-Bissau"                                 
    ## 82  "GNQ"       "Equatorial Guinea"                             
    ## 83  "GRC"       "Greece"                                        
    ## 84  "GRD"       "Grenada"                                       
    ## 85  "GRL"       "Greenland"                                     
    ## 86  "GTM"       "Guatemala"                                     
    ## 87  "GUM"       "Guam"                                          
    ## 88  "GUY"       "Guyana"                                        
    ## 89  "HIC"       "High income"                                   
    ## 90  "HKG"       "Hong Kong SAR, China"                          
    ## 91  "HND"       "Honduras"                                      
    ## 92  "HPC"       "Heavily indebted poor countries (HIPC)"        
    ## 93  "HRV"       "Croatia"                                       
    ## 94  "HTI"       "Haiti"                                         
    ## 95  "HUN"       "Hungary"                                       
    ## 96  "IDN"       "Indonesia"                                     
    ## 97  "IMY"       "Isle of Man"                                   
    ## 98  "IND"       "India"                                         
    ## 99  "IRL"       "Ireland"                                       
    ## 100 "IRN"       "Iran"                                          
    ## 101 "IRQ"       "Iraq"                                          
    ## 102 "ISL"       "Iceland"                                       
    ## 103 "ISR"       "Israel"                                        
    ## 104 "ITA"       "Italy"                                         
    ## 105 "JAM"       "Jamaica"                                       
    ## 106 "JOR"       "Jordan"                                        
    ## 107 "JPN"       "Japan"                                         
    ## 108 "KAZ"       "Kazakhstan"                                    
    ## 109 "KEN"       "Kenya"                                         
    ## 110 "KGZ"       "Kyrgyz Republic"                               
    ## 111 "KHM"       "Cambodia"                                      
    ## 112 "KIR"       "Kiribati"                                      
    ## 113 "KNA"       "St. Kitts and Nevis"                           
    ## 114 "KOR"       "Korea"                                         
    ## 115 "KSV"       "Kosovo"                                        
    ## 116 "KWT"       "Kuwait"                                        
    ## 117 "LAC"       "Latin America & Caribbean (developing only)"   
    ## 118 "LAO"       "Lao PDR"                                       
    ## 119 "LBN"       "Lebanon"                                       
    ## 120 "LBR"       "Liberia"                                       
    ## 121 "LBY"       "Libya"                                         
    ## 122 "LCA"       "St. Lucia"                                     
    ## 123 "LCN"       "Latin America & Caribbean (all income levels)" 
    ## 124 "LDC"       "Least developed countries: UN classification"  
    ## 125 "LIC"       "Low income"                                    
    ## 126 "LIE"       "Liechtenstein"                                 
    ## 127 "LKA"       "Sri Lanka"                                     
    ## 128 "LMC"       "Lower middle income"                           
    ## 129 "LMY"       "Low & middle income"                           
    ## 130 "LSO"       "Lesotho"                                       
    ## 131 "LTU"       "Lithuania"                                     
    ## 132 "LUX"       "Luxembourg"                                    
    ## 133 "LVA"       "Latvia"                                        
    ## 134 "MAC"       "Macao SAR, China"                              
    ## 135 "MAF"       "St. Martin (French part)"                      
    ## 136 "MAR"       "Morocco"                                       
    ## 137 "MCO"       "Monaco"                                        
    ## 138 "MDA"       "Moldova"                                       
    ## 139 "MDG"       "Madagascar"                                    
    ## 140 "MDV"       "Maldives"                                      
    ## 141 "MEA"       "Middle East & North Africa (all income levels)"
    ## 142 "MEX"       "Mexico"                                        
    ## 143 "MHL"       "Marshall Islands"                              
    ## 144 "MIC"       "Middle income"                                 
    ## 145 "MKD"       "Macedonia"                                     
    ## 146 "MLI"       "Mali"                                          
    ## 147 "MLT"       "Malta"                                         
    ## 148 "MMR"       "Myanmar"                                       
    ## 149 "MNA"       "Middle East & North Africa (developing only)"  
    ## 150 "MNE"       "Montenegro"                                    
    ## 151 "MNG"       "Mongolia"                                      
    ## 152 "MNP"       "Northern Mariana Islands"                      
    ## 153 "MOZ"       "Mozambique"                                    
    ## 154 "MRT"       "Mauritania"                                    
    ## 155 "MUS"       "Mauritius"                                     
    ## 156 "MWI"       "Malawi"                                        
    ## 157 "MYS"       "Malaysia"                                      
    ## 158 "NAC"       "North America"                                 
    ## 159 "NAM"       "Namibia"                                       
    ## 160 "NCL"       "New Caledonia"                                 
    ## 161 "NER"       "Niger"                                         
    ## 162 "NGA"       "Nigeria"                                       
    ## 163 "NIC"       "Nicaragua"                                     
    ## 164 "NLD"       "Netherlands"                                   
    ## 165 "NOC"       "High income: nonOECD"                          
    ## 166 "NOR"       "Norway"                                        
    ## 167 "NPL"       "Nepal"                                         
    ## 168 "NZL"       "New Zealand"                                   
    ## 169 "OEC"       "High income: OECD"                             
    ## 170 "OED"       "OECD members"                                  
    ## 171 "OMN"       "Oman"                                          
    ## 172 "PAK"       "Pakistan"                                      
    ## 173 "PAN"       "Panama"                                        
    ## 174 "PER"       "Peru"                                          
    ## 175 "PHL"       "Philippines"                                   
    ## 176 "PLW"       "Palau"                                         
    ## 177 "PNG"       "Papua New Guinea"                              
    ## 178 "POL"       "Poland"                                        
    ## 179 "PRI"       "Puerto Rico"                                   
    ## 180 "PRK"       "Dem. People's Rep. Korea"                      
    ## 181 "PRT"       "Portugal"                                      
    ## 182 "PRY"       "Paraguay"                                      
    ## 183 "PYF"       "French Polynesia"                              
    ## 184 "QAT"       "Qatar"                                         
    ## 185 "ROM"       "Romania"                                       
    ## 186 "RUS"       "Russia"                                        
    ## 187 "RWA"       "Rwanda"                                        
    ## 188 "SAS"       "South Asia"                                    
    ## 189 "SAU"       "Saudi Arabia"                                  
    ## 190 "SDN"       "Sudan"                                         
    ## 191 "SEN"       "Senegal"                                       
    ## 192 "SGP"       "Singapore"                                     
    ## 193 "SLB"       "Solomon Islands"                               
    ## 194 "SLE"       "Sierra Leone"                                  
    ## 195 "SLV"       "El Salvador"                                   
    ## 196 "SMR"       "San Marino"                                    
    ## 197 "SOM"       "Somalia"                                       
    ## 198 "SRB"       "Serbia"                                        
    ## 199 "SSA"       "Sub-Saharan Africa (developing only)"          
    ## 200 "SSD"       "South Sudan"                                   
    ## 201 "SSF"       "Sub-Saharan Africa (all income levels)"        
    ## 202 "STP"       "São Tomé and Principe"                         
    ## 203 "SUR"       "Suriname"                                      
    ## 204 "SVK"       "Slovak Republic"                               
    ## 205 "SVN"       "Slovenia"                                      
    ## 206 "SWE"       "Sweden"                                        
    ## 207 "SWZ"       "Swaziland"                                     
    ## 208 "SXM"       "Sint Maarten (Dutch part)"                     
    ## 209 "SYC"       "Seychelles"                                    
    ## 210 "SYR"       "Syrian Arab Republic"                          
    ## 211 "TCA"       "Turks and Caicos Islands"                      
    ## 212 "TCD"       "Chad"                                          
    ## 213 "TGO"       "Togo"                                          
    ## 214 "THA"       "Thailand"                                      
    ## 215 "TJK"       "Tajikistan"                                    
    ## 216 "TKM"       "Turkmenistan"                                  
    ## 217 "TMP"       "Timor-Leste"                                   
    ## 218 "TON"       "Tonga"                                         
    ## 219 "TTO"       "Trinidad and Tobago"                           
    ## 220 "TUN"       "Tunisia"                                       
    ## 221 "TUR"       "Turkey"                                        
    ## 222 "TUV"       "Tuvalu"                                        
    ## 223 "TZA"       "Tanzania"                                      
    ## 224 "UGA"       "Uganda"                                        
    ## 225 "UKR"       "Ukraine"                                       
    ## 226 "UMC"       "Upper middle income"                           
    ## 227 "URY"       "Uruguay"                                       
    ## 228 "USA"       "United States"                                 
    ## 229 "UZB"       "Uzbekistan"                                    
    ## 230 "VCT"       "St. Vincent and the Grenadines"                
    ## 231 "VEN"       "Venezuela"                                     
    ## 232 "VIR"       "Virgin Islands"                                
    ## 233 "VNM"       "Vietnam"                                       
    ## 234 "VUT"       "Vanuatu"                                       
    ## 235 "WBG"       "West Bank and Gaza"                            
    ## 236 "WLD"       "World"                                         
    ## 237 "WSM"       "Samoa"                                         
    ## 238 "YEM"       "Yemen"                                         
    ## 239 "ZAF"       "South Africa"                                  
    ## 240 "ZAR"       "Dem. Rep. Congo"                               
    ## 241 "ZMB"       "Zambia"                                        
    ## 242 "ZWE"       "Zimbabwe"                                      
    ##     IncomeGroup           
    ## 2   "High income: nonOECD"
    ## 3   "High income: nonOECD"
    ## 4   "Low income"          
    ## 5   "Upper middle income" 
    ## 6   "Upper middle income" 
    ## 7   NA                    
    ## 8   "High income: nonOECD"
    ## 9   "Upper middle income" 
    ## 10  "Lower middle income" 
    ## 11  "Upper middle income" 
    ## 12  "High income: nonOECD"
    ## 13  "High income: OECD"   
    ## 14  "High income: OECD"   
    ## 15  "Upper middle income" 
    ## 16  "Low income"          
    ## 17  "High income: OECD"   
    ## 18  "Low income"          
    ## 19  "Low income"          
    ## 20  "Low income"          
    ## 21  "Upper middle income" 
    ## 22  "High income: nonOECD"
    ## 23  "High income: nonOECD"
    ## 24  "Upper middle income" 
    ## 25  "Upper middle income" 
    ## 26  "Upper middle income" 
    ## 27  "High income: nonOECD"
    ## 28  "Lower middle income" 
    ## 29  "Upper middle income" 
    ## 30  "High income: nonOECD"
    ## 31  "High income: nonOECD"
    ## 32  "Lower middle income" 
    ## 33  "Upper middle income" 
    ## 34  "Low income"          
    ## 35  "High income: OECD"   
    ## 36  "High income: OECD"   
    ## 37  "High income: nonOECD"
    ## 38  "High income: OECD"   
    ## 39  "Upper middle income" 
    ## 40  "Lower middle income" 
    ## 41  "Lower middle income" 
    ## 42  "Lower middle income" 
    ## 43  "Upper middle income" 
    ## 44  "Low income"          
    ## 45  "Lower middle income" 
    ## 46  "Upper middle income" 
    ## 47  "Upper middle income" 
    ## 48  "High income: nonOECD"
    ## 49  "High income: nonOECD"
    ## 50  "High income: nonOECD"
    ## 51  "High income: OECD"   
    ## 52  "High income: OECD"   
    ## 53  "Lower middle income" 
    ## 54  "Upper middle income" 
    ## 55  "High income: OECD"   
    ## 56  "Upper middle income" 
    ## 57  "Upper middle income" 
    ## 58  NA                    
    ## 59  NA                    
    ## 60  NA                    
    ## 61  NA                    
    ## 62  "Upper middle income" 
    ## 63  "Lower middle income" 
    ## 64  NA                    
    ## 65  "Low income"          
    ## 66  "High income: OECD"   
    ## 67  "High income: OECD"   
    ## 68  "Low income"          
    ## 69  NA                    
    ## 70  "High income: OECD"   
    ## 71  "Upper middle income" 
    ## 72  "High income: OECD"   
    ## 73  "High income: nonOECD"
    ## 74  "Lower middle income" 
    ## 75  "Upper middle income" 
    ## 76  "High income: OECD"   
    ## 77  "Lower middle income" 
    ## 78  "Lower middle income" 
    ## 79  "Low income"          
    ## 80  "Low income"          
    ## 81  "Low income"          
    ## 82  "High income: nonOECD"
    ## 83  "High income: OECD"   
    ## 84  "Upper middle income" 
    ## 85  "High income: nonOECD"
    ## 86  "Lower middle income" 
    ## 87  "High income: nonOECD"
    ## 88  "Lower middle income" 
    ## 89  NA                    
    ## 90  "High income: nonOECD"
    ## 91  "Lower middle income" 
    ## 92  NA                    
    ## 93  "High income: nonOECD"
    ## 94  "Low income"          
    ## 95  "Upper middle income" 
    ## 96  "Lower middle income" 
    ## 97  "High income: nonOECD"
    ## 98  "Lower middle income" 
    ## 99  "High income: OECD"   
    ## 100 "Upper middle income" 
    ## 101 "Upper middle income" 
    ## 102 "High income: OECD"   
    ## 103 "High income: OECD"   
    ## 104 "High income: OECD"   
    ## 105 "Upper middle income" 
    ## 106 "Upper middle income" 
    ## 107 "High income: OECD"   
    ## 108 "Upper middle income" 
    ## 109 "Low income"          
    ## 110 "Lower middle income" 
    ## 111 "Low income"          
    ## 112 "Lower middle income" 
    ## 113 "High income: nonOECD"
    ## 114 "High income: OECD"   
    ## 115 "Lower middle income" 
    ## 116 "High income: nonOECD"
    ## 117 NA                    
    ## 118 "Lower middle income" 
    ## 119 "Upper middle income" 
    ## 120 "Low income"          
    ## 121 "Upper middle income" 
    ## 122 "Upper middle income" 
    ## 123 NA                    
    ## 124 NA                    
    ## 125 NA                    
    ## 126 "High income: nonOECD"
    ## 127 "Lower middle income" 
    ## 128 NA                    
    ## 129 NA                    
    ## 130 "Lower middle income" 
    ## 131 "High income: nonOECD"
    ## 132 "High income: OECD"   
    ## 133 "High income: nonOECD"
    ## 134 "High income: nonOECD"
    ## 135 "High income: nonOECD"
    ## 136 "Lower middle income" 
    ## 137 "High income: nonOECD"
    ## 138 "Lower middle income" 
    ## 139 "Low income"          
    ## 140 "Upper middle income" 
    ## 141 NA                    
    ## 142 "Upper middle income" 
    ## 143 "Upper middle income" 
    ## 144 NA                    
    ## 145 "Upper middle income" 
    ## 146 "Low income"          
    ## 147 "High income: nonOECD"
    ## 148 "Low income"          
    ## 149 NA                    
    ## 150 "Upper middle income" 
    ## 151 "Lower middle income" 
    ## 152 "High income: nonOECD"
    ## 153 "Low income"          
    ## 154 "Lower middle income" 
    ## 155 "Upper middle income" 
    ## 156 "Low income"          
    ## 157 "Upper middle income" 
    ## 158 NA                    
    ## 159 "Upper middle income" 
    ## 160 "High income: nonOECD"
    ## 161 "Low income"          
    ## 162 "Lower middle income" 
    ## 163 "Lower middle income" 
    ## 164 "High income: OECD"   
    ## 165 NA                    
    ## 166 "High income: OECD"   
    ## 167 "Low income"          
    ## 168 "High income: OECD"   
    ## 169 NA                    
    ## 170 NA                    
    ## 171 "High income: nonOECD"
    ## 172 "Lower middle income" 
    ## 173 "Upper middle income" 
    ## 174 "Upper middle income" 
    ## 175 "Lower middle income" 
    ## 176 "Upper middle income" 
    ## 177 "Lower middle income" 
    ## 178 "High income: OECD"   
    ## 179 "High income: nonOECD"
    ## 180 "Low income"          
    ## 181 "High income: OECD"   
    ## 182 "Lower middle income" 
    ## 183 "High income: nonOECD"
    ## 184 "High income: nonOECD"
    ## 185 "Upper middle income" 
    ## 186 "High income: nonOECD"
    ## 187 "Low income"          
    ## 188 NA                    
    ## 189 "High income: nonOECD"
    ## 190 "Lower middle income" 
    ## 191 "Lower middle income" 
    ## 192 "High income: nonOECD"
    ## 193 "Lower middle income" 
    ## 194 "Low income"          
    ## 195 "Lower middle income" 
    ## 196 "High income: nonOECD"
    ## 197 "Low income"          
    ## 198 "Upper middle income" 
    ## 199 NA                    
    ## 200 "Lower middle income" 
    ## 201 NA                    
    ## 202 "Lower middle income" 
    ## 203 "Upper middle income" 
    ## 204 "High income: OECD"   
    ## 205 "High income: OECD"   
    ## 206 "High income: OECD"   
    ## 207 "Lower middle income" 
    ## 208 "High income: nonOECD"
    ## 209 "Upper middle income" 
    ## 210 "Lower middle income" 
    ## 211 "High income: nonOECD"
    ## 212 "Low income"          
    ## 213 "Low income"          
    ## 214 "Upper middle income" 
    ## 215 "Low income"          
    ## 216 "Upper middle income" 
    ## 217 "Lower middle income" 
    ## 218 "Upper middle income" 
    ## 219 "High income: nonOECD"
    ## 220 "Upper middle income" 
    ## 221 "Upper middle income" 
    ## 222 "Upper middle income" 
    ## 223 "Low income"          
    ## 224 "Low income"          
    ## 225 "Lower middle income" 
    ## 226 NA                    
    ## 227 "High income: nonOECD"
    ## 228 "High income: OECD"   
    ## 229 "Lower middle income" 
    ## 230 "Upper middle income" 
    ## 231 "Upper middle income" 
    ## 232 "High income: nonOECD"
    ## 233 "Lower middle income" 
    ## 234 "Lower middle income" 
    ## 235 "Lower middle income" 
    ## 236 NA                    
    ## 237 "Lower middle income" 
    ## 238 "Lower middle income" 
    ## 239 "Upper middle income" 
    ## 240 "Low income"          
    ## 241 "Lower middle income" 
    ## 242 "Low income"

Merging the Datasets
--------------------

To have a complete dataset GDA3 and CData were merged on CountryCode.

    #cleanset data set created with all records from 
    cleanset <- merge( GDA3,  CData,  by=1:1, all.GDA3= TRUE)

    #view the new dataset
    cleanset

    ##     CountryCode Ranking                      Country.x    Economy
    ## 1           ADO     162                        Andorra      3,249
    ## 2           AFG     108                    Afghanistan     20,038
    ## 3           AGO      58                         Angola    138,357
    ## 4           ALB     127                        Albania     13,212
    ## 5           ARE      30           United Arab Emirates    399,451
    ## 6           ARG      24                      Argentina    537,660
    ## 7           ARM     136                        Armenia     11,644
    ## 8           ATG     178            Antigua and Barbuda      1,221
    ## 9           AUS      12                      Australia  1,454,675
    ## 10          AUT      27                        Austria    436,888
    ## 11          AZE      69                     Azerbaijan     75,198
    ## 12          BDI     164                        Burundi      3,094
    ## 13          BEL      25                        Belgium    531,547
    ## 14          BEN     140                          Benin      9,575
    ## 15          BFA     131                   Burkina Faso     12,542
    ## 16          BGD      56                     Bangladesh    172,887
    ## 17          BGR      79                       Bulgaria     56,717
    ## 18          BHR      96                        Bahrain     33,851
    ## 19          BHS     143                   Bahamas, The      8,511
    ## 20          BIH     111         Bosnia and Herzegovina     18,521
    ## 21          BLR      68                        Belarus     76,139
    ## 22          BLZ     173                         Belize      1,699
    ## 23          BMU     151                        Bermuda      5,574
    ## 24          BOL      98                        Bolivia     32,996
    ## 25          BRA       7                         Brazil  2,416,636
    ## 26          BRB     160                       Barbados      4,355
    ## 27          BRN     113              Brunei Darussalam     17,105
    ## 28          BTN     170                         Bhutan      1,959
    ## 29          BWA     119                       Botswana     15,813
    ## 30          CAF     172       Central African Republic      1,723
    ## 31          CAN      11                         Canada  1,785,387
    ## 32          CHE      20                    Switzerland    701,037
    ## 33          CHL      42                          Chile    258,062
    ## 34          CHN       2                          China 10,354,832
    ## 35          CIV      95                  Côte d'Ivoire     34,254
    ## 36          CMR      99                       Cameroon     32,051
    ## 37          COG     123                    Congo, Rep.     14,177
    ## 38          COL      32                       Colombia    377,740
    ## 39          COM     187                        Comoros        624
    ## 40          CPV     171                     Cabo Verde      1,871
    ## 41          CRI      82                     Costa Rica     49,553
    ## 42          CUB      67                           Cuba     77,150
    ## 43          CYP     107                         Cyprus     23,226
    ## 44          CZE      51                 Czech Republic    205,270
    ## 45          DEU       4                        Germany  3,868,291
    ## 46          DJI     174                       Djibouti      1,589
    ## 47          DMA     188                       Dominica        524
    ## 48          DNK      34                        Denmark    342,362
    ## 49          DOM      73             Dominican Republic     64,138
    ## 50          DZA      49                        Algeria    213,518
    ## 51          EAP    <NA>            East Asia & Pacific 12,609,716
    ## 52          ECA    <NA>          Europe & Central Asia  1,817,461
    ## 53          ECU      63                        Ecuador    100,917
    ## 54          EGY      38               Egypt, Arab Rep.    301,499
    ## 55          EMU    <NA>                      Euro area 13,410,232
    ## 56          ESP      14                          Spain  1,381,342
    ## 57          EST     105                        Estonia     26,485
    ## 58          ETH      80                       Ethiopia     55,612
    ## 59          FIN      41                        Finland    272,217
    ## 60          FJI     157                           Fiji      4,532
    ## 61          FRA       6                         France  2,829,192
    ## 62          FRO     166                  Faroe Islands      2,613
    ## 63          FSM     191          Micronesia, Fed. Sts.        318
    ## 64          GAB     112                          Gabon     18,180
    ## 65          GBR       5                 United Kingdom  2,988,893
    ## 66          GEO     117                        Georgia     16,530
    ## 67          GHA      92                          Ghana     38,617
    ## 68          GIN     149                         Guinea      6,624
    ## 69          GMB     183                    Gambia, The        851
    ## 70          GNB     180                  Guinea-Bissau      1,022
    ## 71          GNQ     121              Equatorial Guinea     15,530
    ## 72          GRC      45                         Greece    235,574
    ## 73          GRD     181                        Grenada        912
    ## 74          GRL     167                      Greenland      2,441
    ## 75          GTM      76                      Guatemala     58,827
    ## 76          GUY     163                         Guyana      3,097
    ## 77          HIC    <NA>                    High income 52,850,488
    ## 78          HKG      39           Hong Kong SAR, China    290,896
    ## 79          HND     110                       Honduras     19,385
    ## 80          HRV      78                        Croatia     57,113
    ## 81          HTI     142                          Haiti      8,713
    ## 82          HUN      59                        Hungary    138,347
    ## 83          IDN      16                      Indonesia    888,538
    ## 84          IND       9                          India  2,048,517
    ## 85          IRL      43                        Ireland    250,814
    ## 86          IRN      28             Iran, Islamic Rep.    425,326
    ## 87          IRQ      47                           Iraq    223,500
    ## 88          ISL     114                        Iceland     17,036
    ## 89          ISR      37                         Israel    305,675
    ## 90          ITA       8                          Italy  2,141,161
    ## 91          JAM     125                        Jamaica     13,891
    ## 92          JOR      94                         Jordan     35,827
    ## 93          JPN       3                          Japan  4,601,461
    ## 94          KAZ      48                     Kazakhstan    217,872
    ## 95          KEN      75                          Kenya     60,937
    ## 96          KGZ     147                Kyrgyz Republic      7,404
    ## 97          KHM     116                       Cambodia     16,778
    ## 98          KIR     194                       Kiribati        167
    ## 99          KNA     182            St. Kitts and Nevis        852
    ## 100         KOR      13                    Korea, Rep.  1,410,383
    ## 101         KSV     148                         Kosovo      7,387
    ## 102         KWT      57                         Kuwait    163,612
    ## 103         LAC    <NA>      Latin America & Caribbean  4,845,035
    ## 104         LAO     134                        Lao PDR     11,997
    ## 105         LBN      89                        Lebanon     45,731
    ## 106         LBR     169                        Liberia      2,013
    ## 107         LBY      91                          Libya     41,143
    ## 108         LCA     177                      St. Lucia      1,404
    ## 109         LIC    <NA>                     Low income    397,849
    ## 110         LIE     152                  Liechtenstein      5,488
    ## 111         LKA      66                      Sri Lanka     78,824
    ## 112         LMC    <NA>            Lower middle income  5,781,069
    ## 113         LMY    <NA>            Low & middle income 25,148,400
    ## 114         LSO     168                        Lesotho      2,181
    ## 115         LTU      85                      Lithuania     48,354
    ## 116         LUX      71                     Luxembourg     64,874
    ## 117         LVA     100                         Latvia     31,287
    ## 118         MAC      81               Macao SAR, China     55,502
    ## 119         MAR      61                        Morocco    110,009
    ## 120         MDA     145                        Moldova      7,962
    ## 121         MDG     138                     Madagascar     10,593
    ## 122         MDV     165                       Maldives      3,062
    ## 123         MEX      15                         Mexico  1,294,690
    ## 124         MHL     193               Marshall Islands        187
    ## 125         MIC    <NA>                  Middle income 24,748,448
    ## 126         MKD     137                 Macedonia, FYR     11,324
    ## 127         MLI     132                           Mali     12,037
    ## 128         MLT     139                          Malta      9,643
    ## 129         MMR      72                        Myanmar     64,330
    ## 130         MNA    <NA>     Middle East & North Africa  1,556,768
    ## 131         MNE     156                     Montenegro      4,588
    ## 132         MNG     133                       Mongolia     12,016
    ## 133         MOZ     118                     Mozambique     15,938
    ## 134         MRT     154                     Mauritania      5,061
    ## 135         MUS     130                      Mauritius     12,630
    ## 136         MWI     161                         Malawi      4,258
    ## 137         MYS      35                       Malaysia    338,104
    ## 138         NAM     128                        Namibia     12,995
    ## 139         NER     144                          Niger      8,169
    ## 140         NGA      22                        Nigeria    568,508
    ## 141         NIC     135                      Nicaragua     11,806
    ## 142         NLD      17                    Netherlands    879,319
    ## 143         NOR      26                         Norway    499,817
    ## 144         NPL     109                          Nepal     19,770
    ## 145         NZL      53                    New Zealand    199,970
    ## 146         OMN      65                           Oman     81,797
    ## 147         PAK      44                       Pakistan    243,632
    ## 148         PAN      88                         Panama     46,213
    ## 149         PER      52                           Peru    202,596
    ## 150         PHL      40                    Philippines    284,777
    ## 151         PLW     192                          Palau        251
    ## 152         PNG     115               Papua New Guinea     16,929
    ## 153         POL      23                         Poland    544,967
    ## 154         PRI      62                    Puerto Rico    103,135
    ## 155         PRT      46                       Portugal    230,117
    ## 156         PRY     101                       Paraguay     30,881
    ## 157         QAT      50                          Qatar    210,109
    ## 158         ROM      54                        Romania    199,044
    ## 159         RUS      10             Russian Federation  1,860,598
    ## 160         RWA     146                         Rwanda      7,890
    ## 161         SAS    <NA>                     South Asia  2,588,688
    ## 162         SAU      19                   Saudi Arabia    753,832
    ## 163         SDN      70                          Sudan     73,815
    ## 164         SEN     120                        Senegal     15,658
    ## 165         SGP      36                      Singapore    307,860
    ## 166         SLB     179                Solomon Islands      1,158
    ## 167         SLE     155                   Sierra Leone      4,838
    ## 168         SLV     106                    El Salvador     25,164
    ## 169         SOM     150                        Somalia      5,707
    ## 170         SRB      90                         Serbia     43,866
    ## 171         SSA    <NA>             Sub-Saharan Africa  1,728,322
    ## 172         SSD     126                    South Sudan     13,282
    ## 173         STP     190          São Tomé and Principe        337
    ## 174         SUR     153                       Suriname      5,210
    ## 175         SVK      64                Slovak Republic    100,249
    ## 176         SVN      83                       Slovenia     49,491
    ## 177         SWE      21                         Sweden    571,090
    ## 178         SWZ     159                      Swaziland      4,413
    ## 179         SYC     175                     Seychelles      1,423
    ## 180         TCD     124                           Chad     13,922
    ## 181         TGO     158                           Togo      4,518
    ## 182         THA      29                       Thailand    404,824
    ## 183         TJK     141                     Tajikistan      9,242
    ## 184         TKM      87                   Turkmenistan     47,932
    ## 185         TMP     176                    Timor-Leste      1,417
    ## 186         TON     189                          Tonga        434
    ## 187         TTO     102            Trinidad and Tobago     28,883
    ## 188         TUN      84                        Tunisia     48,613
    ## 189         TUR      18                         Turkey    798,429
    ## 190         TUV     195                         Tuvalu         38
    ## 191         TZA      86                       Tanzania     48,057
    ## 192         UGA     104                         Uganda     26,998
    ## 193         UKR      60                        Ukraine    131,805
    ## 194         UMC    <NA>            Upper middle income 18,958,149
    ## 195         URY      77                        Uruguay     57,471
    ## 196         USA       1                  United States 17,419,000
    ## 197         UZB      74                     Uzbekistan     62,644
    ## 198         VCT     186 St. Vincent and the Grenadines        729
    ## 199         VEN      31                  Venezuela, RB    381,286
    ## 200         VNM      55                        Vietnam    186,205
    ## 201         VUT     184                        Vanuatu        815
    ## 202         WBG     129             West Bank and Gaza     12,738
    ## 203         WLD    <NA>                          World 77,960,607
    ## 204         WSM     185                          Samoa        800
    ## 205         YEM      93                    Yemen, Rep.     35,955
    ## 206         ZAF      33                   South Africa    350,141
    ## 207         ZAR      97               Congo, Dem. Rep.     33,121
    ## 208         ZMB     103                         Zambia     27,066
    ## 209         ZWE     122                       Zimbabwe     14,197
    ##                                        Country.y          IncomeGroup
    ## 1                                        Andorra High income: nonOECD
    ## 2                                    Afghanistan           Low income
    ## 3                                         Angola  Upper middle income
    ## 4                                        Albania  Upper middle income
    ## 5                           United Arab Emirates High income: nonOECD
    ## 6                                      Argentina  Upper middle income
    ## 7                                        Armenia  Lower middle income
    ## 8                            Antigua and Barbuda High income: nonOECD
    ## 9                                      Australia    High income: OECD
    ## 10                                       Austria    High income: OECD
    ## 11                                    Azerbaijan  Upper middle income
    ## 12                                       Burundi           Low income
    ## 13                                       Belgium    High income: OECD
    ## 14                                         Benin           Low income
    ## 15                                  Burkina Faso           Low income
    ## 16                                    Bangladesh           Low income
    ## 17                                      Bulgaria  Upper middle income
    ## 18                                       Bahrain High income: nonOECD
    ## 19                                   The Bahamas High income: nonOECD
    ## 20                        Bosnia and Herzegovina  Upper middle income
    ## 21                                       Belarus  Upper middle income
    ## 22                                        Belize  Upper middle income
    ## 23                                       Bermuda High income: nonOECD
    ## 24                                       Bolivia  Lower middle income
    ## 25                                        Brazil  Upper middle income
    ## 26                                      Barbados High income: nonOECD
    ## 27                                        Brunei High income: nonOECD
    ## 28                                        Bhutan  Lower middle income
    ## 29                                      Botswana  Upper middle income
    ## 30                      Central African Republic           Low income
    ## 31                                        Canada    High income: OECD
    ## 32                                   Switzerland    High income: OECD
    ## 33                                         Chile    High income: OECD
    ## 34                                         China  Upper middle income
    ## 35                                 Côte d'Ivoire  Lower middle income
    ## 36                                      Cameroon  Lower middle income
    ## 37                                         Congo  Lower middle income
    ## 38                                      Colombia  Upper middle income
    ## 39                                       Comoros           Low income
    ## 40                                    Cabo Verde  Lower middle income
    ## 41                                    Costa Rica  Upper middle income
    ## 42                                          Cuba  Upper middle income
    ## 43                                        Cyprus High income: nonOECD
    ## 44                                Czech Republic    High income: OECD
    ## 45                                       Germany    High income: OECD
    ## 46                                      Djibouti  Lower middle income
    ## 47                                      Dominica  Upper middle income
    ## 48                                       Denmark    High income: OECD
    ## 49                            Dominican Republic  Upper middle income
    ## 50                                       Algeria  Upper middle income
    ## 51         East Asia & Pacific (developing only)                 <NA>
    ## 52       Europe & Central Asia (developing only)                 <NA>
    ## 53                                       Ecuador  Upper middle income
    ## 54                                         Egypt  Lower middle income
    ## 55                                     Euro area                 <NA>
    ## 56                                         Spain    High income: OECD
    ## 57                                       Estonia    High income: OECD
    ## 58                                      Ethiopia           Low income
    ## 59                                       Finland    High income: OECD
    ## 60                                          Fiji  Upper middle income
    ## 61                                        France    High income: OECD
    ## 62                                Faeroe Islands High income: nonOECD
    ## 63                                    Micronesia  Lower middle income
    ## 64                                         Gabon  Upper middle income
    ## 65                                United Kingdom    High income: OECD
    ## 66                                       Georgia  Lower middle income
    ## 67                                         Ghana  Lower middle income
    ## 68                                        Guinea           Low income
    ## 69                                    The Gambia           Low income
    ## 70                                 Guinea-Bissau           Low income
    ## 71                             Equatorial Guinea High income: nonOECD
    ## 72                                        Greece    High income: OECD
    ## 73                                       Grenada  Upper middle income
    ## 74                                     Greenland High income: nonOECD
    ## 75                                     Guatemala  Lower middle income
    ## 76                                        Guyana  Lower middle income
    ## 77                                   High income                 <NA>
    ## 78                          Hong Kong SAR, China High income: nonOECD
    ## 79                                      Honduras  Lower middle income
    ## 80                                       Croatia High income: nonOECD
    ## 81                                         Haiti           Low income
    ## 82                                       Hungary  Upper middle income
    ## 83                                     Indonesia  Lower middle income
    ## 84                                         India  Lower middle income
    ## 85                                       Ireland    High income: OECD
    ## 86                                          Iran  Upper middle income
    ## 87                                          Iraq  Upper middle income
    ## 88                                       Iceland    High income: OECD
    ## 89                                        Israel    High income: OECD
    ## 90                                         Italy    High income: OECD
    ## 91                                       Jamaica  Upper middle income
    ## 92                                        Jordan  Upper middle income
    ## 93                                         Japan    High income: OECD
    ## 94                                    Kazakhstan  Upper middle income
    ## 95                                         Kenya           Low income
    ## 96                               Kyrgyz Republic  Lower middle income
    ## 97                                      Cambodia           Low income
    ## 98                                      Kiribati  Lower middle income
    ## 99                           St. Kitts and Nevis High income: nonOECD
    ## 100                                        Korea    High income: OECD
    ## 101                                       Kosovo  Lower middle income
    ## 102                                       Kuwait High income: nonOECD
    ## 103  Latin America & Caribbean (developing only)                 <NA>
    ## 104                                      Lao PDR  Lower middle income
    ## 105                                      Lebanon  Upper middle income
    ## 106                                      Liberia           Low income
    ## 107                                        Libya  Upper middle income
    ## 108                                    St. Lucia  Upper middle income
    ## 109                                   Low income                 <NA>
    ## 110                                Liechtenstein High income: nonOECD
    ## 111                                    Sri Lanka  Lower middle income
    ## 112                          Lower middle income                 <NA>
    ## 113                          Low & middle income                 <NA>
    ## 114                                      Lesotho  Lower middle income
    ## 115                                    Lithuania High income: nonOECD
    ## 116                                   Luxembourg    High income: OECD
    ## 117                                       Latvia High income: nonOECD
    ## 118                             Macao SAR, China High income: nonOECD
    ## 119                                      Morocco  Lower middle income
    ## 120                                      Moldova  Lower middle income
    ## 121                                   Madagascar           Low income
    ## 122                                     Maldives  Upper middle income
    ## 123                                       Mexico  Upper middle income
    ## 124                             Marshall Islands  Upper middle income
    ## 125                                Middle income                 <NA>
    ## 126                                    Macedonia  Upper middle income
    ## 127                                         Mali           Low income
    ## 128                                        Malta High income: nonOECD
    ## 129                                      Myanmar           Low income
    ## 130 Middle East & North Africa (developing only)                 <NA>
    ## 131                                   Montenegro  Upper middle income
    ## 132                                     Mongolia  Lower middle income
    ## 133                                   Mozambique           Low income
    ## 134                                   Mauritania  Lower middle income
    ## 135                                    Mauritius  Upper middle income
    ## 136                                       Malawi           Low income
    ## 137                                     Malaysia  Upper middle income
    ## 138                                      Namibia  Upper middle income
    ## 139                                        Niger           Low income
    ## 140                                      Nigeria  Lower middle income
    ## 141                                    Nicaragua  Lower middle income
    ## 142                                  Netherlands    High income: OECD
    ## 143                                       Norway    High income: OECD
    ## 144                                        Nepal           Low income
    ## 145                                  New Zealand    High income: OECD
    ## 146                                         Oman High income: nonOECD
    ## 147                                     Pakistan  Lower middle income
    ## 148                                       Panama  Upper middle income
    ## 149                                         Peru  Upper middle income
    ## 150                                  Philippines  Lower middle income
    ## 151                                        Palau  Upper middle income
    ## 152                             Papua New Guinea  Lower middle income
    ## 153                                       Poland    High income: OECD
    ## 154                                  Puerto Rico High income: nonOECD
    ## 155                                     Portugal    High income: OECD
    ## 156                                     Paraguay  Lower middle income
    ## 157                                        Qatar High income: nonOECD
    ## 158                                      Romania  Upper middle income
    ## 159                                       Russia High income: nonOECD
    ## 160                                       Rwanda           Low income
    ## 161                                   South Asia                 <NA>
    ## 162                                 Saudi Arabia High income: nonOECD
    ## 163                                        Sudan  Lower middle income
    ## 164                                      Senegal  Lower middle income
    ## 165                                    Singapore High income: nonOECD
    ## 166                              Solomon Islands  Lower middle income
    ## 167                                 Sierra Leone           Low income
    ## 168                                  El Salvador  Lower middle income
    ## 169                                      Somalia           Low income
    ## 170                                       Serbia  Upper middle income
    ## 171         Sub-Saharan Africa (developing only)                 <NA>
    ## 172                                  South Sudan  Lower middle income
    ## 173                        São Tomé and Principe  Lower middle income
    ## 174                                     Suriname  Upper middle income
    ## 175                              Slovak Republic    High income: OECD
    ## 176                                     Slovenia    High income: OECD
    ## 177                                       Sweden    High income: OECD
    ## 178                                    Swaziland  Lower middle income
    ## 179                                   Seychelles  Upper middle income
    ## 180                                         Chad           Low income
    ## 181                                         Togo           Low income
    ## 182                                     Thailand  Upper middle income
    ## 183                                   Tajikistan           Low income
    ## 184                                 Turkmenistan  Upper middle income
    ## 185                                  Timor-Leste  Lower middle income
    ## 186                                        Tonga  Upper middle income
    ## 187                          Trinidad and Tobago High income: nonOECD
    ## 188                                      Tunisia  Upper middle income
    ## 189                                       Turkey  Upper middle income
    ## 190                                       Tuvalu  Upper middle income
    ## 191                                     Tanzania           Low income
    ## 192                                       Uganda           Low income
    ## 193                                      Ukraine  Lower middle income
    ## 194                          Upper middle income                 <NA>
    ## 195                                      Uruguay High income: nonOECD
    ## 196                                United States    High income: OECD
    ## 197                                   Uzbekistan  Lower middle income
    ## 198               St. Vincent and the Grenadines  Upper middle income
    ## 199                                    Venezuela  Upper middle income
    ## 200                                      Vietnam  Lower middle income
    ## 201                                      Vanuatu  Lower middle income
    ## 202                           West Bank and Gaza  Lower middle income
    ## 203                                        World                 <NA>
    ## 204                                        Samoa  Lower middle income
    ## 205                                        Yemen  Lower middle income
    ## 206                                 South Africa  Upper middle income
    ## 207                              Dem. Rep. Congo           Low income
    ## 208                                       Zambia  Lower middle income
    ## 209                                     Zimbabwe           Low income

### Final Data Cleansing

There are still rows with NA in fields needed for data analysis. The
code below omits them from the cleanset data set.

    #remove rows with NA and create new data set cleanset.filtered
    cleanset.filtered <- cleanset[complete.cases(cleanset),]

There is an extra country variable in the cleanset.filtered dataset,
this needs to be removed. \`\`\`

    #remove column 5 and only keep columns 1,2,3,4, and 6
    cleanset.filtered <- cleanset.filtered[c(1,2,3,4,6)]

    #view data set to validate column 5 have been removed
    cleanset.filtered

    ##     CountryCode Ranking                      Country.x    Economy
    ## 1           ADO     162                        Andorra      3,249
    ## 2           AFG     108                    Afghanistan     20,038
    ## 3           AGO      58                         Angola    138,357
    ## 4           ALB     127                        Albania     13,212
    ## 5           ARE      30           United Arab Emirates    399,451
    ## 6           ARG      24                      Argentina    537,660
    ## 7           ARM     136                        Armenia     11,644
    ## 8           ATG     178            Antigua and Barbuda      1,221
    ## 9           AUS      12                      Australia  1,454,675
    ## 10          AUT      27                        Austria    436,888
    ## 11          AZE      69                     Azerbaijan     75,198
    ## 12          BDI     164                        Burundi      3,094
    ## 13          BEL      25                        Belgium    531,547
    ## 14          BEN     140                          Benin      9,575
    ## 15          BFA     131                   Burkina Faso     12,542
    ## 16          BGD      56                     Bangladesh    172,887
    ## 17          BGR      79                       Bulgaria     56,717
    ## 18          BHR      96                        Bahrain     33,851
    ## 19          BHS     143                   Bahamas, The      8,511
    ## 20          BIH     111         Bosnia and Herzegovina     18,521
    ## 21          BLR      68                        Belarus     76,139
    ## 22          BLZ     173                         Belize      1,699
    ## 23          BMU     151                        Bermuda      5,574
    ## 24          BOL      98                        Bolivia     32,996
    ## 25          BRA       7                         Brazil  2,416,636
    ## 26          BRB     160                       Barbados      4,355
    ## 27          BRN     113              Brunei Darussalam     17,105
    ## 28          BTN     170                         Bhutan      1,959
    ## 29          BWA     119                       Botswana     15,813
    ## 30          CAF     172       Central African Republic      1,723
    ## 31          CAN      11                         Canada  1,785,387
    ## 32          CHE      20                    Switzerland    701,037
    ## 33          CHL      42                          Chile    258,062
    ## 34          CHN       2                          China 10,354,832
    ## 35          CIV      95                  Côte d'Ivoire     34,254
    ## 36          CMR      99                       Cameroon     32,051
    ## 37          COG     123                    Congo, Rep.     14,177
    ## 38          COL      32                       Colombia    377,740
    ## 39          COM     187                        Comoros        624
    ## 40          CPV     171                     Cabo Verde      1,871
    ## 41          CRI      82                     Costa Rica     49,553
    ## 42          CUB      67                           Cuba     77,150
    ## 43          CYP     107                         Cyprus     23,226
    ## 44          CZE      51                 Czech Republic    205,270
    ## 45          DEU       4                        Germany  3,868,291
    ## 46          DJI     174                       Djibouti      1,589
    ## 47          DMA     188                       Dominica        524
    ## 48          DNK      34                        Denmark    342,362
    ## 49          DOM      73             Dominican Republic     64,138
    ## 50          DZA      49                        Algeria    213,518
    ## 53          ECU      63                        Ecuador    100,917
    ## 54          EGY      38               Egypt, Arab Rep.    301,499
    ## 56          ESP      14                          Spain  1,381,342
    ## 57          EST     105                        Estonia     26,485
    ## 58          ETH      80                       Ethiopia     55,612
    ## 59          FIN      41                        Finland    272,217
    ## 60          FJI     157                           Fiji      4,532
    ## 61          FRA       6                         France  2,829,192
    ## 62          FRO     166                  Faroe Islands      2,613
    ## 63          FSM     191          Micronesia, Fed. Sts.        318
    ## 64          GAB     112                          Gabon     18,180
    ## 65          GBR       5                 United Kingdom  2,988,893
    ## 66          GEO     117                        Georgia     16,530
    ## 67          GHA      92                          Ghana     38,617
    ## 68          GIN     149                         Guinea      6,624
    ## 69          GMB     183                    Gambia, The        851
    ## 70          GNB     180                  Guinea-Bissau      1,022
    ## 71          GNQ     121              Equatorial Guinea     15,530
    ## 72          GRC      45                         Greece    235,574
    ## 73          GRD     181                        Grenada        912
    ## 74          GRL     167                      Greenland      2,441
    ## 75          GTM      76                      Guatemala     58,827
    ## 76          GUY     163                         Guyana      3,097
    ## 78          HKG      39           Hong Kong SAR, China    290,896
    ## 79          HND     110                       Honduras     19,385
    ## 80          HRV      78                        Croatia     57,113
    ## 81          HTI     142                          Haiti      8,713
    ## 82          HUN      59                        Hungary    138,347
    ## 83          IDN      16                      Indonesia    888,538
    ## 84          IND       9                          India  2,048,517
    ## 85          IRL      43                        Ireland    250,814
    ## 86          IRN      28             Iran, Islamic Rep.    425,326
    ## 87          IRQ      47                           Iraq    223,500
    ## 88          ISL     114                        Iceland     17,036
    ## 89          ISR      37                         Israel    305,675
    ## 90          ITA       8                          Italy  2,141,161
    ## 91          JAM     125                        Jamaica     13,891
    ## 92          JOR      94                         Jordan     35,827
    ## 93          JPN       3                          Japan  4,601,461
    ## 94          KAZ      48                     Kazakhstan    217,872
    ## 95          KEN      75                          Kenya     60,937
    ## 96          KGZ     147                Kyrgyz Republic      7,404
    ## 97          KHM     116                       Cambodia     16,778
    ## 98          KIR     194                       Kiribati        167
    ## 99          KNA     182            St. Kitts and Nevis        852
    ## 100         KOR      13                    Korea, Rep.  1,410,383
    ## 101         KSV     148                         Kosovo      7,387
    ## 102         KWT      57                         Kuwait    163,612
    ## 104         LAO     134                        Lao PDR     11,997
    ## 105         LBN      89                        Lebanon     45,731
    ## 106         LBR     169                        Liberia      2,013
    ## 107         LBY      91                          Libya     41,143
    ## 108         LCA     177                      St. Lucia      1,404
    ## 110         LIE     152                  Liechtenstein      5,488
    ## 111         LKA      66                      Sri Lanka     78,824
    ## 114         LSO     168                        Lesotho      2,181
    ## 115         LTU      85                      Lithuania     48,354
    ## 116         LUX      71                     Luxembourg     64,874
    ## 117         LVA     100                         Latvia     31,287
    ## 118         MAC      81               Macao SAR, China     55,502
    ## 119         MAR      61                        Morocco    110,009
    ## 120         MDA     145                        Moldova      7,962
    ## 121         MDG     138                     Madagascar     10,593
    ## 122         MDV     165                       Maldives      3,062
    ## 123         MEX      15                         Mexico  1,294,690
    ## 124         MHL     193               Marshall Islands        187
    ## 126         MKD     137                 Macedonia, FYR     11,324
    ## 127         MLI     132                           Mali     12,037
    ## 128         MLT     139                          Malta      9,643
    ## 129         MMR      72                        Myanmar     64,330
    ## 131         MNE     156                     Montenegro      4,588
    ## 132         MNG     133                       Mongolia     12,016
    ## 133         MOZ     118                     Mozambique     15,938
    ## 134         MRT     154                     Mauritania      5,061
    ## 135         MUS     130                      Mauritius     12,630
    ## 136         MWI     161                         Malawi      4,258
    ## 137         MYS      35                       Malaysia    338,104
    ## 138         NAM     128                        Namibia     12,995
    ## 139         NER     144                          Niger      8,169
    ## 140         NGA      22                        Nigeria    568,508
    ## 141         NIC     135                      Nicaragua     11,806
    ## 142         NLD      17                    Netherlands    879,319
    ## 143         NOR      26                         Norway    499,817
    ## 144         NPL     109                          Nepal     19,770
    ## 145         NZL      53                    New Zealand    199,970
    ## 146         OMN      65                           Oman     81,797
    ## 147         PAK      44                       Pakistan    243,632
    ## 148         PAN      88                         Panama     46,213
    ## 149         PER      52                           Peru    202,596
    ## 150         PHL      40                    Philippines    284,777
    ## 151         PLW     192                          Palau        251
    ## 152         PNG     115               Papua New Guinea     16,929
    ## 153         POL      23                         Poland    544,967
    ## 154         PRI      62                    Puerto Rico    103,135
    ## 155         PRT      46                       Portugal    230,117
    ## 156         PRY     101                       Paraguay     30,881
    ## 157         QAT      50                          Qatar    210,109
    ## 158         ROM      54                        Romania    199,044
    ## 159         RUS      10             Russian Federation  1,860,598
    ## 160         RWA     146                         Rwanda      7,890
    ## 162         SAU      19                   Saudi Arabia    753,832
    ## 163         SDN      70                          Sudan     73,815
    ## 164         SEN     120                        Senegal     15,658
    ## 165         SGP      36                      Singapore    307,860
    ## 166         SLB     179                Solomon Islands      1,158
    ## 167         SLE     155                   Sierra Leone      4,838
    ## 168         SLV     106                    El Salvador     25,164
    ## 169         SOM     150                        Somalia      5,707
    ## 170         SRB      90                         Serbia     43,866
    ## 172         SSD     126                    South Sudan     13,282
    ## 173         STP     190          São Tomé and Principe        337
    ## 174         SUR     153                       Suriname      5,210
    ## 175         SVK      64                Slovak Republic    100,249
    ## 176         SVN      83                       Slovenia     49,491
    ## 177         SWE      21                         Sweden    571,090
    ## 178         SWZ     159                      Swaziland      4,413
    ## 179         SYC     175                     Seychelles      1,423
    ## 180         TCD     124                           Chad     13,922
    ## 181         TGO     158                           Togo      4,518
    ## 182         THA      29                       Thailand    404,824
    ## 183         TJK     141                     Tajikistan      9,242
    ## 184         TKM      87                   Turkmenistan     47,932
    ## 185         TMP     176                    Timor-Leste      1,417
    ## 186         TON     189                          Tonga        434
    ## 187         TTO     102            Trinidad and Tobago     28,883
    ## 188         TUN      84                        Tunisia     48,613
    ## 189         TUR      18                         Turkey    798,429
    ## 190         TUV     195                         Tuvalu         38
    ## 191         TZA      86                       Tanzania     48,057
    ## 192         UGA     104                         Uganda     26,998
    ## 193         UKR      60                        Ukraine    131,805
    ## 195         URY      77                        Uruguay     57,471
    ## 196         USA       1                  United States 17,419,000
    ## 197         UZB      74                     Uzbekistan     62,644
    ## 198         VCT     186 St. Vincent and the Grenadines        729
    ## 199         VEN      31                  Venezuela, RB    381,286
    ## 200         VNM      55                        Vietnam    186,205
    ## 201         VUT     184                        Vanuatu        815
    ## 202         WBG     129             West Bank and Gaza     12,738
    ## 204         WSM     185                          Samoa        800
    ## 205         YEM      93                    Yemen, Rep.     35,955
    ## 206         ZAF      33                   South Africa    350,141
    ## 207         ZAR      97               Congo, Dem. Rep.     33,121
    ## 208         ZMB     103                         Zambia     27,066
    ## 209         ZWE     122                       Zimbabwe     14,197
    ##              IncomeGroup
    ## 1   High income: nonOECD
    ## 2             Low income
    ## 3    Upper middle income
    ## 4    Upper middle income
    ## 5   High income: nonOECD
    ## 6    Upper middle income
    ## 7    Lower middle income
    ## 8   High income: nonOECD
    ## 9      High income: OECD
    ## 10     High income: OECD
    ## 11   Upper middle income
    ## 12            Low income
    ## 13     High income: OECD
    ## 14            Low income
    ## 15            Low income
    ## 16            Low income
    ## 17   Upper middle income
    ## 18  High income: nonOECD
    ## 19  High income: nonOECD
    ## 20   Upper middle income
    ## 21   Upper middle income
    ## 22   Upper middle income
    ## 23  High income: nonOECD
    ## 24   Lower middle income
    ## 25   Upper middle income
    ## 26  High income: nonOECD
    ## 27  High income: nonOECD
    ## 28   Lower middle income
    ## 29   Upper middle income
    ## 30            Low income
    ## 31     High income: OECD
    ## 32     High income: OECD
    ## 33     High income: OECD
    ## 34   Upper middle income
    ## 35   Lower middle income
    ## 36   Lower middle income
    ## 37   Lower middle income
    ## 38   Upper middle income
    ## 39            Low income
    ## 40   Lower middle income
    ## 41   Upper middle income
    ## 42   Upper middle income
    ## 43  High income: nonOECD
    ## 44     High income: OECD
    ## 45     High income: OECD
    ## 46   Lower middle income
    ## 47   Upper middle income
    ## 48     High income: OECD
    ## 49   Upper middle income
    ## 50   Upper middle income
    ## 53   Upper middle income
    ## 54   Lower middle income
    ## 56     High income: OECD
    ## 57     High income: OECD
    ## 58            Low income
    ## 59     High income: OECD
    ## 60   Upper middle income
    ## 61     High income: OECD
    ## 62  High income: nonOECD
    ## 63   Lower middle income
    ## 64   Upper middle income
    ## 65     High income: OECD
    ## 66   Lower middle income
    ## 67   Lower middle income
    ## 68            Low income
    ## 69            Low income
    ## 70            Low income
    ## 71  High income: nonOECD
    ## 72     High income: OECD
    ## 73   Upper middle income
    ## 74  High income: nonOECD
    ## 75   Lower middle income
    ## 76   Lower middle income
    ## 78  High income: nonOECD
    ## 79   Lower middle income
    ## 80  High income: nonOECD
    ## 81            Low income
    ## 82   Upper middle income
    ## 83   Lower middle income
    ## 84   Lower middle income
    ## 85     High income: OECD
    ## 86   Upper middle income
    ## 87   Upper middle income
    ## 88     High income: OECD
    ## 89     High income: OECD
    ## 90     High income: OECD
    ## 91   Upper middle income
    ## 92   Upper middle income
    ## 93     High income: OECD
    ## 94   Upper middle income
    ## 95            Low income
    ## 96   Lower middle income
    ## 97            Low income
    ## 98   Lower middle income
    ## 99  High income: nonOECD
    ## 100    High income: OECD
    ## 101  Lower middle income
    ## 102 High income: nonOECD
    ## 104  Lower middle income
    ## 105  Upper middle income
    ## 106           Low income
    ## 107  Upper middle income
    ## 108  Upper middle income
    ## 110 High income: nonOECD
    ## 111  Lower middle income
    ## 114  Lower middle income
    ## 115 High income: nonOECD
    ## 116    High income: OECD
    ## 117 High income: nonOECD
    ## 118 High income: nonOECD
    ## 119  Lower middle income
    ## 120  Lower middle income
    ## 121           Low income
    ## 122  Upper middle income
    ## 123  Upper middle income
    ## 124  Upper middle income
    ## 126  Upper middle income
    ## 127           Low income
    ## 128 High income: nonOECD
    ## 129           Low income
    ## 131  Upper middle income
    ## 132  Lower middle income
    ## 133           Low income
    ## 134  Lower middle income
    ## 135  Upper middle income
    ## 136           Low income
    ## 137  Upper middle income
    ## 138  Upper middle income
    ## 139           Low income
    ## 140  Lower middle income
    ## 141  Lower middle income
    ## 142    High income: OECD
    ## 143    High income: OECD
    ## 144           Low income
    ## 145    High income: OECD
    ## 146 High income: nonOECD
    ## 147  Lower middle income
    ## 148  Upper middle income
    ## 149  Upper middle income
    ## 150  Lower middle income
    ## 151  Upper middle income
    ## 152  Lower middle income
    ## 153    High income: OECD
    ## 154 High income: nonOECD
    ## 155    High income: OECD
    ## 156  Lower middle income
    ## 157 High income: nonOECD
    ## 158  Upper middle income
    ## 159 High income: nonOECD
    ## 160           Low income
    ## 162 High income: nonOECD
    ## 163  Lower middle income
    ## 164  Lower middle income
    ## 165 High income: nonOECD
    ## 166  Lower middle income
    ## 167           Low income
    ## 168  Lower middle income
    ## 169           Low income
    ## 170  Upper middle income
    ## 172  Lower middle income
    ## 173  Lower middle income
    ## 174  Upper middle income
    ## 175    High income: OECD
    ## 176    High income: OECD
    ## 177    High income: OECD
    ## 178  Lower middle income
    ## 179  Upper middle income
    ## 180           Low income
    ## 181           Low income
    ## 182  Upper middle income
    ## 183           Low income
    ## 184  Upper middle income
    ## 185  Lower middle income
    ## 186  Upper middle income
    ## 187 High income: nonOECD
    ## 188  Upper middle income
    ## 189  Upper middle income
    ## 190  Upper middle income
    ## 191           Low income
    ## 192           Low income
    ## 193  Lower middle income
    ## 195 High income: nonOECD
    ## 196    High income: OECD
    ## 197  Lower middle income
    ## 198  Upper middle income
    ## 199  Upper middle income
    ## 200  Lower middle income
    ## 201  Lower middle income
    ## 202  Lower middle income
    ## 204  Lower middle income
    ## 205  Lower middle income
    ## 206  Upper middle income
    ## 207           Low income
    ## 208  Lower middle income
    ## 209           Low income

### Examining Data Types

The data type of the variables is very important, so one more look at
the data frame structure is needed

    #view data type of the data set cleanset.filtered
    str(cleanset.filtered)

    ## 'data.frame':    195 obs. of  5 variables:
    ##  $ CountryCode: chr  "ADO" "AFG" "AGO" "ALB" ...
    ##  $ Ranking    : chr  "162" "108" "58" "127" ...
    ##  $ Country.x  : chr  "Andorra" "Afghanistan" "Angola" "Albania" ...
    ##  $ Economy    : chr  "3,249" "20,038" "138,357" "13,212" ...
    ##  $ IncomeGroup: Factor w/ 5 levels "High income: nonOECD",..: 1 3 5 5 1 5 4 1 2 2 ...
    ##   ..- attr(*, "names")= chr  "3" "4" "5" "6" ...

The Ranking variable is a character field which will make sorting
difficult. The Ranking data type was changes to a numeric data type to
fix this issue.

    #Change data type of Ranking field
    cleanset.filtered$Ranking <- as.numeric(cleanset.filtered$Ranking)

    #validate data type was changed
     str(cleanset.filtered)

    ## 'data.frame':    195 obs. of  5 variables:
    ##  $ CountryCode: chr  "ADO" "AFG" "AGO" "ALB" ...
    ##  $ Ranking    : num  162 108 58 127 30 24 136 178 12 27 ...
    ##  $ Country.x  : chr  "Andorra" "Afghanistan" "Angola" "Albania" ...
    ##  $ Economy    : chr  "3,249" "20,038" "138,357" "13,212" ...
    ##  $ IncomeGroup: Factor w/ 5 levels "High income: nonOECD",..: 1 3 5 5 1 5 4 1 2 2 ...
    ##   ..- attr(*, "names")= chr  "3" "4" "5" "6" ...

The data cleansing is complete.

Questions
=========

The following are a series of questions that need to be answered to get
a clear look at the data to be analyzed.

Question 1:
-----------

Match the data based on the country short code. This was achieved by the
merge function executed earlier (merge( GDA3, CData, by=1:1, all.GDA3 =
TRUE)). The 1:1 tells the r code to match on the first column of each
dataset.

How many IDs matched? Question 1: Match the data based on the country
shortcode. How many of the IDs match?

    #count the number of rows in cleanset.filtered data set
    nrow(cleanset.filtered)

    ## [1] 195

Question 2:
-----------

Sort the data frame in descending order by GDP rank (so the United
States is last)

    #Create new data set cleanset.sorted and sort by Ranking descending
    cleanset.sorted <- cleanset.filtered[order(-cleanset.filtered$Ranking),]

    #view the new data set cleanset.sorted
    cleanset.sorted

    ##     CountryCode Ranking                      Country.x    Economy
    ## 190         TUV     195                         Tuvalu         38
    ## 98          KIR     194                       Kiribati        167
    ## 124         MHL     193               Marshall Islands        187
    ## 151         PLW     192                          Palau        251
    ## 63          FSM     191          Micronesia, Fed. Sts.        318
    ## 173         STP     190          São Tomé and Principe        337
    ## 186         TON     189                          Tonga        434
    ## 47          DMA     188                       Dominica        524
    ## 39          COM     187                        Comoros        624
    ## 198         VCT     186 St. Vincent and the Grenadines        729
    ## 204         WSM     185                          Samoa        800
    ## 201         VUT     184                        Vanuatu        815
    ## 69          GMB     183                    Gambia, The        851
    ## 99          KNA     182            St. Kitts and Nevis        852
    ## 73          GRD     181                        Grenada        912
    ## 70          GNB     180                  Guinea-Bissau      1,022
    ## 166         SLB     179                Solomon Islands      1,158
    ## 8           ATG     178            Antigua and Barbuda      1,221
    ## 108         LCA     177                      St. Lucia      1,404
    ## 185         TMP     176                    Timor-Leste      1,417
    ## 179         SYC     175                     Seychelles      1,423
    ## 46          DJI     174                       Djibouti      1,589
    ## 22          BLZ     173                         Belize      1,699
    ## 30          CAF     172       Central African Republic      1,723
    ## 40          CPV     171                     Cabo Verde      1,871
    ## 28          BTN     170                         Bhutan      1,959
    ## 106         LBR     169                        Liberia      2,013
    ## 114         LSO     168                        Lesotho      2,181
    ## 74          GRL     167                      Greenland      2,441
    ## 62          FRO     166                  Faroe Islands      2,613
    ## 122         MDV     165                       Maldives      3,062
    ## 12          BDI     164                        Burundi      3,094
    ## 76          GUY     163                         Guyana      3,097
    ## 1           ADO     162                        Andorra      3,249
    ## 136         MWI     161                         Malawi      4,258
    ## 26          BRB     160                       Barbados      4,355
    ## 178         SWZ     159                      Swaziland      4,413
    ## 181         TGO     158                           Togo      4,518
    ## 60          FJI     157                           Fiji      4,532
    ## 131         MNE     156                     Montenegro      4,588
    ## 167         SLE     155                   Sierra Leone      4,838
    ## 134         MRT     154                     Mauritania      5,061
    ## 174         SUR     153                       Suriname      5,210
    ## 110         LIE     152                  Liechtenstein      5,488
    ## 23          BMU     151                        Bermuda      5,574
    ## 169         SOM     150                        Somalia      5,707
    ## 68          GIN     149                         Guinea      6,624
    ## 101         KSV     148                         Kosovo      7,387
    ## 96          KGZ     147                Kyrgyz Republic      7,404
    ## 160         RWA     146                         Rwanda      7,890
    ## 120         MDA     145                        Moldova      7,962
    ## 139         NER     144                          Niger      8,169
    ## 19          BHS     143                   Bahamas, The      8,511
    ## 81          HTI     142                          Haiti      8,713
    ## 183         TJK     141                     Tajikistan      9,242
    ## 14          BEN     140                          Benin      9,575
    ## 128         MLT     139                          Malta      9,643
    ## 121         MDG     138                     Madagascar     10,593
    ## 126         MKD     137                 Macedonia, FYR     11,324
    ## 7           ARM     136                        Armenia     11,644
    ## 141         NIC     135                      Nicaragua     11,806
    ## 104         LAO     134                        Lao PDR     11,997
    ## 132         MNG     133                       Mongolia     12,016
    ## 127         MLI     132                           Mali     12,037
    ## 15          BFA     131                   Burkina Faso     12,542
    ## 135         MUS     130                      Mauritius     12,630
    ## 202         WBG     129             West Bank and Gaza     12,738
    ## 138         NAM     128                        Namibia     12,995
    ## 4           ALB     127                        Albania     13,212
    ## 172         SSD     126                    South Sudan     13,282
    ## 91          JAM     125                        Jamaica     13,891
    ## 180         TCD     124                           Chad     13,922
    ## 37          COG     123                    Congo, Rep.     14,177
    ## 209         ZWE     122                       Zimbabwe     14,197
    ## 71          GNQ     121              Equatorial Guinea     15,530
    ## 164         SEN     120                        Senegal     15,658
    ## 29          BWA     119                       Botswana     15,813
    ## 133         MOZ     118                     Mozambique     15,938
    ## 66          GEO     117                        Georgia     16,530
    ## 97          KHM     116                       Cambodia     16,778
    ## 152         PNG     115               Papua New Guinea     16,929
    ## 88          ISL     114                        Iceland     17,036
    ## 27          BRN     113              Brunei Darussalam     17,105
    ## 64          GAB     112                          Gabon     18,180
    ## 20          BIH     111         Bosnia and Herzegovina     18,521
    ## 79          HND     110                       Honduras     19,385
    ## 144         NPL     109                          Nepal     19,770
    ## 2           AFG     108                    Afghanistan     20,038
    ## 43          CYP     107                         Cyprus     23,226
    ## 168         SLV     106                    El Salvador     25,164
    ## 57          EST     105                        Estonia     26,485
    ## 192         UGA     104                         Uganda     26,998
    ## 208         ZMB     103                         Zambia     27,066
    ## 187         TTO     102            Trinidad and Tobago     28,883
    ## 156         PRY     101                       Paraguay     30,881
    ## 117         LVA     100                         Latvia     31,287
    ## 36          CMR      99                       Cameroon     32,051
    ## 24          BOL      98                        Bolivia     32,996
    ## 207         ZAR      97               Congo, Dem. Rep.     33,121
    ## 18          BHR      96                        Bahrain     33,851
    ## 35          CIV      95                  Côte d'Ivoire     34,254
    ## 92          JOR      94                         Jordan     35,827
    ## 205         YEM      93                    Yemen, Rep.     35,955
    ## 67          GHA      92                          Ghana     38,617
    ## 107         LBY      91                          Libya     41,143
    ## 170         SRB      90                         Serbia     43,866
    ## 105         LBN      89                        Lebanon     45,731
    ## 148         PAN      88                         Panama     46,213
    ## 184         TKM      87                   Turkmenistan     47,932
    ## 191         TZA      86                       Tanzania     48,057
    ## 115         LTU      85                      Lithuania     48,354
    ## 188         TUN      84                        Tunisia     48,613
    ## 176         SVN      83                       Slovenia     49,491
    ## 41          CRI      82                     Costa Rica     49,553
    ## 118         MAC      81               Macao SAR, China     55,502
    ## 58          ETH      80                       Ethiopia     55,612
    ## 17          BGR      79                       Bulgaria     56,717
    ## 80          HRV      78                        Croatia     57,113
    ## 195         URY      77                        Uruguay     57,471
    ## 75          GTM      76                      Guatemala     58,827
    ## 95          KEN      75                          Kenya     60,937
    ## 197         UZB      74                     Uzbekistan     62,644
    ## 49          DOM      73             Dominican Republic     64,138
    ## 129         MMR      72                        Myanmar     64,330
    ## 116         LUX      71                     Luxembourg     64,874
    ## 163         SDN      70                          Sudan     73,815
    ## 11          AZE      69                     Azerbaijan     75,198
    ## 21          BLR      68                        Belarus     76,139
    ## 42          CUB      67                           Cuba     77,150
    ## 111         LKA      66                      Sri Lanka     78,824
    ## 146         OMN      65                           Oman     81,797
    ## 175         SVK      64                Slovak Republic    100,249
    ## 53          ECU      63                        Ecuador    100,917
    ## 154         PRI      62                    Puerto Rico    103,135
    ## 119         MAR      61                        Morocco    110,009
    ## 193         UKR      60                        Ukraine    131,805
    ## 82          HUN      59                        Hungary    138,347
    ## 3           AGO      58                         Angola    138,357
    ## 102         KWT      57                         Kuwait    163,612
    ## 16          BGD      56                     Bangladesh    172,887
    ## 200         VNM      55                        Vietnam    186,205
    ## 158         ROM      54                        Romania    199,044
    ## 145         NZL      53                    New Zealand    199,970
    ## 149         PER      52                           Peru    202,596
    ## 44          CZE      51                 Czech Republic    205,270
    ## 157         QAT      50                          Qatar    210,109
    ## 50          DZA      49                        Algeria    213,518
    ## 94          KAZ      48                     Kazakhstan    217,872
    ## 87          IRQ      47                           Iraq    223,500
    ## 155         PRT      46                       Portugal    230,117
    ## 72          GRC      45                         Greece    235,574
    ## 147         PAK      44                       Pakistan    243,632
    ## 85          IRL      43                        Ireland    250,814
    ## 33          CHL      42                          Chile    258,062
    ## 59          FIN      41                        Finland    272,217
    ## 150         PHL      40                    Philippines    284,777
    ## 78          HKG      39           Hong Kong SAR, China    290,896
    ## 54          EGY      38               Egypt, Arab Rep.    301,499
    ## 89          ISR      37                         Israel    305,675
    ## 165         SGP      36                      Singapore    307,860
    ## 137         MYS      35                       Malaysia    338,104
    ## 48          DNK      34                        Denmark    342,362
    ## 206         ZAF      33                   South Africa    350,141
    ## 38          COL      32                       Colombia    377,740
    ## 199         VEN      31                  Venezuela, RB    381,286
    ## 5           ARE      30           United Arab Emirates    399,451
    ## 182         THA      29                       Thailand    404,824
    ## 86          IRN      28             Iran, Islamic Rep.    425,326
    ## 10          AUT      27                        Austria    436,888
    ## 143         NOR      26                         Norway    499,817
    ## 13          BEL      25                        Belgium    531,547
    ## 6           ARG      24                      Argentina    537,660
    ## 153         POL      23                         Poland    544,967
    ## 140         NGA      22                        Nigeria    568,508
    ## 177         SWE      21                         Sweden    571,090
    ## 32          CHE      20                    Switzerland    701,037
    ## 162         SAU      19                   Saudi Arabia    753,832
    ## 189         TUR      18                         Turkey    798,429
    ## 142         NLD      17                    Netherlands    879,319
    ## 83          IDN      16                      Indonesia    888,538
    ## 123         MEX      15                         Mexico  1,294,690
    ## 56          ESP      14                          Spain  1,381,342
    ## 100         KOR      13                    Korea, Rep.  1,410,383
    ## 9           AUS      12                      Australia  1,454,675
    ## 31          CAN      11                         Canada  1,785,387
    ## 159         RUS      10             Russian Federation  1,860,598
    ## 84          IND       9                          India  2,048,517
    ## 90          ITA       8                          Italy  2,141,161
    ## 25          BRA       7                         Brazil  2,416,636
    ## 61          FRA       6                         France  2,829,192
    ## 65          GBR       5                 United Kingdom  2,988,893
    ## 45          DEU       4                        Germany  3,868,291
    ## 93          JPN       3                          Japan  4,601,461
    ## 34          CHN       2                          China 10,354,832
    ## 196         USA       1                  United States 17,419,000
    ##              IncomeGroup
    ## 190  Upper middle income
    ## 98   Lower middle income
    ## 124  Upper middle income
    ## 151  Upper middle income
    ## 63   Lower middle income
    ## 173  Lower middle income
    ## 186  Upper middle income
    ## 47   Upper middle income
    ## 39            Low income
    ## 198  Upper middle income
    ## 204  Lower middle income
    ## 201  Lower middle income
    ## 69            Low income
    ## 99  High income: nonOECD
    ## 73   Upper middle income
    ## 70            Low income
    ## 166  Lower middle income
    ## 8   High income: nonOECD
    ## 108  Upper middle income
    ## 185  Lower middle income
    ## 179  Upper middle income
    ## 46   Lower middle income
    ## 22   Upper middle income
    ## 30            Low income
    ## 40   Lower middle income
    ## 28   Lower middle income
    ## 106           Low income
    ## 114  Lower middle income
    ## 74  High income: nonOECD
    ## 62  High income: nonOECD
    ## 122  Upper middle income
    ## 12            Low income
    ## 76   Lower middle income
    ## 1   High income: nonOECD
    ## 136           Low income
    ## 26  High income: nonOECD
    ## 178  Lower middle income
    ## 181           Low income
    ## 60   Upper middle income
    ## 131  Upper middle income
    ## 167           Low income
    ## 134  Lower middle income
    ## 174  Upper middle income
    ## 110 High income: nonOECD
    ## 23  High income: nonOECD
    ## 169           Low income
    ## 68            Low income
    ## 101  Lower middle income
    ## 96   Lower middle income
    ## 160           Low income
    ## 120  Lower middle income
    ## 139           Low income
    ## 19  High income: nonOECD
    ## 81            Low income
    ## 183           Low income
    ## 14            Low income
    ## 128 High income: nonOECD
    ## 121           Low income
    ## 126  Upper middle income
    ## 7    Lower middle income
    ## 141  Lower middle income
    ## 104  Lower middle income
    ## 132  Lower middle income
    ## 127           Low income
    ## 15            Low income
    ## 135  Upper middle income
    ## 202  Lower middle income
    ## 138  Upper middle income
    ## 4    Upper middle income
    ## 172  Lower middle income
    ## 91   Upper middle income
    ## 180           Low income
    ## 37   Lower middle income
    ## 209           Low income
    ## 71  High income: nonOECD
    ## 164  Lower middle income
    ## 29   Upper middle income
    ## 133           Low income
    ## 66   Lower middle income
    ## 97            Low income
    ## 152  Lower middle income
    ## 88     High income: OECD
    ## 27  High income: nonOECD
    ## 64   Upper middle income
    ## 20   Upper middle income
    ## 79   Lower middle income
    ## 144           Low income
    ## 2             Low income
    ## 43  High income: nonOECD
    ## 168  Lower middle income
    ## 57     High income: OECD
    ## 192           Low income
    ## 208  Lower middle income
    ## 187 High income: nonOECD
    ## 156  Lower middle income
    ## 117 High income: nonOECD
    ## 36   Lower middle income
    ## 24   Lower middle income
    ## 207           Low income
    ## 18  High income: nonOECD
    ## 35   Lower middle income
    ## 92   Upper middle income
    ## 205  Lower middle income
    ## 67   Lower middle income
    ## 107  Upper middle income
    ## 170  Upper middle income
    ## 105  Upper middle income
    ## 148  Upper middle income
    ## 184  Upper middle income
    ## 191           Low income
    ## 115 High income: nonOECD
    ## 188  Upper middle income
    ## 176    High income: OECD
    ## 41   Upper middle income
    ## 118 High income: nonOECD
    ## 58            Low income
    ## 17   Upper middle income
    ## 80  High income: nonOECD
    ## 195 High income: nonOECD
    ## 75   Lower middle income
    ## 95            Low income
    ## 197  Lower middle income
    ## 49   Upper middle income
    ## 129           Low income
    ## 116    High income: OECD
    ## 163  Lower middle income
    ## 11   Upper middle income
    ## 21   Upper middle income
    ## 42   Upper middle income
    ## 111  Lower middle income
    ## 146 High income: nonOECD
    ## 175    High income: OECD
    ## 53   Upper middle income
    ## 154 High income: nonOECD
    ## 119  Lower middle income
    ## 193  Lower middle income
    ## 82   Upper middle income
    ## 3    Upper middle income
    ## 102 High income: nonOECD
    ## 16            Low income
    ## 200  Lower middle income
    ## 158  Upper middle income
    ## 145    High income: OECD
    ## 149  Upper middle income
    ## 44     High income: OECD
    ## 157 High income: nonOECD
    ## 50   Upper middle income
    ## 94   Upper middle income
    ## 87   Upper middle income
    ## 155    High income: OECD
    ## 72     High income: OECD
    ## 147  Lower middle income
    ## 85     High income: OECD
    ## 33     High income: OECD
    ## 59     High income: OECD
    ## 150  Lower middle income
    ## 78  High income: nonOECD
    ## 54   Lower middle income
    ## 89     High income: OECD
    ## 165 High income: nonOECD
    ## 137  Upper middle income
    ## 48     High income: OECD
    ## 206  Upper middle income
    ## 38   Upper middle income
    ## 199  Upper middle income
    ## 5   High income: nonOECD
    ## 182  Upper middle income
    ## 86   Upper middle income
    ## 10     High income: OECD
    ## 143    High income: OECD
    ## 13     High income: OECD
    ## 6    Upper middle income
    ## 153    High income: OECD
    ## 140  Lower middle income
    ## 177    High income: OECD
    ## 32     High income: OECD
    ## 162 High income: nonOECD
    ## 189  Upper middle income
    ## 142    High income: OECD
    ## 83   Lower middle income
    ## 123  Upper middle income
    ## 56     High income: OECD
    ## 100    High income: OECD
    ## 9      High income: OECD
    ## 31     High income: OECD
    ## 159 High income: nonOECD
    ## 84   Lower middle income
    ## 90     High income: OECD
    ## 25   Upper middle income
    ## 61     High income: OECD
    ## 65     High income: OECD
    ## 45     High income: OECD
    ## 93     High income: OECD
    ## 34   Upper middle income
    ## 196    High income: OECD

What is the 13th country in the resulting data frame?

    #Make sure the data frame is sorted on the Ranking variable
    cleanset.sorted <- cleanset.sorted[order(-cleanset.sorted$Ranking),]

    #display 13th row
    cleanset.sorted[13,]

    ##    CountryCode Ranking   Country.x Economy IncomeGroup
    ## 69         GMB     183 Gambia, The     851  Low income

Question 3:
-----------

What are the average GDP rankings for the "High income: OECD" and the
"High income: nonOECD" groups?

    #calculate mean of IncomeGroup where it equals "High Income: OECD"
    highincome1 <- mean(cleanset.sorted$Ranking[cleanset.sorted$IncomeGroup == "High income: OECD"])

    #View value
    highincome1

    ## [1] 34.35484

Average for "High Income: NonOECD" Group:

    #calculate mean of IncomeGroup where it equals "High Income: nonOECD"
    highincome2 <- mean(cleanset.sorted$Ranking[cleanset.sorted$IncomeGroup == "High income: nonOECD"])

    #View value
    highincome2

    ## [1] 100.9655

Question 4:
-----------

Plot the GDP for all the countries. Use ggplot to color your plot by
Income Group

    #Create a new variable by parsing the IncomeGroup field and assigning numeric values
    cleanset.sorted$IncomeBreak <- ifelse(cleanset.sorted$IncomeGroup == "Upper middle income", 
    +                             c(1), ifelse(cleanset.sorted$IncomeGroup == "Lower middle income",c(2),ifelse(cleanset.sorted$IncomeGroup == "Low income", c(3), ifelse(cleanset.sorted$IncomeGroup == "High income: nonOECD", c(4), ifelse(cleanset.sorted$IncomeGroup == "High income: OECD", c(5), NA)))))

    #view dataset
    cleanset.sorted

    ##     CountryCode Ranking                      Country.x    Economy
    ## 190         TUV     195                         Tuvalu         38
    ## 98          KIR     194                       Kiribati        167
    ## 124         MHL     193               Marshall Islands        187
    ## 151         PLW     192                          Palau        251
    ## 63          FSM     191          Micronesia, Fed. Sts.        318
    ## 173         STP     190          São Tomé and Principe        337
    ## 186         TON     189                          Tonga        434
    ## 47          DMA     188                       Dominica        524
    ## 39          COM     187                        Comoros        624
    ## 198         VCT     186 St. Vincent and the Grenadines        729
    ## 204         WSM     185                          Samoa        800
    ## 201         VUT     184                        Vanuatu        815
    ## 69          GMB     183                    Gambia, The        851
    ## 99          KNA     182            St. Kitts and Nevis        852
    ## 73          GRD     181                        Grenada        912
    ## 70          GNB     180                  Guinea-Bissau      1,022
    ## 166         SLB     179                Solomon Islands      1,158
    ## 8           ATG     178            Antigua and Barbuda      1,221
    ## 108         LCA     177                      St. Lucia      1,404
    ## 185         TMP     176                    Timor-Leste      1,417
    ## 179         SYC     175                     Seychelles      1,423
    ## 46          DJI     174                       Djibouti      1,589
    ## 22          BLZ     173                         Belize      1,699
    ## 30          CAF     172       Central African Republic      1,723
    ## 40          CPV     171                     Cabo Verde      1,871
    ## 28          BTN     170                         Bhutan      1,959
    ## 106         LBR     169                        Liberia      2,013
    ## 114         LSO     168                        Lesotho      2,181
    ## 74          GRL     167                      Greenland      2,441
    ## 62          FRO     166                  Faroe Islands      2,613
    ## 122         MDV     165                       Maldives      3,062
    ## 12          BDI     164                        Burundi      3,094
    ## 76          GUY     163                         Guyana      3,097
    ## 1           ADO     162                        Andorra      3,249
    ## 136         MWI     161                         Malawi      4,258
    ## 26          BRB     160                       Barbados      4,355
    ## 178         SWZ     159                      Swaziland      4,413
    ## 181         TGO     158                           Togo      4,518
    ## 60          FJI     157                           Fiji      4,532
    ## 131         MNE     156                     Montenegro      4,588
    ## 167         SLE     155                   Sierra Leone      4,838
    ## 134         MRT     154                     Mauritania      5,061
    ## 174         SUR     153                       Suriname      5,210
    ## 110         LIE     152                  Liechtenstein      5,488
    ## 23          BMU     151                        Bermuda      5,574
    ## 169         SOM     150                        Somalia      5,707
    ## 68          GIN     149                         Guinea      6,624
    ## 101         KSV     148                         Kosovo      7,387
    ## 96          KGZ     147                Kyrgyz Republic      7,404
    ## 160         RWA     146                         Rwanda      7,890
    ## 120         MDA     145                        Moldova      7,962
    ## 139         NER     144                          Niger      8,169
    ## 19          BHS     143                   Bahamas, The      8,511
    ## 81          HTI     142                          Haiti      8,713
    ## 183         TJK     141                     Tajikistan      9,242
    ## 14          BEN     140                          Benin      9,575
    ## 128         MLT     139                          Malta      9,643
    ## 121         MDG     138                     Madagascar     10,593
    ## 126         MKD     137                 Macedonia, FYR     11,324
    ## 7           ARM     136                        Armenia     11,644
    ## 141         NIC     135                      Nicaragua     11,806
    ## 104         LAO     134                        Lao PDR     11,997
    ## 132         MNG     133                       Mongolia     12,016
    ## 127         MLI     132                           Mali     12,037
    ## 15          BFA     131                   Burkina Faso     12,542
    ## 135         MUS     130                      Mauritius     12,630
    ## 202         WBG     129             West Bank and Gaza     12,738
    ## 138         NAM     128                        Namibia     12,995
    ## 4           ALB     127                        Albania     13,212
    ## 172         SSD     126                    South Sudan     13,282
    ## 91          JAM     125                        Jamaica     13,891
    ## 180         TCD     124                           Chad     13,922
    ## 37          COG     123                    Congo, Rep.     14,177
    ## 209         ZWE     122                       Zimbabwe     14,197
    ## 71          GNQ     121              Equatorial Guinea     15,530
    ## 164         SEN     120                        Senegal     15,658
    ## 29          BWA     119                       Botswana     15,813
    ## 133         MOZ     118                     Mozambique     15,938
    ## 66          GEO     117                        Georgia     16,530
    ## 97          KHM     116                       Cambodia     16,778
    ## 152         PNG     115               Papua New Guinea     16,929
    ## 88          ISL     114                        Iceland     17,036
    ## 27          BRN     113              Brunei Darussalam     17,105
    ## 64          GAB     112                          Gabon     18,180
    ## 20          BIH     111         Bosnia and Herzegovina     18,521
    ## 79          HND     110                       Honduras     19,385
    ## 144         NPL     109                          Nepal     19,770
    ## 2           AFG     108                    Afghanistan     20,038
    ## 43          CYP     107                         Cyprus     23,226
    ## 168         SLV     106                    El Salvador     25,164
    ## 57          EST     105                        Estonia     26,485
    ## 192         UGA     104                         Uganda     26,998
    ## 208         ZMB     103                         Zambia     27,066
    ## 187         TTO     102            Trinidad and Tobago     28,883
    ## 156         PRY     101                       Paraguay     30,881
    ## 117         LVA     100                         Latvia     31,287
    ## 36          CMR      99                       Cameroon     32,051
    ## 24          BOL      98                        Bolivia     32,996
    ## 207         ZAR      97               Congo, Dem. Rep.     33,121
    ## 18          BHR      96                        Bahrain     33,851
    ## 35          CIV      95                  Côte d'Ivoire     34,254
    ## 92          JOR      94                         Jordan     35,827
    ## 205         YEM      93                    Yemen, Rep.     35,955
    ## 67          GHA      92                          Ghana     38,617
    ## 107         LBY      91                          Libya     41,143
    ## 170         SRB      90                         Serbia     43,866
    ## 105         LBN      89                        Lebanon     45,731
    ## 148         PAN      88                         Panama     46,213
    ## 184         TKM      87                   Turkmenistan     47,932
    ## 191         TZA      86                       Tanzania     48,057
    ## 115         LTU      85                      Lithuania     48,354
    ## 188         TUN      84                        Tunisia     48,613
    ## 176         SVN      83                       Slovenia     49,491
    ## 41          CRI      82                     Costa Rica     49,553
    ## 118         MAC      81               Macao SAR, China     55,502
    ## 58          ETH      80                       Ethiopia     55,612
    ## 17          BGR      79                       Bulgaria     56,717
    ## 80          HRV      78                        Croatia     57,113
    ## 195         URY      77                        Uruguay     57,471
    ## 75          GTM      76                      Guatemala     58,827
    ## 95          KEN      75                          Kenya     60,937
    ## 197         UZB      74                     Uzbekistan     62,644
    ## 49          DOM      73             Dominican Republic     64,138
    ## 129         MMR      72                        Myanmar     64,330
    ## 116         LUX      71                     Luxembourg     64,874
    ## 163         SDN      70                          Sudan     73,815
    ## 11          AZE      69                     Azerbaijan     75,198
    ## 21          BLR      68                        Belarus     76,139
    ## 42          CUB      67                           Cuba     77,150
    ## 111         LKA      66                      Sri Lanka     78,824
    ## 146         OMN      65                           Oman     81,797
    ## 175         SVK      64                Slovak Republic    100,249
    ## 53          ECU      63                        Ecuador    100,917
    ## 154         PRI      62                    Puerto Rico    103,135
    ## 119         MAR      61                        Morocco    110,009
    ## 193         UKR      60                        Ukraine    131,805
    ## 82          HUN      59                        Hungary    138,347
    ## 3           AGO      58                         Angola    138,357
    ## 102         KWT      57                         Kuwait    163,612
    ## 16          BGD      56                     Bangladesh    172,887
    ## 200         VNM      55                        Vietnam    186,205
    ## 158         ROM      54                        Romania    199,044
    ## 145         NZL      53                    New Zealand    199,970
    ## 149         PER      52                           Peru    202,596
    ## 44          CZE      51                 Czech Republic    205,270
    ## 157         QAT      50                          Qatar    210,109
    ## 50          DZA      49                        Algeria    213,518
    ## 94          KAZ      48                     Kazakhstan    217,872
    ## 87          IRQ      47                           Iraq    223,500
    ## 155         PRT      46                       Portugal    230,117
    ## 72          GRC      45                         Greece    235,574
    ## 147         PAK      44                       Pakistan    243,632
    ## 85          IRL      43                        Ireland    250,814
    ## 33          CHL      42                          Chile    258,062
    ## 59          FIN      41                        Finland    272,217
    ## 150         PHL      40                    Philippines    284,777
    ## 78          HKG      39           Hong Kong SAR, China    290,896
    ## 54          EGY      38               Egypt, Arab Rep.    301,499
    ## 89          ISR      37                         Israel    305,675
    ## 165         SGP      36                      Singapore    307,860
    ## 137         MYS      35                       Malaysia    338,104
    ## 48          DNK      34                        Denmark    342,362
    ## 206         ZAF      33                   South Africa    350,141
    ## 38          COL      32                       Colombia    377,740
    ## 199         VEN      31                  Venezuela, RB    381,286
    ## 5           ARE      30           United Arab Emirates    399,451
    ## 182         THA      29                       Thailand    404,824
    ## 86          IRN      28             Iran, Islamic Rep.    425,326
    ## 10          AUT      27                        Austria    436,888
    ## 143         NOR      26                         Norway    499,817
    ## 13          BEL      25                        Belgium    531,547
    ## 6           ARG      24                      Argentina    537,660
    ## 153         POL      23                         Poland    544,967
    ## 140         NGA      22                        Nigeria    568,508
    ## 177         SWE      21                         Sweden    571,090
    ## 32          CHE      20                    Switzerland    701,037
    ## 162         SAU      19                   Saudi Arabia    753,832
    ## 189         TUR      18                         Turkey    798,429
    ## 142         NLD      17                    Netherlands    879,319
    ## 83          IDN      16                      Indonesia    888,538
    ## 123         MEX      15                         Mexico  1,294,690
    ## 56          ESP      14                          Spain  1,381,342
    ## 100         KOR      13                    Korea, Rep.  1,410,383
    ## 9           AUS      12                      Australia  1,454,675
    ## 31          CAN      11                         Canada  1,785,387
    ## 159         RUS      10             Russian Federation  1,860,598
    ## 84          IND       9                          India  2,048,517
    ## 90          ITA       8                          Italy  2,141,161
    ## 25          BRA       7                         Brazil  2,416,636
    ## 61          FRA       6                         France  2,829,192
    ## 65          GBR       5                 United Kingdom  2,988,893
    ## 45          DEU       4                        Germany  3,868,291
    ## 93          JPN       3                          Japan  4,601,461
    ## 34          CHN       2                          China 10,354,832
    ## 196         USA       1                  United States 17,419,000
    ##              IncomeGroup IncomeBreak
    ## 190  Upper middle income           1
    ## 98   Lower middle income           2
    ## 124  Upper middle income           1
    ## 151  Upper middle income           1
    ## 63   Lower middle income           2
    ## 173  Lower middle income           2
    ## 186  Upper middle income           1
    ## 47   Upper middle income           1
    ## 39            Low income           3
    ## 198  Upper middle income           1
    ## 204  Lower middle income           2
    ## 201  Lower middle income           2
    ## 69            Low income           3
    ## 99  High income: nonOECD           4
    ## 73   Upper middle income           1
    ## 70            Low income           3
    ## 166  Lower middle income           2
    ## 8   High income: nonOECD           4
    ## 108  Upper middle income           1
    ## 185  Lower middle income           2
    ## 179  Upper middle income           1
    ## 46   Lower middle income           2
    ## 22   Upper middle income           1
    ## 30            Low income           3
    ## 40   Lower middle income           2
    ## 28   Lower middle income           2
    ## 106           Low income           3
    ## 114  Lower middle income           2
    ## 74  High income: nonOECD           4
    ## 62  High income: nonOECD           4
    ## 122  Upper middle income           1
    ## 12            Low income           3
    ## 76   Lower middle income           2
    ## 1   High income: nonOECD           4
    ## 136           Low income           3
    ## 26  High income: nonOECD           4
    ## 178  Lower middle income           2
    ## 181           Low income           3
    ## 60   Upper middle income           1
    ## 131  Upper middle income           1
    ## 167           Low income           3
    ## 134  Lower middle income           2
    ## 174  Upper middle income           1
    ## 110 High income: nonOECD           4
    ## 23  High income: nonOECD           4
    ## 169           Low income           3
    ## 68            Low income           3
    ## 101  Lower middle income           2
    ## 96   Lower middle income           2
    ## 160           Low income           3
    ## 120  Lower middle income           2
    ## 139           Low income           3
    ## 19  High income: nonOECD           4
    ## 81            Low income           3
    ## 183           Low income           3
    ## 14            Low income           3
    ## 128 High income: nonOECD           4
    ## 121           Low income           3
    ## 126  Upper middle income           1
    ## 7    Lower middle income           2
    ## 141  Lower middle income           2
    ## 104  Lower middle income           2
    ## 132  Lower middle income           2
    ## 127           Low income           3
    ## 15            Low income           3
    ## 135  Upper middle income           1
    ## 202  Lower middle income           2
    ## 138  Upper middle income           1
    ## 4    Upper middle income           1
    ## 172  Lower middle income           2
    ## 91   Upper middle income           1
    ## 180           Low income           3
    ## 37   Lower middle income           2
    ## 209           Low income           3
    ## 71  High income: nonOECD           4
    ## 164  Lower middle income           2
    ## 29   Upper middle income           1
    ## 133           Low income           3
    ## 66   Lower middle income           2
    ## 97            Low income           3
    ## 152  Lower middle income           2
    ## 88     High income: OECD           5
    ## 27  High income: nonOECD           4
    ## 64   Upper middle income           1
    ## 20   Upper middle income           1
    ## 79   Lower middle income           2
    ## 144           Low income           3
    ## 2             Low income           3
    ## 43  High income: nonOECD           4
    ## 168  Lower middle income           2
    ## 57     High income: OECD           5
    ## 192           Low income           3
    ## 208  Lower middle income           2
    ## 187 High income: nonOECD           4
    ## 156  Lower middle income           2
    ## 117 High income: nonOECD           4
    ## 36   Lower middle income           2
    ## 24   Lower middle income           2
    ## 207           Low income           3
    ## 18  High income: nonOECD           4
    ## 35   Lower middle income           2
    ## 92   Upper middle income           1
    ## 205  Lower middle income           2
    ## 67   Lower middle income           2
    ## 107  Upper middle income           1
    ## 170  Upper middle income           1
    ## 105  Upper middle income           1
    ## 148  Upper middle income           1
    ## 184  Upper middle income           1
    ## 191           Low income           3
    ## 115 High income: nonOECD           4
    ## 188  Upper middle income           1
    ## 176    High income: OECD           5
    ## 41   Upper middle income           1
    ## 118 High income: nonOECD           4
    ## 58            Low income           3
    ## 17   Upper middle income           1
    ## 80  High income: nonOECD           4
    ## 195 High income: nonOECD           4
    ## 75   Lower middle income           2
    ## 95            Low income           3
    ## 197  Lower middle income           2
    ## 49   Upper middle income           1
    ## 129           Low income           3
    ## 116    High income: OECD           5
    ## 163  Lower middle income           2
    ## 11   Upper middle income           1
    ## 21   Upper middle income           1
    ## 42   Upper middle income           1
    ## 111  Lower middle income           2
    ## 146 High income: nonOECD           4
    ## 175    High income: OECD           5
    ## 53   Upper middle income           1
    ## 154 High income: nonOECD           4
    ## 119  Lower middle income           2
    ## 193  Lower middle income           2
    ## 82   Upper middle income           1
    ## 3    Upper middle income           1
    ## 102 High income: nonOECD           4
    ## 16            Low income           3
    ## 200  Lower middle income           2
    ## 158  Upper middle income           1
    ## 145    High income: OECD           5
    ## 149  Upper middle income           1
    ## 44     High income: OECD           5
    ## 157 High income: nonOECD           4
    ## 50   Upper middle income           1
    ## 94   Upper middle income           1
    ## 87   Upper middle income           1
    ## 155    High income: OECD           5
    ## 72     High income: OECD           5
    ## 147  Lower middle income           2
    ## 85     High income: OECD           5
    ## 33     High income: OECD           5
    ## 59     High income: OECD           5
    ## 150  Lower middle income           2
    ## 78  High income: nonOECD           4
    ## 54   Lower middle income           2
    ## 89     High income: OECD           5
    ## 165 High income: nonOECD           4
    ## 137  Upper middle income           1
    ## 48     High income: OECD           5
    ## 206  Upper middle income           1
    ## 38   Upper middle income           1
    ## 199  Upper middle income           1
    ## 5   High income: nonOECD           4
    ## 182  Upper middle income           1
    ## 86   Upper middle income           1
    ## 10     High income: OECD           5
    ## 143    High income: OECD           5
    ## 13     High income: OECD           5
    ## 6    Upper middle income           1
    ## 153    High income: OECD           5
    ## 140  Lower middle income           2
    ## 177    High income: OECD           5
    ## 32     High income: OECD           5
    ## 162 High income: nonOECD           4
    ## 189  Upper middle income           1
    ## 142    High income: OECD           5
    ## 83   Lower middle income           2
    ## 123  Upper middle income           1
    ## 56     High income: OECD           5
    ## 100    High income: OECD           5
    ## 9      High income: OECD           5
    ## 31     High income: OECD           5
    ## 159 High income: nonOECD           4
    ## 84   Lower middle income           2
    ## 90     High income: OECD           5
    ## 25   Upper middle income           1
    ## 61     High income: OECD           5
    ## 65     High income: OECD           5
    ## 45     High income: OECD           5
    ## 93     High income: OECD           5
    ## 34   Upper middle income           1
    ## 196    High income: OECD           5

In order to provide plots of the data that include the Economy variable,
the data type needed to be changed from a character to numeric.

    #convert Economy variable to a numeric data type but first remove the commas.
    cleanset.sorted$Economy <- as.numeric(gsub(",", "", cleanset.sorted$Economy))

    #check the data type to make sure 
    str(cleanset.sorted)

    ## 'data.frame':    195 obs. of  6 variables:
    ##  $ CountryCode: chr  "TUV" "KIR" "MHL" "PLW" ...
    ##  $ Ranking    : num  195 194 193 192 191 190 189 188 187 186 ...
    ##  $ Country.x  : chr  "Tuvalu" "Kiribati" "Marshall Islands" "Palau" ...
    ##  $ Economy    : num  38 167 187 251 318 337 434 524 624 729 ...
    ##  $ IncomeGroup: Factor w/ 5 levels "High income: nonOECD",..: 5 4 5 5 4 4 5 5 3 5 ...
    ##   ..- attr(*, "names")= chr  "222" "112" "143" "176" ...
    ##  $ IncomeBreak: num  1 2 1 1 2 2 1 1 3 1 ...

Plot of Countries by Income Group

    library(ggplot2)
    #Creates ggplot 
    Dataplot <- ggplot(data = cleanset.sorted,aes( x = cleanset.sorted$CountryCode, y = cleanset.sorted$Ranking, group = 1))+geom_point(aes(color=factor(cleanset.sorted$IncomeGroup)))+theme(legend.title = element_text(colour = "chocolate", size = 10, face="bold")) +theme(axis.text.x=element_text(angle= 50, size=10,vjust = 0.5))+scale_color_brewer(palette = "Set1", name="Country Income Groups")+labs(x = "Country Code", y = "Country Rank")+scale_x_discrete(breaks = seq(cleanset.sorted$CountryQuartile))

    #Displays plot
    Dataplot

![](CaseStudy6_files/figure-markdown_strict/unnamed-chunk-25-1.png)<!-- -->

### Interpertation of Plot

The Income Group correlates with the GDP ranking for the country. There
are some high ranking countries that have Low to Low middle income
groupings.

Question 5:
-----------

Cut the GDP ranking in 5 separate quantile groups. Make a table versus
Income Group.

### First create Income Group table

    #Create income group table with variables Ranking, IncomeGroup, and CountryQuartile variables
    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    cleanset.sorted$CountryQuartile <- ntile(cleanset.sorted$Ranking,5)

    IncomeGoups <- cleanset.sorted[c(2,5,7)]

    #View data in IncomeGroups Table
    IncomeGoups

    ##     Ranking          IncomeGroup CountryQuartile
    ## 190     195  Upper middle income               5
    ## 98      194  Lower middle income               5
    ## 124     193  Upper middle income               5
    ## 151     192  Upper middle income               5
    ## 63      191  Lower middle income               5
    ## 173     190  Lower middle income               5
    ## 186     189  Upper middle income               5
    ## 47      188  Upper middle income               5
    ## 39      187           Low income               5
    ## 198     186  Upper middle income               5
    ## 204     185  Lower middle income               5
    ## 201     184  Lower middle income               5
    ## 69      183           Low income               5
    ## 99      182 High income: nonOECD               5
    ## 73      181  Upper middle income               5
    ## 70      180           Low income               5
    ## 166     179  Lower middle income               5
    ## 8       178 High income: nonOECD               5
    ## 108     177  Upper middle income               5
    ## 185     176  Lower middle income               5
    ## 179     175  Upper middle income               5
    ## 46      174  Lower middle income               5
    ## 22      173  Upper middle income               5
    ## 30      172           Low income               5
    ## 40      171  Lower middle income               5
    ## 28      170  Lower middle income               5
    ## 106     169           Low income               5
    ## 114     168  Lower middle income               5
    ## 74      167 High income: nonOECD               5
    ## 62      166 High income: nonOECD               5
    ## 122     165  Upper middle income               5
    ## 12      164           Low income               5
    ## 76      163  Lower middle income               5
    ## 1       162 High income: nonOECD               5
    ## 136     161           Low income               5
    ## 26      160 High income: nonOECD               5
    ## 178     159  Lower middle income               5
    ## 181     158           Low income               5
    ## 60      157  Upper middle income               5
    ## 131     156  Upper middle income               4
    ## 167     155           Low income               4
    ## 134     154  Lower middle income               4
    ## 174     153  Upper middle income               4
    ## 110     152 High income: nonOECD               4
    ## 23      151 High income: nonOECD               4
    ## 169     150           Low income               4
    ## 68      149           Low income               4
    ## 101     148  Lower middle income               4
    ## 96      147  Lower middle income               4
    ## 160     146           Low income               4
    ## 120     145  Lower middle income               4
    ## 139     144           Low income               4
    ## 19      143 High income: nonOECD               4
    ## 81      142           Low income               4
    ## 183     141           Low income               4
    ## 14      140           Low income               4
    ## 128     139 High income: nonOECD               4
    ## 121     138           Low income               4
    ## 126     137  Upper middle income               4
    ## 7       136  Lower middle income               4
    ## 141     135  Lower middle income               4
    ## 104     134  Lower middle income               4
    ## 132     133  Lower middle income               4
    ## 127     132           Low income               4
    ## 15      131           Low income               4
    ## 135     130  Upper middle income               4
    ## 202     129  Lower middle income               4
    ## 138     128  Upper middle income               4
    ## 4       127  Upper middle income               4
    ## 172     126  Lower middle income               4
    ## 91      125  Upper middle income               4
    ## 180     124           Low income               4
    ## 37      123  Lower middle income               4
    ## 209     122           Low income               4
    ## 71      121 High income: nonOECD               4
    ## 164     120  Lower middle income               4
    ## 29      119  Upper middle income               4
    ## 133     118           Low income               4
    ## 66      117  Lower middle income               3
    ## 97      116           Low income               3
    ## 152     115  Lower middle income               3
    ## 88      114    High income: OECD               3
    ## 27      113 High income: nonOECD               3
    ## 64      112  Upper middle income               3
    ## 20      111  Upper middle income               3
    ## 79      110  Lower middle income               3
    ## 144     109           Low income               3
    ## 2       108           Low income               3
    ## 43      107 High income: nonOECD               3
    ## 168     106  Lower middle income               3
    ## 57      105    High income: OECD               3
    ## 192     104           Low income               3
    ## 208     103  Lower middle income               3
    ## 187     102 High income: nonOECD               3
    ## 156     101  Lower middle income               3
    ## 117     100 High income: nonOECD               3
    ## 36       99  Lower middle income               3
    ## 24       98  Lower middle income               3
    ## 207      97           Low income               3
    ## 18       96 High income: nonOECD               3
    ## 35       95  Lower middle income               3
    ## 92       94  Upper middle income               3
    ## 205      93  Lower middle income               3
    ## 67       92  Lower middle income               3
    ## 107      91  Upper middle income               3
    ## 170      90  Upper middle income               3
    ## 105      89  Upper middle income               3
    ## 148      88  Upper middle income               3
    ## 184      87  Upper middle income               3
    ## 191      86           Low income               3
    ## 115      85 High income: nonOECD               3
    ## 188      84  Upper middle income               3
    ## 176      83    High income: OECD               3
    ## 41       82  Upper middle income               3
    ## 118      81 High income: nonOECD               3
    ## 58       80           Low income               3
    ## 17       79  Upper middle income               3
    ## 80       78 High income: nonOECD               2
    ## 195      77 High income: nonOECD               2
    ## 75       76  Lower middle income               2
    ## 95       75           Low income               2
    ## 197      74  Lower middle income               2
    ## 49       73  Upper middle income               2
    ## 129      72           Low income               2
    ## 116      71    High income: OECD               2
    ## 163      70  Lower middle income               2
    ## 11       69  Upper middle income               2
    ## 21       68  Upper middle income               2
    ## 42       67  Upper middle income               2
    ## 111      66  Lower middle income               2
    ## 146      65 High income: nonOECD               2
    ## 175      64    High income: OECD               2
    ## 53       63  Upper middle income               2
    ## 154      62 High income: nonOECD               2
    ## 119      61  Lower middle income               2
    ## 193      60  Lower middle income               2
    ## 82       59  Upper middle income               2
    ## 3        58  Upper middle income               2
    ## 102      57 High income: nonOECD               2
    ## 16       56           Low income               2
    ## 200      55  Lower middle income               2
    ## 158      54  Upper middle income               2
    ## 145      53    High income: OECD               2
    ## 149      52  Upper middle income               2
    ## 44       51    High income: OECD               2
    ## 157      50 High income: nonOECD               2
    ## 50       49  Upper middle income               2
    ## 94       48  Upper middle income               2
    ## 87       47  Upper middle income               2
    ## 155      46    High income: OECD               2
    ## 72       45    High income: OECD               2
    ## 147      44  Lower middle income               2
    ## 85       43    High income: OECD               2
    ## 33       42    High income: OECD               2
    ## 59       41    High income: OECD               2
    ## 150      40  Lower middle income               2
    ## 78       39 High income: nonOECD               1
    ## 54       38  Lower middle income               1
    ## 89       37    High income: OECD               1
    ## 165      36 High income: nonOECD               1
    ## 137      35  Upper middle income               1
    ## 48       34    High income: OECD               1
    ## 206      33  Upper middle income               1
    ## 38       32  Upper middle income               1
    ## 199      31  Upper middle income               1
    ## 5        30 High income: nonOECD               1
    ## 182      29  Upper middle income               1
    ## 86       28  Upper middle income               1
    ## 10       27    High income: OECD               1
    ## 143      26    High income: OECD               1
    ## 13       25    High income: OECD               1
    ## 6        24  Upper middle income               1
    ## 153      23    High income: OECD               1
    ## 140      22  Lower middle income               1
    ## 177      21    High income: OECD               1
    ## 32       20    High income: OECD               1
    ## 162      19 High income: nonOECD               1
    ## 189      18  Upper middle income               1
    ## 142      17    High income: OECD               1
    ## 83       16  Lower middle income               1
    ## 123      15  Upper middle income               1
    ## 56       14    High income: OECD               1
    ## 100      13    High income: OECD               1
    ## 9        12    High income: OECD               1
    ## 31       11    High income: OECD               1
    ## 159      10 High income: nonOECD               1
    ## 84        9  Lower middle income               1
    ## 90        8    High income: OECD               1
    ## 25        7  Upper middle income               1
    ## 61        6    High income: OECD               1
    ## 65        5    High income: OECD               1
    ## 45        4    High income: OECD               1
    ## 93        3    High income: OECD               1
    ## 34        2  Upper middle income               1
    ## 196       1    High income: OECD               1

How many countries are Lower Middle Income but among the 38 nations with
the highest GDP?

    library(proto)
    library(gsubfn)
    library(RSQLite)

    ## Loading required package: DBI

    library(sqldf)
    #pulling how many countries have an IncomeGroup equal to Lower Middle Income and is one of the top 38 nations with the highest GDP
    sqldf("select count(*) as totalCountries from IncomeGoups where IncomeGroup = 'Lower middle income' and Ranking between 1 and 38")

    ## Loading required package: tcltk

    ##   totalCountries
    ## 1              4
