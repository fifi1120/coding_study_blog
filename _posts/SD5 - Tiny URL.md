# Clarification

user给我长url，我返回给user一个短url。

<img width="871" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/56be8ebf-45c3-444e-ae23-cf3112c7c79b">

<img width="901" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/12fdfb78-0c75-48b8-84eb-78a4491df5b2">

其实301或者302都行，但是301 server的work load比较低；302能得到更多别的信息（比如谁用了我的短连接）。

<img width="853" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/3bf34acc-39b2-49f8-b00a-82117f1628d7">

# Data Size 5min

<img width="872" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/36b292e2-419c-4557-ace1-77a5832cfd79">

是read heavy，因为我可能generate 1次，但是无数人去点去使用。

一秒要写200个URL（写：长变短）
一秒要读2W个URL（读：点击）

<img width="872" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/ed00671c-aa92-4f81-b1f0-25da9cf27e0f">

不管什么design，都要用cache的。

# API Interface 5min

<img width="758" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/bc68b8fa-5292-4c42-8766-b52c9e8cea50">

# Database

<img width="892" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/f7e60580-4a5d-4d55-a4d3-440a7cb1061b">

object之间没有什么相互的关系，只是一些key value pair，是noSQL的经典应用。这样的noSQL比SQL有超过10倍的吞吐量，scale up比较容易。

# Draw

<img width="863" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/a7a40b5e-de17-44b0-8433-bfdff182b8fa">

1. rehashing：generate一个hash之后，如果已经在DB中存在，那么append一个string，然后rehash again，（再看在不在数据库里）直到conflict被解决；

3. key generation service.

<img width="864" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/d4caf033-6465-465d-a65a-58b9fba1c6e8">

<img width="868" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/a8753955-207c-49b2-b591-9782e45b68bc">

2. 用base 62把长数字串转化成unique ID。

<img width="825" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/f3af19ee-ad6e-456b-8876-6de830b9f75d">

两种hash的比较：

<img width="823" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/b51132dc-fa96-405a-b513-c92fb17eea6a">



<img width="682" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/6d38c699-bf64-4a4c-9ec7-ee8382804c76">

# Corner cases, Load Balance, Cache
一些Follow Ups：

<img width="863" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/599a9e80-0d93-4638-ad13-18b02fa547f1">

<img width="867" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/be87cedc-4bdb-444b-85b6-4b67dbb00512">

<img width="883" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/51660cb6-1503-4ada-a4ee-12dc055b0d9f">

<img width="828" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/3e1de443-d4c6-4763-8400-9cc2145a4d9d">

<img width="891" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/e4eda905-ff3d-42df-a6a8-4adcfa462739">

# Bottlenecks
