Weather impact in United States public health and economic problems from 1950 to November of 2011
========================================
Peer assigment 2 of the Reproducible Research of the Coursera Data Science specialization.

Fernando Crema   
Central University of Venezuela  
Wireless and Mobile Networks Institute

## Synopsis
The basic goal of this assignment is to explore the  U.S. National Oceanic and Atmospheric Administration's (NOAA) Storm Database to answer some basic questions about severe weather events.

Before answering the question we have one section to process the raw data and obtain the tidy data. After that, we have the results section answering 2 questions which are:

1. Across the United States, which types of events (as indicated in the EVTYPE variable) are most harmful with respect to population health?
1. Across the United States, which types of events have the greatest economic consequences?


## Data Processing
First of all, set the working directory of your work. I will output my working directory.


```r
setwd("C:\\Users\\Alejandro Crema\\Desktop\\Soporte\\Data Science\\Reproducible Research")
```

To make the project reproducible, I unzip the original raw data into working directory so I don't make any changes. Then I read raw data.


```r
storm <- read.csv("repdata-data-StormData.csv")
```

Format edition to the names of storm

```r
storm$EVTYPE = toupper(storm$EVTYPE)
```



## Results

### Harmful events to population
The first thing we will do is to map variables according to our needs. We will use the aggregate function 3 times, all the times with the same format:

```r
# tidy_table<-aggregate(COLUMN_TO_COMPARE ~ EVTYPE, data = storm, sum)
# positive<- condition to positive
# ordering the tidy_table using order
# tidy[order(tidy$COLUMN_TO_COMPARE, decreasing = TRUE), ]
```

The first time we will use our scheme we will view the effects of nature into humans, with to important variables: Fatalities and injuries.


```r
fatal <- aggregate(FATALITIES ~ EVTYPE, data = storm, sum)
positive<-fatal$FATALITIES > 0
fatal <- fatal[positive, ]
fatalorder <- fatal[order(fatal$FATALITIES, decreasing = TRUE), ]
head(fatalorder)
```

```
##                EVTYPE FATALITIES
## 755           TORNADO       5633
## 116    EXCESSIVE HEAT       1903
## 138       FLASH FLOOD        978
## 243              HEAT        937
## 417         LIGHTNING        816
## 683 THUNDERSTORM WIND        701
```


```r
injury <- aggregate(INJURIES ~ EVTYPE, data = storm, sum)
positive <- injury$INJURIES > 0
injury <- injury[positive, ]
injuryorder <- injury[order(injury$INJURIES, decreasing = TRUE), ]
head(injuryorder)
```

```
##                EVTYPE INJURIES
## 755           TORNADO    91346
## 683 THUNDERSTORM WIND     9353
## 154             FLOOD     6791
## 116    EXCESSIVE HEAT     6525
## 417         LIGHTNING     5230
## 243              HEAT     2100
```


```r
barplot(fatalorder[1:10, 2],,col=heat.colors(10), legend.text = fatalorder[1:10, 
    1],xlab="Natural event", ylab = "Number of deaths", main = "Number of fatalities by natural event")
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 


```r
barplot(injuryorder[1:10, 2],,col=heat.colors(10), legend.text = injuryorder[1:10, 
    1],,xlab="Natural event", ylab = "Number of Injuries", main = "Number of injuries by natural event")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 

  
Without a doubt, tornados are the first cause of human problems (by a natural event). Althought, tornados doesn't are in first place in Injuries they win in fatalities.
### Economic results


```r
type<-c("k","K","M","m","B","b")

for(t in type){
  if(t=="k"||t=="K"){
    storm[storm$PROPDMGEXP==t,]$PROPDMG<-storm[storm$PROPDMGEXP==t,]$PROPDMG*1e+03
  }else if(t=="M"||t=="m"){
    storm[storm$PROPDMGEXP==t,]$PROPDMG<-storm[storm$PROPDMGEXP==t,]$PROPDMG*1e+06
  }else{
    storm[storm$PROPDMGEXP==t,]$PROPDMG<-storm[storm$PROPDMGEXP==t,]$PROPDMG*1e+09
  }
}
```


```r
damage <- aggregate(PROPDMG ~ EVTYPE, data = storm, sum)
head(damage)
```

```
##                  EVTYPE PROPDMG
## 1    HIGH SURF ADVISORY  200000
## 2         COASTAL FLOOD       0
## 3           FLASH FLOOD   50000
## 4             LIGHTNING       0
## 5             TSTM WIND 8100000
## 6       TSTM WIND (G45)    8000
```

```r
positive<-damage$PROPDMG > 0
damage<-damage[positive,]
damageorder <- damage[order(damage$PROPDMG, decreasing = TRUE), ]
head(damageorder)
```

```
##                EVTYPE   PROPDMG
## 154             FLOOD 1.498e+11
## 364 HURRICANE-TYPHOON 8.117e+10
## 755           TORNADO 5.694e+10
## 597       STORM SURGE 4.332e+10
## 138       FLASH FLOOD 1.614e+10
## 212              HAIL 1.573e+10
```


```r
barplot(damageorder[1:10, 2],col=heat.colors(10),legend.text = damageorder[1:10, 
    1],,xlab="Natural event", ylab = "Damage in dollars", main = "Property damage by natural event in dollars")
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12.png) 
  
So you can see that tornados causes human problems AND property damage in dollars. But we can see the first 6 problems using the head command.
