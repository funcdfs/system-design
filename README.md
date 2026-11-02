# system-design

讲述市面上可能遇到的各类系统设计问题，以及设计方案的优缺点和具体取舍。

## 目录结构

- **`案例/` — 主内容**：一个具体场景一篇（短链服务、限流器、Feed 流……）。读者带着问题来，从这里进入。
- **`引用/` — 引用子块**：一个概念一篇的"词汇表"，由案例按需链接，不在案例里重复展开：
  - [`基础理论/`](引用/基础理论/) — 基础理论：CAP、一致性模型、时钟与序
  - [`存储/`](引用/存储/) — 存储选型：SQL / NoSQL / 时序 / 对象存储
    - [`wide-column.md`](引用/存储/wide-column.md) — 宽列存储（Cassandra / HBase 模型）与它放弃的东西
  - [`缓存/`](引用/缓存/) — 缓存：一致性、雪崩 / 穿透 / 击穿
  - [`消息队列/`](引用/消息队列/) — 消息队列：投递语义、顺序、幂等
    - [`realtime-channel.md`](引用/消息队列/realtime-channel.md) — 实时下行通道：WebSocket / 长轮询 / SSE / 厂商推送
    - [`delivery-semantics.md`](引用/消息队列/delivery-semantics.md) — 投递语义：at-most / at-least / exactly-once
    - [`idempotency.md`](引用/消息队列/idempotency.md) — 幂等与去重：键设计、分层去重
    - [`ordering.md`](引用/消息队列/ordering.md) — 消息顺序：分区有序、单调 seq、时钟方案
  - [`分布式协调/`](引用/分布式协调/) — 分布式协调：锁、共识、选举
  - [`架构模式/`](引用/架构模式/) — 架构模式：分库分表、CQRS、读写分离
    - [`fan-out.md`](引用/架构模式/fan-out.md) — 写扩散 vs 读扩散：受众规模决定分岔
  - [`稳定性/`](引用/稳定性/) — 稳定性：限流、熔断、降级、重试 / 超时预算
- **`设计模式/` — 顶层章节（本次新建，目录骨架）**：按微软 *Cloud Design Patterns* 一书中「Design Patterns」章的排布初始化，24 个模式各一个子文件夹，暂为空目录（以 `.gitkeep` 占位），详见下文「设计模式（依据微软章节）」。
- **`模板/`** — 写作模板，新增文章从这里复制。

维护约定（命名、链接纪律、提交前自检）见 [agent.md](agent.md)。

## 案例索引

> 新增案例后在此登记一行。命名用「领域-主题」小写 kebab-case，通用案例不加前缀。

| 领域 | 案例 | 一句话结论 |
| ---- | ---- | ---------- |
| 社交 | [social-messenger](案例/social-messenger.md) | 长连接网关 + 会话分区宽列存储 + at-least-once/端侧去重；顺序只承诺会话内有序，用 seq 在端侧收敛 |

## 设计模式（依据微软章节）

来源：微软归档书 *Cloud Design Patterns: Prescriptive Architecture Guidance for Cloud Applications*，目标页 [Circuit Breaker Pattern（dn589784）](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/dn589784(v=pandp.10))。

该书的目录（由该书目录接口 `dn600223(v=pandp.10)/toc.json` 与「Design Patterns」章落地页 dn600223 互相核对）为：

```text
Cloud Design Patterns（全书）
├─ Problem Areas in the Cloud
├─ Design Patterns          ← 本仓库顶层文件夹「设计模式」对应这一章
│  └─ 24 个模式（见下表，扁平并列、无再分组）
├─ Guidance
├─ Code Samples
├─ Bibliography
└─ Acknowledgements
```

说明：`Guidance` 等是与「Design Patterns」并列的同书章节，并非该章的子目录，故未放入 `设计模式/` 内，以免虚构层级。`设计模式/` 内 24 个子文件夹与下表一一对应，顺序与微软目录一致；英文原名为微软原文标题，中文名为本仓库文件夹名（CQRS 的英文缩写只保留在此对照表中，文件夹只用中文）。

| # | 微软原标题 | 本仓库子文件夹 |
| - | ---------- | -------------- |
| 1 | Cache-Aside Pattern | `设计模式/旁路缓存模式/` |
| 2 | Circuit Breaker Pattern（目标页 dn589784） | `设计模式/断路器模式/` |
| 3 | Compensating Transaction Pattern | `设计模式/补偿事务模式/` |
| 4 | Competing Consumers Pattern | `设计模式/竞争消费者模式/` |
| 5 | Compute Resource Consolidation Pattern | `设计模式/计算资源整合模式/` |
| 6 | Command and Query Responsibility Segregation (CQRS) Pattern | `设计模式/命令与查询职责分离模式/` |
| 7 | Event Sourcing Pattern | `设计模式/事件溯源模式/` |
| 8 | External Configuration Store Pattern | `设计模式/外部配置存储模式/` |
| 9 | Federated Identity Pattern | `设计模式/联合身份模式/` |
| 10 | Gatekeeper Pattern | `设计模式/守门员模式/` |
| 11 | Health Endpoint Monitoring Pattern | `设计模式/健康端点监控模式/` |
| 12 | Index Table Pattern | `设计模式/索引表模式/` |
| 13 | Leader Election Pattern | `设计模式/领导者选举模式/` |
| 14 | Materialized View Pattern | `设计模式/物化视图模式/` |
| 15 | Pipes and Filters Pattern | `设计模式/管道与过滤器模式/` |
| 16 | Priority Queue Pattern | `设计模式/优先队列模式/` |
| 17 | Queue-Based Load Leveling Pattern | `设计模式/基于队列的负载调平模式/` |
| 18 | Retry Pattern | `设计模式/重试模式/` |
| 19 | Runtime Reconfiguration Pattern | `设计模式/运行时重新配置模式/` |
| 20 | Scheduler Agent Supervisor Pattern | `设计模式/调度器代理监督者模式/` |
| 21 | Sharding Pattern | `设计模式/分片模式/` |
| 22 | Static Content Hosting Pattern | `设计模式/静态内容托管模式/` |
| 23 | Throttling Pattern | `设计模式/节流模式/` |
| 24 | Valet Key Pattern | `设计模式/代客密钥模式/` |

## 阅读方式

案例是文章，引用是词条：读案例碰到共性概念时跳去引用看方案谱系，再回到案例看本场景怎么取舍。
