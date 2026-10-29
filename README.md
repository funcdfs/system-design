# system-design

讲述市面上可能遇到的各类系统设计问题，以及设计方案的优缺点和具体取舍。

## 目录结构

- **`cases/` — 主内容**：一个具体场景一篇（短链服务、限流器、Feed 流……）。读者带着问题来，从这里进入。
- **`refs/` — 引用子块**：一个概念一篇的"词汇表"，由 case 按需链接，不在 case 里重复展开：
  - [`foundations/`](refs/foundations/) — 基础理论：CAP、一致性模型、时钟与序
  - [`storage/`](refs/storage/) — 存储选型：SQL / NoSQL / 时序 / 对象存储
  - [`cache/`](refs/cache/) — 缓存：一致性、雪崩 / 穿透 / 击穿
  - [`messaging/`](refs/messaging/) — 消息队列：投递语义、顺序、幂等
  - [`coordination/`](refs/coordination/) — 分布式协调：锁、共识、选举
  - [`patterns/`](refs/patterns/) — 架构模式：分库分表、CQRS、读写分离
  - [`resilience/`](refs/resilience/) — 稳定性：限流、熔断、降级、重试 / 超时预算
- **`templates/`** — 写作模板，新增文章从这里复制。

维护约定（命名、链接纪律、提交前自检）见 [agent.md](agent.md)。

## 案例索引

> 新增案例后在此登记一行。命名用「领域-主题」小写 kebab-case，通用案例不加前缀。

| 领域 | 案例 | 一句话结论 |
| ---- | ---- | ---------- |
| —    | 暂无案例，第一篇 case 诞生后在此登记 | — |

## 阅读方式

case 是文章，ref 是词条：读 case 碰到共性概念时跳去 ref 看方案谱系，再回到 case 看本场景怎么取舍。
