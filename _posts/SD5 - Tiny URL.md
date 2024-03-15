# Clarification

user给我长url，我返回给user一个短url。

<img width="871" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/56be8ebf-45c3-444e-ae23-cf3112c7c79b">

<img width="901" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/12fdfb78-0c75-48b8-84eb-78a4491df5b2">

其实301或者302都行，但是301 server的work load比较低；302能得到更多别的信息（比如谁用了我的短连接）。

<img width="853" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/3bf34acc-39b2-49f8-b00a-82117f1628d7">

# Data Size

<img width="872" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/36b292e2-419c-4557-ace1-77a5832cfd79">

是read heavy，因为我可能generate 1次，但是无数人去点去使用。

一秒要写200个URL（写：长变短）
一秒要读2W个URL（读：点击）

<img width="872" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/ed00671c-aa92-4f81-b1f0-25da9cf27e0f">



# API Interface

# Database

# Draw

# Corner cases, Load Balance, Cache

# Bottlenecks
