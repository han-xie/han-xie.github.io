---
layout: post
title: 软件架构设计案例分析与最佳实践
tags: [note, programming]
---

主讲老师：罗贤虎

## 业务系统枚举值处理

建一张字典表

## 用例图

- 表示什么角色做什么事
- UML图有9种，常用的是UML类图
- 领域模型--概念类

UML中的六大关系: [https://www.cnblogs.com/hoojo/p/uml_design.html](https://www.cnblogs.com/hoojo/p/uml_design.html)

- 接口类中定义的变量都是常量
- 采取面向接口的编程

## 开闭原则

[https://www.cnblogs.com/ldq2016/p/12978890.html](https://www.cnblogs.com/ldq2016/p/12978890.html)

- 重写与重载的区别
  - 重写只能发生在子类和父类之间
  - 重载：参数不一样

## 里氏替换原则

[https://www.cnblogs.com/o-andy-o/p/10315188.html](https://www.cnblogs.com/o-andy-o/p/10315188.html)

## 贫血模型和充血模型

[https://www.jianshu.com/p/450cd726ca8f](https://www.jianshu.com/p/450cd726ca8f)

- 使用充血模型时内聚性更高、耦合性更低
- 充血模型更复杂

## 分布式程序设计考虑的问题

1. 无状态应用

- 例如：web项目
  - 记住登录状态
  - 每个应用都有自身的状态处理，会有一个问题：不好进行横向扩展
- 可以做横向扩展的应用必须是无状态应用

Redis

2. 应用间如何交互

- tcp/http API的方式不合适
- 使用RPC
  - 服务发布
  - 服务订阅

3. 服务的治理

- 日志

4. 批量布署的问题
