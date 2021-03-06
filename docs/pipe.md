
# Pipe Operator (`%>%`)

`magrittr` 패키지는 코드의 가독성을 높이는 다양한 오퍼레이터를 제공합니다.

## Pipe Operator (`%>%`)

파이프 오퍼레이터는 값 혹은 결과물을 다음 함수로 전달하는 역할을 합니다.


```r
filter(data, variable == numeric_value)

data %>% filter(variable == numeric_value)
```

위의 두 코드는 동일한 작업을 수행합니다. 적용된 함수가 `filter()` 하나일 때는 그 효용성을 못느끼지만, 적용 함수가 많아질수록 파이프 오퍼레이터를 사용해야 할 필요성을 느낄 것입니다.

예제로써 다음 작업을 수행하고자 합니다.

1. 필터링
2. 조건 별 그룹
3. 요약
4. 정렬

위 작업을 수행하는 3가지 방식의 코드에 대해 알아보며, 사용되는 `dplyr` 패키지 관련 함수는 다음 장에서 확인할 수 있습니다.

### 중첩


```r
library(magrittr)
library(dplyr)

arrange(
  summarize(
    group_by(
      filter(mtcars, carb > 1),
      cyl
    ),
    Avg_mpg = mean(mpg)
  ),
  desc(Avg_mpg)
)
```

```
## # A tibble: 3 x 2
##     cyl Avg_mpg
##   <dbl>   <dbl>
## 1     4    25.9
## 2     6    19.7
## 3     8    15.1
```

계산된 결과를 괄호를 통해 감싸나가는 방법으로써, 가독성이 매우 떨어집니다.

### 다중 객체


```r
a = filter(mtcars, carb > 1)
b = group_by(a, cyl)
c = summarize(b, Avg_mpg = mean(mpg))
d = arrange(c, desc(Avg_mpg))

print(d)
```

```
## # A tibble: 3 x 2
##     cyl Avg_mpg
##   <dbl>   <dbl>
## 1     4    25.9
## 2     6    19.7
## 3     8    15.1
```

각각의 계산과정을 저장하는 방법으로써, 불필요한 복사가 일어나게 됩니다. 

### `%>%`


```r
mtcars %>%
  filter(carb > 1) %>%
  group_by(cyl) %>%
  summarize(Avg_mpg = mean(mpg)) %>%
  arrange(desc(Avg_mpg))
```

```
## # A tibble: 3 x 2
##     cyl Avg_mpg
##   <dbl>   <dbl>
## 1     4    25.9
## 2     6    19.7
## 3     8    15.1
```

파이프 오퍼레이터(`%>%`)를 사용할 경우 코드가 매우 효율적이고 읽기 쉬워집니다. [mtcars → 필터 → 그룹 → 요약 → 정렬]의 계산과정과 동일하게 코드가 전개됩니다.


```r
mtcars %>%
  filter(carb > 1) %>%
  lm(mpg ~ cyl + hp, data = .) 
```

```
## 
## Call:
## lm(formula = mpg ~ cyl + hp, data = .)
## 
## Coefficients:
## (Intercept)          cyl           hp  
##     35.6765      -2.2201      -0.0141
```

파이프 오퍼레이터에서 마침표(.)는 이전 단계까지 계산된 결과를 의미합니다.

