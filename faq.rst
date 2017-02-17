
.. _faq:

=============
常见问题(FAQ)
=============

1. TCP连接测试，是否配置了重连。如果不配置重连，是否可用认为实际连接数可能并没有达到测试设置的最大连接数。

EMQ答复：TCP连接测试未设置重连。实际连接数是通过EMQ的stats、listener指标统计，与是否重连没关系。EMQ本次测试并未测试最高的连接数，只是例行测试100万连接的CPU、内存占用情况。1.0版本最高测试到130万连接，ZAKER新闻客户端产品环境下90万。

XMeter答复: 所有连接测试都没有配置重连，连接建立后每5分钟发起一个ping包，通过EMQ的stats、listener指标统计可以看到实际就是有100万连接，与是否重连没关系。

2. 连接测试中的响应时间，是指创建连接到连接成功的时间吗？

EMQ答复：响应时间为TCP连接建立，发送CONNECT报文，接收到CONNACK报文。

3. 吞吐量测试中，没有丢包数量的统计。 请问下，是否有这个结果的统计？

EMQ答复：吞吐测试在青云北京三区主要测试EMQ每秒处理消息数量，没有丢包率的指标。在EMQ处理能力之内，QoS0消息内网一般不会丢包，QoS1/2消息支持回执与重传可以避免丢包。

XMeter答复：我们没有仔细统计这个值，在测试过程中发现这样的现象：1）如果服务器端的工作负载在正常情况下，能将pub的包进行实时转发，没有丢包现象; 2）如果服务器端的工作负载比较重，除了消息转发的时间拉长，基本也没有丢包现象，但是没有做过精确统计。

4. 吞吐量测试中，topic的数量是怎么设计的呢？

EMQ答复：吞吐测试是先创建10万线背景连接和20万Topic。

XMeter：所有的连接测试、以及背景连接，每个连接都会sub一个单独的topic。100万连接就sub了100万个topic，而每个吞吐测试中的10万背景连接都有10万个topic。但是所有的吞吐测试的实际payload的pub和sub间都是用的是一个topic。

5. 吞吐量测试中，测试分别统计的fan-in和fan-out，fan-in测试的时候，没有sub。有没有两个值都同时统计的测试结果呢？

EMQ答复: 共享订阅测试是双向。因为大部分应用场景下PUB消费需要用共享订阅平衡负载。

6. 我们的应用场景中，流量更多是从多publisher到少量的subscriber

EMQ答复：共享订阅或Fastlane订阅，专门处理数据采集类的多PUB少SUB场景。
