<img width="685" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/f3f02cf6-3bff-49e7-87ae-cef5dad7c8cd">

1. 什么要求？
2. size大不大
3. 要有什么API interface
4. SQL还是NoSQL
5. 画画，画data flow
6. 用户的corner case等：过年时候信息很多，我们会cut掉其他这不重要的功能
7. 如果只有一个数据库，如果这个down了，我们还有什么backup。。。如果数据丢失，我们还有哪里的备份。。。

<img width="702" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/39580c4c-8be1-4352-aef3-6a77ecf40cc1">

# 1. 要有什么feature，是要自己提出来的，不要等到面试官和你说：

<img width="753" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/7caaf45e-300e-4b16-9ac8-3b66fa737053">

# 2. 问问多少用户，多少数据？

<img width="745" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/4fc813b7-58f9-41d9-8d2a-792ed5fb8079">

<img width="756" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/8805fd0e-e545-4dce-ae85-542ea69af621">

# 3. 4. API和datatbase的设计

<img width="657" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/4b8f17e6-e588-40af-b310-5058a5ad9db5">

# 5. 画图

<img width="754" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/73f94990-f1a5-4bac-8187-7fac09e360ac">

## 5.1 One To One Chat

gateway：是用来连接2个网络的（这2个网络的协议不同），左边外网右边内网。

session service：告诉你哪个用户在哪个gateway。比如这里：session service连着数据库，在数据库里能看到：用户A连着gateway1，用户B连着gateway2。----> 用户A想要发信息给用户B，A就通过HTTP协议通过gateway1发出请求，请求去session service一看，发现B在gateway2，于是把A的信息传到了gateway2，然后gateway2再想一种方法传到B（HTTP必须是从client发起，所以不能再用HTTP了。）。
所以那用什么协议呢？有2种选择：
a) HTTP-long pulling: 用户B每隔5秒去和service敲门：有没有我的信息？但这个在聊天系统不适用，因为我想要即时通信。
b) WSS - websocket on SSL/TSL for security：是一个类似于HTTP，但是更upgrade的东西，支持双工。HTTP是只能是client发起，所以是单向half的。但websocket不仅客户可以给server发，server也可以给client发，相当于开辟了client和server的双向通道，是full的。这些都是基于TCP的（TCP是guarantee delivered，UDP是not guarantee）。

### 补充资料：
<img width="676" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/08f1d851-efa2-460e-a1bc-2230e67b60a4">

<img width="740" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/ca447679-07f5-4067-b6ca-9d0f7fd53aa5">

WebSocket 的双工通信是指在单个连接上，客户端和服务器可以同时进行数据的发送和接收。这与传统的HTTP请求不同，HTTP请求通常是单向的，即客户端发送请求后服务器响应，完成一次交互。

在双工通信中，WebSocket保持连接开放，使得数据可以在任何时刻从客户端发送到服务器，或者从服务器发送到客户端。这种方式非常适合需要实时数据交换的应用，比如在线游戏、聊天应用或实时数据监控系统。通过WebSocket实现的双工通信能够大大减少延迟，因为不需要为每个消息建立新的连接。

<img width="562" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/3cdc43c5-b5f5-4f9b-b1bb-aef8f71b6261">

## 5.2 信息的sent/delivered/read

session service其实还连接着另一个数据库（储存比如:A发给B“hi”，B发给A”hello“）。当用户A想给B发信息，把信息"hi"传到了session service，就会记录到这个数据库，如果记录成功了，就会对A说“我知道了，会发的，你回去吧”，这时候会显示sent。当这个数据库继续发给B，给到B了之后，session service就会知道，并且显示"delivered"，表示“已经送到啦，但是主人B还没有打开看”。当B打开这个app，当他看到消息，从B端就会给服务器发消息说“主人看到啦”，就会显示read。

## 5.3 User Last Seen

在之前session service的基础上，再加一个service叫：last seen。如果用户A有一个请求，传达到last seen这个service上就是表示“online”，20s不做其他动作，service就会显示“last seen 20s ago”。

但是这里有一个trick，就是这个系统中，常常有2个活动，user activity或者app request（比如app想确认登陆成功，app想拿权限等等），我们要区别这两个（只有真正的user activity才能让last seen service显示“online”。所以在session service和last seen service之间还有一个boolean值flag，如果flag == True就是真正的user activity，这种情况下我们才认为A是online的状态。


