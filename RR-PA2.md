# Economic and health costs of harmful weather events in the USA
Aranya Koshy  
August 2017  



## Synopsis

This analysis aims to answer the following two questions:

1. Which types of weather events cause the greatest harm to population health in the United States?
2. Which types of weather events cause the most economic damage?

The data used was from the NOAA storm database which contains records of weather events over 61 years. After processing the data, we create two plots that show the most damaging weather events with respect to population health and economic costs.

The analysis shows that the most dangerous events for human health and safety are tornadoes, excessive heat and floods. The weather events that cause the most damage to crops and property are floods, hurricanes and tornadoes.

## Data Processing

The data for this analysis was found [here](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2). The [National Weather Service Storm Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf) contains more information about the data, including explanations of the weather event types, and how the data is entered. The [National Climatic Data Center Storm Events FAQ](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf) has explanations for how some numbers (like the crop damage figures) are arrived at.

We begin by loading the libraries we will use, and reading in the file with weather data.


```r
library(ggplot2)
library(dplyr)
library(readr)
library(tidyr)

# Reading in the data
data <- read_csv("repdata%2Fdata%2FStormData.csv")

# Let's take a look at the data
glimpse(data)
```

```
## Observations: 902,297
## Variables: 37
## $ STATE__    <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, ...
## $ BGN_DATE   <chr> "4/18/1950 0:00:00", "4/18/1950 0:00:00", "2/20/195...
## $ BGN_TIME   <chr> "0130", "0145", "1600", "0900", "1500", "2000", "01...
## $ TIME_ZONE  <chr> "CST", "CST", "CST", "CST", "CST", "CST", "CST", "C...
## $ COUNTY     <dbl> 97, 3, 57, 89, 43, 77, 9, 123, 125, 57, 43, 9, 73, ...
## $ COUNTYNAME <chr> "MOBILE", "BALDWIN", "FAYETTE", "MADISON", "CULLMAN...
## $ STATE      <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL...
## $ EVTYPE     <chr> "TORNADO", "TORNADO", "TORNADO", "TORNADO", "TORNAD...
## $ BGN_RANGE  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
## $ BGN_AZI    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ BGN_LOCATI <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ END_DATE   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ END_TIME   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ COUNTY_END <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
## $ COUNTYENDN <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ END_RANGE  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
## $ END_AZI    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ END_LOCATI <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ LENGTH     <dbl> 14.0, 2.0, 0.1, 0.0, 0.0, 1.5, 1.5, 0.0, 3.3, 2.3, ...
## $ WIDTH      <dbl> 100, 150, 123, 100, 150, 177, 33, 33, 100, 100, 400...
## $ F          <int> 3, 2, 2, 2, 2, 2, 2, 1, 3, 3, 1, 1, 3, 3, 3, 4, 1, ...
## $ MAG        <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
## $ FATALITIES <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 4, 0, ...
## $ INJURIES   <dbl> 15, 0, 2, 2, 2, 6, 1, 0, 14, 0, 3, 3, 26, 12, 6, 50...
## $ PROPDMG    <dbl> 25.0, 2.5, 25.0, 2.5, 2.5, 2.5, 2.5, 2.5, 25.0, 25....
## $ PROPDMGEXP <chr> "K", "K", "K", "K", "K", "K", "K", "K", "K", "K", "...
## $ CROPDMG    <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
## $ CROPDMGEXP <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ WFO        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ STATEOFFIC <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ ZONENAMES  <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ LATITUDE   <dbl> 3040, 3042, 3340, 3458, 3412, 3450, 3405, 3255, 333...
## $ LONGITUDE  <dbl> 8812, 8755, 8742, 8626, 8642, 8748, 8631, 8558, 874...
## $ LATITUDE_E <dbl> 3051, 0, 0, 0, 0, 0, 0, 0, 3336, 3337, 3402, 3404, ...
## $ LONGITUDE_ <dbl> 8806, 0, 0, 0, 0, 0, 0, 0, 8738, 8737, 8644, 8640, ...
## $ REMARKS    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...
## $ REFNUM     <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, ...
```

We are concerned with the population health impact and economic consequences of weather events. To examine the impact of population health, we'll look at the `FATALITIES` and `INJURIES` columns of the dataset. Economic consequences in this dataset are divided into crop damage and property damage. The columns of the dataset that we need are `CROPDMG, CROPDMGEXP, PROPDMG, PROPDMGEXP`. Event type is stored in `EVTYPE`. We can discard the other columns.


```r
data <- select(data, c(8, 23:28)) # Subsetting the relevant columns
colnames(data) <- tolower(colnames(data)) # I like my variable names in lowercase
```

Next we will convert the crop damage and property damage data into their actual values.


```r
# propdmgexp and cropdmgexp contain the order of magnitude of the cost
data$propdmgexp <- tolower(data$propdmgexp)
data$cropdmgexp <- tolower(data$cropdmgexp)

data$propdmgexp <- data$propdmgexp %>%
  gsub("h", 2, .) %>%
  gsub("k", 3, .) %>%
  gsub("m", 6, .) %>%
  gsub("b", 9, .) %>%
  as.numeric()
```

```
## Warning in function_list[[k]](value): NAs introduced by coercion
```

```r
data$cropdmgexp <- data$cropdmgexp %>%
  gsub("k", 3, .) %>%
  gsub("m", 6, .) %>%
  gsub("b", 9, .) %>%
  as.numeric()
```

