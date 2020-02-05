
# `dplyr`을 이용한 데이터 변형하기

데이터를 필터링 하거나, 요약하거나, 정렬하거나, 새로운 변수를 만드는 등 데이터 분석을 위해서는 데이터 변형하고 가공해야 하는 경우가 많습니다. R의 기본 함수도 이러한 기능을 제공하지만. `dplyr` 패키지를 이용할 경우 훨씬 빠르고 효율적으로 업무를 처리할 수 있습니다.

먼저 다음 데이터셋을 만들어 줍니다.


```r
library(dplyr)

expenditure = tbl_df(read.table (header = TRUE, text = "
Division State X1980 X1990 X2000 X2001 X2002 X2003 X2004 X2005 X2006 X2007 X2008 X2009 X2010 X2011
6 Alabama 1146713 2275233 4176082 4354794 4444390 4657643 4812479 5164406 5699076 6245031 6832439 6683843 6670517 6592925
9 Alaska 377947 828051 1183499 1229036 1284854 1326226 1354846 1442269 1529645 1634316 1918375 2007319 2084019 2201270
8 Arizona 949753 2258660 4288739 4846105 5395814 5892227 6071785 6579957 7130341 7815720 8403221 8726755 8482552 8340211
7 Arkansas 666949 1404545 2380331 2505179 2822877 2923401 3109644 3546999 3808011 3997701 4156368 4240839 4459910 4578136
9 California 9172158 21485782 38129479 42908787 46265544 47983402 49215866 50918654 53436103 57352599 61570555 60080929 58248662 57526835
8 Colorado 1243049 2451833 4401010 4758173 5151003 5551506 5666191 5994440 6368289 6579053 7338766 7187267 7429302 7409462
"))

expenditure
```

```
## # A tibble: 6 x 16
##   Division State  X1980  X1990  X2000  X2001  X2002  X2003  X2004  X2005  X2006
##      <int> <fct>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>
## 1        6 Alab… 1.15e6 2.28e6 4.18e6 4.35e6 4.44e6 4.66e6 4.81e6 5.16e6 5.70e6
## 2        9 Alas… 3.78e5 8.28e5 1.18e6 1.23e6 1.28e6 1.33e6 1.35e6 1.44e6 1.53e6
## 3        8 Ariz… 9.50e5 2.26e6 4.29e6 4.85e6 5.40e6 5.89e6 6.07e6 6.58e6 7.13e6
## 4        7 Arka… 6.67e5 1.40e6 2.38e6 2.51e6 2.82e6 2.92e6 3.11e6 3.55e6 3.81e6
## 5        9 Cali… 9.17e6 2.15e7 3.81e7 4.29e7 4.63e7 4.80e7 4.92e7 5.09e7 5.34e7
## 6        8 Colo… 1.24e6 2.45e6 4.40e6 4.76e6 5.15e6 5.55e6 5.67e6 5.99e6 6.37e6
## # … with 5 more variables: X2007 <int>, X2008 <int>, X2009 <int>, X2010 <int>,
## #   X2011 <int>
```

## `select()`: 원하는 열 선택하기

`select()` 함수를 이용해 특정 열만을 선택할 수 있습니다.


```r
sub_exp = expenditure %>% select(Division, State, X2007:X2011)

head(sub_exp)
```

```
## # A tibble: 6 x 7
##   Division State         X2007    X2008    X2009    X2010    X2011
##      <int> <fct>         <int>    <int>    <int>    <int>    <int>
## 1        6 Alabama     6245031  6832439  6683843  6670517  6592925
## 2        9 Alaska      1634316  1918375  2007319  2084019  2201270
## 3        8 Arizona     7815720  8403221  8726755  8482552  8340211
## 4        7 Arkansas    3997701  4156368  4240839  4459910  4578136
## 5        9 California 57352599 61570555 60080929 58248662 57526835
## 6        8 Colorado    6579053  7338766  7187267  7429302  7409462
```

Division과 State 열, 그리고 X2007부터 X2011까지의 열을 선택했습니다.


