 

# Case Study -BellaBeat
Gayathri Chandrasekaran

## Introduction : About the company

Bellabeat is a wellness Company ,that develops wearable computers for women.It was founded in 2013 by Sandro Mur and Urška Sršen. BellaBeat develop wearables and accompanying products that monitor biometric and lifestyle data. By collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits.Since 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women. Bellabeat empowers women to reconnect with themselves, unleash their inner strengths and be what they were meant to be via products like:

\*Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.

\*Leaf: Bellabeat's classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.

\*Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.

\*Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.

\*Bellabeat membership: Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.

## Ask Phase

#### Questions for Analysis.

1.What are some trends in the usage of non-bellabeat smart devices?

2.How could these trends apply tobellabeat customers?

3.how could these trends help influence bellabeat marketing strategy?

**Business Task**: To identify the frequency in the usage of smart devices among the users, further utilizing the insights to develop and improve the BellaBeat products thereby improving their marketing strategy.

#### Key Stakeholders:

Urška Sršen: Bellabeat's cofounder and Chief Creative Officer.

Sando Mur: Mathematician and Bellabeat's cofounder; key member of the Bellabeat executive team.

## Prepare Phase

DataSets Used:

FitBit Fitness Tracker Data (CC0: Public Domain, dataset made available through Mobius): This Kaggle data setcontains personal fitness tracker from thirty fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users' habits. I chose the hourly_tracking datas for my analysis. the datasets are in narrow format. The datasets are organized by Activitydate and Activityhour. The datasets have thirty three unique Id's in common for all dataset.The dataset doesn't hold any details like height, age, location and other factors, so the bias in this dataset cannot be determined.The dataset has limited number of entries to analyse and also the data here is

1.reliable 2.open 3.comprehensive 4.current(data was collected on 2016). 5.cited.

## Process Phase

The datasets have values for a month and are small.So sheets and SQL is used for data sorting and organizing.The data are collected for 30 users with distinct 33 Ids'. Not everyone in the group had their data fully logged also the documented properly ensuring its integrity.To avoid any errors, only data considered is formatted in the table. The datasets are named according to the data being considered.

```{r  installing packages}
```


```{r  installing packages}
install.packages('tidyverse')
library(tidyverse)
install.packages("ggplot2")
library(ggplot2)
install.packages("tidyselect")
library(tidyselect)
library(lubridate)
install.packages("dplyr")
library(dplyr)
#import dataset

activity<- read.csv("dailyActivity.csv")
View(activity)

calorie<- read.csv("hourlyCalories_merged.csv")
View(calorie)

intensity<-read.csv("hourlyIntensities_merged.csv")
View(intensity)

step<-read.csv("hourlySteps_merged.csv")
View(step)

weight<-read.csv("weightLogInfo_merged.csv")
View(weight)

 sleep<-read.csv("sleepDay_merged.csv")
 View(sleep)
```

```{r understanding the dataset}
head(activity)
colnames(activity)

nrow(activity)
nrow(calorie)
nrow(step)
nrow(sleep)
nrow(weight)
```

```{r summary}
activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes,
         Calories) %>%
  summary() 

sleep %>% 
  select(TotalMinutesAsleep) %>% 
  summary()

```

## Analyse Phase

Usage of smart device

The users were all active for a week and their usage decline as days move on.The below plot shows their decline in usage of non-BellaBeat smart device

![](Dashboard%201.png){alt="Usage of smart device"}

![](Dashboard%202.png){width="682"}

Organizing data From the given dataset, mean steps, mean calorie, mean distance and mean sleep are calculated.

```{r  organize by id}
#organize
 MeanSteps<- activity %>% group_by(Id) %>% 
  summarise(mean_steps=mean(TotalSteps))
 View(MeanSteps)
 MeanCalorie<- activity %>% group_by(Id) %>%  
   summarise(mean_calorie=mean(Calories))
 View(MeanCalorie)
 MeanDistance<- activity %>% group_by(Id) %>% 
   summarise(mean_distance=mean(TotalDistance))
 View(MeanDistance)
 MeanSleep<- sleep %>% group_by(Id) %>%  
   summarise(mean_sleep=mean(TotalMinutesAsleep))
View(MeanSleep)
```

```{r  activity status}
 ActivityStatus<-MeanSteps %>% mutate(activityStatus=case_when(mean_steps>10000 ~ "Active user", mean_steps<5000 ~ "sedentary user", TRUE ~ "Fair user"))

 View(ActivityStatus)
```

plotting based on activity status

```{r plottting}
ggplot(data=ActivityStatus)+geom_bar(mapping=aes(x=activityStatus,color=activityStatus,fill=activityStatus))
```

```{r combine table by id}
Mean_table_1 <- merge(ActivityStatus,MeanDistance, by="Id")
View(Mean_table_1)

Mean_table_2 <- merge(MeanSleep,MeanCalorie, by="Id")
View(Mean_table_2)

FinalTable <- merge(Mean_table_1,Mean_table_2, by="Id")
View(FinalTable)

ggplot(data = FinalTable)+geom_point(mapping = aes(x=mean_calorie,y=mean_sleep))
```

## Share & Act Phase

Key Findings

1.The Usage of smart devices reduces gradually as days passes.

2.From the analysis, it is found that out of 33 users only 24 users had their data logged.

Recommendations:

The BellaBeat products must be designed in a way that it can be worn everyday also must send notifications to its user via the BellaBeat app about the poor battery or any defect in case of failure to record the data.

The app must ask its user for their age, heigth, weight and their daily active status and must give them various plans accordingly.

the app also should let its user to set their daily step target , daily sleep minutes and also their calorie intake.

It should also send prior notifications to its user if they fail to meet their daily step ,sleep,calorie target also a weekly report of their past week activity status should be given to improve their involvement.
