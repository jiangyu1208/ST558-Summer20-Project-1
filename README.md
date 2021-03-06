ST558 Project 1
================
Yu Jiang
6/10/2020

  - [1. Describe JSON Data](#describe-json-data)
      - [What is JSON?](#what-is-json)
      - [Where does it get used?](#where-does-it-get-used)
      - [Why is it a good way to store
        data?](#why-is-it-a-good-way-to-store-data)
  - [2. Three Packages for Reading JSON Data into
    R](#three-packages-for-reading-json-data-into-r)
      - [‘rjson’](#rjson)
      - [‘RJSONIO’](#rjsonio)
      - [‘jsonlite’](#jsonlite)
  - [3. Write Functions to Contact the NHL
    Records](#write-functions-to-contact-the-nhl-records)
  - [4. Data Analysis](#data-analysis)

# 1\. Describe JSON Data

## What is JSON?

  - Json stands for JavaScript Object Notation.

  - The lightweight format for storing and transporting data, specified
    by Douglas Crockford.

  - A way to store information in an organized, easy-to-access manner.

  - It has been extended from the JavaScript scripting language.

  - It was designed for human-readable data interchange.

## Where does it get used?

  - It is used when data is sent from a server to a web page.

  - The JSON format is used for serializing and transmitting structured
    data over network connection.

  - It is used while writing JavaScript based applications that includes
    browser extensions and websites.

  - Web services and APIs use JSON format to provide public data.

  - It can be used with modern programming languages.

## Why is it a good way to store data?

  - It is ‘self-describing’ and easy to understand.

  - It is a generic data format with a minimal number of value types:
    strings, numbers, booleans, lists, objects, and null.

  - It can be supported by many databases like PostgreSQL and MySQL.

  - JSON data is stored in a set of key-value pairs.

  - Spacing (spaces, tabs, new lines) does not matter in a JSON file.

For more information about JSON, please check [this
website](https://en.wikipedia.org/wiki/JSON).

# 2\. Three Packages for Reading JSON Data into R

## ‘rjson’

The package ‘rjson’ converts a JSON object into an R object.

## ‘RJSONIO’

The package ‘RJSONIO’ allows conversion to and from data in Javascript
object notation (JSON) format. This allows R objects to be inserted into
Javascript/ECMAScript/ActionScript code and allows R programmers to read
and convert JSON content to R objects.

## ‘jsonlite’

A fast JSON parser and generator optimized for statistical data and the
web.

I am going to choose the package ‘jsonlite’ since it is the fastest
among these three packages and this package offers flexible, robust,
high performance tools for working with JSON in R and is particularly
powerful for building pipelines and interacting with a web API.

# 3\. Write Functions to Contact the NHL Records

``` r
# Load the libraries
library(httr)
library(jsonlite)
library(dplyr)

# A function for the first two calls
base_url_1 <- 'https://records.nhl.com/site/api'

api <- function(path){
 json_1 <- GET(paste0(base_url_1, "/", path)) %>%
    content('text') %>%
    fromJSON(flatten = TRUE) 
 
 attributes(json_1)
 return(tbl_df(json_1$data))
}

# Table for /frachise
api('franchise') %>% head()
```

    ## # A tibble: 6 x 6
    ##      id firstSeasonId lastSeasonId mostRecentTeamId teamCommonName teamPlaceName
    ##   <int>         <int>        <int>            <int> <chr>          <chr>        
    ## 1     1      19171918           NA                8 Canadiens      Montréal     
    ## 2     2      19171918     19171918               41 Wanderers      Montreal     
    ## 3     3      19171918     19341935               45 Eagles         St. Louis    
    ## 4     4      19191920     19241925               37 Tigers         Hamilton     
    ## 5     5      19171918           NA               10 Maple Leafs    Toronto      
    ## 6     6      19241925           NA                6 Bruins         Boston

``` r
# Table for /franchise-team-totals
api('franchise-team-totals') %>% head()
```

    ## # A tibble: 6 x 30
    ##      id activeFranchise firstSeasonId franchiseId gameTypeId gamesPlayed
    ##   <int>           <int>         <int>       <int>      <int>       <int>
    ## 1     1               1      19821983          23          2        2937
    ## 2     2               1      19821983          23          3         257
    ## 3     3               1      19721973          22          2        3732
    ## 4     4               1      19721973          22          3         272
    ## 5     5               1      19261927          10          2        6504
    ## 6     6               1      19261927          10          3         515
    ## # ... with 24 more variables: goalsAgainst <int>, goalsFor <int>,
    ## #   homeLosses <int>, homeOvertimeLosses <int>, homeTies <int>, homeWins <int>,
    ## #   lastSeasonId <int>, losses <int>, overtimeLosses <int>,
    ## #   penaltyMinutes <int>, pointPctg <dbl>, points <int>, roadLosses <int>,
    ## #   roadOvertimeLosses <int>, roadTies <int>, roadWins <int>,
    ## #   shootoutLosses <int>, shootoutWins <int>, shutouts <int>, teamId <int>,
    ## #   teamName <chr>, ties <int>, triCode <chr>, wins <int>

``` r
# A function for the last three calls
base_url_2 <- 'https://records.nhl.com/site/api/franchise'

ID <- function(arg1, arg2){
 json_2 <- GET(paste0(
    base_url_2, "-", arg1, '-records?cayenneExp=franchiseId=', arg2)) %>%
    content('text') %>%
    fromJSON(flatten = TRUE)
 
 attributes(json_2)
 return(tbl_df(json_2$data))
}

# Table for /franchise-season-records?cayenneExp=franchiseId=ID
ID('season', '12') %>% head()
```

    ## # A tibble: 1 x 57
    ##      id fewestGoals fewestGoalsAgai~ fewestGoalsAgai~ fewestGoalsSeas~
    ##   <int>       <int>            <int> <chr>            <chr>           
    ## 1    17         145              132 1953-54 (70)     2019-20 (82)    
    ## # ... with 52 more variables: fewestLosses <int>, fewestLossesSeasons <chr>,
    ## #   fewestPoints <int>, fewestPointsSeasons <chr>, fewestTies <int>,
    ## #   fewestTiesSeasons <chr>, fewestWins <int>, fewestWinsSeasons <chr>,
    ## #   franchiseId <int>, franchiseName <chr>, homeLossStreak <int>,
    ## #   homeLossStreakDates <chr>, homePointStreak <int>,
    ## #   homePointStreakDates <chr>, homeWinStreak <int>, homeWinStreakDates <chr>,
    ## #   homeWinlessStreak <int>, homeWinlessStreakDates <chr>, lossStreak <int>,
    ## #   lossStreakDates <chr>, mostGameGoals <int>, mostGameGoalsDates <chr>,
    ## #   mostGoals <int>, mostGoalsAgainst <int>, mostGoalsAgainstSeasons <chr>,
    ## #   mostGoalsSeasons <chr>, mostLosses <int>, mostLossesSeasons <chr>,
    ## #   mostPenaltyMinutes <int>, mostPenaltyMinutesSeasons <chr>,
    ## #   mostPoints <int>, mostPointsSeasons <chr>, mostShutouts <int>,
    ## #   mostShutoutsSeasons <chr>, mostTies <int>, mostTiesSeasons <chr>,
    ## #   mostWins <int>, mostWinsSeasons <chr>, pointStreak <int>,
    ## #   pointStreakDates <chr>, roadLossStreak <int>, roadLossStreakDates <chr>,
    ## #   roadPointStreak <int>, roadPointStreakDates <chr>, roadWinStreak <int>,
    ## #   roadWinStreakDates <chr>, roadWinlessStreak <int>,
    ## #   roadWinlessStreakDates <chr>, winStreak <int>, winStreakDates <chr>,
    ## #   winlessStreak <int>, winlessStreakDates <chr>

``` r
# Table for /franchise-goalie-records?cayenneExp=franchiseId=ID
ID('goalie', '12') %>% head()
```

    ## # A tibble: 6 x 29
    ##      id activePlayer firstName franchiseId franchiseName gameTypeId gamesPlayed
    ##   <int> <lgl>        <chr>           <int> <chr>              <int>       <int>
    ## 1   254 FALSE        Terry              12 Detroit Red ~          2         734
    ## 2   267 FALSE        Chris              12 Detroit Red ~          2         565
    ## 3   319 TRUE         Jimmy              12 Detroit Red ~          2         543
    ## 4   350 FALSE        Allan              12 Detroit Red ~          2           4
    ## 5   377 FALSE        Alain              12 Detroit Red ~          2           3
    ## 6   386 FALSE        Joe                12 Detroit Red ~          2          29
    ## # ... with 22 more variables: lastName <chr>, losses <int>,
    ## #   mostGoalsAgainstDates <chr>, mostGoalsAgainstOneGame <int>,
    ## #   mostSavesDates <chr>, mostSavesOneGame <int>, mostShotsAgainstDates <chr>,
    ## #   mostShotsAgainstOneGame <int>, mostShutoutsOneSeason <int>,
    ## #   mostShutoutsSeasonIds <chr>, mostWinsOneSeason <int>,
    ## #   mostWinsSeasonIds <chr>, overtimeLosses <int>, playerId <int>,
    ## #   positionCode <chr>, rookieGamesPlayed <int>, rookieShutouts <int>,
    ## #   rookieWins <int>, seasons <int>, shutouts <int>, ties <int>, wins <int>

``` r
# Table for //franchise-skater-records?cayenneExp=franchiseId=ID
ID('skater', '12') %>% head()
```

    ## # A tibble: 6 x 30
    ##      id activePlayer assists firstName franchiseId franchiseName gameTypeId
    ##   <int> <lgl>          <int> <chr>           <int> <chr>              <int>
    ## 1 16906 FALSE           1023 Gordie             12 Detroit Red ~          2
    ## 2 17006 FALSE           1063 Steve              12 Detroit Red ~          2
    ## 3 17028 FALSE            145 Bob                12 Detroit Red ~          2
    ## 4 17136 FALSE            281 John               12 Detroit Red ~          2
    ## 5 17192 FALSE            878 Nicklas            12 Detroit Red ~          2
    ## 6 17196 FALSE              2 Ron                12 Detroit Red ~          2
    ## # ... with 23 more variables: gamesPlayed <int>, goals <int>, lastName <chr>,
    ## #   mostAssistsGameDates <chr>, mostAssistsOneGame <int>,
    ## #   mostAssistsOneSeason <int>, mostAssistsSeasonIds <chr>,
    ## #   mostGoalsGameDates <chr>, mostGoalsOneGame <int>, mostGoalsOneSeason <int>,
    ## #   mostGoalsSeasonIds <chr>, mostPenaltyMinutesOneSeason <int>,
    ## #   mostPenaltyMinutesSeasonIds <chr>, mostPointsGameDates <chr>,
    ## #   mostPointsOneGame <int>, mostPointsOneSeason <int>,
    ## #   mostPointsSeasonIds <chr>, penaltyMinutes <int>, playerId <int>,
    ## #   points <int>, positionCode <chr>, rookiePoints <int>, seasons <int>

# 4\. Data Analysis

``` r
# Load the libraries
library(tidyverse)
library(knitr)
library(ggplot2)

# Create some new variables, the percent of home wins/losses and road wins/losses
team_total <- api('franchise-team-totals') %>%
  select(
    -c('id', 'firstSeasonId', 'penaltyMinutes', 'pointPctg', 'points')) %>% 
  mutate(HomeWinsPercent = homeWins/wins, 
         HomeLossesPercent = homeLosses/losses, 
         RoadWinsPercent = roadWins/wins, 
         RoadLossesPercent = roadLosses/losses)


# Convert activeFranchise and gameTypeId to factors and rename their with levels
team_total$activeFranchise <- as.factor(team_total$activeFranchise)
levels(team_total$activeFranchise) <- list('Active'= 1, 'Non-Active'= 0)

team_total$gameTypeId <- as.factor(team_total$gameTypeId)
levels(team_total$gameTypeId) <- list('Type 2'= 2, 'Type 3'= 3)

# Select the rows of gameType ID = 2 and gameType ID = 3
id_2 <- team_total[team_total$gameTypeId == 'Type 2', ]
id_3 <- team_total[team_total$gameTypeId == 'Type 3', ]

# Table about the preview of team total data
knitr::kable(
 head(team_total),
  caption = 'Preview of Team Total Data')
```

| activeFranchise | franchiseId | gameTypeId | gamesPlayed | goalsAgainst | goalsFor | homeLosses | homeOvertimeLosses | homeTies | homeWins | lastSeasonId | losses | overtimeLosses | roadLosses | roadOvertimeLosses | roadTies | roadWins | shootoutLosses | shootoutWins | shutouts | teamId | teamName           | ties | triCode | wins | HomeWinsPercent | HomeLossesPercent | RoadWinsPercent | RoadLossesPercent |
| :-------------- | ----------: | :--------- | ----------: | -----------: | -------: | ---------: | -----------------: | -------: | -------: | -----------: | -----: | -------------: | ---------: | -----------------: | -------: | -------: | -------------: | -----------: | -------: | -----: | :----------------- | ---: | :------ | ---: | --------------: | ----------------: | --------------: | ----------------: |
| Active          |          23 | Type 2     |        2937 |         8708 |     8647 |        507 |                 82 |       96 |      783 |           NA |   1181 |            162 |        674 |                 80 |      123 |      592 |             79 |           78 |      193 |      1 | New Jersey Devils  |  219 | NJD     | 1375 |       0.5694545 |         0.4292972 |       0.4305455 |         0.5707028 |
| Active          |          23 | Type 3     |         257 |          634 |      697 |         53 |                  0 |       NA |       74 |           NA |    120 |              0 |         67 |                  0 |       NA |       63 |              0 |            0 |       25 |      1 | New Jersey Devils  |   NA | NJD     |  137 |       0.5401460 |         0.4416667 |       0.4598540 |         0.5583333 |
| Active          |          22 | Type 2     |        3732 |        11779 |    11889 |        674 |                 81 |      170 |      942 |           NA |   1570 |            159 |        896 |                 78 |      177 |      714 |             67 |           82 |      167 |      2 | New York Islanders |  347 | NYI     | 1656 |       0.5688406 |         0.4292994 |       0.4311594 |         0.5707006 |
| Active          |          22 | Type 3     |         272 |          806 |      869 |         46 |                  1 |       NA |       84 |           NA |    124 |              0 |         78 |                  0 |       NA |       64 |              0 |            0 |        9 |      2 | New York Islanders |   NA | NYI     |  148 |       0.5675676 |         0.3709677 |       0.4324324 |         0.6290323 |
| Active          |          10 | Type 2     |        6504 |        19863 |    19864 |       1132 |                 73 |      448 |     1600 |           NA |   2693 |            147 |       1561 |                 74 |      360 |     1256 |             66 |           78 |      403 |      3 | New York Rangers   |  808 | NYR     | 2856 |       0.5602241 |         0.4203491 |       0.4397759 |         0.5796509 |
| Active          |          10 | Type 3     |         515 |         1436 |     1400 |        103 |                  0 |        1 |      137 |           NA |    263 |              0 |        160 |                  0 |        7 |      107 |              0 |            0 |       44 |      3 | New York Rangers   |    8 | NYR     |  244 |       0.5614754 |         0.3916350 |       0.4385246 |         0.6083650 |

Preview of Team Total Data

``` r
# Table about the ActiveFranchise vs Game Types
knitr::kable(
 table(team_total$activeFranchise, team_total$gameTypeId),
  caption = 'ActiveFranchise vs Game Types')
```

|            | Type 2 | Type 3 |
| ---------- | -----: | -----: |
| Active     |     44 |     42 |
| Non-Active |     13 |      5 |

ActiveFranchise vs Game Types

``` r
# Table about the wins data for game type = 2
wins2_a <- id_2 %>% select(activeFranchise,wins, homeWins, roadWins) %>%
  filter(activeFranchise == 'Active') %>% 
  summarise(
    Wins = sum(wins), HomeWins = sum(homeWins), RoadWins = sum(roadWins))

wins2_non <- id_2 %>% select(activeFranchise,wins, homeWins, roadWins) %>%
  filter(activeFranchise == 'Non-Active') %>% 
  summarise(
    Wins = sum(wins), HomeWins = sum(homeWins), RoadWins = sum(roadWins))

table_type_2_win <- data.frame(
  Status = c('Active', 'Non-Active'), Wins = c(50938, 1147), HomeWins = c(29436, 718), RoadWins = c(21502, 429))

knitr::kable(
  (table_type_2_win),
  caption = 'Detailed Wins for Game Type 2 '
  )
```

| Status     |  Wins | HomeWins | RoadWins |
| :--------- | ----: | -------: | -------: |
| Active     | 50938 |    29436 |    21502 |
| Non-Active |  1147 |      718 |      429 |

Detailed Wins for Game Type 2

``` r
# Table about the loss data for game type = 2
loss2_a <- id_2 %>% select(activeFranchise,losses, homeLosses, roadLosses) %>%
  filter(activeFranchise == 'Active') %>% 
  summarise(
    Losses = sum(losses), HomeLosses = sum(homeLosses), RoadLosses = sum(roadLosses))

loss2_non <- id_2 %>% 
  select(activeFranchise,losses, homeLosses, roadLosses) %>%
  filter(activeFranchise == 'Non-Active') %>% 
  summarise(
    Losses = sum(losses), HomeLosses = sum(homeLosses), RoadLosses = sum(roadLosses))

table_type_2_loss <- data.frame(
  Status = c('Active', 'Non-Active'), Losses = c(45595, 1663), HomeLosses = c(18925, 660), RoadLosses = c(26670, 1003))

knitr::kable(
  (table_type_2_loss),
  caption = 'Detailed Losses for Game Type 2 '
  )
```

| Status     | Losses | HomeLosses | RoadLosses |
| :--------- | -----: | ---------: | ---------: |
| Active     |  45595 |      18925 |      26670 |
| Non-Active |   1663 |        660 |       1003 |

Detailed Losses for Game Type 2

``` r
# Table about the wins data for game type = 3
wins3_a <- id_3 %>% select(activeFranchise,wins, homeWins, roadWins) %>%
  filter(activeFranchise == 'Active') %>% 
  summarise(
    Wins = sum(wins), HomeWins = sum(homeWins), RoadWins = sum(roadWins))

wins3_non <- id_3 %>% select(activeFranchise,wins, homeWins, roadWins) %>%
  filter(activeFranchise == 'Non-Active') %>% 
  summarise(
    Wins = sum(wins), HomeWins = sum(homeWins), RoadWins = sum(roadWins))

table_type_3_win <- data.frame(
  Status = c('Active', 'Non-Active'), Wins = c(4375, 48), HomeWins = c(2505, 21), RoadWins = c(1870, 27))

knitr::kable(
  (table_type_3_win),
  caption = 'Detailed Wins for Game Type 3 '
  )
```

| Status     | Wins | HomeWins | RoadWins |
| :--------- | ---: | -------: | -------: |
| Active     | 4375 |     2505 |     1870 |
| Non-Active |   48 |       21 |       27 |

Detailed Wins for Game Type 3

``` r
# Table about the loss data for game type = 3
loss3_a <- id_3 %>% select(activeFranchise,losses, homeLosses, roadLosses) %>%
  filter(activeFranchise == 'Active') %>% 
  summarise(
    Losses = sum(losses), HomeLosses = sum(homeLosses), RoadLosses = sum(roadLosses))

loss3_non <- id_3 %>% 
  select(activeFranchise,losses, homeLosses, roadLosses) %>%
  filter(activeFranchise == 'Non-Active') %>% 
  summarise(
    Losses = sum(losses), HomeLosses = sum(homeLosses), RoadLosses = sum(roadLosses))

table_type_3_loss <- data.frame(
  Status = c('Active', 'Non-Active'), Losses = c(4352, 59), HomeLosses = c(1866, 27), RoadLosses = c(2486, 32))

knitr::kable(
  (table_type_3_loss),
  caption = 'Detailed Losses for Game Type 3 '
  )
```

| Status     | Losses | HomeLosses | RoadLosses |
| :--------- | -----: | ---------: | ---------: |
| Active     |   4352 |       1866 |       2486 |
| Non-Active |     59 |         27 |         32 |

Detailed Losses for Game Type 3

``` r
# One Variables for 'activeFranchise' and 'type id'

g1 <- ggplot(data = team_total, aes(x=activeFranchise))
g1 + geom_bar(fill = 'green') +
  labs(x = "Active Status") +
  ggtitle("Bar Plot for Active Status")
```

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
g2 <- ggplot(data = team_total, aes(x=gameTypeId))
g2 + geom_bar(fill = 'blue') +
  labs(x = "Game Type ID") +
  ggtitle("Bar Plot for Game Types")
```

![](README_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

From these two plots above, we can see that the number of ‘Active
Franchise’ is much larger than that of ‘Non-Active Franchise’ and the
number of game type 2 is slightly larger than that of game type 3.
Therefore, when it comes to the data about the wins/losses, we can
expexct that the number of ‘Active Franchise’ with game type ID should
be the biggest one and we can explore it later.

``` r
ggplot(team_total, aes(x=gameTypeId)) + 
  geom_bar(aes(fill = activeFranchise), 
  position = 'dodge') + xlab('Type ID') + 
  scale_fill_discrete(name = "Active Status") +
  ggtitle("Side-by-Side Bar Plot for Game Types and Active Status")
```

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

From this side-by-side above, we can clear see that most of the active
status is active compared with non-active and the number of type 2 is
much larger than that of type 3.

``` r
# Boxplots for wins/losses 

ggplot(team_total, aes(x = gameTypeId , y = wins)) + 
  geom_boxplot() + 
  geom_jitter(aes(color = activeFranchise)) +
  ggtitle("Boxplot for Wins") + 
  xlab('Type ID')
```

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
ggplot(team_total, aes(x = gameTypeId , y = losses)) + 
  geom_boxplot() + 
  geom_jitter(aes(color = activeFranchise)) +
  ggtitle("Boxplot for Losses") + 
  xlab('Type ID')
```

![](README_files/figure-gfm/unnamed-chunk-6-2.png)<!-- -->

From these two boxplots above, we can see that the range of type 3 is
much smaller than type 2 and also the number of active status is bigger
no matter it is a win or loss.

``` r
# Scatter plot between wins and failures for status 
ggplot(team_total, aes(x = wins, y = losses, group = activeFranchise)) +  geom_point(aes(color = activeFranchise)) +     
   geom_smooth(method = 'lm', color = 'green') + 
   ggtitle("Wins vs Losses for Total Data")
```

![](README_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
# Scatter plot between wins and failures for status for two IDs 
ggplot(id_2, aes(x = wins, y = losses, group = activeFranchise)) + geom_point(aes(color = activeFranchise)) +     
   geom_smooth(method = 'lm', color = 'green') +
   ggtitle("Wins vs Losses for Type ID 2")
```

![](README_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
ggplot(id_3, aes(x = wins, y = losses, group = activeFranchise)) + geom_point(aes(color = activeFranchise)) +     
   geom_smooth(method = 'lm', color = 'green') +
   ggtitle("Wins vs Losses for Type ID 3")
```

![](README_files/figure-gfm/unnamed-chunk-8-2.png)<!-- -->

From these three scatter plots above, we can see that most of the points
are red, corresponding to the fact mentioned before that the majority of
Franchise status is active. And the relationship between wins and losses
is almost a straight line.
