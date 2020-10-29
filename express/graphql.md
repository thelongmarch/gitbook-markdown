## 简介

- GraphQL是facebook开发的一种数据查询语言，并于2015年公开发布。它是REST API的替代品

- GraphQL既是一种用于API的查询语言也是一种使用现有数据完成这些查询运行时的查询语言。GraphQL对于的API中的数据提供了一套易于理解的完整描述，使得客户端能够准确地获取它需要的数据，而且没有任何冗余，也让API更容易地随着时间推移而演进。

- 官网：https://graphql.org/

- 中文网：https://graphql.cn/

### 特点

1. 请求需要的数据，不多不少

例如：account中name,age,age,sex,department等。可以只取的需要字段。

2. 获取多个资源，只用一个请求

3. 描述所有可能类型的系统。 便于维护，根据需求平滑演进，添加或者隐藏字段。 

### 与restful对比

- restful ： Representational  State Transfer：属性状态转移。本质上就是用定义url
，通过api接口来取得资源。通过系统架构，不受语言限制。

- restful一个接口只能返回一个资源，graphql一次可以获取多个资源。

- restful用不同的url来区分资源，graphql用类型区分资源。