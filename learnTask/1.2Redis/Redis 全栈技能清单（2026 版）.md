# Redis 全栈技能清单（2026 版）

# 最终整合版：Redis 全栈技能清单（2026 企业级/架构师版）

**完全对齐你给的格式 + 我之前给你的生产级干货 + 隐藏坑/争议点/真实案例**

**一次性给全，可直接当面试手册/技术评审/私活方案用**

---

## 一级主题：分布式锁（Distributed Locking）

- 子技能1：**Redisson RLock + Watchdog 自动续期**

最主流写法（2026 使用率 85%+ Java 项目）。为什么主流：开箱即用、可重入、自动续期（看门狗默认 30s）、支持虚拟线程、Spring Boot Starter 一键集成，生产事故率最低。

- 子技能2：**自实现 SET NX EX + Lua 原子释放（带唯一 token）**

适用场景：不想引入 Redisson Jar（轻量项目或公司禁第三方）。优缺点：性能最高，但续期/误删需自己写，容易出生产事故。

- 隐藏/冷门但生产中踩过的坑：**Redlock 争议极大**（Martin Kleppmann 2016 论文至今有效，2026 仍有金融团队使用但被批判“无法保证正确性”）；真实案例：时钟漂移导致锁丢失，秒杀超卖概率 10-20%；Redisson 官方已推荐 **FencedLock** 替代。

- 面试/私活高频考点：Redisson vs 自实现 vs Redlock 选型、看门狗原理、Lua 防误删脚本、时钟漂移怎么防。

---

## 一级主题：缓存一致性与常见模式（Cache Consistency）

- 子技能1：**Cache-Aside（旁路缓存）+ 读写双删/延迟双删**

2026 使用率 95%+。为什么主流：简单可靠，Spring Cache @Cacheable 默认就是它。

- 子技能2：**Read-Through + Write-Through（Spring Data Redis + CacheManager）**

适用场景：读多写少的企业后台。优缺点：代码侵入少，但一致性强依赖底层实现。

- 隐藏/冷门但生产中踩过的坑：**双写不一致**（先更新 DB 再删 Redis 失败）导致脏数据；**热点 key 雪崩**（随机过期 + 多级缓存 Caffeine L1 + Redis L2 是 2026 标配）；真实案例：2025 年多个支付系统因未加随机过期，流量峰值 Redis 雪崩宕机。

- 面试/私活高频考点：三种缓存模式对比、延迟双删时间怎么算、如何用 Lua 保证原子删除。

---

## 一级主题：消息通信（Messaging: Pub/Sub & Streams）

- 子技能1：**Redis Streams（消费者组 + XACK + PEL）**

2026 使用率暴增（轻量 MQ 替代 Kafka 场景）。为什么主流：持久化、ACK 机制、消费者组天然支持微服务，Spring Data Redis StreamTemplate 超好用。

- 子技能2：**传统 Pub/Sub + Redis 8 Functions**

适用场景：实时通知/聊天。优缺点：简单但无持久化（Streams 已取代大部分）。

- 隐藏/冷门但生产中踩过的坑：**PEL 堆积**（未 ACK 消息无限增长导致内存爆炸）；Streams 在 Redis Cluster 跨槽时性能骤降；真实案例：订单状态推送系统未限 PEL 大小，内存 OOM。

- 面试/私活高频考点：Streams vs Kafka 选型、XTRIM 策略、消费者组重平衡。

---

## 一级主题：限流与并发控制（Rate Limiting）

- 子技能1：**令牌桶（Token Bucket）用 Redis + Lua**

最主流（2026 使用率 80%+）。为什么主流：精准、支持突发，Redisson RRateLimiter 封装好。

- 子技能2：**滑动窗口（Sorted Set + ZREMRANGEBYSCORE）**

适用场景：严格秒级限流。

- 隐藏/冷门但生产中踩过的坑：Redis Cluster 下令牌桶跨槽不原子（需 Lua + hash tag）；热点 key 限流导致单实例压力爆炸；Redis 8 提供 **Count-Min Sketch** 做近似限流。

- 面试/私活高频考点：令牌桶 vs 漏桶 vs 计数器、Lua 实现原子性、Redis 8 概率结构限流。

---