```r
expenditure %>%
  select(starts_with("X")) %>%
  head()
```

```
## # A tibble: 6 x 14
##    X1980  X1990  X2000  X2001  X2002  X2003  X2004  X2005  X2006  X2007  X2008
##    <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>  <int>
## 1 1.15e6 2.28e6 4.18e6 4.35e6 4.44e6 4.66e6 4.81e6 5.16e6 5.70e6 6.25e6 6.83e6
## 2 3.78e5 8.28e5 1.18e6 1.23e6 1.28e6 1.33e6 1.35e6 1.44e6 1.53e6 1.63e6 1.92e6
## 3 9.50e5 2.26e6 4.29e6 4.85e6 5.40e6 5.89e6 6.07e6 6.58e6 7.13e6 7.82e6 8.40e6
## 4 6.67e5 1.40e6 2.38e6 2.51e6 2.82e6 2.92e6 3.11e6 3.55e6 3.81e6 4.00e6 4.16e6
## 5 9.17e6 2.15e7 3.81e7 4.29e7 4.63e7 4.80e7 4.92e7 5.09e7 5.34e7 5.74e7 6.16e7
## 6 1.24e6 2.45e6 4.40e6 4.76e6 5.15e6 5.55e6 5.67e6 5.99e6 6.37e6 6.58e6 7.34e6
## # … with 3 more variables: X2009 <int>, X2010 <int>, X2011 <int>
```

`select()` 함수 내에 `starts_with()` 함수를 이용할 경우, 해당 문자로 시작하는 열을 모두 선택할 수 있습니다.


```r
expenditure %>% select(-X1980:-X2006)
```

```
## # A tibble: 6 x 7
##   Division State         X2007    X2008    X2009    X2010    X2011
##      <int> <fct>         <int>    <int>    <int>    <int>    <int>
## 1        6 Alabama     6245031  6832439  6683843  6670517  6592925
## 2        9 Alaska      1634316  1918375  2007319  2084019  2201270
## 3        8 Arizona     7815720  8403221  8726755  8482552  8340211
## 4        7 Arkansas    3997701  4156368  4240839  4459910  4578136
## 5        9 California 57352599 61570555 60080929 58248662 57526835
## 6        8 Colorado    6579053  7338766  7187267  7429302  7409462
```

```r
expenditure %>% select(-starts_with("X"))
```

```
## # A tibble: 6 x 2
##   Division State     
##      <int> <fct>     
## 1        6 Alabama   
## 2        9 Alaska    
## 3        8 Arizona   
## 4        7 Arkansas  
## 5        9 California
## 6        8 Colorado
```

마이너스 부호를 사용할 경우, 해당 부분을 제외하고 데이터를 선택할 수 있습니다.

## `rename()`: 이름 새로 부여하기


```r
expenditure %>% select(X2011) %>% rename(`2011` = X2011)
```

```
## # A tibble: 6 x 1
##     `2011`
##      <int>
## 1  6592925
## 2  2201270
## 3  8340211
## 4  4578136
## 5 57526835
## 6  7409462
```

`rename()` 함수를 이용해 X2011 이던 열 이름을 2011로 변형하였습니다.

## `filter()`: 필터링

특정 열에 원하는 데이터가 있는 부분만 필터링을 해야하는 경우가 많으며, `filter()` 함수를 사용해 손쉽게 해결할 수 있습니다.


```r
sub_exp %>% filter(Division == 8)
```

```
## # A tibble: 2 x 7
##   Division State      X2007   X2008   X2009   X2010   X2011
##      <int> <fct>      <int>   <int>   <int>   <int>   <int>
## 1        8 Arizona  7815720 8403221 8726755 8482552 8340211
## 2        8 Colorado 6579053 7338766 7187267 7429302 7409462
```

Division 열의 데이터가 8인 부분만 선택되었습니다. 이러한 필터에는 다양한 조건이 들어갈 수도 있습니다.


```r
sub_exp %>% filter(Division == 7, X2011 > 800000)
```

