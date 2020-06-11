ST558 Project 1
================
Yu Jiang
6/10/2020

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
 return((json_1)$data)
}

# Table for /frachise
api('franchise') %>% head()
```

    ##   id firstSeasonId lastSeasonId mostRecentTeamId teamCommonName teamPlaceName
    ## 1  1      19171918           NA                8      Canadiens      Montréal
    ## 2  2      19171918     19171918               41      Wanderers      Montreal
    ## 3  3      19171918     19341935               45         Eagles     St. Louis
    ## 4  4      19191920     19241925               37         Tigers      Hamilton
    ## 5  5      19171918           NA               10    Maple Leafs       Toronto
    ## 6  6      19241925           NA                6         Bruins        Boston

``` r
# Table for /franchise-team-totals
api('franchise-team-totals') %>% head()
```

    ##   id activeFranchise firstSeasonId franchiseId gameTypeId gamesPlayed
    ## 1  1               1      19821983          23          2        2937
    ## 2  2               1      19821983          23          3         257
    ## 3  3               1      19721973          22          2        3732
    ## 4  4               1      19721973          22          3         272
    ## 5  5               1      19261927          10          2        6504
    ## 6  6               1      19261927          10          3         515
    ##   goalsAgainst goalsFor homeLosses homeOvertimeLosses homeTies homeWins
    ## 1         8708     8647        507                 82       96      783
    ## 2          634      697         53                  0       NA       74
    ## 3        11779    11889        674                 81      170      942
    ## 4          806      869         46                  1       NA       84
    ## 5        19863    19864       1132                 73      448     1600
    ## 6         1436     1400        103                  0        1      137
    ##   lastSeasonId losses overtimeLosses penaltyMinutes pointPctg points roadLosses
    ## 1           NA   1181            162          44397    0.5330   3131        674
    ## 2           NA    120              0           4266    0.0039      2         67
    ## 3           NA   1570            159          57422    0.5115   3818        896
    ## 4           NA    124              0           5356    0.0147      8         78
    ## 5           NA   2693            147          85564    0.5125   6667       1561
    ## 6           NA    263              0           8132    0.0000      0        160
    ##   roadOvertimeLosses roadTies roadWins shootoutLosses shootoutWins shutouts
    ## 1                 80      123      592             79           78      193
    ## 2                  0       NA       63              0            0       25
    ## 3                 78      177      714             67           82      167
    ## 4                  0       NA       64              0            0        9
    ## 5                 74      360     1256             66           78      403
    ## 6                  0        7      107              0            0       44
    ##   teamId           teamName ties triCode wins
    ## 1      1  New Jersey Devils  219     NJD 1375
    ## 2      1  New Jersey Devils   NA     NJD  137
    ## 3      2 New York Islanders  347     NYI 1656
    ## 4      2 New York Islanders   NA     NYI  148
    ## 5      3   New York Rangers  808     NYR 2856
    ## 6      3   New York Rangers    8     NYR  244

``` r
# A function for the last three calls
base_url_2 <- 'https://records.nhl.com/site/api/franchise'

