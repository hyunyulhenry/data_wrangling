
# (PART) 데이터 불러오기 및 내보내기 {-}

# Import Data

데이터 분석의 첫번째 단계는 데이터를 불러오는 것이며, 일반적으로 텍스트 혹은 엑셀 파일로 저장된 데이터를 사용합니다.

아래와 같은 데이터가 각각 csv, excel, RDS 파일로 저장된 예제를 살펴보겠습니다.

```
variable 1, variable 2, variable 3
10,beer,TRUE
25,wine,TRUE
8,cheese,FALSE
```

해당 파일의 주소는 다음과 같습니다.

```
csv: https://github.com/hyunyulhenry/data_wrangling/blob/master/mydata.csv
txt: https://github.com/hyunyulhenry/data_wrangling/blob/master/mydata.txt
excel: https://github.com/hyunyulhenry/data_wrangling/blob/master/mydata.xlsx
rds: https://github.com/hyunyulhenry/data_wrangling/blob/master/mydata.rds
```

먼저 다음 코드를 이용해 해당 파일들을 다운로드 받도록 합니다.


```r
download.file('https://raw.githubusercontent.com/hyunyulhenry/data_wrangling/master/mydata.csv', 'mydata.csv')
download.file('https://raw.githubusercontent.com/hyunyulhenry/data_wrangling/master/mydata.txt', 'mydata.txt')
download.file('https://github.com/hyunyulhenry/data_wrangling/raw/master/mydata.xlsx','mydata.xlsx')
download.file('https://github.com/hyunyulhenry/data_wrangling/raw/master/mydata.rds','mydata.rds')
```


## csv 파일 불러오기

먼저 R의 기본함수와 `readr` 패키지를 이용하여 csv 파일을 불러오는 법에 대해 살펴보겠습니다.

### 기본함수

`read.table()` 함수는 데이터를 불러오는 R의 가장 기본인 함수며, `read.csv()`와 `read.delim()` 함수는 특수한 경우의 래퍼 함수라 볼 수 있습니다.


```r
mydata = read.csv('mydata.csv')
mydata
```

```
##   variable.1 variable.2 variable.3
## 1         10       beer       TRUE
## 2         25       wine       TRUE
## 3          8     cheese      FALSE
```

```r
str(mydata)
```

```
## 'data.frame':	3 obs. of  3 variables:
##  $ variable.1: int  10 25 8
##  $ variable.2: Factor w/ 3 levels "beer","cheese",..: 1 3 2
##  $ variable.3: logi  TRUE TRUE FALSE
```

먼저 열이름인 variable 다음에 점(.)이 붙은 것이 확인됩니다. 이처럼 기본함수를 사용할 경우 열 이름의 공백은 점으로 바뀌게 됩니다.

또한 variable 2의 문자열이 자동으로 팩터 형태로 바뀌게 되었습니다. 이를 방지하기 위해서는 stringAsFactors를 FALSE로 지정해주어야 합니다.


```r
mydata2 = read.csv('mydata.csv', stringsAsFactors = FALSE)
str(mydata2)
```

```
## 'data.frame':	3 obs. of  3 variables:
##  $ variable.1: int  10 25 8
##  $ variable.2: chr  "beer" "wine" "cheese"
##  $ variable.3: logi  TRUE TRUE FALSE
```

`read.table()` 함수를 이용하여 csv 데이터를 불러올 수도 있습니다.


```r
read.table('mydata.csv', sep = ',', header = TRUE, stringsAsFactors = FALSE)
```

```
##   variable.1 variable.2 variable.3
## 1         10       beer       TRUE
## 2         25       wine       TRUE
## 3          8     cheese      FALSE
```

구분자에 해당하는 sep를 ','로 지정해 주며, 열이름에 해당하는 header를 TRUE로 지정해줍니다.


```r
read.table('mydata.csv', sep = ',', header = TRUE, stringsAsFactors = FALSE,
           row.names = c('Row 1', 'Row 2', 'Row 3'),
           col.names = c('Var 1', 'Var 2', 'Var 3'))
```

```
##       Var.1  Var.2 Var.3
## Row 1    10   beer  TRUE
## Row 2    25   wine  TRUE
## Row 3     8 cheese FALSE
```

