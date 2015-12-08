---
layout: post
coments: true
title: Crunching Apache Camel Netty Component
tags:
- camel
- jmh
- java
- netty
- non-blocking
---
Recently I've run some prformance test and couldn't understand why tool, I'd written gives me so low load. The system under test uses TCP transport based on netty. My tool also uses netty wrapped by camel component.

This is pretty simple [Camel](http://camel.apache.org/) route, and I decided to look into the guts of what is going on there on producer side. So I took [jmh]( http://openjdk.java.net/projects/code-tools/jmh/) tool and wrote small benchmark to measure performance


{% gist 8ed9362d8ceda972f802 %}

The key point here is **sync** parameter on consumer and producer side. **false** means that we don't want to wait for response from server. It's pretty straightforward, I assumed that Camel behaves like "Fire and Forget", send a message and release the thread. I wanted to check if it's true. I started the test with special JVM arguments to run JMC Flight Recorder against it. And here is what "latency" tab revealed. Looks like Camel does some thread locking even with **sync=false** 

![Stack](/assets/stack.png)

Further, code analisys showed me that Camel ensures that message has been successfully sent by wire and then unleashed the thread for further processing.

Here is how it's done

{% gist 0e469a07335db10af3c1 %}

The essential part is notifying callback that Netty has completed the body write. Netty is non-blocking by its nature that's why Camel mimics synchronous write here:

{% gist 1434b155217ba5427850 %}

The essential part in this snippet is a callback that passed into the processor delegate. This is the same callback which is notified by NettyProducer above when Netty write completed. 

The reason why it's done this way is that camel supports **transacted** behaviour which may be executed only in **syncronus** way, because transaction context is palced in thread-local. Doing like that ensures that global transaction will be rolled back if message hasn't been delivered by Netty. 

Here I would like to have freedom of configuration. Let's say I don't care about success or failure, maybe I build reactive application, and assume that I'll receive some aknowledgemet from downstream system, say message has been successfully received. Offcourse this acknowledgement will be handled by another thread and using distributed transaction will not be possible here. 

Now the most interesting part of the experiment is a benchmark and patch.

Here is a result of original benchmark, when Camel synchronises Netty write with originated route flow:

Benchmark with original code

|  Benchmark                                         |  Mode   |  Cnt   |     Score   |     Error   |  Units   |
| -------------------------------------------------- | :------: | ------ | ----------- | ----------- | -------- |
|  FixEngineMicroBenchmark.measure_netty_throughput  |  thrpt   |  200   |  30596.430  |  ± 368.647  |  ops/s   |


Let's patch NettyProducer class in camel codebase and run bechmark again.

Benchmark with 'fire and forget'  changes

|Benchmark    |                                      Mode |  Cnt  |    Score  |    Error |  Units |
| --- | --- | --- | --- | --- | --- |
| FixEngineMicroBenchmark.measure_netty_throughput | thrpt |  200 |  68066.654 |  ± 1471.666 |  ops/s |

I gained 2X performance having 'fire and forget' logic here. Of course this approach will not allow use global transactions here, but i's not 100% needed, sometimes you have just put message on the wire and release thread for next unit of work.

