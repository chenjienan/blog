---
title: System Design 系统设计小结
date: 2019-03-06 16:39:05
tags: 
category: design
---

### Introduction
上个月参加了一场面试，其中问了一条system design的题目。面试前咨询了IT界资深大牛，他语重心长地建议我好好准备算法，系统设计可以胡扯一下。面试后深深觉得这种场景不能适用在我自己身上。

最近一直在思考一个问题：在下工作几年，又好好准备了2个月，为何还是有点无所适从的感觉？ 题目不算难，但确实缺少面试经验。

<!--more-->
#### 思考人生的结论：

1. 题目没有标准答案，解释一个架构时，面试官会有主观好坏的判断
2. 两个月的突击，知识只停留在背诵了解的层面上，还没有变成自己的东西。题目或需求改变就GG
3. 面试没有大局观，对某个部分的理解不够深入，心里没数
4. 设计经验不足，被面试官挤牙膏，十分影响结果
5. 有些设计（large scale）在工作当中不一定用得上，特别是小公司

系统设计是一个长期经验积累的一个过程，面试加入这样一轮，主要考验candidate知识面的深度与广度，而不是仅仅的刷题达人。当然能成为刷题达人也是在下的一个目标。


### Structure 
综合众多培训机构和各种讲座的总结，在下得出如下结构：

#### Scenario 
- 用户量
- 峰值

依照以上两个数值推算出：

1. QPS
2. 存储size (为后面的storage做铺垫)
3. 如果数据有关联，需要罗列出schema (数据之间的关系)
4. 必要的算法，或数据结构 (e.g. typeahead - trie, rate limiter - window)

按照大众的评论，大概3-4个需求足以

#### Implementation (主要环节)
- 标注，添加必要service （组件）
- 重点：了解组件的功能，为什么需要这个组件
- 重点：画图搭架 - 如何安排各个组件之间的联系(protocols, communications)
- 考虑数据贮存
- 考虑是否有model
- 检查是否架构合理性(经验之谈)
  
目标是解决Scenario 所罗列的需求。多看多积累是重中之重。建议把下面介绍的资料都了解一下。这里就不多说了。这次主要死在这个环节，没有把主要的架构搭建好，有些service的连接不make sense，导致后面准备得十分的scalability一点没有，显得整个面试苍白无力。

#### Enhancement 
主要解决三大问题： 
- maintainability
- flexibility
- scalability

简单来说就是做优化，如果上一个环节没有做好，这个环节就没有很多意义。如果考背诵，无非就是service scale out多加几个nodes,再来个load balancer。NoSQL vs SQL, Database来个主从备份或者读写分离，加速就装Cache，再上consistent hashing或CP/AP model等等等等。

### 个人总结几大类系统：
- __location-sensitive__：Yelp，Uber matching, find nearby friend
- __storage-based__: Pastein, Dropbox
- __social network (timeline)__: Facebook news feed, Twitter
- __search-based__: typeahead, twitter search
- __notification/message queue__: Web crawler, Mint.com
- __service-based__: TicketMaster, YouTube/Netflex
- __chat-based__: Facebook messenger, WhatsApp, Tinder
- __functional__: TinyURL, Rate Limiter

熟悉这几大类型，对搭架子会有很大帮助。大多系统架构都有相似的地方，因地制宜。


### 这里补充基础知识：
- [system design premier](https://github.com/donnemartin/system-design-primer)
- [YouTube印度小哥系列](https://www.youtube.com/playlist?list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX)
- [YouTube黑哥系列](https://www.youtube.com/playlist?list=PLkQkbY7JNJuBoTemzQfjym0sqbOHt5fnV)
- [The Architecture of Open Source Applications](http://aosabook.org/en/index.html)
- [Grokking the System Design Interview](https://www.educative.io/collection/5668639101419520/5649050225344512)

### 总结
在日后多看别人家的架构以及云端的各种服务和功能，多考虑不同的场景，不同的处理方式。(有点象在讲废话)

有更多好的资源或者建议和意见，欢迎和我联系！