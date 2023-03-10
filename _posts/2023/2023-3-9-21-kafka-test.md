---
layout: post
title: "[Scala] Kafka Produce, Consume 해보기"
excerpt: "Scala3에서 Kakfa Produce, Consume을 테스트 해보자."
categories: ['Kafka']
last_modified_at: 2023-03-10
published: True
---

## 들어가며

Scala 강의를 듣고 있는데, 지금 약간 이해가 안 가기도 하고, 뭔가 실용적이지 않은 내용만 하는 것 같아서 지루한 면이 있었다. 역시 실무적인 내용도 같이 해야 한다는 생각이 들어서서 Scala3에서 Kafka Producer, Consumer로 간단히 메시지를 주고 받는 애플리케이션을 작성하였다.   

Scala 3.2.2, sbt 프로젝트로 작성하였다.

## Kafka Producer

먼저 `build.sbt`에 Kafka dependency를 추가하였다.

```
libraryDependencies += "org.apache.kafka" % "kafka-clients" % "3.2.0"
```

그리고 `ProduceTest.scala` 파일을 작성하기 시작하였다. 먼저 produce할 Kafka Broker 노드들의 ip 주소들과 topic 이름을 설정하였다.

```scala
val kafkaIp = List("ip주소1","ip주소2","ip주소3")
val port = 9092
val bootstrapServers = kafkaIp.map(_ + ":" + port).mkString(", ")
val topic = "test01"
```

Kafka cluster 노드들의 ip 주소들을 이어서 하나의 String으로 만드는 코드이다. Kafka에서는 클러스터의 한 노드의 ip 주소에 메시지를 전송해도 클러스터 전체에 메시지가 들어가는 것으로 작동을 한다. 그렇지만 전송할 노드가 작동이 불가능한 경우가 있을 수 있으므로 보통 위와 같이 2~3개 노드의 ip 주소를 이용하여 메시지를 produce 한다고 한다.   

map 메서드를 통해 ip 주소 리스트의 element마다 포트번호를 붙이고, mkString 메서드를 이용하여 각 element 사이에 ", "를 붙여서 String을 생성하고 있다. 위 `bootstrapservers`를 출력한 결과는 다음과 같다.

```scala
println(bootstrapServers)

// ip주소1:9092, ip주소2:9092, ip주소3:9092
```

그 다음 노드들의 ip 주소와 전송할 토픽 이름을 인자로 받고 메시지를 produce하는 `prodTest` 함수를 작성하였다.

```scala
def prodTest(bootServers: String, topic: String) = 
  val properties = Properties()
  properties.put("bootstrap.servers", bootServers)
  properties.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer")
  properties.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer")

  val producer = KafkaProducer[String, String](properties)

  val producerRecord = ProducerRecord[String, String](topic, "key", "value")
  producer.send(producerRecord)

  for (i <- 0 until 10)
    val testRecord = ProducerRecord[String, String](topic, i.toString(), i.toString())
    producer.send(testRecord)

  producer.close()
```

먼저 Producer의 설정을 저장할 `java.util.Properties` 인스턴스를 생성하고 Producer에 필요한 설정들을 저장하였다. scala3 부터는 일반 클래스도 new 없이 인스턴스 생성이 가능하다. 입력받은 ip 주소와 key, value serializer를 저장한다.   

그 다음 생성한 Properties를 이용하여 KafkaProducer를 생성하고 전송할 메시지인 ProducerRecord를 만들어 전송하고 있다. 입력받은 topic 이름과 key, value값으로 메시지를 만들고 `KafkaProducer.send` 메서드를 이용하여 토픽에 메시지를 produce 하고 있다. 아래는 for문을 이용하여 10개의 메시지를 produce 하고 있다.

마지막으로 모든 작업이 끝나면 KafkaProducer를 close 한다.

```scala
@main
def produceTest: Unit = 
  def prodTest(bootServers: String, topic: String) = 
    ...
  prodTest(bootstrapServers, topic)
```

그리고 main 함수에서 prodTest 함수를 실행한다. scala3에서는 `@main` 어노테이션을 이용하면 쉽게 main 함수를 설정할 수 있다. 그리고 scala에서는 함수 안에서도 함수 선언을 할 수 있기 때문에 main 함수 안에 prodTest 함수를 선언하고 사용하는 모습이다.

## Kafka Consumer

이제 Producer에서 보낸 메시지들을 consume하는 Consumer 코드를 작성해보자. 먼저 Producer 때와 같이 Kafka dependency를 추가하고 ip string을 만들고 consume을 수행하는 `consumerTest` 함수를 작성하였다.

