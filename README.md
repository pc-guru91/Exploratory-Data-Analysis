## FIFA 2018 Data Exploration and Visualization with R

### Table of Contents
1. Loading dataset
2. Ranking
	- 2.1 Players and Euro values
	- 2.2 Players and Overall rating
	- 2.3 Club teams and Overall rating
	- 2.4 Club teams and Potential rating
3. Correlation matrix - Overall rating
4. Distributions
	- 4.1 Age
	- 4.2 Overall
	- 4.3 Height (cm)
	- 4.4 Weight (kg)
5. Data visualization: Part 1
	- 5.1 Age vs Overall rating
	- 5.2 Club teams vs Euro values
6. Data visualization: Part 2
	- 6.1 Position analysis
7. World map

### 1. Load data and libraries
```
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
### 2. Ranking

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


### 3. Correlation Matrix
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

### 4. Distributions

#### 4.1 Age
![age distr](https://user-images.githubusercontent.com/38479244/41645413-cdd04746-7425-11e8-8fa3-c3bd5dc4b3e6.png)
#### 4.2 Overall
![overall distr](https://user-images.githubusercontent.com/38479244/41645412-cdb5a486-7425-11e8-8394-39028dfea786.png)
#### 4.3 Height (cm)
![height distr](https://user-images.githubusercontent.com/38479244/41645410-cd983c66-7425-11e8-9c7a-349f641a32a0.png)
#### 4.4 Weight (kg)
![weight distr](https://user-images.githubusercontent.com/38479244/41645414-cdeae146-7425-11e8-8ebe-73f6ef1696e0.png)
