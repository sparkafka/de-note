---
layout: post
title: "[Scala] Lazy Evaluation"
excerpt: "Scala의 Lazy Evaluation을 알아보자."
categories: ['Scala']
last_modified_at: 2023-02-26
published: True
---

## 들어가며

[Coursera Scala 강의](https://www.coursera.org/learn/scala2-functional-program-design)의 Water Pouring 문제를 정리하다가 LazyList에 대한 내용 정리가 필요할 것 같았고, LazyList에 대한 내용을 정리하려면 결국 Lazy Evaluation에 대한 내용을 정리해야 될 것 같아서 포스트를 따로 작성하게 되었다.   

이 포스트는 [Coursera Scala 강의](https://www.coursera.org/learn/scala2-functional-program-design)의 2-3, 2-4 강의를 정리한 내용이다.

## Lazy Evaluation

Lazy Evaluation이란 어떤 값을 선언하는 즉시 평가하는 것이 아닌, 그 값을 사용할 때 평가하는 것이다. 선언될 때 평가하는 기존의 평가 방식은 strict evaluation이라 한다. 

scala에서는 다음과 같이 변수를 선언할 때 앞에 `lazy`를 붙여 lazy evaluation 할 값을 정할 수 있다.

```scala
lazy val x = expr
```

다음은 strict evaluation한 list와 lazy evauation한 list의 차이점을 보여주는 예제이다.

```scala
scala> val list1 = List(1,2,3,4,5)
val list1: List[Int] = List(1, 2, 3, 4, 5)
                      
scala> val output = list1.map(x => x * 2)
val output: List[Int] = List(2, 4, 6, 8, 10)

scala> lazy val outputLazy = list1.map(x => x * 3)
lazy val outputLazy: List[Int]

scala> outputLazy.foreach(println)
3
6
9
12
15
```

위 예제에서 output은 전통적인 방식의 평가법이다. 선언되는 즉시 값이 평가되는 것을 볼 수 있다. lazy로 선언한 outputLazy는 선언될 때 평가되지 않고 값들을 사용할 때 평가가 되는 모습이다.   

순수 함수형 프로그래밍 언어에서는 기본적으로 lazy evaluation으로 변수 선언이 이루어지지만, lazy evaluation은 평가가 될 때 환경이 어떻게 변할지 알 수 없기 때문에 기본적으로는 전통적인 strict evluation을 사용하고 필요할 때 lazy evulation을 사용할 수 있다고 한다.   

lazy evaluation은 선언한 값이 필요할 때에만 평가를 하면 되므로 소모되는 자원에서 이득을 볼 수 있고, lazy evaluation을 사용하면 무한한 값이 나오는 재귀 함수를 사용할 수 있다는 것이 장점이다.

다음과 같은 함수가 있다고 하자. 출력값이 어떻게 될까?

```scala
def expr = {
  val x = {
    print("x")
    1
  }
  lazy val y = {
    print("y")
    2
  }
  def z = {
    print("z")
    3
  }
  z + y + x + z + y + x
}
expr
```

먼저 `x`는 일반적인 변수로, 선언될 때 평가된다. 그러므로 `x`는 `expr` 함수가 호출될 때 바로 평가되어 `x`가 출력된다.   

`y`는 lazy evaluation되는 변수로, 호출될 때 평가되고 한 번 호출 됐으면 나중에 다시 평가되지 않고 계속 재사용할 수 있다. 그러므로 `y`가 처음 쓰일 때 `y`가 출력되고, 그 뒤에는 `y`가 다시 재사용 되더라도 `y`가 다시 출력되지는 않는다.   

`z`는 함수로, 호출될 때마다 수행된다. 그러므로 `z`는 호출될 때마다 `z`가 출력된다.   

따라서 ```xzyz```가 출력되고 expr 값은 12가 된다.

## LazyList

LazyList는 scala에서 그 안의 요소들이 쓰이는 순간에 평가를 하는 linked list이다. 2.12까지는 Stream이었으나 용어 혼동이 된다고 하여 뜻이 더 명확한 LazyList로 이름을 변경하게 됐다고 한다.

```scala
scala> val lazyList: LazyList[Int] = LazyList(1,2,3,4,5)
val lazyList: LazyList[Int] = LazyList(<not computed>)
                   
scala> lazyList(1)
val res2: Int = 2
```

위와 같이 LazyList 안의 요소들이 사용되기 전에는 계산되지 않는 것을 볼 수 있다.   

이 LazyList가 자주 쓰이는 때는 위에서 말했던 무한한 값이 나오는 재귀 함수를 사용할 때이다.

```scala
scala> def from(n: Int): LazyList[Int] = n #:: from(n+1)
def from(n: Int): LazyList[Int]
    
scala> from(2).take(3).toList
val res0: List[Int] = List(2, 3, 4)
```

LazyList에서는 head에 요소를 추가하기 위해 `#::`를 사용한다.   

위 함수같은 경우는 기존의 List사용할 경우 함수가 호출되는 순간 무한한 값이 나와 에러가 발생되겠지만, LazyList는 안의 값들이 호출될 때 계산되므로 값이 정상적으로 계산될 수 있다.   

위 from 함수에서 0부터 10개의 요소에 4를 곱한 결과 list를 얻으려면 다음과 같다.

```scala
scala> val nats = from(0)
val nats: LazyList[Int] = LazyList(<not computed>)

scala> val m4s = nats map (_ * 4)
val m4s: LazyList[Int] = LazyList(<not computed>)

scala> m4s.take(10).toList
val res2: List[Int] = List(0, 4, 8, 12, 16, 20, 24, 28, 32, 36)
```

## 마치며

lazy evulation에 대해 헷갈리던 것을 다시 정리할 수 있어서 좋은 시간이었다. Spark에서도 많이 쓰이는 개념이니 만큼 잘 활용할 수 있도록 노력해야 한다.

## Reference

- [https://www.scala-lang.org/api/2.13.4/scala/collection/immutable/LazyList.html](https://www.scala-lang.org/api/2.13.4/scala/collection/immutable/LazyList.html)   
- [https://www.coursera.org/learn/scala2-functional-program-design](https://www.coursera.org/learn/scala2-functional-program-design)   
