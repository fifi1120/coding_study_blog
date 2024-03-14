### 1. Clarify
<img width="1124" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/ac58c80b-0371-44c3-a47c-8e4e99e204c1">

<img width="931" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/abd28cea-d07e-471b-bc86-98559b5840a9">

### 2. Data Size （5min）
<img width="885" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/122be3c3-bfe0-4294-b38f-9e83b2566158">

<img width="856" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/a8871242-c7b1-449c-b35e-339aeb73537c">

### 3. API interface （5min）

<img width="786" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/358a92a5-a2b2-4390-8054-e4434f287e42">

offset是上次看到哪里，看了多少（time stamp要一样），return median stream（不要从头看）。

codec想看哪个分辨率的。

### 4. database

用SQL + noSQL + Amazon S3

#### SQL table

用来存user info，billing info，很重要，因为要ACID。要compliance，同步很重要：sybchronous repliancation protocol。

<img width="738" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/9455a5c1-c952-4b18-9911-2189135994c3">

#### noSQL：

用Cassandra保存历史记录，历史记录不断增长，Cassandra有可扩展性、高性能。优化：对于久远的历史记录就compress。

<img width="890" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/0f30e5b2-ecb4-4525-8c2f-40b4c8c3031a">

### 画图

ISP: internet service provider

或者把ISP换成API GATEWAY。

<img width="905" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/adced5a2-3430-4385-a185-7534e8d7244a">

很多人看视频，就是一个read heavy system。

<img width="841" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/f7bb3c31-8df7-4a04-a27f-e01da8ee9af4">

这张图就是async。这里有trade off的。

<img width="904" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/39a9079f-770a-4c56-b178-47486b97331f">

<img width="912" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/71a4887d-f5b2-44d7-906e-587e9dca7e62">

<img width="802" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/a791a4ee-c90a-47c8-916f-504fec2d6319">

preprocessor会做分割，对于high light就提高优先级。切成trunk，每段10s以内，一般4-7s。

<img width="924" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/af61aceb-5361-4c9d-9124-ab02c0a12dda">

task scheduler是用来协调的。

<img width="916" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/b3acd726-2936-47d4-a2ed-6ebee3bb31cb">

netflix想把100%的东西都放CDN。但是CDN很贵，所以你可以CDN只放热门视频。

<img width="920" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/77034539-f447-4066-83b0-af4d429a9bb6">

CDN，content delivering network，用来加速浏览速度，像是个local cache。

<img width="872" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/93c3d0cf-c410-494a-9549-1b55034edd74">

<img width="872" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/779eae54-272b-45a4-9924-7ea95e0bd913">

<img width="878" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/add1c375-5230-4a7e-922d-1b78aae72424">

<img width="476" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/72ac0eaa-764f-42c3-a17a-d0212e9a5329">

视频搜索还要了解：Elastic search （AWS有现成的library）