```
## # A tibble: 1 x 7
##   Division State      X2007   X2008   X2009   X2010   X2011
##      <int> <fct>      <int>   <int>   <int>   <int>   <int>
## 1        7 Arkansas 3997701 4156368 4240839 4459910 4578136
```

Division이 7이며, X2011이 800000인 조건을 동시에 만족하는 행이 선택되었습니다.

## `group_by()`: 원하는 조건으로 그룹화

각 그룹별 통계량을 계산할 때는 `group_by()` 함수를 통해 그룹을 묶고, 계산하는 것이 편합니다.


```r
group.exp = sub_exp %>% group_by(Division)

group.exp
```

```
## # A tibble: 6 x 7
## # Groups:   Division [4]
##   Division State         X2007    X2008    X2009    X2010    X2011
##      <int> <fct>         <int>    <int>    <int>    <int>    <int>
## 1        6 Alabama     6245031  6832439  6683843  6670517  6592925
## 2        9 Alaska      1634316  1918375  2007319  2084019  2201270
## 3        8 Arizona     7815720  8403221  8726755  8482552  8340211
## 4        7 Arkansas    3997701  4156368  4240839  4459910  4578136
## 5        9 California 57352599 61570555 60080929 58248662 57526835
## 6        8 Colorado    6579053  7338766  7187267  7429302  7409462
```

Division을 기준으로 그룹을 묶었습니다. 아직 계산을 하지 않았으므로 데이터프레임 자체는 원래와 동일하며, **Groups**를 통해 어떠한 조건으로 그룹이 묶여있는지 확인됩니다.

## `summarize()`: 요약값 계산하기

요약 통계값은 `summarize()` 함수를 통해 쉽게 계산할 수 있습니다.


```r
sub_exp %>% summarize(Mean_2011 = mean(X2011))
```

```
## # A tibble: 1 x 1
##   Mean_2011
##       <dbl>
## 1 14441473.
```

X2011 열의 평균(mean)을 계산해 Mean_2011로 저장하였습니다. 


```r
sub_exp %>% summarize(
  Min = min(X2011, na.rm = TRUE),
  Median = median(X2011, na.rm = TRUE),
  Var = var(X2011, na.rm = TRUE),
  SD = sd(X2011, na.rm = TRUE),
  Max = max(X2011, na.rm = TRUE),
  N = n()
)
```

```
## # A tibble: 1 x 6
##       Min   Median     Var        SD      Max     N
##     <int>    <dbl>   <dbl>     <dbl>    <int> <int>
## 1 2201270 7001194. 4.50e14 21221360. 57526835     6
```

한 번에 여러 통계량을 계산할 수도 습니다. `n()`은 전체 행 갯수를 의미합니다. `group_by()`를 통해 그룹으로 묶여진 데이터에 `summarize()` 함수를 적용할 경우, 그룹 별 통계량이 계산됩니다.


```r
sub_exp %>% group_by(Division) %>%
  summarize(Mean_2010 = mean(X2010, na.rm = TRUE),
            Mean_2011 = mean(X2011, na.rm = TRUE))
```

```
## # A tibble: 4 x 3
##   Division Mean_2010 Mean_2011
##      <int>     <dbl>     <dbl>
## 1        6  6670517   6592925 
## 2        7  4459910   4578136 
## 3        8  7955927   7874836.
## 4        9 30166340. 29864052.
```

Division별 2010열과 2011열의 평균값이 계산되었습니다.

## `arrange()`: 데이터 정렬하기

`arrange()` 함수를 통해 원하는 열을 기준으로 데이터를 순서대로 정렬할 수 있으며, 오름차순을 기본으로 합니다.


```r
sub_exp %>% group_by(Division) %>%
  summarize(Mean_2010 = mean(X2010, na.rm = TRUE),
            Mean_2011 = mean(X2011, na.rm = TRUE)) %>%
  arrange(Mean_2011)
```

```
## # A tibble: 4 x 3
##   Division Mean_2010 Mean_2011
##      <int>     <dbl>     <dbl>
## 1        7  4459910   4578136 
## 2        6  6670517   6592925 
## 3        8  7955927   7874836.
## 4        9 30166340. 29864052.
```