```
## Warning in function_list[[k]](value): NAs introduced by coercion
```

```r
# Adding columns that hold the crop damage, property damage and total damage amounts
data <- mutate(data, cropsum=cropdmg*10^cropdmgexp, propsum=propdmg*10^propdmgexp, damage=apply(cbind(cropsum, propsum), 1, sum, na.rm=TRUE))
```

Now we need to clean the `evtype` variable into more standard categories. From the documentation, we know that there are meant to be only 48 unique weather event types, but in the dataset there are 977 unique weather events recorded. This is because in practise, the data collection has resulted in inconsistencies ranging from multiple entries to summaries of months and spelling mistakes.


```r
length(unique(data$evtype))
```

```
## [1] 977
```

```r
head(unique(data$evtype), 20)
```

```
##  [1] "TORNADO"                   "TSTM WIND"                
##  [3] "HAIL"                      "FREEZING RAIN"            
##  [5] "SNOW"                      "ICE STORM/FLASH FLOOD"    
##  [7] "SNOW/ICE"                  "WINTER STORM"             
##  [9] "HURRICANE OPAL/HIGH WINDS" "THUNDERSTORM WINDS"       
## [11] "RECORD COLD"               "HURRICANE ERIN"           
## [13] "HURRICANE OPAL"            "HEAVY RAIN"               
## [15] "LIGHTNING"                 "THUNDERSTORM WIND"        
## [17] "DENSE FOG"                 "RIP CURRENT"              
## [19] "THUNDERSTORM WINS"         "FLASH FLOOD"
```


We will bring the number of unique event types down by collecting the names that are obviously variations of a single event name. The list of event types in the documentation is a good guideline.


```r
data$evtype <-tolower(data$evtype)

data<-filter(data, !grepl("summary|month|year", data$evtype)) # don't want to double count events

# Gathering event types into standard names
data$evtype <- data$evtype %>%
  replace(grepl("tstm|thunderstorm", .), "thunderstorm wind") %>%
  replace(grepl("funnel cloud", .), "tornado") %>%
  replace(grepl("mix", .), "winter weather") %>%
  replace(grepl("warm|heat|hot", .), "excessive heat") %>%
  replace(grepl("cool|cold|chil", .), "extreme cold/wind chill") %>%
  replace(grepl("fld", .), "flood") %>%
  replace(grepl("tornado", .), "tornado") %>%
  replace(grepl("hurricane", .), "hurricane") %>%
  replace(grepl("light", .), "lightning") %>%
  replace(grepl("flash", .), "flash flood")
```

At this point by looking at the 20 most common events we can see that we've covered about 98% of the recorded data. We will move on to finding patterns of health and economic impact of weather events.


```r
sort(table(data$evtype), decreasing=TRUE)[1:20]
```

```
## 
##       thunderstorm wind                    hail                 tornado 
##                  336808                  288661                   67633 
##             flash flood                   flood               high wind 
##                   55675                   28723                   20214 
##               lightning              heavy snow              heavy rain 
##                   15975                   15708                   11742 
##            winter storm          winter weather              waterspout 
##                   11433                    8309                    3797 
##             strong wind          excessive heat                wildfire 
##                    3569                    2991                    2761 
## extreme cold/wind chill                blizzard                 drought 
##                    2747                    2719                    2488 
##               ice storm              high winds 
##                    2006                    1533
```

```r
sum(sort(table(data$evtype), decreasing=TRUE)[1:20])/nrow(data)
```

```
## [1] 0.9815309
```

## Results

Here is a plot of the population health impact of weather events.


```r
data_health<-data %>%
  group_by(evtype) %>%
  summarise(fatalities=sum(fatalities), injuries=sum(injuries),total_health=sum(fatalities, injuries)) %>%
  arrange(desc(total_health)) %>%
  .[1:20,] %>%
  gather(value, ind, injuries:fatalities)

p<-ggplot(data_health, aes(x=reorder(evtype, -total_health), y=total_health))

p+geom_bar(stat="identity", aes(fill=value))+
  scale_fill_manual(name=NULL, values=c("firebrick3", "darkorange2"), labels=c("Fatalities", "Injuries"))+
  theme(axis.text.x = element_text(angle = 90, hjust=1.0))+
  labs(x="Weather event", y="Number of incidents", title="Effects of weather events in the USA on population health")
```

![](RR-PA2_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

This plot shows that the weather event that causes most harm to the population is tornadoes, followed by excessive heat and floods.

Here is a similar plot for economic impacts of weather events.


```r
data_econ<-data %>%
  group_by(evtype) %>%
  summarise(crop=sum(cropsum, na.rm=T), prop=sum(propsum, na.rm=T),total=sum(crop, prop)) %>%
  arrange(desc(total)) %>%
  .[1:20,] %>%
  gather(value, ind, crop:prop)

q<-ggplot(data_econ, aes(x=reorder(evtype, -total), y=total/10^9))

q+geom_bar(stat="identity", aes(fill=value))+
  scale_fill_manual(name=NULL, values=c("springgreen3", "slateblue2"), labels=c("Crop damage", "Property damage"))+
  theme(axis.text.x = element_text(angle = 90, hjust=1.0))+
  labs(x="Weather event", y="Cost in billions (USD)", title="Economic consequences of weather events in the USA")
```

![](RR-PA2_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

This plot shows that floods cause the most economic costs, followed by hurricanes and tornadoes.
