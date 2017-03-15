---
layout: post
title:  beggining r 12th
date: "2017-03-15 17:35:31"
published: false
tags: [example1, example2]
---

## cbind 와 rbind


```r
#벡터 세개 생성 후 
sport <- c("Hockey", "Baseball", "Football")
league <- c("NHL", "MLB", "NFL")
trophy <- c("Stanley Cup", "Commissioner's Trophy", "Vince lombardi trophy")

trophies1 <- cbind(sport, league, trophy)
trophies1
```

```
##      sport      league trophy                 
## [1,] "Hockey"   "NHL"  "Stanley Cup"          
## [2,] "Baseball" "MLB"  "Commissioner's Trophy"
## [3,] "Football" "NFL"  "Vince lombardi trophy"
```

```r
# data.frame() 을 사용하여 data.frame 을 하나 더 만든다

trophies2 <- data.frame(sport = c("Baseketball", "Golf"), league = c("NBA", "PGA"), trophy = c("Larry trophy", "Wanamaker"), stringsAsFactors = FALSE)
trophies2
```

```
##         sport league       trophy
## 1 Baseketball    NBA Larry trophy
## 2        Golf    PGA    Wanamaker
```

```r
#rbind를 사용하여 두 data.frame을 결합하고 하나의 data.frame을 생성한다.
trophies <- rbind(trophies1, trophies2)
trophies
```

```
##         sport league                trophy
## 1      Hockey    NHL           Stanley Cup
## 2    Baseball    MLB Commissioner's Trophy
## 3    Football    NFL Vince lombardi trophy
## 4 Baseketball    NBA          Larry trophy
## 5        Golf    PGA             Wanamaker
```

```r
#cbind 에서는 vector에 새로운 열 이름을 지정가능
cbind(Sport = sport, Association = league, Prize = trophy)
```

```
##      Sport      Association Prize                  
## [1,] "Hockey"   "NHL"       "Stanley Cup"          
## [2,] "Baseball" "MLB"       "Commissioner's Trophy"
## [3,] "Football" "NFL"       "Vince lombardi trophy"
```

## 조인(join)


```r
download.file(url = "http://jaredlander.com/data/US_Foreign_Aid.zip", destfile = "ForeignAid.zip")
unzip("ForeignAid.zip")
require(stringr)
```

```
## Loading required package: stringr
```

```r
theFiles <- dir("", pattern = "nn.csv")
for(a in theFiles) {
  
  #데이터 할당할 이름을 생성한다.
  nameToUse <- str_sub(string = a, start = 12, end = 18)
  #read.table을 사용하여 csv를 읽는다.
  #file.path 는 폴더와 파일명을 명시하는 편리한 방법이다.
  temp <- read.table(file = file.path("", a),
                     header = TRUE, sep = ",", stringsAsFactors = FALSE)
  #생성 객체를 작업공간에 할당한다.
  assign(x = nameToUse, value = temp)
  
}
```

## Merge 

R은 두 data.frame 을 결합하기 위해 merge 라는 내장함수를 가지고 있다.

