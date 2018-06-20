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
![player vs value](https://user-images.githubusercontent.com/38479244/41644220-475004c0-7422-11e8-8897-d2a85e549afc.png)

#### 2.2 Players and Overall rating
![player vs overall](https://user-images.githubusercontent.com/38479244/41644218-473282ba-7422-11e8-8ecd-930ff48d1fe7.png)

#### 2.3 Club teams and Overall rating
![team vs overall](https://user-images.githubusercontent.com/38479244/41644698-b215a1ce-7423-11e8-8e3b-a54357712fd5.jpg)

#### 2.4 Club teams and Potential rating
![team vs potential](https://user-images.githubusercontent.com/38479244/41644222-4796bcf8-7422-11e8-8949-87a49c49fd7d.png)


### 3. Correlation Matrix
![rplot01](https://user-images.githubusercontent.com/38479244/41644557-40a96728-7423-11e8-89e2-548aa23f1d92.png)