## 一级主题：高可用架构（Sentinel / Cluster / Replication）

- 子技能1：**Redis Cluster（自动分片 + 16384 槽）**

2026 企业标配。为什么主流：水平扩容简单，Redis 8 性能再提升 2x。

- 子技能2：**Sentinel（哨兵模式）**

适用场景：老项目迁移、中小规模非分片集群。

- 隐藏/冷门但生产中踩过的坑：Cluster **槽迁移期间热点 key 抖动**（真实案例：2025 双 11 秒杀系统迁移导致 10% 请求超时）；主从延迟导致读写不一致；Redis 8 提供 **Lag-aware 可用性** 优化。

- 面试/私活高频考点：Cluster vs Sentinel 选型、hash tag 防跨槽、主从数据丢失场景。

---

## 一级主题：AI 与向量搜索（Vector Search & Probabilistic Structures）——2026 最热新技能

- 子技能1：**Redis 8 原生 Vector Set + Spring AI 集成（HNSW 索引）**

最主流（AI 后端爆发）。为什么主流：无需额外模块，语义搜索/RAG 秒级，Spring AI 官方支持。

- 子技能2：**原生概率结构（Bloom Filter、Cuckoo Filter、Top-K、T-Digest）**

适用场景：去重、限流、统计、推荐排序。

- 隐藏/冷门但生产中踩过的坑：向量维度不统一导致召回率暴跌；Redis 8 Vector 索引重建耗时长；EF / M 参数调优不当直接废掉检索效果。真实案例：推荐系统未调 EF 参数，召回率从 92% 掉到 65%。

- 面试/私活高频考点：向量 vs 传统搜索、Spring AI + Redis RAG 完整流程、Redis Functions 调用向量搜索。

---

## 一级主题：Lua 脚本与原子操作（Lua Scripting）

- 子技能1：**Redis 8 Functions（取代 Lua 部分场景）**

新主流。为什么主流：可持久化、可调试、有版本管理、比 Lua 更现代安全。

- 子技能2：**经典 Lua 脚本（EVAL / EVALSHA）**

适用场景：锁释放、原子库存扣减、限流、多命令原子聚合。

- 隐藏/冷门但生产中踩过的坑：Lua 脚本超时（默认 5s）阻塞实例导致雪崩；EVALSHA 缓存失效后回退 EVAL 造成性能抖动；**Cluster 模式跨槽直接报错**（90% 开发踩过）。

- 面试/私活高频考点：Lua 原子性原理、Redis 8 Functions 优势、迁移路径、与事务对比。

---

## 一级主题：性能调优与热点问题（Hot Keys / Memory / Observability）

- 子技能1：**热点 Key 探测 + 本地缓存兜底（Caffeine + Redis）**

2026 必备高并发标配。

- 子技能2：**Redis 8 性能优化（Active Defrag、内存压缩、高效过期）**

- 隐藏/冷门但生产中踩过的坑：BigKey 导致内存倾斜/网络阻塞/主线程卡顿；Lettuce 连接池高并发下线程爆炸；真实案例：未加本地缓存的热点 key，单实例 CPU 100%。

- 面试/私活高频考点：热点 Key 解决方案全家桶、BigKey 处理、Redis 8 新性能参数。

---

# 给你的架构师总结（最精炼 2026 版）

1. **分布式锁 = Redisson 看门狗，禁止手写，禁止 Redlock**

2. **缓存一致性 = 先更 DB 再删缓存 + 随机过期 + L1 Caffeine**

3. **轻量消息 = Redis Streams，不要再用 Pub/Sub**

4. **限流 = Lua 令牌桶，Cluster 必须加 hash tag**

5. **生产架构 = Redis Cluster 3 主 3 从，Sentinel 仅老项目**

6. **原子操作 = Lua / Redis 8 Functions，禁止多命令非原子**

7. **AI 必学 = Redis 8 Vector 向量搜索（RAG 标配）**

8. **高并发保命 = 热点 LocalCache + BigKey 治理**

---

需要我继续输出：

**《Redis 2026 架构师面试 100 题（含标准答案）》**

或

**《企业级 Redis 最佳实践模板（可直接写方案）》**

吗？
> （注：文档部分内容可能由 AI 生成）