row.names와 col.names 인자를 통해 행 및 열이름을 직접 지정할 수도 있습니다.


```r
set_classes = read.table('mydata.csv', sep = ',', header = TRUE,
                         colClasses = c('numeric', 'character', 'character'))
str(set_classes)
```

```
## 'data.frame':	3 obs. of  3 variables:
##  $ variable.1: num  10 25 8
##  $ variable.2: chr  "beer" "wine" "cheese"
##  $ variable.3: chr  "TRUE" "TRUE" "FALSE"
```

colClasses 인자를 통해 각 열에 해당하는 클래스를 직접 지정해줄 수도 있으며, 2번째 열이 팩터가 아닌 문자 형태로 입력됩니다.


```r
read.table('mydata.csv', sep = ',', header = TRUE, nrows = 2)
```

```
##   variable.1 variable.2 variable.3
## 1         10       beer       TRUE
## 2         25       wine       TRUE
```

nrows 인자를 통해 전체 데이터가 아닌 지정한 행까지의 데이터만 불러올 수도 있습니다.

### `readr` 패키지

기본 함수 대비 readr 패키지를 이용할 경우 10배 정도 빠르게 데이털르 불러올 수 있습니다.


```r
library(readr)
mydata3 = read_csv('mydata.csv')
str(mydata3)
```

```
## Classes 'spec_tbl_df', 'tbl_df', 'tbl' and 'data.frame':	3 obs. of  3 variables:
##  $ variable 1: num  10 25 8
##  $ variable 2: chr  "beer" "wine" "cheese"
##  $ variable 3: logi  TRUE TRUE FALSE
##  - attr(*, "spec")=
##   .. cols(
##   ..   `variable 1` = col_double(),
##   ..   `variable 2` = col_character(),
##   ..   `variable 3` = col_logical()
##   .. )
```

`read_csv()` 함수를 이용해 csv 파일을 불러올 수 있습니다. 열이름의 공백이 그대로 유지되며, 문자 형태 역시 팩터가 아닌 원래 형식이 그대로 유지됩니다.


```r
read_csv('mydata.csv', col_types = list(col_double(),
                                        col_character(),
                                        col_character()))
```

```
## # A tibble: 3 x 3
##   `variable 1` `variable 2` `variable 3`
##          <dbl> <chr>        <chr>       
## 1           10 beer         TRUE        
## 2           25 wine         TRUE        
## 3            8 cheese       FALSE
```

col_types 인자를 통해 원하는 타입을 직접 입력할 수도 있습니다.


```r
read_csv('mydata.csv', col_names = c('Var 1', 'Var 2', 'Var 3'),
         skip = 1)
```

```
## # A tibble: 3 x 3
##   `Var 1` `Var 2` `Var 3`
##     <dbl> <chr>   <lgl>  
## 1      10 beer    TRUE   
## 2      25 wine    TRUE   
## 3       8 cheese  FALSE
```

col_names 인자를 통해 열이름을 입력할 수 있으며, skip을 통해 위에서 n번째 행을 건너뛰고 데이터를 불러올 수 있습니다.


```r
read_csv('mydata.csv', n_max = 2)
```

```
## # A tibble: 2 x 3
##   `variable 1` `variable 2` `variable 3`
##          <dbl> <chr>        <lgl>       
## 1           10 beer         TRUE        
## 2           25 wine         TRUE
```

n_max 인자를 통해 전체 데이터가 아닌 지정한 행까지의 데이터만 불러올 수도 있습니다.

## 텍스트 파일 불러오기

`.txt` 파일로 저장된 텍스트 파일 역시 `read.delim()` 혹은 `read.table()` 함수를 이용해 불러올 수 있습니다.


```r
read.delim('mydata.txt', sep = ',')
```

```
##   variable.1 variable.2 variable.3
## 1         10       beer       TRUE
## 2         25       wine       TRUE
## 3          8     cheese      FALSE
```


```r
read.table('mydata.txt', sep = ',', header = TRUE)
```

```
##   variable.1 variable.2 variable.3
## 1         10       beer       TRUE
## 2         25       wine       TRUE
## 3          8     cheese      FALSE
```

## 엑셀 파일 불러오기