Mean_2010이 낮은 값부터 높은 순으로 정렬됩니다.


```r
sub_exp %>% group_by(Division) %>%
  summarize(Mean_2010 = mean(X2010, na.rm = TRUE),
            Mean_2011 = mean(X2011, na.rm = TRUE)) %>%
  arrange(desc(Mean_2011))
```

```
## # A tibble: 4 x 3
##   Division Mean_2010 Mean_2011
##      <int>     <dbl>     <dbl>
## 1        9 30166340. 29864052.
## 2        8  7955927   7874836.
## 3        6  6670517   6592925 
## 4        7  4459910   4578136
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

다음의 데이터프레임을 만들도록 합니다.


```r
inflation = tbl_df(read.table (header = TRUE, text = "
Year Annual Inflation
2007 207.342 0.9030811
2008 215.303 0.9377553
2009 214.537 0.9344190
2010 218.056 0.9497461
2011 224.939 0.9797251
2012 229.594 1.0000000
"))

inflation
```

```
## # A tibble: 6 x 3
##    Year Annual Inflation
##   <int>  <dbl>     <dbl>
## 1  2007   207.     0.903
## 2  2008   215.     0.938
## 3  2009   215.     0.934
## 4  2010   218.     0.950
## 5  2011   225.     0.980
## 6  2012   230.     1
```

기존 sub_exp 데이터를 `gather()` 함수를 사용해 세로로 긴 형태로 만들어줍니다.


```r
library(tidyr)

long_exp = sub_exp %>%
  gather(Year, Expenditure, X2007:X2011) %>%
  separate(Year, into = c("x", "Year"), sep = "X") %>%
  select(-x) %>%
  mutate(Year = as.numeric(Year))

head(long_exp)
```

```
## # A tibble: 6 x 4
##   Division State       Year Expenditure
##      <int> <fct>      <dbl>       <int>
## 1        6 Alabama     2007     6245031
## 2        9 Alaska      2007     1634316
## 3        8 Arizona     2007     7815720
## 4        7 Arkansas    2007     3997701
## 5        9 California  2007    57352599
## 6        8 Colorado    2007     6579053
```

`left_join()` 함수를 이용해 왼쪽 데이터프레임을 기준으로 합쳐보도록 합니다. 두 데이터 모두 Year가 있으므로 이를 기준으로 데이터가 합쳐집니다.


```r
join_exp = long_exp %>% left_join(inflation)

join_exp
```

```
## # A tibble: 30 x 6
##    Division State       Year Expenditure Annual Inflation
##       <int> <fct>      <dbl>       <int>  <dbl>     <dbl>
##  1        6 Alabama     2007     6245031   207.     0.903
##  2        9 Alaska      2007     1634316   207.     0.903
##  3        8 Arizona     2007     7815720   207.     0.903
##  4        7 Arkansas    2007     3997701   207.     0.903
##  5        9 California  2007    57352599   207.     0.903
##  6        8 Colorado    2007     6579053   207.     0.903
##  7        6 Alabama     2008     6832439   215.     0.938
##  8        9 Alaska      2008     1918375   215.     0.938
##  9        8 Arizona     2008     8403221   215.     0.938
## 10        7 Arkansas    2008     4156368   215.     0.938
## # … with 20 more rows
```

Division과 Start, Year, Expenditure는 long_exp 데이터를 그대로 가져오며, Year에 매핑되는 Anuual, Inlation 값이 새로운 열로 생성됩니다.

## `mutate()`: 새로운 열 생성하기

`mutate()` 함수를 사용해 기존 열끼리 계산을 하여 새로운 열을 생성할 수 있습니다.


```r
inflation_adj = join_exp %>%
  mutate(Adj_Exp = Expenditure / Inflation)

head(inflation_adj)
```

```
## # A tibble: 6 x 7
##   Division State       Year Expenditure Annual Inflation   Adj_Exp
##      <int> <fct>      <dbl>       <int>  <dbl>     <dbl>     <dbl>
## 1        6 Alabama     2007     6245031   207.     0.903  6915249.
## 2        9 Alaska      2007     1634316   207.     0.903  1809711.
## 3        8 Arizona     2007     7815720   207.     0.903  8654505.
## 4        7 Arkansas    2007     3997701   207.     0.903  4426735.
## 5        9 California  2007    57352599   207.     0.903 63507695.
## 6        8 Colorado    2007     6579053   207.     0.903  7285119.
```

Expenditure를 Inflation으로 나눈 값을 Adj_Exp 열로 생성하였습니다.


```r
rank_exp = inflation_adj %>%
  filter(Year == 2010) %>%
  arrange(desc(Adj_Exp)) %>%
  mutate(Rank = 1:length(Adj_Exp))

head(rank_exp)
```

```
## # A tibble: 6 x 8
##   Division State       Year Expenditure Annual Inflation   Adj_Exp  Rank
##      <int> <fct>      <dbl>       <int>  <dbl>     <dbl>     <dbl> <int>
## 1        9 California  2010    58248662   218.     0.950 61330773.     1
## 2        8 Arizona     2010     8482552   218.     0.950  8931389.     2
## 3        8 Colorado    2010     7429302   218.     0.950  7822409.     3
## 4        6 Alabama     2010     6670517   218.     0.950  7023474.     4
## 5        7 Arkansas    2010     4459910   218.     0.950  4695897.     5
## 6        9 Alaska      2010     2084019   218.     0.950  2194291.     6
```

1. Year가 2010인 행만 선택합니다.
2. Adj_Exp를 내림차순으로 정리합니다.
3. Rank열에 순서를 생성합니다.


```r
inflation_adj %>%
  filter(State == 'Arizona') %>%
  mutate(Perc_Chg = (Adj_Exp - lag(Adj_Exp)) / lag(Adj_Exp))
```

```
## # A tibble: 5 x 8
##   Division State    Year Expenditure Annual Inflation  Adj_Exp Perc_Chg
##      <int> <fct>   <dbl>       <int>  <dbl>     <dbl>    <dbl>    <dbl>
## 1        8 Arizona  2007     7815720   207.     0.903 8654505.  NA     
## 2        8 Arizona  2008     8403221   215.     0.938 8960995.   0.0354
## 3        8 Arizona  2009     8726755   215.     0.934 9339231.   0.0422
## 4        8 Arizona  2010     8482552   218.     0.950 8931389.  -0.0437
## 5        8 Arizona  2011     8340211   225.     0.980 8512807.  -0.0469
```

1. State가 Arizona인 행만 선택합니다.
2. `lag()` 함수는 한 단계 이전을 선택하므로, 이를 통해 전년 대비 증가율을 계산할 수 있습니다.


```r
inflation_adj %>%
  filter(Year == 2011) %>%
  arrange(desc(Adj_Exp)) %>%
  mutate(Pct_of_Total = Adj_Exp / sum(Adj_Exp),
         Cum_Perc = cumsum(Pct_of_Total)) %>%
  select(-Expenditure, -Annual, -Inflation) %>%
  slice(1: 5)
```

```
## # A tibble: 5 x 6
##   Division State       Year   Adj_Exp Pct_of_Total Cum_Perc
##      <int> <fct>      <dbl>     <dbl>        <dbl>    <dbl>
## 1        9 California  2011 58717323.       0.664     0.664
## 2        8 Arizona     2011  8512807.       0.0963    0.760
## 3        8 Colorado    2011  7562797.       0.0855    0.846
## 4        6 Alabama     2011  6729362.       0.0761    0.922
## 5        7 Arkansas    2011  4672878.       0.0528    0.975
```

1. Year가 2011인 행만 선택합니다.
2. Adj_Exp를 기준으로 내림차순 정렬합니다.
3. Adj_Exp를 전체 합으로 나누어 비율을 계산합니다.
4. `cumsum()` 함수를 통해 누적합을 구합니다.
5. `select()` 함수를 통해 불필요한 열을 제거합니다.
6. `slice()` 함수를 통해 1부터 5행 까지만 보여줍니다.