ID <- function(arg1, arg2){
 json_2 <- GET(paste0(
    base_url_2, "-", arg1, '-records?cayenneExp=franchiseId=', arg2)) %>%
    content('text') %>%
    fromJSON(flatten = TRUE)
 
 attributes(json_2)
 return((json_2)$data)
}
# Table for /franchise-season-records?cayenneExp=franchiseId=ID
ID('season', '12') %>% head()
```

    ##   id fewestGoals fewestGoalsAgainst fewestGoalsAgainstSeasons
    ## 1 17         145                132              1953-54 (70)
    ##   fewestGoalsSeasons fewestLosses        fewestLossesSeasons fewestPoints
    ## 1       2019-20 (82)           13 1950-51 (70), 1995-96 (82)           39
    ##   fewestPointsSeasons fewestTies fewestTiesSeasons fewestWins fewestWinsSeasons
    ## 1        2019-20 (82)          4      1966-67 (70)         16      1976-77 (80)
    ##   franchiseId     franchiseName homeLossStreak       homeLossStreakDates
    ## 1          12 Detroit Red Wings              7 Feb 20 1982 - Mar 25 1982
    ##   homePointStreak      homePointStreakDates homeWinStreak
    ## 1              24 Nov 05 2011 - Feb 23 2012            23
    ##          homeWinStreakDates homeWinlessStreak    homeWinlessStreakDates
    ## 1 Nov 05 2011 - Feb 19 2012                10 Dec 11 1985 - Jan 18 1986
    ##   lossStreak           lossStreakDates mostGameGoals
    ## 1         14 Feb 24 1982 - Mar 25 1982            15
    ##             mostGameGoalsDates mostGoals mostGoalsAgainst
    ## 1 Jan 23 1944 - NYR 0 @ DET 15       369              415
    ##   mostGoalsAgainstSeasons mostGoalsSeasons mostLosses mostLossesSeasons
    ## 1            1985-86 (80)     1992-93 (84)         57      1985-86 (80)
    ##   mostPenaltyMinutes mostPenaltyMinutesSeasons mostPoints mostPointsSeasons
    ## 1               2391              1987-88 (80)        131      1995-96 (82)
    ##   mostShutouts mostShutoutsSeasons mostTies
    ## 1           13        1953-54 (70)       18
    ##                            mostTiesSeasons mostWins mostWinsSeasons pointStreak
    ## 1 1952-53 (70), 1980-81 (80), 1996-97 (82)       62    1995-96 (82)          20
    ##            pointStreakDates roadLossStreak       roadLossStreakDates
    ## 1 Mar 09 2006 - Apr 17 2006             14 Oct 19 1966 - Dec 21 1966
    ##   roadPointStreak      roadPointStreakDates roadWinStreak
    ## 1              15 Oct 18 1951 - Dec 20 1951            12
    ##          roadWinStreakDates roadWinlessStreak    roadWinlessStreakDates
    ## 1 Mar 01 2006 - Apr 15 2006                26 Dec 15 1976 - Apr 03 1977
    ##   winStreak
    ## 1         9
    ##                                                                                                                                                                                winStreakDates
    ## 1 Mar 03 1951 - Mar 21 1951, Feb 27 1955 - Mar 20 1955, Dec 12 1995 - Dec 31 1995, Mar 03 1996 - Mar 22 1996, Oct 13 2005 - Nov 01 2005, Oct 25 2006 - Nov 14 2006, Oct 18 2007 - Nov 09 2007
    ##   winlessStreak        winlessStreakDates
    ## 1            12 Nov 14 2019 - Dec 10 2019

``` r
# Table for /franchise-goalie-records?cayenneExp=franchiseId=ID
ID('goalie', '12') %>% head()
```

    ##    id activePlayer firstName franchiseId     franchiseName gameTypeId
    ## 1 254        FALSE     Terry          12 Detroit Red Wings          2
    ## 2 267        FALSE     Chris          12 Detroit Red Wings          2
    ## 3 319         TRUE     Jimmy          12 Detroit Red Wings          2
    ## 4 350        FALSE     Allan          12 Detroit Red Wings          2
    ## 5 377        FALSE     Alain          12 Detroit Red Wings          2
    ## 6 386        FALSE       Joe          12 Detroit Red Wings          2
    ##   gamesPlayed lastName losses              mostGoalsAgainstDates
    ## 1         734  Sawchuk    245                         1959-03-07
    ## 2         565   Osgood    149 2009-03-07, 2008-11-11, 1997-02-06
    ## 3         543   Howard    196                         2015-03-14
    ## 4           4   Bester      3                         1991-03-09
    ## 5           3 Chevrier      2             1991-01-30, 1990-10-20
    ## 6          29    Daley     10                         1972-02-26
    ##   mostGoalsAgainstOneGame         mostSavesDates mostSavesOneGame
    ## 1                      10             1959-11-14               50
    ## 2                       7 2010-12-27, 2000-10-22               46
    ## 3                       7             2010-01-07               51
    ## 4                       6             1991-03-09               36
    ## 5                       5             1991-01-30               26
    ## 6                       8             1971-12-25               37
    ##    mostShotsAgainstDates mostShotsAgainstOneGame mostShutoutsOneSeason
    ## 1             1957-11-07                      53                    12
    ## 2 2010-12-27, 2009-01-12                      49                     6
    ## 3             2010-01-07                      52                     6
    ## 4             1991-03-09                      42                     0
    ## 5             1991-01-30                      31                     0
    ## 6             1971-12-25                      42                     0
    ##          mostShutoutsSeasonIds mostWinsOneSeason  mostWinsSeasonIds
    ## 1 19511952, 19531954, 19541955                44 19501951, 19511952
    ## 2 19961997, 19971998, 19992000                39           19951996
    ## 3                     20112012                37 20092010, 20102011
    ## 4           19901991, 19911992                 0 19901991, 19911992
    ## 5                     19901991                 0           19901991
    ## 6                     19711972                11           19711972
    ##   overtimeLosses playerId positionCode rookieGamesPlayed rookieShutouts
    ## 1             NA  8450111            G                70             11
    ## 2             29  8458568            G                41              2
    ## 3             70  8470657            G                63              3
    ## 4             NA  8445458            G                NA             NA
    ## 5             NA  8446052            G                NA             NA
    ## 6             NA  8446290            G                NA             NA
    ##   rookieWins seasons shutouts ties wins
    ## 1         44      14       85  132  350
    ## 2         23      14       39   46  317
    ## 3         37      14       24    0  246
    ## 4         NA       2        0    0    0
    ## 5         NA       1        0    0    0
    ## 6         NA       1        0    5   11

``` r
# Table for //franchise-skater-records?cayenneExp=franchiseId=ID
ID('skater', '12') %>% head()
```

    ##      id activePlayer assists firstName franchiseId     franchiseName gameTypeId
    ## 1 16906        FALSE    1023    Gordie          12 Detroit Red Wings          2
    ## 2 17006        FALSE    1063     Steve          12 Detroit Red Wings          2
    ## 3 17028        FALSE     145       Bob          12 Detroit Red Wings          2
    ## 4 17136        FALSE     281      John          12 Detroit Red Wings          2
    ## 5 17192        FALSE     878   Nicklas          12 Detroit Red Wings          2
    ## 6 17196        FALSE       2       Ron          12 Detroit Red Wings          2
    ##   gamesPlayed goals  lastName
    ## 1        1687   786      Howe
    ## 2        1514   692   Yzerman
    ## 3         474   114   Probert
    ## 4         558   265 Ogrodnick
    ## 5        1564   264  Lidstrom
    ## 6          33     5    Hudson
    ##                                                                                         mostAssistsGameDates
    ## 1                                                                                                 1950-12-28
    ## 2 1986-12-27, 1989-03-15, 1990-01-09, 1990-02-16, 1993-01-17, 1993-03-05, 1994-04-05, 1994-04-13, 1995-02-07
    ## 3                                                                                                 1987-11-14
    ## 4 1980-03-31, 1981-02-17, 1983-01-25, 1983-11-03, 1984-11-23, 1986-01-13, 1986-11-05, 1986-12-11, 1986-12-27
    ## 5                                                                                     1994-01-06, 2003-03-07
    ## 6                                                                                     1938-02-27, 1938-03-06
    ##   mostAssistsOneGame mostAssistsOneSeason mostAssistsSeasonIds
    ## 1                  5                   59             19681969
    ## 2                  4                   90             19881989
    ## 3                  4                   33             19871988
    ## 4                  3                   50             19841985
    ## 5                  4                   64             20052006
    ## 6                  1                    2             19371938
    ##                                                                                                                                                                                                                   mostGoalsGameDates
    ## 1 1950-02-11, 1950-03-19, 1951-01-17, 1951-01-23, 1951-03-17, 1951-12-31, 1952-03-23, 1953-01-11, 1953-01-29, 1955-03-03, 1956-01-19, 1956-12-25, 1961-12-31, 1965-03-21, 1965-12-12, 1968-03-16, 1969-02-06, 1969-02-16, 1969-11-02
    ## 2                                                                                                                                                                                                                         1990-01-31
    ## 3                                                                                                                                                                                                                         1987-12-31
    ## 4                                                                                                                                                             1981-10-29, 1981-11-05, 1982-12-12, 1983-12-23, 1984-12-04, 1985-03-13
    ## 5                                                                                                                                                                                                                         2010-12-15
    ## 6                                                                                                                                                                                                                         1938-02-06
    ##   mostGoalsOneGame mostGoalsOneSeason mostGoalsSeasonIds
    ## 1                3                 49           19521953
    ## 2                4                 65           19881989
    ## 3                3                 29           19871988
    ## 4                3                 55           19841985
    ## 5                3                 20           19992000
    ## 6                2                  5           19371938
    ##   mostPenaltyMinutesOneSeason mostPenaltyMinutesSeasonIds
    ## 1                         109                    19531954
    ## 2                          79                    19891990
    ## 3                         398                    19871988
    ## 4                          30          19821983, 19841985
    ## 5                          50                    20052006
    ## 6                           2                    19371938
    ##                                                                                                      mostPointsGameDates
    ## 1                                                                                                             1956-12-25
    ## 2                                                                                                 1989-03-15, 1990-02-16
    ## 3                                                                                                             1987-11-14
    ## 4                                                                                                             1983-12-23
    ## 5 1991-11-07, 1994-01-06, 1998-03-23, 1999-12-03, 2003-03-07, 2006-03-04, 2006-03-09, 2006-04-13, 2010-12-15, 2010-12-27
    ## 6                                                                                                             1938-02-06
    ##   mostPointsOneGame mostPointsOneSeason mostPointsSeasonIds penaltyMinutes
    ## 1                 6                 103            19681969           1643
    ## 2                 6                 155            19881989            924
    ## 3                 6                  62            19871988           2090
    ## 4                 5                 105            19841985            150
    ## 5                 4                  80            20052006            514
    ## 6                 2                   7            19371938              2
    ##   playerId points positionCode rookiePoints seasons
    ## 1  8448000   1809            R           22      25
    ## 2  8452578   1755            C           87      22
    ## 3  8450561    259            L           21       9
    ## 4  8449972    546            L           32       9
    ## 5  8457063   1142            D           60      20
    ## 6  8444852      7            C            7       2

# 4\. Data Analysis

``` r
# Load the libraries
library(DT)
library(knitr)
library(ggplot2)
```
