---
layout: post
title: Spring @Transactional事务管理
---

### 事务是什么？

### Spring的事务管理

**Spring的事务管理由注解@Transactional实现**

既可以在类上添加@Transactional，也可以在方法上添加@Transactional。在类前加上@Transactional，声明这个类的所有方法都需要事务管理，每一个业务方法开始时都会打开一个事务。在方法前添加@Transactional，表示这一个方法需要进行事务管理。
 
Spring默认情况下会对运行期异常(RunTimeException)进行事务回滚，Unchecked Exception异常除外。

如何改变默认规则： 

1 让checked例外也回滚：在整个方法前加上 @Transactional(rollbackFor=Exception.class) 

2 让unchecked例外不回滚： @Transactional(notRollbackFor=RunTimeException.class) 

3 不需要事务管理的(只查询的)方法：@Transactional(propagation=Propagation.NOT_SUPPORTED) 

在整个方法运行前就不会开启事务 


单独使用 @Transactional 注释时，事务传播模式被设置成什么呢？只读标志被设置成什么呢？事务隔离级别的设置是怎样的？更重要的是，事务应何时回滚工作？理解如何使用这个注释对于确保在应用程序中获得合适的事务支持级别非常重要。回答我刚才提出的问题：在单独使用不带任何参数的 @Transactional 注释时，传播模式要设置为 REQUIRED，只读标志设置为 false，事务隔离级别设置为 READ_COMMITTED，而且事务不会针对受控异常（checked exception）回滚。 

### @Transactional的事务传递
id	|	amount
----------------| ---------------
1 |   1000



## 问题是什么

假设有一张表T_ACCOUNT，它的字段如下：

id	|	amount
----------------| ---------------
1 |   1000


这里有两个线程：A和B。A需要给id=1的用户增加300元，而B需要给id=1的记录减掉100元。 可能的方法如下：

	void updateWithId(int id, int deltaAmount)

如果A先执行，B后执行，最后的amount的值为1200。如果B先执行，A后执行，最后的结果也是1200.最担心的情况的是，A和B同时执行。A、B两个线程同时读取到当前的值（1000），每个线程会将这个数据保存在自身的栈里。然后，A、B会竞争去做update操作，数据库引擎会保证同一时刻只有一个线程在update。如果A先做了update操作，DB中的amount会变成1300。这时B再做update，由于B已经将amount的值保留在栈内了（值为之前取到的1000），update之后，amount会变为900. 这明显不符合逻辑，多并发引起了数据的不一致。

## 如何解决

### 方式1（利用versionId）

在设计表的时候，多预留一个字段为version_id。它是一个普通的int类型。

id	|	amount  |  version_id
------- | -------- | -------
1 |   1000  | 1

在定义方法的时候，要增加一个versionId进去。

	void updateWithIdAndVersionId(int id, int versionId,int deltaAmount)

这样，当A、B同时执行。A和B先查询记录，发现id=1，versionId=1，并将结果保存在自己的栈内。当执行update操作时，总会有一个先执行。假设A先执行，A在调用updateWithIdAndVersionId方法时，程序需要**将versionId加1**，这样一来，DB中id=1的记录的version_id就是2了。当B再执行updateWithIdAndVersionId操作，它根据2个条件（id=1、versionId=1）去更新。因为此时DB中已不存在这条记录了，所以更新失败。通过这种方法就可以保证多并发对数据操作的一致性。















