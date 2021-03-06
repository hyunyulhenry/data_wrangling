
# Missing Value

데이터 분석에서 결측치를 처리하는 것은 매우 중요하며, R에서 결측치는 `NA`로 표시됩니다. 

## 결측치 테스트

`is.na()` 함수를 통해 결측치 여부를 확인할 수 있습니다.


```r
x = c(1:4, NA, 6:7, NA)
x
```

```
## [1]  1  2  3  4 NA  6  7 NA
```

```r
is.na(x)
```

```
## [1] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE  TRUE
```

NA의 경우 TRUE, 그렇지 않을 경우 FALSE를 반환합니다.


```r
df = data.frame (col1 = c (1:3, NA),
                 col2 = c ("this", NA,"is", "text"),
                 col3 = c (TRUE, FALSE, TRUE, TRUE),
                 col4 = c (2.5, 4.2, 3.2, NA),
                 stringsAsFactors = FALSE)

df
```

```
##   col1 col2  col3 col4
## 1    1 this  TRUE  2.5
## 2    2 <NA> FALSE  4.2
## 3    3   is  TRUE  3.2
## 4   NA text  TRUE   NA
```


```r
is.na(df)
```

```
##       col1  col2  col3  col4
## [1,] FALSE FALSE FALSE FALSE
## [2,] FALSE  TRUE FALSE FALSE
## [3,] FALSE FALSE FALSE FALSE
## [4,]  TRUE FALSE FALSE  TRUE
```

```r
which(is.na(df))
```

```
## [1]  4  6 16
```

데이터프레임 역시 `is.na()` 함수를 적용할 수 있으며, `which()` 함수를 통해 NA 데이터의 위치를 찾을 수도 있습니다.

## 결측치 제거

### 평균값 대체


```r
y = c(1, 3, NA, 4)
y
```

```
## [1]  1  3 NA  4
```


```r
mean(y)
```

```
## [1] NA
```

데이터에 결측치가 존재하면, 일반적인 연산함수의 결과로 NA를 반환합니다.


```r
mean(y, na.rm = TRUE)
```

```
## [1] 2.667
```

na.rm = TRUE를 추가해주면, NA 데이터를 제외한 나머지 데이터를 대상으로 연산을 합니다.

### 데이터 제거

결측치가 있는 데이터를 삭제해 주는 경우도 있습니다.


```r
df = data.frame (col1 = c (1:3, NA),
                 col2 = c ("this", NA,"is", "text"),
                 col3 = c (TRUE, FALSE, TRUE, TRUE),
                 col4 = c (2.5, 4.2, 3.2, NA),
                 stringsAsFactors = FALSE)

df
```

```
##   col1 col2  col3 col4
## 1    1 this  TRUE  2.5
## 2    2 <NA> FALSE  4.2
## 3    3   is  TRUE  3.2
## 4   NA text  TRUE   NA
```

2행과 4행에 NA 데이터가 존재합니다.


```r
na.omit(df)
```

```
##   col1 col2 col3 col4
## 1    1 this TRUE  2.5
## 3    3   is TRUE  3.2
```

`na.omit()` 함수를 이용해 NA가 존재하는 행을 삭제해주도록 합니다.

## 결측치 대체

결측치가 존재할 경우 평균값을 대신 사용하기도 합니다.


```r
x
```

```
## [1]  1  2  3  4 NA  6  7 NA
```

```r
x[is.na(x)] = mean(x, na.rm = TRUE)
x
```

```
## [1] 1.000 2.000 3.000 4.000 3.833 6.000 7.000 3.833
```

결측치 대신 나머지 값들의 평균인 3.833을 대체하였습니다.


