
# Loop Statement

루프 구문을 사용할 경우 반복된 작업을 간단히 수행할 수 있습니다. 

## 기본 제어 구문 (`if`, `for`, `while` 등)

### `if` 구문

if 구문은 다음과 같이 구성됩니다.


```r
if (test_expression) {
  statement
}
```

괄호 안의 test_expression이 TRUE일 경우에만 statement 코드가 실행됩니다.


```r
x = c(8, 3, -2, 5)

if (any(x < 0)) {
  print('x contains negative number')
}
```

```
## [1] "x contains negative number"
```

만일 x중 음수가 존재하면 *x contains negative number*가 출력되는 조건이며, -2가 음수이므로 조건을 만족합니다.


```r
y = c (8, 3, 2, 5)

if (any (y < 0)) {
  print ("y contains negative numbers")
}
```

y에는 음수가 존재하지 않으므로, 구문이 실행되지 않습니다.

### `if...else` 구문

if 구문만 존재할 시 이를 만족하지 않는 경우 아무런 구문도 실행되지 않습니다. if else 구문의 경우 조건을 만족하지 않을 경우 else에 해당하는 구문이 실행됩니다.


```r
if (test_expression) {
  statement 1
} else {
  statement 2
}
```

만일 test_expression 구문이 TRUE이면 statement 1이 실행되며, 그렇지 않을 경우 statement 2가 실행됩니다.


```r
y = c (8, 3, 2, 5)

if (any (y < 0)) {
  print ("y contains negative numbers")
} else {
  print ("y contains all positive numbers")
}
```

```
## [1] "y contains all positive numbers"
```

y에 음수가  존재하는 if구문이 FALSE 이므로, else에 해당하는 메세지가 출력됩니다.

if...else 구문은 `ifelse()` 함수로 간단히 나타낼 수도 있습니다.


```r
x = c (8, 3, 2, 5)

ifelse(any(x < 0), "x contains negative numbers", "x contains all positive numbers")
```

```
## [1] "x contains all positive numbers"
```

또한 if와 else 사이에 else if 조건을 통해, 여러 조건을 추가할 수도 있습니다.


```r
x = 7

if (x >= 10) {
  print ("x exceeds acceptable tolerance levels")
} else if(x >= 0 & x < 10) {
  print ("x is within acceptable tolerance levels")
} else {
  print ("x is negative")
}
```

```
## [1] "x is within acceptable tolerance levels"
```

위 조건은 다음과 같습니다.

1. x가 10 이상일 경우 **x exceeds acceptable tolerance levels**을 출력합니다.
2. 만일 x가 10 이상, 10 미만일경우 **x is within acceptable tolerance levels**을 출력합니다.
3. 그렇지 않을 경우 **x is negative**을 출력합니다.

x는 7 이므로 else에 해당하는 내용이 출력됩니다. 

### `for loop`

for loop 구문은 특정한 부분의 코드가 반복적으로 수행될 수 있도록 합니다. 


```r
for (i in 1:100) {
  <code: do stuff here with i>
}
```

먼저 i에 1이 들어간 뒤 code에 해당하는 부분이 실행됩니다. 그 후, i에 2가 들어간 뒤 다시 code가 실행되며 이 작업이 100까지 반복됩니다. 실제 예제를 살펴보도록 하겠습니다.


```r
for (i in 2010:2016) {
  output = paste("The year is", i)
  print(output)
}
```

```
## [1] "The year is 2010"
## [1] "The year is 2011"
## [1] "The year is 2012"
## [1] "The year is 2013"
## [1] "The year is 2014"
## [1] "The year is 2015"
## [1] "The year is 2016"
```

i에 2010부터 2016 까지 대입되어 코드가 실행됩니다.

### `while loop`

while 구문은 for 구문과 비슷하며, 조건이 충족되는한 루프가 계속해서 실행됩니다.


```r
counter = 1

while (test_expression) {
  statement
  counter = counter + 1
}
```

