# 停车场 

Link：https://www.youtube.com/watch?v=odS_mmxDASc

<img width="615" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/c2fc5f76-45bb-4020-896a-e85ea84bfcbc">

别忘记最外层的：我会有本身停车场系统这样一个类ParkingLot。

时间：是变量，是class里面的一个privite

地点：停车场

人物：vehicles --（enums, protected, final, abstract)

起因：车子进去停车场（method）

经过：车在停车场怎么停（method）-- （必用class, public static final）

结果：车子出去得到ticket（method）-- （exception, design pattern)

别的topic：OOD的钱相关：人来->干活->结账

### 你把自己想象成无情的机器，我现在就是一个停车场（静态角度），我关注的是：这辆车来了，我要怎么办？

# 加分技巧：

1. 用enums
2. 用abstract/interface （把车变成abstract）
3. final/protected/public static final num
4. exception
5. design pattern（不是必须的）

<img width="590" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/280dc500-e2b1-40b9-bb14-68e3214a8a61">

# 编码

## 一、枚举类型 VehicleType 大中小

<img width="133" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/f3be6600-1003-4919-a435-4f6f288c6157">

## 二、车（抽象类abstract Vehicle & 具体类 car extends vehicle）

<img width="158" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/cc9bbdb7-1742-426a-b515-aec22b367c50">

精修：变成final（一旦设定不可改），父类和子类都要改

<img width="200" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/39a3b5f5-7df5-4528-9b89-8cad7cc0efb3">

子类：

<img width="178" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/7d99a794-f414-4973-bbd4-362ee0643e6b">

<img width="178" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/b2b17a78-cd2f-4d01-825e-1bd45e412c6f">

<img width="196" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/1a05b10e-07ba-4f03-b385-686985510903">

精修2：把父类的private name变成protected

这样子类就可以直接用name了（如果是private的话，我在子类不能直接调用）

改后：

<img width="194" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/6ff9bab7-dc99-4191-875d-48c0e61215c6">

<img width="194" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/dfc96038-b57e-4360-9804-b52a29673456">

## 三、停车场

<img width="152" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/78334b12-7777-4dfa-8b95-23f03668a647">

【顺序很重要：从小到大。】

顺序：停车位 -> level -> 停车场

3.1 先写停车位ParkingSpot：(这里也可以继承，但是不建议真实这样去操作，你可以和面试官提一嘴，说这里也可以继承）

<img width="261" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/a1e54ca3-d872-44db-8f3d-f304ec832e51">

3.2 再写level（一层有哪些ParkingSlot）：

用list表示一层有哪些停车位

<img width="239" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/247a804b-b216-4292-b9d2-78432f56a787">

3.3 自己加一个class Constants：

<img width="226" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/5204f3d1-96bb-43de-84e0-be54d111ba6d">

3.4 停车场

<img width="158" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/eda8f2ec-4ff2-4ff3-a7a2-cb81fa272ca7">

## 四、收费

<img width="267" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/876f05db-7842-4b3b-9794-7b7ed377289e">


# method相关一般只写input和output是什么就好了


# 建议把enter这种method写在ParkingLot里面，形成一个API。。。（？？？）


对着最小->最大的颗粒，依次检查写下5个method：


<img width="542" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/b131f820-092d-4e18-a37c-8eb1f39df334">

<img width="353" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/66b881af-5fa2-477c-ba5c-f0f447a94a44">


<img width="248" alt="image" src="https://github.com/fifi1120/coding_study_blog/assets/98888516/3211bb63-3be4-42fa-8521-91398fe5a6eb">



