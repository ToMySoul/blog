---
title: MondoDB
categories: 数据库
tags: 
- 数据库
- MondoDB
- 部署
- 非关系型数据库
---

## 下载
https://www.mongodb.com/try/download/community

## 配置
 
mongod.exe --dbpath D:\software\mongodb\mongodb6.0.6\data --logpath D:\software\mongodb\mongodb6.0.6\logs\mongodb.log

创建服务
注册MongoDB为系统服务（此步骤必须以系统管理员身份运行cmd，否则会报错）以系统管理员身份运行cmd输入并切换至MongoDB的bin目录运行以下语句
mongod.exe --dbpath=D:\software\mongodb\mongodb6.0.6\data --logpath=D:\software\mongodb\mongodb6.0.6\logs\mongodb.log --install --serviceName "MongoDB"
回车如果控制台出现类似Tue Oct 09 12:05:15 Service can be started from the command line with 'net start MongoDB'这样的语句，说明服务已经注册成功。

在cmd中输入net start MongoDB
即可启动MongoDB数据库服务，此时控制台输出Mongo DB 服务已经启动成功，说明系统启动成功。
如果出现发生系统错误 1067 请把db目录下的mongod.lock文件删除后重新输入net start MongoDB启动服务即可。

## 使用


```


//新增集合并插入第一条数据
db.collection.insert({
title:'支付',
payType:'AliPay',
total: 23,
time: Date(),
appId: 332541321

})
//查询 appId=332541321 的一行
db.collection.findOne({appId:332541321})
//查询 支付方式为AliPay
db.collection.find({payType:"AliPay"})
//查询 支付方式为AliPay 显示列total,payType
db.collection.find({payType:"AliPay"},{total:1,payType:2})
 //$lt $ge $lte $gte 分别表示：<、>、<=、>=
//查询支付金额大于等于23 
db.collection.find({total:{$gte:23}})
// 更新id 为64631ee0cd2f000061001a4c total 值
db.collection.update( { _id:ObjectId("64631ee0cd2f000061001a4c") }, { $set: { total: 28 } } )









```