test_expression이 TRUE이면 statement 코드가 실행되며, counter를 1씩 더합니다. 즉, test_expression이 FALSE가 될때까지 loop가 반복됩니다.


```r
counter = 1

while (counter <= 5) {
  print(counter)
  counter = counter + 1
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
## [1] 5
```

counter가 5 이하일 경우 이를 출력하며, 6일 경우 test_expression이 FALSE가 되어 loop가 멈추게 됩니다.

### `next`

next 명령어는 loop 구문에서 특정 단계를 실행하지 않고 넘어가기 위해 사용됩니다.


```r
x = 1:5

for (i in x) {
  if (i == 3) {
    next
  }
  print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 4
## [1] 5
```

일반적인 for loop 구문으로써 i를 출력하며, if 구문을 통해 i가 3일 경우 명령을 실행하지 않고 다음 for 구문(i = 4)으로 건너뜁니다.

## Apply 계열 함수

apply 계열 함수는 loop 구문과 비슷한 역할을 하며, 훨씬 간결하게 표현할 수 있습니다.

### `apply()`

`apply()` 함수는 매트릭스나 데이터프레임의 행 혹은 열단위 계산에 자주 사용됩니다. 해당 함수는 다음과 같이 구성됩니다.


```r
apply(x, MARGIN, FUN, ...)
```
- x: 매트릭스, 데이터프레임, 혹은 어레이
- MARGIN: 함수가 적용될 벡. 1은 행을, 2는 열을, c(1, 2)는 행과 열을 의미
- FUN: 적용될 함수
- ...: 기타


```r
head(mtcars)
```

```
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```


```r
apply(mtcars, 2, mean)
```

```
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  20.0906   6.1875 230.7219 146.6875   3.5966   3.2172  17.8487   0.4375 
##       am     gear     carb 
##   0.4062   3.6875   2.8125
```

mtcars 데이터에서 2, 즉 열의 방향으로 평균(mean)을 구합니다.


```r
apply(mtcars, 1, sum)
```

```
##           Mazda RX4       Mazda RX4 Wag          Datsun 710      Hornet 4 Drive 
##               329.0               329.8               259.6               426.1 
##   Hornet Sportabout             Valiant          Duster 360           Merc 240D 
##               590.3               385.5               656.9               271.0 
##            Merc 230            Merc 280           Merc 280C          Merc 450SE 
##               299.6               350.5               349.7               510.7 
##          Merc 450SL         Merc 450SLC  Cadillac Fleetwood Lincoln Continental 
##               511.5               509.9               728.6               726.6 
##   Chrysler Imperial            Fiat 128         Honda Civic      Toyota Corolla 
##               725.7               213.8               195.2               207.0 
##       Toyota Corona    Dodge Challenger         AMC Javelin          Camaro Z28 
##               273.8               519.6               506.1               646.3 
##    Pontiac Firebird           Fiat X1-9       Porsche 914-2        Lotus Europa 
##               631.2               208.2               272.6               273.7 
##      Ford Pantera L        Ferrari Dino       Maserati Bora          Volvo 142E 
##               670.7               379.6               694.7               288.9
```

이번에는 1 즉 행의 방향으로 합계(sum)를 구합니다.


```r
apply(mtcars, 2, quantile, probs = c(0.10, 0.25, 0.05, 0.75, 0.90))
```

```
##       mpg cyl   disp     hp  drat    wt  qsec vs am gear carb
## 10% 14.34   4  80.61  66.00 3.007 1.956 15.53  0  0    3    1
## 25% 15.43   4 120.83  96.50 3.080 2.581 16.89  0  0    3    2
## 5%  12.00   4  77.35  63.65 2.853 1.736 15.05  0  0    3    1
## 75% 22.80   8 326.00 180.00 3.920 3.610 18.90  1  1    4    4
## 90% 30.09   8 396.00 243.50 4.209 4.048 19.99  1  1    5    4
```

각 열의 분위수(quantile)를 구하게 되며, 분위는 probs 인자를 통해 직접 입력할 수 있습니다.

### `lapply()`

