---
layout: post
title: "[Scala] 꼬리 재귀(Tail recursion)"
excerpt: 꼬리 재귀에 대해 알아보자.
categories:
    - Scala
last_modified_at: 2022-05-22
---

## 들어가며

&nbsp; Coursera 강의 중 꼬리 재귀(tail recursion)에 대한 내용이 나와서 정리한다. 꼬리 재귀에 대해 알아보고 Scala로 꼬리 재귀 함수를 표현하는 법을 알아보자.

&nbsp; [Coursera의 Functional Programming Principles in Scala](https://www.coursera.org/learn/scala2-functional-programming) 및 다른 곳의 내용을 참조하였다.

<br/><br/>

## 꼬리 재귀

- Scala에서의 block

&nbsp; 꼬리 재귀를 알아보기 전에 먼저 Scala에서 block이 의미하는 것을 먼저 알아보자. Scala에서는 블록의 마지막 요소가 그 블록의 값을 표현하는 표현식이다. 이 것을 이용하여, Scala에서 함수를 이런 식으로 return을 쓰지 않고 쉽게 표현할 수 있다.

```scala
def testFunc(x: Int, y: Int): Int = {
  val n=3
  x * y + n
}

testFunc(4, 5)
```
```
val res0: Int = 23
```

- 재귀 함수란

&nbsp; 이제 재귀 함수가 무엇인지 다시 한 번 짚고 넘어가자. 재귀 함수란, 자기 자신을 호출하는 함수를 말한다. Factorial 재귀 함수를 Scala로 표현해보자.
```scala
// factorial 함수를 재귀 함수로 표현
def factorialRec(n: Int): Int =
  if (n==0) 1
  else n * factorialRec(n-1)

```
```
val res1: Int = 24
```
&nbsp; 여기서 주의할 게, Scala에서 함수를 선언할 때 타입 추론을 해주는데, 재귀 함수를 쓸 경우에는 리턴 타입을 명시해주어야 한다는 것이다. 명시하지 않을 경우 에러가 발생한다.
```scala
def factorialRecTest(n: Int) =
  if (n==0) 1
  else n * factorialRecTest(n-1)
```
```
-- [E044] Cyclic Error: --------------------------------------------------------
3 |  else n * factorialRecTest(n-1)
  |           ^
  |         Overloaded or recursive method factorialRecTest needs return type

longer explanation available when compiling with `-explain`
1 error found
```

&nbsp; 재귀 함수를 쓰는 이유는, 재귀를 쓰지 않았을 때에 비해 코드가 더 깔끔해지고, 변수들을 더 적게 쓸 수 있기 때문이다. 재귀를 이용하지 않고 표현하면 다음과 같다.
```scala
// factorial 함수를 재귀를 사용하지 않고 표현
def factorialNor(n: Int): Int = {
  var res=1
  var i=1
  if (n==0) 1
  else {
    while (i <= n) {
      res *= i
      i += 1
    }
    res
  }
}

factorialNor(4)
```
```
val res2: Int = 24
```

&nbsp; 하지만 백준 좀 풀어보신 분들은 아시겠지만, 재귀를 쓰면서 문제를 통과하기는 매우 어렵다. 재귀를 사용하면 일반적인 반복문에 비해 성능이 좋지 않기 때문이다.

&nbsp; 먼저, 함수를 호출할 때마다 메모리의 stack에 쌓기 때문에 메모리를 많이 사용하고, stack 공간을 넘을 경우 stack overflow가 발생할 수 있다. 또 각 재귀 호출이 스택 프레임을 새로 만들고 해제를 할 때마다 오버헤드가 발생하기 때문에 일반적인 반복문을 사용했을 때보다 성능이 낮다.

&nbsp; 이런 단점을 해결하기 위해 꼬리 재귀를 사용한다.

- 꼬리 재귀

&nbsp; 꼬리 재귀를 사용하면 Scala 컴파일러가 꼬리 재귀 함수를 자동으로 반복문을 사용하는것 처럼 컴파일하여 성능 저하가 없다.

&nbsp; 꼬리 재귀는 함수가 끝날 때 자기 자신을 호출하면서 끝나도록 만들면 된다.꼬리 재귀 함수를 만들기 위해서는 반환을 그 재귀 함수만 해야 하는데, 위처럼 함수 인자가 하나일 경우 이 것이 불가능하다. 위의 Factorial 함수를 꼬리 재귀로 표현해보자. @tailerec annotation을 통해 꼬리 재귀임을 명시할 수 있다.
```scala
// factorial 함수를 꼬리 재귀로 표현
@tailrec
def factorialTail(acc: Int, n: Int): Int =
  if (n==0) acc
  else factorialTail(acc * n, n-1)

factorialTail(1, 4)
```
```
val res3: Int = 24
```

&nbsp; 그런데 명색이 Factorial 함수인데 인자를 2개 써야한다는 것이 상당히 맘에 안 든다. 이럴 때 Scala의 inner function을 사용할 수 있다.

&nbsp; Inner function은 함수 안에서 선언하는 함수로, 그 함수의 scope 안에서만 사용할 수 있다. Inner function을 사용해서 위 꼬리 재귀 함수를 바꿔보자.
```scala
// Inner function을 이용해 표현
def factorialInner(n: Int): Int =
  @tailrec
  def loop(acc: Int, n: Int): Int =
    if (n==1) acc
    else loop(acc * n, n-1)
  loop(1, n)

factorialInner(4)
```
```
val res4: Int = 24
```

&nbsp; 여기서는 factorialInner 함수의 inner function인 꼬리 재귀함수 loop를 사용하였다.

<br/><br/>

## 마치며

&nbsp; 꼬리 재귀에 대해 알아보고 Scala를 이용해 꼬리 재귀 함수를 표현하는 법을 알아보았다. Scala의 성능 향상 측면에서 꼬리 재귀가 중요한 역할을 하는 것으로 알고 있다. 처음 factorial을 꼬리 재귀로 표현하려고 했을 때, 생각보다 쉽지 않았다. 꼬리 재귀로 생각하는 것에 익숙해지도록 노력해야 할 것이다.
