
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

if(any(x < 0)) {
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

if(any (y < 0)) {
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

ifelse (any (x < 0), "x contains negative numbers", "x contains all positive numbers")
```

```
## [1] "x contains all positive numbers"
```

또한 if와 else 사이에 else if 조건을 통해, 여러 조건을 추가할 수도 있습니다.


```r
x = 7

if  (x >= 10){
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