`lapply()` 함수는 리스트에 적용되며, 결과 또한 리스트로 반환됩니다. 해당 함수는 다음과 같이 구성됩니다.


```r
lapply(x, FUN, ...)
```
- x: 리스트
- FUN: 적용될 함수
- ...: 기타


```r
data = list(item1 = 1:4,
            item2 = rnorm(10),
            item3 = rnorm(20, 1),
            item4 = rnorm(100, 5))

data
```

```
## $item1
## [1] 1 2 3 4
## 
## $item2
##  [1] -0.56048 -0.23018  1.55871  0.07051  0.12929  1.71506  0.46092 -1.26506
##  [9] -0.68685 -0.44566
## 
## $item3
##  [1]  2.22408  1.35981  1.40077  1.11068  0.44416  2.78691  1.49785 -0.96662
##  [9]  1.70136  0.52721 -0.06782  0.78203 -0.02600  0.27111  0.37496 -0.68669
## [17]  1.83779  1.15337 -0.13814  2.25381
## 
## $item4
##   [1] 5.426 4.705 5.895 5.878 5.822 5.689 5.554 4.938 4.694 4.620 4.305 4.792
##  [13] 3.735 7.169 6.208 3.877 4.597 4.533 5.780 4.917 5.253 4.971 4.957 6.369
##  [25] 4.774 6.516 3.451 5.585 5.124 5.216 5.380 4.498 4.667 3.981 3.928 5.304
##  [37] 5.448 5.053 5.922 7.050 4.509 2.691 6.006 4.291 4.312 6.026 4.715 3.779
##  [49] 5.181 4.861 5.006 5.385 4.629 5.644 4.780 5.332 6.097 5.435 4.674 6.149
##  [61] 5.994 5.548 5.239 4.372 6.361 4.400 7.187 6.533 4.764 3.974 4.290 5.257
##  [73] 4.753 4.652 4.048 4.955 4.215 3.332 4.620 5.919 4.425 5.608 3.382 4.944
##  [85] 5.519 5.301 5.106 4.359 4.150 3.976 5.118 4.053 4.509 4.744 6.844 4.348
##  [97] 5.235 5.078 4.038 4.929
```


```r
lapply(data, mean)
```

```
## $item1
## [1] 2.5
## 
## $item2
## [1] 0.07463
## 
## $item3
## [1] 0.892
## 
## $item4
## [1] 5.022
```

`lapply()` 함수를 통해 각 항목의 평균을 구하며, 결과 또한 리스트 형태입니다.


```r
beaver_data = list(beaver1 = beaver1, beaver2 = beaver2)

lapply(beaver_data, head)
```

```
## $beaver1
##   day time  temp activ
## 1 346  840 36.33     0
## 2 346  850 36.34     0
## 3 346  900 36.35     0
## 4 346  910 36.42     0
## 5 346  920 36.55     0
## 6 346  930 36.69     0
## 
## $beaver2
##   day time  temp activ
## 1 307  930 36.58     0
## 2 307  940 36.73     0
## 3 307  950 36.93     0
## 4 307 1000 37.15     0
## 5 307 1010 37.23     0
## 6 307 1020 37.24     0
```

위 데이터의 각 항목에서 열 별 평균을 구하고자 할 경우 `lapply()` 함수 만으로는 계산이 불가능합니다. 이러한 경우 해당 함수 내부에 직접 `function()`을 정의하여 복잡한 계산을 수행할 수 있습니다.


```r
lapply(beaver_data, function(x) {
  round(apply(x, 2, mean), 2)
})
```

```
## $beaver1
##     day    time    temp   activ 
##  346.20 1312.02   36.86    0.05 
## 
## $beaver2
##     day    time    temp   activ 
##  307.13 1446.20   37.60    0.62
```

`function(x)`를 통해 각 항목에 적용될 함수를 직접 정의할 수 있으며, 열의 방향으로 평균을 구한 뒤 소수 둘째 자리까지 반올림을 하게 됩니다.

### `sapply()`

