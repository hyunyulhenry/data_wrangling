
# `dplyr`을 이용한 데이터 변형하기

데이터를 필터링 하거나, 요약하거나, 정렬하거나, 새로운 변수를 만드는 등 데이터 분석을 위해서는 데이터 변형하고 가공해야 하는 경우가 많습니다. R의 기본 함수도 이러한 기능을 제공하지만. `dplyr` 패키지를 이용할 경우 훨씬 빠르고 효율적으로 업무를 처리할 수 있습니다.

nycflights13 패키지의 flights 데이터셋을 예제로 사용하도록 하겠습니다.


```r
library(dplyr)
library(nycflights13)

flights
```

```
## # A tibble: 336,776 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

## `select()`: 원하는 열 선택하기

`select()` 함수를 이용해 특정 열만을 선택할 수 있습니다.


```r
flights %>% select(year, month, day)
```

```
## # A tibble: 336,776 x 3
##     year month   day
##    <int> <int> <int>
##  1  2013     1     1
##  2  2013     1     1
##  3  2013     1     1
##  4  2013     1     1
##  5  2013     1     1
##  6  2013     1     1
##  7  2013     1     1
##  8  2013     1     1
##  9  2013     1     1
## 10  2013     1     1
## # … with 336,766 more rows
```

year, month, day 열을 선택했습니다.


```r
flights %>% select(year:day)
```

```
## # A tibble: 336,776 x 3
##     year month   day
##    <int> <int> <int>
##  1  2013     1     1
##  2  2013     1     1
##  3  2013     1     1
##  4  2013     1     1
##  5  2013     1     1
##  6  2013     1     1
##  7  2013     1     1
##  8  2013     1     1
##  9  2013     1     1
## 10  2013     1     1
## # … with 336,766 more rows
```

콜론(:)을 이용해 year부터 day 까지의 열을 한번에 선택할 수도 있습니다.


```r
flights %>% select(-(year:day))
```

```
## # A tibble: 336,776 x 16
##    dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier
##       <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>  
##  1      517            515         2      830            819        11 UA     
##  2      533            529         4      850            830        20 UA     
##  3      542            540         2      923            850        33 AA     
##  4      544            545        -1     1004           1022       -18 B6     
##  5      554            600        -6      812            837       -25 DL     
##  6      554            558        -4      740            728        12 UA     
##  7      555            600        -5      913            854        19 B6     
##  8      557            600        -3      709            723       -14 EV     
##  9      557            600        -3      838            846        -8 B6     
## 10      558            600        -2      753            745         8 AA     
## # … with 336,766 more rows, and 9 more variables: flight <int>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
```

마이너스(-)를 이용할 경우 해당 열을 제외한 모든 열이 선택됩니다.


```r
flights %>% select(starts_with("dep"))
```

```
## # A tibble: 336,776 x 2
##    dep_time dep_delay
##       <int>     <dbl>
##  1      517         2
##  2      533         4
##  3      542         2
##  4      544        -1
##  5      554        -6
##  6      554        -4
##  7      555        -5
##  8      557        -3
##  9      557        -3
## 10      558        -2
## # … with 336,766 more rows
```

`select()` 함수 내에 `starts_with()` 함수를 이용할 경우, 해당 문자로 시작하는 열을 모두 선택할 수 있습니다.

## `rename()`: 이름 새로 부여하기


```r
flights %>% rename(tail_num = tailnum) %>% select(tail_num)
```

```
## # A tibble: 336,776 x 1
##    tail_num
##    <chr>   
##  1 N14228  
##  2 N24211  
##  3 N619AA  
##  4 N804JB  
##  5 N668DN  
##  6 N39463  
##  7 N516JB  
##  8 N829AS  
##  9 N593JB  
## 10 N3ALAA  
## # … with 336,766 more rows
```

`rename()` 함수를 이용해 tailnum 이던 열 이름을 tail_num 으로 변경하였습니다.

## `filter()`: 필터링

특정 열에 원하는 데이터가 있는 부분만 필터링을 해야하는 경우가 많으며, `filter()` 함수를 사용해 손쉽게 해결할 수 있습니다.


```r
flights %>% filter(month == 1, day == 1)
```

```
## # A tibble: 842 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # … with 832 more rows, and 11 more variables: arr_delay <dbl>, carrier <chr>,
## #   flight <int>, tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
## #   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

month 열이 1이고, day 열이 1인 부분만 선택되었습니다.

## `group_by()`: 원하는 조건으로 그룹화

각 그룹별 통계량을 계산할 때는 `group_by()` 함수를 통해 그룹을 묶고, 계산하는 것이 편합니다.


```r
by_day = group_by(flights, year, month, day)

by_day
```

