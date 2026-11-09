# 本地生活点评平台

基于 Spring Boot + Redis 的高并发本地生活点评系统，还原大众点评类产品的核心后端链路，涵盖商户检索、优惠券秒杀、探店笔记、关注 Feed 与数据统计等场景。

## 技术栈
- 后端：Spring Boot、MyBatis-Plus
- 存储：MySQL、Redis（含 Redisson、Lua）
- 其他：Redis GEO、Redis Stream、JMeter

## 核心功能
- 用户登录：基于 Redis 的短信验证码登录与登录态管理
- 商户检索：Redis GEO 附近商铺检索 + 距离排序 + 分页
- 优惠券秒杀：Redis 库存预扣 + Lua 原子校验 + 消息队列异步下单，防止超卖
- 探店笔记：笔记发布、点赞、达人探店
- 关注 Feed：好友关注 + Feed 滚动分页
- 数据统计：签到（BitMap）、UV 统计（HyperLogLog）

## 核心难点
- 缓存穿透 / 击穿 / 雪崩：空值缓存、互斥锁 + 逻辑过期、缓存预热
- 分布式锁演进：Redis setnx → Lua 脚本 → Redisson 分布式锁
- 秒杀优化：Redis 资格判断 → 阻塞队列 → Redis 消息队列异步秒杀

## 如何运行
1. 环境要求：JDK 8、Maven、MySQL 8、Redis
2. 执行 `src/main/resources/db/hmdp.sql` 初始化数据库
3. 修改 `src/main/resources/application.yaml` 中的 MySQL / Redis 连接信息
4. 启动：`mvn spring-boot:run`
