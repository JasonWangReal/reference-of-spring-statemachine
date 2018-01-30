# Part I. 简介 {#introduction}

Spring Statemachine（SSM）是提供给应用开发者使其可以在Spring应用中使用传统状态机的一套框架。SSM致力于提供以下特性：
- 单级状态机用于简单场景
- 多级状态机用于简化复杂的状态配置
- 区域状态机用于更复杂的状态配置
- triggers, transitions, guards 和 actions 的使用
- 类型安全配置适配器
- 状态机时间监听器
- 集成Spring IOC管理状态机beans

继续阅读之前推荐先浏览附录部分的`B.2 术语表`和`B.3 状态机速成课`以获得状态机的基本概念，本篇文档后续部分将假定读者对状态机的概念比较熟悉。