```scala
def consumertest(bootServers: String, topics: List[String]) = 
  val properties = Properties()
  properties.put("bootstrap.servers", bootServers)
  properties.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
  properties.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
  properties.put("auto.offset.reset", "latest")
  properties.put("group.id","test01")

  val consumer = KafkaConsumer[String, String](properties)
  consumer.subscribe(topics.asJava)
  while (true) {
    val record = consumer.poll(1000).asScala
    for (data <- record.toList)
      println(s"key: ${data.key()}, value: ${data.value()}")
  }
```

Producer 때와 같이 먼저 설정을 저장할 Properties를 만들고 그 안에 필요한 설정들을 저장하였다. offset reset 옵션은 `latest`로 설정하였다. consumer와 broker가 연결이 끊겼다가 다시 연결됐을 때, 연결이 되지 않았을 때 들어온 데이터들은 무시하고 연결된 이후의 데이터부터 다시 받기 시작하겠다는 뜻이다. `earliest`는 다시 연결되었을 때 토픽 안의 읽지 않은 데이터부터 읽는다는 뜻이다.   

그 다음 설정들을 이용하여 KafkaConsumer를 만들고 입력받은 토픽들을 구독한다. 입력받은 토픽들이 scala collection 형태이므로 java 형식으로 바꾸어 subscribe 메서드에 넣어줘야 한다. `scala.jdk.CollectionConverters`를 import하면 asJava 메서드를 사용할 수 있다.   

그 다음 매번 1초씩을 기다려 토픽에 들어오는 메시지들을 consume한다. 메시지 record collection이 Java 형태이므로 asScala 메서드로 다시 scala 형태로 바꾸어 줘야 한다. 받은 record collection을 list로 바꾸고 각 element들의 key와 value를 출력하는 모습이다.

```scala
@main
def consumeTest: Unit = 
  def consumertest(bootServers: String, topics: List[String]) = 
    ...

  consumertest(bootstrapServers, List("test01"))
```

그리고 main 함수를 이용하여 실행하였다.   

![Consume Result](/de-note/assets/images/21st/consume_result.png)

위 프로젝트들을 실행하면 consumer 애플리케이션에 정상적으로 메시지들의 key와 value들이 출력되는 것을 확인할 수 있다.

## 전체 코드

```scala
// ProduceTest.scala
import org.apache.kafka.clients.producer.KafkaProducer
import java.util.Properties
import org.apache.kafka.clients.producer.ProducerRecord

val kafkaIp = List("ip주소1","ip주소2","ip주소3")
val port = 9092
val bootstrapServers = kafkaIp.map(_ + ":" + port).mkString(", ")
val topic = "test01"
@main
def produceTest: Unit = 
  def prodTest(bootServers: String, topic: String) = 
    val properties = Properties()
    properties.put("bootstrap.servers", bootServers)
    properties.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer")
    properties.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer")
    
    val producer = KafkaProducer[String, String](properties)

    val producerRecord = ProducerRecord[String, String](topic, "key", "value")
    producer.send(producerRecord)

    for (i <- 0 until 10)
      val testRecord = ProducerRecord[String, String](topic, i.toString(), i.toString())
      producer.send(testRecord)
    
    producer.close()
  prodTest(bootstrapServers, topic)
```
```scala
// ConsumeTest.scala
import java.util.Properties
import org.apache.kafka.clients.consumer.Consumer
import org.apache.kafka.clients.consumer.KafkaConsumer
import scala.jdk.CollectionConverters._

val kafkaIp = List("ip주소1","ip주소2","ip주소3")
val port = 9092
val bootstrapServers = kafkaIp.map(s => s+":"+port).mkString(", ")

@main
def consumeTest: Unit = 
  def consumertest(bootServers: String, topics: List[String]) = 
    val properties = Properties()
    properties.put("bootstrap.servers", bootServers)
    properties.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
    properties.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
    properties.put("auto.offset.reset", "latest")
    properties.put("group.id","test01")

    val consumer = KafkaConsumer[String, String](properties)
    consumer.subscribe(topics.asJava)
    while (true) {
      val record = consumer.poll(1000).asScala
      for (data <- record.toList)
        println(s"key: ${data.key()}, value: ${data.value()}")
    }

  consumertest(bootstrapServers, List("test01"))
```

## 마치며

오랜만에 뭔가 결과가 나오는 스칼라 코드를 작성하니 더 재밌었고, 앞으로 스칼라를 이용하여 더 많은 Kafka 애플리케이션을 만들어 보고 싶다.