`sapply()` 함수는 `lapply()` 함수와 거의 동일하며, 결과가 리스트가 아닌 벡터 혹은 매트릭스로 출력된다는 점만 차이가 있습니다.


```r
lapply(beaver_data, function(x) {
  round(apply(x, 2, mean), 2)
})
```

```
## $beaver1
##     day    time    temp   activ 
##  346.20 1312.02   36.86    0.05 
## 
## $beaver2
##     day    time    temp   activ 
##  307.13 1446.20   37.60    0.62
```


```r
sapply(beaver_data, function(x) {
  round(apply(x, 2, mean), 2)
})
```

```
##       beaver1 beaver2
## day    346.20  307.13
## time  1312.02 1446.20
## temp    36.86   37.60
## activ    0.05    0.62
```

## 기타 함수

열과 행이 합계나 평균을 구할 때는 `apply()` 함수보다 간단하게 표현할 수 있는 함수들이 있습니다.


```r
rowSums(mtcars)
```

```
##           Mazda RX4       Mazda RX4 Wag          Datsun 710      Hornet 4 Drive 
##               329.0               329.8               259.6               426.1 
##   Hornet Sportabout             Valiant          Duster 360           Merc 240D 
##               590.3               385.5               656.9               271.0 
##            Merc 230            Merc 280           Merc 280C          Merc 450SE 
##               299.6               350.5               349.7               510.7 
##          Merc 450SL         Merc 450SLC  Cadillac Fleetwood Lincoln Continental 
##               511.5               509.9               728.6               726.6 
##   Chrysler Imperial            Fiat 128         Honda Civic      Toyota Corolla 
##               725.7               213.8               195.2               207.0 
##       Toyota Corona    Dodge Challenger         AMC Javelin          Camaro Z28 
##               273.8               519.6               506.1               646.3 
##    Pontiac Firebird           Fiat X1-9       Porsche 914-2        Lotus Europa 
##               631.2               208.2               272.6               273.7 
##      Ford Pantera L        Ferrari Dino       Maserati Bora          Volvo 142E 
##               670.7               379.6               694.7               288.9
```

```r
colSums(mtcars)
```

```
##    mpg    cyl   disp     hp   drat     wt   qsec     vs     am   gear   carb 
##  642.9  198.0 7383.1 4694.0  115.1  103.0  571.2   14.0   13.0  118.0   90.0
```

`rowSums()` 함수는 행의 합계를, `colSums()` 함수는 열의 합계는 구하며 이는 `apply(mtcars, 1 or 2, sum)` 과 동일합니다.


```r
rowMeans(mtcars)
```

```
##           Mazda RX4       Mazda RX4 Wag          Datsun 710      Hornet 4 Drive 
##               29.91               29.98               23.60               38.74 
##   Hornet Sportabout             Valiant          Duster 360           Merc 240D 
##               53.66               35.05               59.72               24.63 
##            Merc 230            Merc 280           Merc 280C          Merc 450SE 
##               27.23               31.86               31.79               46.43 
##          Merc 450SL         Merc 450SLC  Cadillac Fleetwood Lincoln Continental 
##               46.50               46.35               66.23               66.06 
##   Chrysler Imperial            Fiat 128         Honda Civic      Toyota Corolla 
##               65.97               19.44               17.74               18.81 
##       Toyota Corona    Dodge Challenger         AMC Javelin          Camaro Z28 
##               24.89               47.24               46.01               58.75 
##    Pontiac Firebird           Fiat X1-9       Porsche 914-2        Lotus Europa 
##               57.38               18.93               24.78               24.88 
##      Ford Pantera L        Ferrari Dino       Maserati Bora          Volvo 142E 
##               60.97               34.51               63.16               26.26
```

```r
colMeans(mtcars)
```

```
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  20.0906   6.1875 230.7219 146.6875   3.5966   3.2172  17.8487   0.4375 
##       am     gear     carb 
##   0.4062   3.6875   2.8125
```

`rowMeans()`와 `colMeans()` 함수 역시 각각 행과 열의 평균을 구합니다.
