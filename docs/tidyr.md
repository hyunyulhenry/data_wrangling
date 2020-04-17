
# (PART) 데이터 구조 변형하기 {-}

# `tidyr`을 이용한 데이터 모양 바꾸기

깔끔한 데이터(tidy data)는 다음과 같이 구성되 있습니다.

1. 각 변수(variable)는 열로 구성됩니다.
2. 각 관측값(observation)은 행으로 구성됩니다.
3. 각 타입의 관측치는 테이블을 구성합니다.

<div class="figure" style="text-align: center">
<img src="images/tidy_data.png" alt="tidy 데이터 요건" width="70%" />
<p class="caption">(\#fig:unnamed-chunk-2)tidy 데이터 요건</p>
</div>

`tidyr` 패키지는 이러한 깔끔한 데이터를 만드는데 필요한 여러 함수가 있습니다.

## 세로로 긴 데이터 만들기

먼저 가로로 긴(Wide) 데이터를 세로로 길게 만드는데는 `gather()` 함수가 사용됩니다. 이 함수는 여러 열을 key-value 페어로 변형해줍니다.


```r
library(tidyr)

table4a
```

```
## # A tibble: 3 x 3
##   country     `1999` `2000`
## * <chr>        <int>  <int>
## 1 Afghanistan    745   2666
## 2 Brazil       37737  80488
## 3 China       212258 213766
```

세 국가의 1999, 2000년 데이터가 있습니다. 이 중 country를 제외한 연도별 데이터를 세로로 길게 만들도록 하겠습니다.


```r
long = table4a %>% gather(key = years, value = cases, -country)

print(long)
```

```
## # A tibble: 6 x 3
##   country     years  cases
##   <chr>       <chr>  <int>
## 1 Afghanistan 1999     745
## 2 Brazil      1999   37737
## 3 China       1999  212258
## 4 Afghanistan 2000    2666
## 5 Brazil      2000   80488
## 6 China       2000  213766
```

열 이름에 해당하던 데이터가 year 열에 들어왔으며, 관측치에 해당하는 값이 cases 열에 왔습니다.

<table class="kable_wrapper table table-striped" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> country </th>
   <th style="text-align:right;"> 1999 </th>
   <th style="text-align:right;"> 2000 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:right;"> 745 </td>
   <td style="text-align:right;"> 2666 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:right;"> 37737 </td>
   <td style="text-align:right;"> 80488 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:right;"> 212258 </td>
   <td style="text-align:right;"> 213766 </td>
  </tr>
</tbody>
</table>

 </td>
   <td> 

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> country </th>
   <th style="text-align:left;"> years </th>
   <th style="text-align:right;"> cases </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> 1999 </td>
   <td style="text-align:right;"> 745 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> 1999 </td>
   <td style="text-align:right;"> 37737 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> 1999 </td>
   <td style="text-align:right;"> 212258 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Afghanistan </td>
   <td style="text-align:left;"> 2000 </td>
   <td style="text-align:right;"> 2666 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Brazil </td>
   <td style="text-align:left;"> 2000 </td>
   <td style="text-align:right;"> 80488 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> China </td>
   <td style="text-align:left;"> 2000 </td>
   <td style="text-align:right;"> 213766 </td>
  </tr>
</tbody>
</table>

 </td>
  </tr>
</tbody>
</table>


## 가로로 긴 데이터 만들기

`gather()` 함수와 반대로, `spread()` 함수를 이요할 경우 세로로 긴 데이터를 가로로 길게 만들 수 있습니다. 위의 데이터에 year 열에 있는 항목들을 열 이름으로, cases 열에 있는 항목들을 가로로 길게 되돌려야 합니다.


```r
back2wide = long %>% spread(years, cases)

back2wide
```

```
## # A tibble: 3 x 3
##   country     `1999` `2000`
##   <chr>        <int>  <int>
## 1 Afghanistan    745   2666
## 2 Brazil       37737  80488
## 3 China       212258 213766
```

원래와 동일한 데이터로 되돌아 왔습니다.

## 하나의 열을 여러 열로 나누기


```r
table3
```

```
## # A tibble: 6 x 3
##   country      year rate             
## * <chr>       <int> <chr>            
## 1 Afghanistan  1999 745/19987071     
## 2 Afghanistan  2000 2666/20595360    
## 3 Brazil       1999 37737/172006362  
## 4 Brazil       2000 80488/174504898  
## 5 China        1999 212258/1272915272
## 6 China        2000 213766/1280428583
```

rate 열에는 ###/#### 형태로 데이터가 들어가 있습니다. 이를 / 기준으로 앞과 뒤로 각각 나누어보도록 하겠습니다.


```r
table3 %>% 
  separate(rate, into = c("cases", "population"))
```

```
## # A tibble: 6 x 4
##   country      year cases  population
##   <chr>       <int> <chr>  <chr>     
## 1 Afghanistan  1999 745    19987071  
## 2 Afghanistan  2000 2666   20595360  
## 3 Brazil       1999 37737  172006362 
## 4 Brazil       2000 80488  174504898 
## 5 China        1999 212258 1272915272
## 6 China        2000 213766 1280428583
```

rate 열이 "/"를 기준으로 cases와 population 열로 분리되었습니다.


```r
table3 %>% 
  separate(rate, into = c("cases", "population"), remove = FALSE)
```

```
## # A tibble: 6 x 5
##   country      year rate              cases  population
##   <chr>       <int> <chr>             <chr>  <chr>     
## 1 Afghanistan  1999 745/19987071      745    19987071  
## 2 Afghanistan  2000 2666/20595360     2666   20595360  
## 3 Brazil       1999 37737/172006362   37737  172006362 
## 4 Brazil       2000 80488/174504898   80488  174504898 
## 5 China        1999 212258/1272915272 212258 1272915272
## 6 China        2000 213766/1280428583 213766 1280428583
```

remove = FALSE를 추가해주면 원래의 열을 유지합니다.

## 여러 열을 하나로 합치기

`separate()` 함수와는 반대로, `unite()` 함수를 이용시 여러 열을 하나로 합칠 수 있습니다.


```r
table5
```

```
## # A tibble: 6 x 4
##   country     century year  rate             
## * <chr>       <chr>   <chr> <chr>            
## 1 Afghanistan 19      99    745/19987071     
## 2 Afghanistan 20      00    2666/20595360    
## 3 Brazil      19      99    37737/172006362  
## 4 Brazil      20      00    80488/174504898  
## 5 China       19      99    212258/1272915272
## 6 China       20      00    213766/1280428583
```

century와 year 열을 하나로 합쳐보도록 하겠습니다.


```r
table5 %>% 
  unite(new, century, year, sep = "")
```

```
## # A tibble: 6 x 3
##   country     new   rate             
##   <chr>       <chr> <chr>            
## 1 Afghanistan 1999  745/19987071     
## 2 Afghanistan 2000  2666/20595360    
## 3 Brazil      1999  37737/172006362  
## 4 Brazil      2000  80488/174504898  
## 5 China       1999  212258/1272915272
## 6 China       2000  213766/1280428583
```

new 열에 지정한 데이터가 합쳐지게 됩니다. 


```r
table5 %>% 
  unite(new, century, year, sep = "_")
```

```
## # A tibble: 6 x 3
##   country     new   rate             
##   <chr>       <chr> <chr>            
## 1 Afghanistan 19_99 745/19987071     
## 2 Afghanistan 20_00 2666/20595360    
## 3 Brazil      19_99 37737/172006362  
## 4 Brazil      20_00 80488/174504898  
## 5 China       19_99 212258/1272915272
## 6 China       20_00 213766/1280428583
```

sep 인자를 통해 구분자를 선택할 수도 있습니다.

## `tidyr`의 기타 함수

### `fill()`

많은 부분이 결측치로 비어 있으므로, 이를 채워줄 필요가 있습니다.


```r
treatment = tribble(
  ~ person,           ~ treatment, ~response,
  "Derrick Whitmore", 1,           7,
  NA,                 2,           10,
  NA,                 NA,           9,
  "Katherine Burke",  1,           4
)

treatment
```

```
## # A tibble: 4 x 3
##   person           treatment response
##   <chr>                <dbl>    <dbl>
## 1 Derrick Whitmore         1        7
## 2 <NA>                     2       10
## 3 <NA>                    NA        9
## 4 Katherine Burke          1        4
```

treatment의 2번째와 3번째 행에 `NA` 데이터가 있어 이를 채워줄 필요가 있습니다.


```r
treatment %>% 
  fill(person, treatment)
```

```
## # A tibble: 4 x 3
##   person           treatment response
##   <chr>                <dbl>    <dbl>
## 1 Derrick Whitmore         1        7
## 2 Derrick Whitmore         2       10
## 3 Derrick Whitmore         2        9
## 4 Katherine Burke          1        4
```

`fill()` 함수는 결측치가 있을 경우, 각 열의 이전 데이터를 이용해 채워줍니다.

### `replce_na()`

NA 데이터를 특정 값으로 변경할 수도 있습니다.


```r
treatment %>% replace_na(replace = list (person = "unknown"))
```

```
## # A tibble: 4 x 3
##   person           treatment response
##   <chr>                <dbl>    <dbl>
## 1 Derrick Whitmore         1        7
## 2 unknown                  2       10
## 3 unknown                 NA        9
## 4 Katherine Burke          1        4
```

`replace_na()` 함수를 이용해 person 열의 NA 데이터를 **unknown**으로 변경하였습니다.


```r
treatment %>% replace_na(replace = list (person = "unknown", treatment = 1))
```

```
## # A tibble: 4 x 3
##   person           treatment response
##   <chr>                <dbl>    <dbl>
## 1 Derrick Whitmore         1        7
## 2 unknown                  2       10
## 3 unknown                  1        9
## 4 Katherine Burke          1        4
```

각 열마다 서로 다른 값으로 변경할 수 있습니다.
