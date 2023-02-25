---
layout: post
title: "[Scala] Water Pouring 문제"
excerpt: "Scala로 Water Pouring 퍼즐을 풀어보자."
categories: ['Scala']
last_modified_at: 2023-02-25
published: False
---

## 들어가며

이 내용은 [Coursera Scala 강의](https://www.coursera.org/learn/scala2-functional-program-design)의 2주차 5번째 강의에 있는 내용이다. 강의에서 Water Pouring Puzzle 알고리즘을 풀어보면서 그 동안 배웠던 내용을 정리하고 있는데, 나도 그에 대한 내용을 포스트로 정리해보면 좋을 것이라는 생각이 들어서 작성하게 되었다. [지난 번](https://sparkafka.github.io/de-note/18-scala-class)에는 class에 대한 내용을 작성하였는데, 이번에는 나머지 내용에 대해 정리를 해볼 것이다. 코드를 한 줄씩 분석해보며 관련 내용을 정리해보자. 나도 잘 모르기 때문에 자세한 내용은 강의를 직접 보시길 바란다.

## Class

```scala
class Pouring(capacity: Vector[Int]) {
  // States
  type State = Vector[Int]
  val initialState = capacity map (x => 0)
  ...
}
```

강의의 코드는 위와 같이 ```Vector[Int]``` 타입의 ```capacity``` 인자를 받으면서 Pouring 클래스를 선언하는 모습이다. 여기서 ```capacity```는 사용할 물통들의 용량을 뜻한다. 클래스에 대한 내용은 [이전 포스트](https://sparkafka.github.io/de-note/18-scala-class)를 참조 해주길 바란다.   

다음 줄에서는 ```type``` 키워드를 통해 클래스 안에서 쓰일 타입을 선언하고 있다. ```Vector[Int]```를 ```State``` 타입으로 선언하고 있다. 이런 식으로 타입을 선언해 두면 어떤 때 이 타입을 사용하는지 명확하게 표현할 수 있다.   

다음 줄에서는 ```initialState```를 초기화 하고 있다. ```map``` 메서드를 통해 ```capacity```의 크기만큼의 0으로 된 ```Vector[Int]```를 만들고 있다.   
이후에 나오는 코드들은 모드 ```Pouring``` 클래스 안에 들어있는 내용이다.

## Trait과 Case Class

```scala
// Moves
trait Move {
  def change(state: State): State
}
case class Empty(glass: Int) extends Move {
  def change(state: State): State = state updated (glass, 0)
}
case class Fill(glass: Int) extends Move {
  def change(state: State): State = state updated (glass, capacity(glass))
}
case class Pour(from: Int, to: Int) extends Move {
  def change(state: State): State = {
    val amount = state(from) min (capacity(to) - state(to))
    state updated (from, state(from) - amount) updated (to, state(to) + amount)
  }
}
```

다음에는 trait과 case class를 통해 Water Pouring Puzzle에서 필요한 동작들을 선언하고 있다. 위 예제와 같이 trait 혹은 abstract class와 case class의 조합을 통해 계층구조를 쉽게 표현할 수 있다.   

trait은 자바의 interface와 비슷하지만, 하나의 클래스는 하나의 interface만 상속받을 수 있는 것과 달리 스칼라에서는 하나의 클래스가 두 개 이상의 trait을 상속받을 수 있다.

```scala
scala> trait trait1
// defined trait trait1

scala> trait trait2
// defined trait trait2

scala> class class1 extends trait1, trait2
// defined class class1
```

[공식 문서](https://docs.scala-lang.org/ko/tour/case-classes.html)에 따르면, case class는 다음과 같은 특징을 가지는 일반 클래스라고 한다.

- 기본적으로 불변
- 패턴 매칭을 통해 분해가능
- 레퍼런스가 아닌 구조적인 동등성으로 비교됨
- 초기화와 운영이 간결함

위에서 말했다시피, case class와 trait을 쓰면 계층 클래스 구조를 쉽게 표현할 수 있다. 다음은 trait과 case class를 통해 fruit 종류를 표현하는 예제이다.

```scala
scala> trait fruit
// defined trait fruit
scala> case class apple() extends fruit
// defined case class apple
scala> case class strawberry(name: String) extends fruit
// defined case class strawberry
scala> case class kiwi(name: String, grade: Int) extends fruit
// defined case class kiwi
```

case class에서는 일반적인 class와는 달리 클래스를 선언할 때 꼭 인자 목록을 넣어줘야 한다(비어 있어도 가능).

```scala
scala> case class pear extends fruit
-- [E004] Syntax Error: --------------------------------------------------------
1 |case class pear extends fruit
  |           ^^^^
  |           A case class must have at least one parameter list
  |
  | longer explanation available when compiling with `-explain`
1 error found

scala> case class pear() extends fruit
// defined case class pear
```

원래 예제에서, ```state.updated(a,b)```라는 메서드를 사용하고 있다. state는 이전에 선언했던 State 타입, 즉 ```Vector[Int]``` 타입의 객체이다. updated 메서드는 ```Vector``` 클래스의 내장 함수로, a번째 요소를 b로 바꾸는 메서드이다.

```scala
scala> val vector1: Vector[Int]  = Vector[Int](1,2,3,4,5)
val vector1: Vector[Int] = Vector(1, 2, 3, 4, 5)

scala> vector1.updated(2, 0)
val res0: Vector[Int] = Vector(1, 2, 0, 4, 5)
```

원래 스칼라에서, case class를 선언할 때 자동적으로 apply 메서드를 생성하였고, 그래서 case class의 인스턴스를 선언할 때 new를 붙이지 않아도 됐는데 scala3 에서는 이것을 확장하여 일반적인 클래스에도 적용이 되었다.   

```scala
// case class의 인스턴스를 생성할 시에는 new를 붙이지 않아도 된다.
scala> case class cClassTest1(val a:Int)
// defined case class cClassTest1

scala> val test1 = cClassTest1(2)
val test1: cClassTest1 = cClassTest1(2)
```

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