엑셀 파일을 불러오기 위한 패키지는 매우 많으며, 이 중 `xlsx`와 `readxl` 패키지를 살펴보도록 하겠습니다.

### `xlsx` 패키지

`xlsx` 패캐지의 `read.xlsx()` 함수를 이용해 엑셀 데이터를 불러올 수 있습니다.


```r
library(xlsx)

read.xlsx('mydata.xlsx', sheetName = 'Sheet1')
```

```
##   variable.1 variable.2 variable.3
## 1         10       beer       TRUE
## 2         25       wine       TRUE
## 3          8     cheese      FALSE
```


```r
read.xlsx('mydata.xlsx', sheetName = 'Sheet2')
```

```
##   variable.4 variable.5
## 1     Dayton     johnny
## 2   Columbus      amber
## 3  Cleveland       tony
## 4 Cincinnati      alice
```

엑셀은 여러 시트로 구성되어 있으므로, sheetName 인자를 통해 원하는 시트명의 내용만 불러올 수 있습니다.


```r
read.xlsx('mydata.xlsx', sheetName = 'Sheet3')
```

```
##                                         Header..Company.A        NA.
## 1 What if we want to disregard header text in Excel file?       <NA>
## 2                                              variable 6 variable 7
## 3                                                     200       Male
## 4                                                     225     Female
## 5                                                     400     Female
## 6                                                     310       Male
```

간혹 엑셀 파일에는 데이터가 아닌 설명을 위한 텍스트가 적힌 경우도 있으므로, 이를 제외하고 데이터를 불러올 필요가 있습니다.


```r
read.xlsx('mydata.xlsx', sheetName = 'Sheet3', startRow = 3)
```

```
##   variable.6 variable.7
## 1        200       Male
## 2        225     Female
## 3        400     Female
## 4        310       Male
```

startRow 인자를 통해 3번째 행부터 데이터를 불러오게 되어, 불필요한 텍스트가 제거되었습니다.


```r
read.xlsx('mydata.xlsx', sheetName = 'Sheet3', rowIndex = 3:5)
```

```
##   variable.6 variable.7
## 1        200       Male
## 2        225     Female
```
rowIndex 인자를 통해 원하는 행의 데이터만 불러올 수 있습니다.


```r
read.xlsx('mydata.xlsx', sheetName = 'Sheet4')
```

```
##   Future.Value  Rate Period Present.Value
## 1          500 0.065     10         266.4
## 2          600 0.085      6         367.8
## 3          750 0.080     11         321.7
## 4         1000 0.070     16         338.7
```

불러온 데이터 중 Present Value에 해당하는 열은 $\frac{Future\  Value}{(1+Rate)^{Period}}$를 통해 엑셀 내에서 계산된 값입니다.



```r
read.xlsx('mydata.xlsx', sheetName = 'Sheet4', keepFormulas= TRUE)
```

```
##   Future.Value  Rate Period Present.Value
## 1          500 0.065     10  A2/(1+B2)^C2
## 2          600 0.085      6  A3/(1+B3)^C3
## 3          750 0.080     11  A4/(1+B4)^C4
## 4         1000 0.070     16  A5/(1+B5)^C5
```

keepFormulas인자를 TRUE로 설정할 경우 엑셀 내에서 셀 간의 계산이 되지 않고 수식이 그대로 표현됩니다.

### `readxl` 패키지

`readxl` 패키지는 `readr` 패키지와 거의 비슷하며, C++을 기반으로 하여 매우 빠른 속도를 자랑합니다.


```r
library(readxl)

mydata = read_excel('mydata.xlsx', sheet = 'Sheet5')
mydata
```

```
## # A tibble: 3 x 5
##   `Variable 1` `Variable 2` `Variable 3` `Variable 4`        `Variable 5`       
##          <dbl> <chr>               <dbl> <dttm>              <dttm>             
## 1           10 beer                    1 2015-11-20 00:00:00 2015-11-20 13:30:00
## 2           25 wine                    1 NA                  2015-11-21 16:30:00
## 3            8 <NA>                    0 2015-11-22 00:00:00 2015-11-22 14:45:00
```

