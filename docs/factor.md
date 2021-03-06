
# Factors

R에서 Factor 형태는 범주형(categorical) 데이터를 다룰때 사용됩니다.

## 팩터 생성 및 탐색


```r
gender = factor(c('male', 'female', 'female', 'male', 'female'))
gender
```

```
## [1] male   female female male   female
## Levels: female male
```

`factor()` 함수를 통해 팩터를 생성하며, Levels에는 값들의 고유값인 female과 male이 설정되어 있습니다.


```r
unclass(gender)
```

```
## [1] 2 1 1 2 1
## attr(,"levels")
## [1] "female" "male"
```

```r
levels(gender)
```

```
## [1] "female" "male"
```

```r
summary(gender)
```

```
## female   male 
##      3      2
```

female은 1, male은 2의 integer가 매칭되어 있으므로, `unclass()` 함수를 이용하여 대표값을 출력할 수 있습니다. 또한 `levels()` 함수를 이용해 레벨을 출력할 수 있으며, `summary()` 함수를 이용할 경우 각 레벨의 빈도가 출력됩니다.

## 레벨에 순서 부여하기


```r
gender = factor(c('male', 'female', 'female', 'male', 'female'))
gender
```

```
## [1] male   female female male   female
## Levels: female male
```

레벨을 정의하지 않을 시, 알파벳 순서인 female, male의 순서로 레벨이 정의됩니다.


```r
gender = factor(c('male', 'female', 'female', 'male', 'female'),
                levels = c('male', 'female'))
gender
```

```
## [1] male   female female male   female
## Levels: male female
```

반면 levels 인자를 입력하면, 레벨의 순서가 정의됩니다.


```r
ses = c('low', 'middle', 'low', 'low', 'low', 'low', 'middle', 'low', 'middle',
        'middle', 'middle', 'middle', 'middle', 'high', 'high', 'low', 'middle',
        'middle', 'low', 'high')

ses = factor(ses, levels = c('low', 'middle', 'high'), ordered = TRUE)
ses
```

```
##  [1] low    middle low    low    low    low    middle low    middle middle
## [11] middle middle middle high   high   low    middle middle low    high  
## Levels: low < middle < high
```

또한 ordered 인자를 TRUE로 지정할 시, levels의 크기도 정의됩니다.

## 순서 재정의


```r
library(forcats)

fct_recode(ses, small = 'low', medium = 'middle', large = 'high')
```

```
##  [1] small  medium small  small  small  small  medium small  medium medium
## [11] medium medium medium large  large  small  medium medium small  large 
## Levels: small < medium < large
```

`forcats` 패키지의 `fct_recode()` 함수를 이용해 팩터의 레벨을 재입력 할 수 있습니다. 이 외에도 해당 패키지에는 팩터를 다룰수 있는 다양한 함수가 존재합니다.

