---
layout: post
title: "[Scala] Scala Class"
excerpt: "Scala class에 대해 정리하자. Scala3 에서의 변경점을 알아보자."
categories: ['Scala']
last_modified_at: 2023-02-23
published: True
---

## 들어가며

원래는 [Coursera Scala 강의](https://www.coursera.org/learn/scala2-functional-program-design)의 2주차 5번째 강의의 Water Pouring Puzzle에 대한 내용을 정리하면서 그동안 배웠던 내용의 총정리 포스트를 작성하려고 했으나 하다보니 내용이 길어질 것 같아서 따로 포스트를 작성하게 되었다. Water Pouring Puzzle에 대한 내용은 나중에 작성을 할 예정이다. 먼저 class에 대한 내용을 정리해보도록 하겠다.

## Class

스칼라에서는 자바에서와 같이 기본적으로 ```class [클래스명]```으로 클래스를 선언할 수 있다.

```scala
scala> class classTest1
// defined class classTest1
```

scala2 까지는 일반 클래스의 인스턴스를 생성할 때 new를 붙여야 했지만, scala3 부터는 new를 붙이지 않고도 인스턴스를 생성할 수 있다.

```scala
// scala2
scala> val a = new classTest1
val a: classTest1 = classTest1@6a5dd083

// scala3 부터는 new를 붙이지 않아도 된다.
scala> val b= classTest1()
val b: classTest1 = classTest1@7196a8f1
```

원래 스칼라에서, case class를 선언할 때 자동적으로 apply 메서드를 생성하였고, 그래서 case class의 인스턴스를 선언할 때 new를 붙이지 않아도 됐는데 scala3 에서는 이것을 확장하여 일반적인 클래스에도 적용이 되었다.   

[공식 문서](https://docs.scala-lang.org/scala3/reference/other-new-features/creator-applications.html)에 따르면, 클래스를 선언할 때 다음과 같이 apply method를 가진 companion object가 같이 생성된다고 한다.

```scala
// class 선언
class TestBuilder(s: String):
  def this() = this("")
end TestBuilder

// 같이 생성되는 companion object
object TestBuilder:
  inline def apply(s: String): TestBuilder = new TestBuilder(s)
  inline def apply(): TestBuilder = new TestBuilder()
```
```scala
// apply 메서드로 인스턴스가 생성되는 모습
scala> TestBuilder.apply()
val res3: TestBuilder = TestBuilder@66075347
```

또 위와 같이 클래스를 선언하는 문법이 추가가 되었다. ```:```을 쓰고 들여쓰기로 구분을 하고 선언이 끝날 때 ```end [클래스명]```을 붙여주면서 선언이 끝난 것을 명시할 수 있다. 콜론과 들여쓰기로 구분을 하는 것은 파이썬에서 클래스를 선언하는 것이 생각난다. 물론 기존과 같이 ```{}```를 이용해서 선언하는 것도 가능하다.   

클래스를 선언할 때 인자를 넣고 싶으면 위와 같이  ```class [클래스명](인자,...)``` 형식으로 클래스를 선언해야 한다.

```scala
scala> class classTest2(x: Int, y: Int)
// defined class classTest2

scala> val c = classTest2(1, 2)
val c: classTest2 = classTest2@3f357c9d
```

인자에 기본값을 넣고 싶으면 다음과 같이 기본값을 넣으면서 클래스를 선언하면 된다.

```scala
scala> class classTest3(x: Int=1, y: Int=2)
// defined class classTest3

scala> val d = classTest3()
val d: classTest3 = classTest3@5cb4e255
```

인스턴스를 생성하면서 멤버 변수도 같이 생성하고 싶으면 다음과 같이 인자 앞에 val이나 var를 붙여주면 된다.

```scala
scala> class classTest4(val x: Int=1, val y: Int=2)
// defined class classTest4

scala> val test5 = classTest4(3,4)
val test5: classTest4 = classTest4@5d8edf2

scala> test5.x
val res4: Int = 3
```

기본값이 있으면 다음과 같이 특정 인자만 명시해서 인스턴스를 생성할 수 있다.

```scala
scala> val test6 = classTest4(y=4)
val test6: classTest4 = classTest4@3622c9f0
    
scala> test6.x
val res5: Int = 1
```

인스턴스를 생성할 때마다 어떤 동작을 하고 싶으면 다음과 같이 동작할 함수를 클래스에 넣으면 된다.

```scala
class printClassTest(val x: Int = 1, val y: Int = 2){
  println("Hello, World!")
  println(s"x: $x, y: $y")
}
// defined class printClassTest
           
scala> val test7 = printClassTest()
Hello, World!
x: 1, y: 2
val test7: printClassTest = printClassTest@485902cd
```

위에 잠깐 나왔지만, 클래스를 선언할 때 ```this()```라는 보조 생성자를 사용하여 다양한 상황에서 사용할 수 있는 생성자들을 만들 수 있다.

```scala
class constTest(val x: Int, val y: Int, val name: String) {
  def this(x: Int, y: Int) = {
    this(x, y, "John")
    println("Default name")
  }
  def this() = {
    this(1, 2, "John")
    println("No parameter")
  }
  println("Original")
}

val a = constTest(3, 4, "James")
/*
Original
*/
val b = constTest(1, 2)
/*
Original
Default name
*/
val c = constTest()
/*
Original
No parameter
*/
```

## 마치며

이렇게 class 부터 보니까 머리 속이 정리가 되는 기분이다. 특이 scala3 에서의 변경점은 이번에 정리를 안 했으면 모를 뻔 했다. 근데 콜론을 이용하여 클래스를 만드는 것은 굳이 쓸까 싶긴 한데, 뭐 알아두면 나쁘진 않겠지. 좋은 시간이었다.

## Reference

- [https://docs.scala-lang.org/scala3/reference/other-new-features/creator-applications.html](https://docs.scala-lang.org/scala3/reference/other-new-features/creator-applications.html)   
- [https://docs.scala-lang.org/scala3/book/domain-modeling-tools.html#classes](https://docs.scala-lang.org/scala3/book/domain-modeling-tools.html#classes)   