```r
str(mydata)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	3 obs. of  5 variables:
##  $ Variable 1: num  10 25 8
##  $ Variable 2: chr  "beer" "wine" NA
##  $ Variable 3: num  1 1 0
##  $ Variable 4: POSIXct, format: "2015-11-20" NA ...
##  $ Variable 5: POSIXct, format: "2015-11-20 13:30:00" "2015-11-21 16:30:00" ...
```

`read_excel()` 함수 역시 sheet 인자를 통해 원하는 시트의 데이터를 불러올 수 있습니다. 빈 칸의 경우 결측치(NA)로 변형되며, 날짜의 경우 POSIXct 형식으로 불러옵니다.


```r
read_excel('mydata.xlsx', sheet = 'Sheet5', skip = 1,
           col_names = paste('Var', 1:5))
```

```
## # A tibble: 3 x 5
##   `Var 1` `Var 2` `Var 3` `Var 4`             `Var 5`            
##     <dbl> <chr>     <dbl> <dttm>              <dttm>             
## 1      10 beer          1 2015-11-20 00:00:00 2015-11-20 13:30:00
## 2      25 wine          1 NA                  2015-11-21 16:30:00
## 3       8 <NA>          0 2015-11-22 00:00:00 2015-11-22 14:45:00
```

skip 인자를 통해 위에서 n 번째까지 행을 건너뛸 수 있으며, col_names 인자를 통해 원하는 열이름을 직접 입력할 수 있습니다.

간혹 결측치가 NA가 아닌 999와 같은 특정 값으로 입력되는 경우도 있으므로, 이를 찾아 NA로 변형해주어야 합니다.


```r
read_excel('mydata.xlsx', sheet = 'Sheet6')
```

```
## # A tibble: 3 x 4
##   `variable 1` `variable 2` `variable 3` `variable 4`
##          <dbl> <chr>               <dbl>        <dbl>
## 1           10 beer                    3        42328
## 2           25 wine                    1          999
## 3            8 999                     0        42330
```


```r
read_excel('mydata.xlsx', sheet = 'Sheet6', na = '999')
```

```
## # A tibble: 3 x 4
##   `variable 1` `variable 2` `variable 3` `variable 4`
##          <dbl> <chr>               <dbl>        <dbl>
## 1           10 beer                    3        42328
## 2           25 wine                    1           NA
## 3            8 <NA>                    0        42330
```

NA로 지정하고 싶은 값을 직접 입력할 수 있습니다.


```r
read_excel('mydata.xlsx', sheet = 'Sheet5')
```

```
## # A tibble: 3 x 5
##   `Variable 1` `Variable 2` `Variable 3` `Variable 4`        `Variable 5`       
##          <dbl> <chr>               <dbl> <dttm>              <dttm>             
## 1           10 beer                    1 2015-11-20 00:00:00 2015-11-20 13:30:00
## 2           25 wine                    1 NA                  2015-11-21 16:30:00
## 3            8 <NA>                    0 2015-11-22 00:00:00 2015-11-22 14:45:00
```


```r
read_excel('mydata.xlsx', sheet = 'Sheet5',
           col_type = c('numeric', 'blank', 'numeric', 'blank', 'date'))
```

```
## # A tibble: 3 x 3
##   `Variable 1` `Variable 3` `Variable 5`       
##          <dbl>        <dbl> <dttm>             
## 1           10            1 2015-11-20 13:30:00
## 2           25            1 2015-11-21 16:30:00
## 3            8            0 2015-11-22 14:45:00
```

col_types 인자를 통해 원하는 클래스를 직접 입력할 수 있으며, blank를 입력할 경우 해당 셀은 불러오지 않습니다.

## R object File

R을 통해 작업한 내용을 csv나 excel 파일이 아닌 Rdata/object 형식(.RData, .rda, .rds)으로 저장하여 공유하는 경우가 있습니다. 


```r
readRDS('mydata.rds')
```

```
##           V1         V2         V3
## 1 variable 1 variable 2 variable 3
## 2         10       beer       TRUE
## 3         25       wine       TRUE
## 4          8     cheese      FALSE
```

각 파일 형식에 따라 `load('mydata.RData)`, `load(file = 'mydata.rda)`, `readRDS(mydata.rds)`와 같은 형태로 데이터를 불러올 수 있습니다.
