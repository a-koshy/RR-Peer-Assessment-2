# Consequences of severe weather events
Aranya Koshy  
July 2017  



## Synopsis



## Data Processing

We begin by reading in the file with storm data. The data was found [here](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2). The [National Weather Service Storm Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf) contains more information about the data, including explanations of the weather event types, and how the data is entered. The [National Climatic Data Center Storm Events FAQ](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf) has explanations for how some numbers are arrived at, for example the crop damage figures.


```r
library(ggplot2)
library(dplyr)
data <- read.csv("repdata%2Fdata%2FStormData.csv")
colnames(data) <- tolower(colnames(data))
```

We are concerned with the population health impact and economic consequences of weather events. To examine the impact of population health, we'll look at the `fatalities` and `injuries` columns of the dataset. Economic consequences in this dataset are divided into crop damage and property damage. The columns of the dataset that we need are `cropdmg, cropdmgexp, propdmg, propdmgexp`. Event type is stored in `evtype`.

## Results
