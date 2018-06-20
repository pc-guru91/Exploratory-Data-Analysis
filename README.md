## FIFA 2018 Data Exploration and Visualization with R

### Table of Contents
1. [Loading dataset](###load-data-and-libraries)
2. [Ranking](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [2.1 Players and Euro values](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [2.2 Players and Overall rating](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [2.3 Club teams and Overall rating](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [2.4 Club teams and Potential rating](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
3. [Correlation matrix - Overall rating](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
4. [Distributions](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [4.1 Age](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [4.2 Overall](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [4.3 Height (cm)](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [4.4 Weight (kg)](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
5. [Data visualization: Part 1](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [5.1 Age vs Overall rating](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [5.2 Club teams vs Euro values](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
6. [Data visualization: Part 2](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
	- [6.1 Nationality analysis from top 5 teams](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)
7. [World map](https://github.com/pc-guru91/Exploratory-Data-Analysis/blob/master/README.md)

### 1. Load data and libraries
``` R
# Importing library
library(ggplot2)
library(corrplot)
library(dplyr)
library(data.table)
library(gridExtra)
library(treemapify)
library(ggplotify)
library(rworldmap)
library(ggmap)
```
```
#Importing data
fifa = read.csv("fifa.csv", stringsAsFactors = FALSE)
fifa = tbl_df(fifa)
```
### 2. [Ranking](README.md)

#### 2.1 Players and Euro values
```
fifa %>% select(name, club, eur_value) %>% arrange(desc(eur_value)) %>% head(10) %>% data.table()
```
![player vs value](https://user-images.githubusercontent.com/38479244/41644841-204e5492-7424-11e8-8ce0-da247113f760.jpg)

#### 2.2 Players and Overall rating
```
fifa %>% select(name, club, overall) %>% arrange(desc(overall)) %>% head(10) %>% data.table()
```
![player vs overall](https://user-images.githubusercontent.com/38479244/41644819-11a83912-7424-11e8-9c63-7dce9d18d1ef.jpg)

#### 2.3 Top 10 club teams and Overall rating
```
club_overall = fifa %>% select(club, league, overall) %>% group_by(club) %>% summarise(Overall = mean(overall)) %>% arrange(desc(Overall)) %>% head(10) %>% data.table()
```
![team vs overall](https://user-images.githubusercontent.com/38479244/41644820-11d6fa22-7424-11e8-8268-032e03eff4f3.jpg)

#### 2.4 Top 10 club teams and Potential rating
```
fifa %>% select(club, league, potential) %>% group_by(club) %>% summarise(Potential = mean(potential)) %>% 
arrange(desc(Potential)) %>% head(10) %>% data.table()
```
![team vs potential](https://user-images.githubusercontent.com/38479244/41644762-e6c38dc8-7423-11e8-894f-9f918e697613.jpg)


### 3. [Correlation Matrix](README.md)
```
  numvar = which(sapply(fifa, is.numeric))
  numvar = fifa[, numvar]
  corr = cor(numvar, use = "pairwise.complete.obs")
  corr_sorted = as.matrix(sort(corr[, 'overall']), decreasing = TRUE)
  corr_sorted = names(which(apply(corr_sorted, 1, function(x) abs(x)>0.6)))
  corr_final = corr[corr_sorted, corr_sorted]
  corr_final = corr_final[-29, -29]
  corrplot.mixed(corr_final, tl.col = 'black', number.cex = 0.70, tl.pos = 'lt')
```
![correlation matrix](https://user-images.githubusercontent.com/38479244/41644953-7302402c-7424-11e8-98d3-a23d184f1b64.png)

### 4. [Distributions](README.md)

#### 4.1 Age
```
  ggplot(fifa, aes(x = age))+
    geom_histogram(aes(y = ..density..), binwidth = 2, color = "dark grey", fill = "white")+
    geom_density(alpha = .2, fill = "steelblue", size = 0.9)
```
![age distr](https://user-images.githubusercontent.com/38479244/41645413-cdd04746-7425-11e8-8fa3-c3bd5dc4b3e6.png)
#### 4.2 Overall
```
  ggplot(fifa, aes(x = overall))+
    geom_histogram(aes(y = ..density..), binwidth = 2, color = "dark grey", fill = "white")+
    geom_density(alpha = .2, fill = "steelblue", size = 0.9)
```
![overall distr](https://user-images.githubusercontent.com/38479244/41645412-cdb5a486-7425-11e8-8394-39028dfea786.png)
#### 4.3 Height (cm)
```
  ggplot(fifa, aes(x = height_cm))+
    geom_histogram(aes(y = ..density..), alpha=0.5, binwidth = 3, color = "dark grey", fill = "white")
```
![height distr](https://user-images.githubusercontent.com/38479244/41645410-cd983c66-7425-11e8-9c7a-349f641a32a0.png)
#### 4.4 Weight (kg)
```
  ggplot(fifa, aes(x = weight_kg))+
    geom_histogram(aes(y = ..density..), alpha=0.5, binwidth = 3, color = "dark grey", fill = "white")
```
![weight distr](https://user-images.githubusercontent.com/38479244/41645414-cdeae146-7425-11e8-8ebe-73f6ef1696e0.png)


### 5. [Data visualization: Part 1](README.md)

#### 5.1 Age vs Overall
```
  age = fifa %>% filter(age<40)  %>% select(age, overall) %>% group_by(age) %>% 
    summarize(Overall = mean(overall))
  
  ggplot(age, aes(x = age, y = Overall))+
    geom_line(color = 'red', size = 1.5)+
    geom_vline(color = 'blue', xintercept = 30, linetype = "dashed")+
    labs(title = "Age vs Overall", 
         x = "Age",
         y = "Overall")
```
![age vs overall part1 vis](https://user-images.githubusercontent.com/38479244/41645893-2c3e349a-7427-11e8-8014-13d118b42a25.png)

#### 5.2 Club team vs Euro value
```
  club.value = fifa %>% select(club, eur_value) %>% group_by(club) %>% summarize(Value = mean(eur_value)) %>% 
    arrange(desc(Value)) %>% head(10)
  
  ggplot(fifa %>% filter(club %in% club.value$club), aes(x = reorder(club, eur_value), y = eur_value))+
    geom_boxplot(color = "purple", outlier.colour = "red")+
    scale_y_continuous(breaks = seq(0, 125000000, by = 10000000))+
    labs(title = "Club Team vs Euro Value", 
         x = "Club Team",
         y = "Euro Value")+
    theme(axis.text.x = element_text(angle = 90, hjust = 1))
```
![club team vs value part1 vis](https://user-images.githubusercontent.com/38479244/41645892-2c1f15d8-7427-11e8-8522-5721047da697.png)

### 6. [Data visualization: Part 2](README.md)

#### 6.1 Nationality analysis from top 5 club teams
 ```
 Top_5 = fifa %>% select(club, eur_value, overall) %>% group_by(club) %>% 
 summarize(Value = mean(eur_value), Avg_Overall = mean(overall)) %>% 
 arrange(desc(Value)) %>% head(5)
    
 Top_teams = fifa %>% filter(club %in% Top_5$club) %>% group_by(club, nationality) %>% 
 summarise(num_nationality = n(), Sub_overall = mean(overall))
  
 Top_teams = merge(Top_teams, club_overall)
  
 ggplot(Top_teams, aes(area = Overall, fill = num_nationality, label = nationality, subgroup = club))+
    geom_treemap()+
    geom_treemap_text(colour = "white", 
                      alpha = 0.75, 
                      place = "middle")+
    geom_treemap_subgroup_text(colour = "black", 
                               grow = T, 
                               fontface = 'italic', 
                               min.size = 0, 
                               place = "middle",
                               alpha = 0.55)+
    facet_wrap(~club)+
    theme(legend.position = "bottom")+
    labs(title = "Nationality from Top 5 Teams", 
         fill = "# of Nationality",
         caption = "The area of each tile represents the mean of nationality's overall rating as a proportion of 
         all nationalities in that team")+
    scale_fill_gradient(low = "antiquewhite4", high = "dark red")
 ```
 
![treemap](https://user-images.githubusercontent.com/38479244/41645977-734af40e-7427-11e8-9c5a-68cfc7d4d0b4.png)

### 7. [World map](README.md)
```
  # Create a data frame that you would like to map countries for
  countries = fifa %>% select(nationality) %>% group_by(nationality) %>% summarise(count = n())
  countries$nationality[countries$nationality %in% c('England', 'Wales')] = "Great Britain"
  countries$nationality[countries$nationality %in% c('ChinaPR', 'Hong Kong')] = "China"
  countries$nationality[countries$nationality == 'Korea DPR'] = 'North Korea'
  countries$nationality[countries$nationality == 'Korea Republic'] = 'South Korea'
 
  # Join data to a map
  countries_points = joinCountryData2Map(countries, joinCode = "NAME", nameJoinColumn = "nationality")
  
  # Display the map
  mapCountryData(mapToPlot = countries_points, 
                 nameColumnToPlot = "count", 
                 oceanCol = "lightblue",
                 missingCountryCol = "white",
                 mapTitle = 'Origin of Nationality',
                 catMethod = "fixedWidth"
                 )
```
![worldmap](https://user-images.githubusercontent.com/38479244/41646070-b6d35054-7427-11e8-8d4b-b60bb2a39533.png)