```
## # A tibble: 336,776 x 19
## # Groups:   year, month, day [365]
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

Division을 기준으로 그룹을 묶었습니다. 아직 계산을 하지 않았으므로 데이터프레임 자체는 원래와 동일하며, **Groups**를 통해 어떠한 조건으로 그룹이 묶여있는지 확인됩니다.

## `summarize()`: 요약값 계산하기

요약 통계값은 `summarize()` 함수를 통해 쉽게 계산할 수 있습니다.


```r
by_day %>% summarise(delay = mean(dep_delay, na.rm = TRUE))
```

```
## # A tibble: 365 x 4
## # Groups:   year, month [12]
##     year month   day delay
##    <int> <int> <int> <dbl>
##  1  2013     1     1 11.5 
##  2  2013     1     2 13.9 
##  3  2013     1     3 11.0 
##  4  2013     1     4  8.95
##  5  2013     1     5  5.73
##  6  2013     1     6  7.15
##  7  2013     1     7  5.42
##  8  2013     1     8  2.55
##  9  2013     1     9  2.28
## 10  2013     1    10  2.84
## # … with 355 more rows
```

각 그룹 별 dep_delay의 평균을 구하며, `na.rm` 인자를 TRUE로 설정하여 `NA` 데이터는 제거해 줍니다.


```r
flights %>% group_by(dest) %>%
  summarize(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
)
```

```
## # A tibble: 105 x 4
##    dest  count  dist delay
##    <chr> <int> <dbl> <dbl>
##  1 ABQ     254 1826   4.38
##  2 ACK     265  199   4.85
##  3 ALB     439  143  14.4 
##  4 ANC       8 3370  -2.5 
##  5 ATL   17215  757. 11.3 
##  6 AUS    2439 1514.  6.02
##  7 AVL     275  584.  8.00
##  8 BDL     443  116   7.05
##  9 BGR     375  378   8.03
## 10 BHM     297  866. 16.9 
## # … with 95 more rows
```

한 번에 여러 통계량을 계산할 수도 습니다. `n()`은 전체 행 갯수를 의미합니다. `group_by()`를 통해 그룹으로 묶여진 데이터에 `summarize()` 함수를 적용할 경우, 그룹 별 통계량이 계산됩니다.

## `arrange()`: 데이터 정렬하기

`arrange()` 함수를 통해 원하는 열을 기준으로 데이터를 순서대로 정렬할 수 있으며, 오름차순을 기본으로 합니다.


```r
flights %>% arrange(year, month, day)
```

```
## # A tibble: 336,776 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     1      517            515         2      830            819
##  2  2013     1     1      533            529         4      850            830
##  3  2013     1     1      542            540         2      923            850
##  4  2013     1     1      544            545        -1     1004           1022
##  5  2013     1     1      554            600        -6      812            837
##  6  2013     1     1      554            558        -4      740            728
##  7  2013     1     1      555            600        -5      913            854
##  8  2013     1     1      557            600        -3      709            723
##  9  2013     1     1      557            600        -3      838            846
## 10  2013     1     1      558            600        -2      753            745
## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

[year -> month -> day] 순으로 내림차순 정렬이 됩니다.


```r
flights %>% arrange(desc(dep_delay))
```

```
## # A tibble: 336,776 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
##  1  2013     1     9      641            900      1301     1242           1530
##  2  2013     6    15     1432           1935      1137     1607           2120
##  3  2013     1    10     1121           1635      1126     1239           1810
##  4  2013     9    20     1139           1845      1014     1457           2210
##  5  2013     7    22      845           1600      1005     1044           1815
##  6  2013     4    10     1100           1900       960     1342           2211
##  7  2013     3    17     2321            810       911      135           1020
##  8  2013     6    27      959           1900       899     1236           2226
##  9  2013     7    22     2257            759       898      121           1026
## 10  2013    12     5      756           1700       896     1058           2020
## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>
```

`arrange()` 내에 `desc()` 함수를 추가할 경우 내림차순으로 정렬됩니다.

## join(): 데이터 합치기

각기 다른 데이터를 하나로 합치기 위해서는 join 함수를 사용해야 합니다. `dplyr`에는 다양한 관련 함수가 있습니다.

- `inner_join()`
- `left_join()`
- `right_join()`
- `full_join()`
- `semi_join()`
- `anti_join()`

다음 두개의 데이터 테이블을 이용하도록 합니다.


```r
flights2 = flights %>% 
  select(year:day, hour, tailnum, carrier)

flights2
```

