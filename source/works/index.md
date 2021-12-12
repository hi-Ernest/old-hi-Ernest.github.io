---
title: resume
date: 2021-12-12 02:29:47
---

## <img src="" height="20px"> 教育经历

### 重庆理工大学，重庆<span class="right" style="float:right">2016.9 ~ 2020.7</span>

- 本科，软件工程

## <img src="" height="20px"> 工作和项目经历

### **虎扑（上海）文化传播公司**，上海，Java工程师<span class="right" style="float:right">2020.07 ~ 2021.01</span>

参与虎扑APP中篮球、电竞频道的研发。期间参与拆分颗粒度合适的微服务应用，完成篮球赛程列表、赛事直播间、比分牌等重要功能的Java化迁移，以及球队球员、比赛等数据同步，将外部数据源与虎扑数据进行映射和解耦，增强数据信息的容错性。

1. 业务**主力开发**，参与篮球球队、球员以及比赛数据的同步，以及根据球员、球队、比赛的细节数据分析各纬度数据用以各端的使用。

   使用DS同步框架（Java范型特性），统一同步链路，建立核心数据与外部数据映射，解决赛事数据同步链路不一致，重复开发和测试问题。

2. 参与篮球**赛程列表**Java化（之前是PHP链路将其替换为Java链路）。

   通过xxl-job定时任务，构建Handle定时执行，使用DS同步模版进行比赛数据转换（数据源OriginalModel到我们所需要的standardModel），先通过Apache的httpClient请求数据源接口，将返回的JSON数据用OriginalModel进行装载，并添加外键关联，再提取OriginalModel数据构建我们所需要的MatchModel，并通过雪花算法生成唯一matchId，将MatchModel存储在MySQL数据库以及其他操作封装成一个事务，用RocketMQ发事务消息，返回matchId将外部比赛id和我们matchId关联信息存储MySQL。使用xxl-job，定时取MySQL中matches并分页取比赛ID列表，使用CompletableFuture加上ThreadPoolExecutor异步通过matchId拿到比赛Model并用matchList存储起来。将matchList分100批次，通过stream操作分段截取，将比赛信息放在Redis缓存中，供api层项目获取。Api层从Redis中获取全量比赛，为了防止脏数据，再从MySQL获取最近七天的比赛，将其覆盖Redis中那七天数据，然后放在本地缓存ConcurrentMap当中。之后赛程列表的接口，通过读取本地缓存数据，进行一系列的业务层面操作。

3. 参与比赛直播间**比分牌**的Java化。

   Api层通过matchId从本地缓存取单个比赛信息，包括比分、主客队犯规数等，考虑比分牌缓存命中率较高，放在Redis中并设置过期时间为5分钟，再做完其他操作后将数据返回前端。对于比赛中比分牌变更，采用xxl-job定时去查询直播间统计实时数据，再与缓存中比分牌信息进行比较，根据缓存是否存在快照版本，如果存在，比较最新数据，有数据变更则添加快照版本，增量推送到MQ(Redis)，并更新Api层比赛列表Redis缓存。Node.js通过订阅的topic拿到数据，再将数据给前端。

### 成都聚思力信息技术有限公司，成都，Java工程师<span class="right" style="float:right">2019.11 ~ 2019.12</span>

参与公司ERP系统的研发，负责对订单系统的历史数据迁移。

1. 负责订单请求(RP/MR)状态的更新。将 Sybase 数据库的存放的零件采购订单采用逻辑删除，为了防止

   删除表的数据量太大定时将部分删除的记录转移到 MySQL 数据库指定 history 表中，订单状态首先查询 Sybase 数据库表，已经删除的记录则需要查询 MySQL 的 history 表，但定时查询如每周一次，为了解决时间太久返回问题，所以一旦订单为删除状态立即返回该状态。

## <img src="" height="20px"> 技能

★★★ 扎实的后端基础，良好的编码能力以及编码习惯

★★★ Java面对对象编程，以及集合、IO、多线程

★★★ 常用数据结构和算法，链表、二叉树等以及排序算法等

★★★ 计算机网络和TCP、UDP等常用网络协议

★★☆ MySQL数据库操作，了解NoSQL，使用过Redis

★★☆ JVM、使用SpringCloud、MyBatis等开源框架

★★☆ 常见的设计模式，单例、策略、工厂等

★★☆了解Git操作，基本命令以及版本控制