---
title: "패키지 만들기"
author: "JuyoungMaeng"
date: '2016-11-29'
published: false
---




## 패키지 만들기 101

r을 활용하여 패키지 만드는 방법을 소개하고자 합니다.

패키지 만드는 것에는 다양한 방법이 있습니다만 본 글에서는

R Studio 를 활용하여 간편하게 만드는 방법을 알아보도록 하겠습니다.

## 패키지 만들기 101에 사용된 예제

패키지 만들기101 의 설명을 돕기 위해 현재 개발중인 패키지를 사용하였습니다.

예제에 사용된 seoulmap 패키지는 좌표값 혹은 시/구/동 단위의 값을 parameter 에 넘기면

해당범위를 시각화하여 보여주는 패키지입니다.



```r
seoulmap <- function(gu = NULL, dong = NULL, fill) {

  if(is.null(dong)) {
    libpath <- .libPaths()
    datapath <- "/r/seoulmap/data"
    path <- paste(libpath, datapath, sep = "")
    seoul = readOGR(dsn=path.expand(datapath), layer="seoul_municipalities")
    seoul@data$id = rownames(seoul@data)
    gpclibPermit()
    seoul.points = fortify(seoul, region="id")
    seoul.df = join(seoul.points, seoul@data, by="id")
    ggplot(seoul.df) +
      aes(long,lat,group=group,fill=EMD_KOR_NM) +
      geom_polygon(data=subset(seoul.df,EMD_KOR_NM==gu), fill=fill) +
      geom_path() +
      coord_equal() +
      theme_tufte()

  } else if(is.null(gu)) {
    libpath <- .libPaths()
    datapath <- "/r/seoulmap/data"
    path <- paste(libpath, datapath, sep = "")
    seoul = readOGR(dsn=path.expand(datapath), layer="seoul_neighborhoods")
    seoul@data$id = rownames(seoul@data)
    gpclibPermit()
    seoul.points = fortify(seoul, region="id")
    seoul.df = join(seoul.points, seoul@data, by="id")
    ggplot(seoul.df) +
      aes(long,lat,group=group,fill=EMD_KOR_NM) +
      geom_polygon(data=subset(seoul.df,EMD_KOR_NM==dong), fill=fill) +
      geom_path() +
      coord_equal() +
      theme_tufte()

  } else {


  }

}
seoulmap(dong=("구로동"),fill="red")
```

```
## OGR data source with driver: ESRI Shapefile 
## Source: "/r/seoulmap/data", layer: "seoul_neighborhoods"
## with 467 features
## It has 6 fields
```

```
## Warning in gpclibPermit(): support for gpclib will be withdrawn from
## maptools at the next major release
```

![plot of chunk seoulmap](/figure/source/making-package/2016-11-29-making-package/seoulmap-1.png)


## 따라하기만 하면 되는 패키지 만들기


<img src="./img/1.png" width="500">

RStudio 에서 File -> New Project 를 누릅니다.

<img src="/r/project/testpackage/img/2.png" width="500">

새로운 폴더를 만들 것이므로 New Directory 로 갑니다.

<img src="/r/project/testpackage/img/3.png" width="500">

R Project 를 클릭하여 새로운 프로젝트를 만듭니다.

<img src="/r/project/testpackage/img/4.png" width="500">

Type 은 Package, Package name 은 내가 만들 package의 이름

Directory 선택 메뉴에서는 내가 만들 패키지의 폴더가 들어갈 디렉토리를 선택 후

create project 버튼을 누릅니다.

<img src="/r/project/testpackage/img/5.png" width="500">

Package 가 완성되었습니다.

Package의 구성요소가 되는 파일과 폴더 구조에 대해 더 자세히 알아보도록 하겠습니다.


## 패키지 구성요소 파일과 폴더구조

<img src="/r/project/testpackage/img/6.png" width="500">

RStudio를 이용하여 패키지를 생성하면 위와 같은 파일들이 나옵니다.

패키지를 구성하기 위해서는 단순한 R 코드 뿐만이 아니라 문서화 작업이 굉장히 중요합니다.

**DESCRIPTION 파일**

<img src="/r/project/testpackage/img/7.png" width="500">

Description 파일을 열어보면 Package 의 정보가 한눈에 요약되어있습니다.

위 그림에 있는 항목은 하나도 빠짐없이 모두 명시되어야 합니다.

Depends 항목 이외에 다른 항목들은 RStudio 가 프로젝트 생성시 자동으로 만들어주는 항목입니다.

Depends 항목은 패키지에 사용된 함수들을 구동하기 위하여 먼저 깔려 있어야 하는 패키지들의 목록입니다.

Depends 항목에 있는 패키지들은 여러분이 만든 패키지를 로드할때 자동으로 로딩 및 설치가 됩니다.


**man 폴더**

<img src="/r/project/testpackage/img/8.png" width="500">

Man 폴더는 Manual 즉 패키지 안에 들어있는 모든 함수들의 설명서 입니다.

패키지 안에 들어있는 모든 함수의 갯수만큼 man 폴더에 함수에 대한 설명이 들어있어야 합니다.



**R 폴더**

<img src="/r/project/testpackage/img/9.png" width="500">

R 폴더에는 패키지에 사용되는 모든 함수들의 소스코드가 저장됩니다.

이곳에 들어있는 모든 파일들은 패키지 로딩시 자동으로 로드가 됩니다.

## 파일의 상대위치와 절대위치

예제의 활용된 패키지는 shp 파일 형식으로 서울시 지도의 좌표값이 저장되어 있습니다

내 컴퓨터에서 작업을 할 때에는 파일의 위치가 고정되어 있지만 

다른 사람 컴퓨터에서 작업을 할때에는 파일의 위치가 상대적으로 변화하게 됩니다.


```r
libpath <- .libPaths()
datapath <- "/seoulmap/data"
path <- paste(libpath, datapath, sep = "")
```

이럴 경우에는 libPath() 함수를 사용하여 시스템 상의 해당 라이브러리의 주소를 얻어서

내 라이브러리 내의 파일 주소와 결합하면 됩니다.

##배포
<img src="/r/project/testpackage/img/10.png" width="500">

배포는 Build 메뉴에 Build Source 패키지를 누르면 자동으로 해당 폴더에 

패키지를 만들어 주게 됩니다.
