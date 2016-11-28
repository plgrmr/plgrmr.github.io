---
layout: post
title:  데이터 나누고 바꾸고 붙이기 1탄 Apply
date: "2016-11-28 21:43:41"
published: true
tags: [data-science, apply]
description: 데이터 가공의 필수적인 함수 중 apply 시리즈를 알아봅니다.
---

R을 공부하면서 혼돈이 오는 파트중 하나가 바로 이 `apply` 친구들을 마주쳤을 때입니다.

열공해보도록 합시다. 

## apply 패밀리

R에는 다양한 Apply 들이 있습니다. `tapply`, `lapply`, `mapply` 와 같은 관련 함수들이 내장되어 있다고 합니다.

### apply

- `apply` 는 Matrix에 대해서만 사용될 수 있습니다. 이는 모든 원소가 Character, numeric, logical 중 어느 것이든지 같은 형이어야만 합니다. 

- `apply` 인자 설명 ```apply(theMatrix, 2, sum)``` 
  * 첫번째 인자 : 다루고자 하는 객체입니다.
  * 두번째 인자 : 함수를 어느기준으로 적용해야할지 지정하는 인자입니다.(1은 행단위 2는 열단위) 
  * 세번째 인자 : 적용하고자 하는 함수입니다.
  

```r
#행렬생성
theMatrix <- matrix(1:9, nrow = 3)

#행렬 프린트
theMatrix
```

```
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
```

```r
#행을 합한다
apply(theMatrix, 1, sum)
```

```
## [1] 12 15 18
```

```r
#열을 합한다
apply(theMatrix, 2, sum)
```

```
## [1]  6 15 24
```

```r
# 결측치 다루어보기 위해 임의로 NA 만들어보기
theMatrix[2, 1] <- NA
apply(theMatrix, 1, sum)
```

```
## [1] 12 NA 18
```

```r
#NA 값 제거하고 apply 수행
apply(theMatrix, 1, sum, na.rm = TRUE)
```

```
## [1] 12 13 18
```

### lapply

* `lapply`는 list의 각 원소에 함수를 적용하고, list 로 그 결과를 리턴합니다.


```r
# 리스트를 만듭니다. 안에 matrix 도 들어있고 숫자도 들어있고 다양한 아이들이 들어있습니다.
theList <- list(A = matrix(1:9, 3), B = 1:5, 
                C = matrix(1:4, 2), D = 2)

# 잘 만들었는지 확인해봅니다.
theList
```

```
## $A
##      [,1] [,2] [,3]
## [1,]    1    4    7
## [2,]    2    5    8
## [3,]    3    6    9
## 
## $B
## [1] 1 2 3 4 5
## 
## $C
##      [,1] [,2]
## [1,]    1    3
## [2,]    2    4
## 
## $D
## [1] 2
```

```r
# 정말 List 각 원소를 더해줍니다. List Apply 여서 lapply 인가봅니다.
lapply(theList, sum)
```

```
## $A
## [1] 45
## 
## $B
## [1] 15
## 
## $C
## [1] 10
## 
## $D
## [1] 2
```


### sapply

* `sapply` 는 lapply 와 기능면에서 모든 것이 동일하며 결과값 반환이 vector 로 된다는 차이점이 있습니다.


```r
sapply(theList, sum)
```

```
##  A  B  C  D 
## 45 15 10  2
```

```r
# 기술적으로 vector 는 list 이기 때문에 lapply, sapply 둘다 입력값으로 vector 를 취할 수 있습니다.

theNames <- c("AAA", "BBBBBB", "CCCCCCCC")
lapply(theNames, nchar)
```

```
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 6
## 
## [[3]]
## [1] 8
```