```
## # A tibble: 336,776 x 6
##     year month   day  hour tailnum carrier
##    <int> <int> <int> <dbl> <chr>   <chr>  
##  1  2013     1     1     5 N14228  UA     
##  2  2013     1     1     5 N24211  UA     
##  3  2013     1     1     5 N619AA  AA     
##  4  2013     1     1     5 N804JB  B6     
##  5  2013     1     1     6 N668DN  DL     
##  6  2013     1     1     5 N39463  UA     
##  7  2013     1     1     6 N516JB  B6     
##  8  2013     1     1     6 N829AS  EV     
##  9  2013     1     1     6 N593JB  B6     
## 10  2013     1     1     6 N3ALAA  AA     
## # … with 336,766 more rows
```

```r
airlines
```

```
## # A tibble: 16 x 2
##    carrier name                       
##    <chr>   <chr>                      
##  1 9E      Endeavor Air Inc.          
##  2 AA      American Airlines Inc.     
##  3 AS      Alaska Airlines Inc.       
##  4 B6      JetBlue Airways            
##  5 DL      Delta Air Lines Inc.       
##  6 EV      ExpressJet Airlines Inc.   
##  7 F9      Frontier Airlines Inc.     
##  8 FL      AirTran Airways Corporation
##  9 HA      Hawaiian Airlines Inc.     
## 10 MQ      Envoy Air                  
## 11 OO      SkyWest Airlines Inc.      
## 12 UA      United Air Lines Inc.      
## 13 US      US Airways Inc.            
## 14 VX      Virgin America             
## 15 WN      Southwest Airlines Co.     
## 16 YV      Mesa Airlines Inc.
```

`left_join()` 함수를 이용해 왼쪽 데이터프레임을 기준으로 합쳐보도록 합니다. 두 데이터 모두 carrier 열이 있으므로 이를 기준으로 데이터가 합쳐집니다.


```r
flights2 %>%
  left_join(airlines, by = "carrier")
```

```
## # A tibble: 336,776 x 7
##     year month   day  hour tailnum carrier name                    
##    <int> <int> <int> <dbl> <chr>   <chr>   <chr>                   
##  1  2013     1     1     5 N14228  UA      United Air Lines Inc.   
##  2  2013     1     1     5 N24211  UA      United Air Lines Inc.   
##  3  2013     1     1     5 N619AA  AA      American Airlines Inc.  
##  4  2013     1     1     5 N804JB  B6      JetBlue Airways         
##  5  2013     1     1     6 N668DN  DL      Delta Air Lines Inc.    
##  6  2013     1     1     5 N39463  UA      United Air Lines Inc.   
##  7  2013     1     1     6 N516JB  B6      JetBlue Airways         
##  8  2013     1     1     6 N829AS  EV      ExpressJet Airlines Inc.
##  9  2013     1     1     6 N593JB  B6      JetBlue Airways         
## 10  2013     1     1     6 N3ALAA  AA      American Airlines Inc.  
## # … with 336,766 more rows
```

flights2의 모든 데이터를 가져오며, airlines의 name 열이 기존 테이블에 추가됩니다. join 구문에 대한 더욱 상세한 예제 및 애니메이션은 다음 주소를 참조하시기 바랍니다.

https://github.com/gadenbuie/tidyexplain

## `mutate()`: 새로운 열 생성하기

`mutate()` 함수를 사용해 기존 열끼리 계산을 하여 새로운 열을 생성할 수 있습니다.


```r
flights_sml = flights %>%
  select(
    year:day, 
    ends_with("delay"), 
    distance, 
    air_time
)

flights_sml %>% mutate(
  gain = dep_delay - arr_delay,
  speed = distance / air_time * 60
)
```

```
## # A tibble: 336,776 x 9
##     year month   day dep_delay arr_delay distance air_time  gain speed
##    <int> <int> <int>     <dbl>     <dbl>    <dbl>    <dbl> <dbl> <dbl>
##  1  2013     1     1         2        11     1400      227    -9  370.
##  2  2013     1     1         4        20     1416      227   -16  374.
##  3  2013     1     1         2        33     1089      160   -31  408.
##  4  2013     1     1        -1       -18     1576      183    17  517.
##  5  2013     1     1        -6       -25      762      116    19  394.
##  6  2013     1     1        -4        12      719      150   -16  288.
##  7  2013     1     1        -5        19     1065      158   -24  404.
##  8  2013     1     1        -3       -14      229       53    11  259.
##  9  2013     1     1        -3        -8      944      140     5  405.
## 10  2013     1     1        -2         8      733      138   -10  319.
## # … with 336,766 more rows
```

먼저 flights에서 일부 열을 선택한 후, `mutate()` 함수를 이용해 새로운 열을 만들어 줍니다. gain 열에는 dep_delay와 arr_delay의 차이가, speed 열에는 distance와 air_time 비에 60을 곱한 값이 새롭게 생성됩니다.
