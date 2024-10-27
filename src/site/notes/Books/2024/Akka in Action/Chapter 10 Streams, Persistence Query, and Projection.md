---
{"dg-publish":true,"permalink":"/books/2024/akka-in-action/chapter-10-streams-persistence-query-and-projection/","tags":["scala","akka"]}
---

> A stream is a sequence of data. It can be infinite, but it doesn't have to be.

## Akka Stream library
```
val AkkaVersion = "2.6.20"
libraryDependencies += "com.typesafe.akka" %% "akka-stream" % Akk
```
## Potential Streaming issues
> The consumer speed cannot keep up with the producer speed without flow control

1. A Consumer will crash after taking each new item and keep them in the memory - OOM
2. There is a upper limit set for a consumer so messages will be dropped after the limit was reached. A resend is needed for every dropped message. A list of messages to be resent is growing infinitely.

## Akka Stream - Reactive Streams
> Enable "live" streaming through asynchronous communication and backpressure.

- Asynchronous communication is non-blocking. A producer doesn't have to synchronize with the consumer(s)
- Sending/Receiving messages happens indenpdently.
- Backpressure - the flow is controlled by the consumer, not the producer. A consumer (downstream) has to signal a producer a a receiving confirmation; then the producer can continue sending the next message

![Pasted image 20240303105452.png](/img/user/Books/2024/Akka%20in%20Action/assets/Pasted%20image%2020240303105452.png)

## Basic semantics
> The basic abstractions of Akka Streams are three: **Source**, **Sink** and **Flow**
> **Source** - Producer
> **Flow** - Transformation
> **Sink** - Consumer

![Pasted image 20240303105610.png](/img/user/Books/2024/Akka%20in%20Action/assets/Pasted%20image%2020240303105610.png)

> [!note]+
The combination of the three stages is called a **blueprint** or **graph**
### Source
The *Source* is lazy, that is, it delays its evaluation until its values are needed
```scala
val producer: Source[Int, NotUsed] = Source(List(1, 2, 3))  #A finite stream source
```
### Flow
The transformation function - also called processor - is just the filtering part of the previous stream
```scala
val processor: Flow[Int, Int, NotUsed] = Flow[Int].filter(_ % 2 ==
```
### Sink
The consumer
```scala
val consumer: Sink[Int, Future[Done]] = Sink.foreach(i => storeDB(i))
```
### Blueprint / Graph
The *blueprint* is the product of mix and match the different stages. An abstraction that gives you composability and testability.
```scala
val blueprint: RunnableGraph[scala.concurrent.Future[akka.Done]] = producer.via(processor).toMat(consumer)(Keep.right)

...

val future: Future[Done] = blueprint.run
```

## Materialization
> Materialization is the execution of **blueprint** and its by-products is called the **materialized values**
> 
> With Akka streams, you have one graph and each time you run it, you get its instance - A tuple of materialized values

## Akka Persistence Query
> Akka provides you with an interface to read the journal using the **Akka persistence module**
> The journal contains the events of the persistent entity that represent a state change, the is, the event sourcing part of the entity.

- The reading of the journal is done in a stream fashion.
- To retrieve events by tag, you must pass the **tag** and **offset**. Events have a `sequence_number` when they are stored in the journal; the **offset** is the corresponding number that marks what is the latest `sequence_number` read.
- If reading from the beginning, there is a special object to pass - `Offset.noOffset`




