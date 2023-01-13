---
layout: post
title: "[Scala] 구구단 출력하기"
excerpt: Scala의 이중 for문과 String interpolation을 통해 구구단을 출력해보자
categories: ['Scala']
last_modified_at: 2023-01-12
published: True
---

## 들어가며

&nbsp; 회사에서 바쁜 업무가 지나가서, 오랜만에 Scala에 대한 기억을 되살리려고 했으나 어떻게 써야 하는지 전혀 기억이 나지 않았다! 그래서 간단히 구구단을 출력하는 프로그램을 작성하여 Scala에 대한 기억을 되살려보고자 한다. 구구단을 출력하기 전에 필요한 개념들을 정리하고 간단한 구구단 프로그램을 작성하겠다.  
&nbsp; [Scastie](https://scastie.scala-lang.org/)에서 실행했으며, Scala 3.2.0 버전을 사용하였다.

<br/>

## 1. Scala의 main 함수

&nbsp; 기존에 Scala 2까지는 다음과 같이 main 함수를 선언해야 했다.
```scala
object HelloWorld {
  def main(args: Array[String]): Unit = {
    println("Hello, world!")
  }
}

// Hello, World!
```
&nbsp; 아니면 App trait을 상속받아 다음과 같이 표현해야 했다.
```scala
object HelloWorld extends App {
  println("Hello, world!")
}

// Hello, World!
```

&nbsp; Scala 3부터는 다음과 같이 @main annotation을 이용하여 간단히 main 함수를 표현할 수 있다.
```scala
@main
def hello() = println("Hello, World")

// Hello, World!
```

&nbsp; 다음과 같은 활용도 가능하다.
```scala
@main def hello: Unit =
  println(msg)

def msg = "Hello, World!"

// Hello, World!
```

&nbsp; object 안에서도 @main annotation을 쓸 수 있다.
```scala
object HelloWorld {
  @main
  def hello() = println("Hello, world!")
}

// Hello, World!
```

<br/>

## 2. Scala String Interpolation

&nbsp; String Interpolation이란 String 안에 변수들을 직접 넣을 수 있는 기법이다. Scala에서 지원하는 Interpolation은 ```s```, ```f```, ```raw```가 있다.

<br/>

- ```s``` Interpolator

&nbsp; ```s``` Interpolator를 String 앞에 추가하면 String에서 직접 변수를 사용할 수 있다. $와 {}를 사용하면 변수를 사용할 수 있다.

```scala
val a: Int = 10
println(s"a = ${a}")

// a = 10
```

&nbsp; {} 안에서 표현식도 사용 가능하다.

```scala
println(s"1 + 1 = ${1 + 1}")

// 1 + 1 = 2
```

- ```f``` Interpolator

&nbsp; ```f``` Interpolator는 c언어의 printf와 같이, 변수를 formatting하여 쓸 수 있도록 만드는 interpolator이다. ${}뒤에 %를 붙이는 식으로, printf와 비슷하게 쓰면 된다.

```scala
val b = 12.3456
println(f"b = ${b}%.2f")

// b = 12.35
```

- ```raw``` Interpolator

&nbsp; ```raw``` Interpolator는 ```s```와 비슷하지만, escape 문자를 동작시키지 않고 그대로 출력시킨다.

```scala
println("General: first\nsecond")
println(raw"Raw: first\nsecond")

// General: first
// second
// Raw: first\nsecond
```

<br/>

## 3. for 문

&nbsp; Scala에서 for 문은 보통 collection을 조회하는 데에 쓰인다. to, until 구문을 이용해 for 문을 쉽게 사용할 수 있다. to는 끝 포함, until은 끝 미포함이다.

```scala
for (i <- 1 to 5)
  print(i)
println()
for (i <- 1 until 5)
  print(i)

// 12345
// 1234
```

&nbsp; if 문을 같이 써서 조건 처리를 할 수 있다.

```scala
for (i <- 1 to 10 if i % 2 == 0)
  print(i)

// 246810
```

&nbsp; 다음과 같이 같은 괄호 안에 넣어 2중 for 문을 쉽게 구현할 수 있다. 뒤에 오는 iterator가 inner for 문 역할을 한다.

```scala
for (x <- 1 to 3; y <- 1 to 3) {
  println((x,y))
}

// (1,2)
// (1,3)
// (2,1)
// (2,2)
// (2,3)
// (3,1)
// (3,2)
// (3,3)
```

&nbsp; 이제 이것들을 활용하여 구구단을 출력해보자.

<br/>

## 3. 구구단 출력

&nbsp; @main annotation을 통해 main 함수를 구현하고, 이중 for 문과 ```f``` Interpolator를 통해 문자열을 formatting 할 것이다. y가 9일 경우 줄바꿈을 해준다.

```scala
@main
def hello() = {
  for (x <- 2 to 9; y <- 1 to 9) {
    print(f"${x} * ${y} = ${x * y}%2d ")
    if (y == 9) println()
  }
}
```

![구구단 결과](/de-note/assets/images/9th/result_times-table.PNG)

&nbsp; 위와 같이 간단하게 정렬된 구구단이 출력되는 것을 알 수 있다.

<br/>

## 마치며

&nbsp; 오랜만에 Scala를 쓰는데, 다 잊어버려서 지금까지 왜 했나 하는 생각이 들었지만, 그래도 좀 하다보니 적응이 되었다. 다음부터는 잊지 않게 자주자주 봐야겠다.

## Reference

[https://docs.scala-lang.org/scala3/book/methods-main-methods.html](https://docs.scala-lang.org/scala3/book/methods-main-methods.html)<br/>
[https://docs.scala-lang.org/overviews/core/string-interpolation.html](https://docs.scala-lang.org/overviews/core/string-interpolation.html)<br/>
[https://docs.scala-lang.org/scala3/book/control-structures.html#for-loops](https://docs.scala-lang.org/scala3/book/control-structures.html#for-loops)<br/>
[https://docs.scala-lang.org/tour/for-comprehensions.html#inner-main](https://docs.scala-lang.org/tour/for-comprehensions.html#inner-main)
