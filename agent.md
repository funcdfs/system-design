# system-design 维护约定

给后续维护者（人类或 AI agent）的约定。动这个仓库之前先读完本文。改动时最要紧的是保住 cases 与 refs 之间的引用关系——这个仓库的价值密度取决于引用是否完整、克制，而不是单篇文章写得多漂亮。

## 内容架构

只有两种正文，主从关系不可颠倒：

| 目录 | 角色 | 粒度 |
| ---- | ---- | ---- |
| `cases/` | 主内容，读者带着具体问题进入这里 | 一个场景一篇，扁平，永不建子目录 |
| `refs/` | 引用子块，被 case 按需链接的"词汇表" | 一个概念一篇，两层：`refs/<主题域>/<概念>.md` |

`refs/` 的主题域固定为 7 个：`foundations` / `storage` / `cache` / `messaging` / `coordination` / `patterns` / `resilience`。默认不新增；确有必要时，先同步更新本文档与 `README.md` 再动目录。

`templates/` 是写作模板，不属于正文。`.gitkeep` 只用来在目录还没有正文时撑住层级；某目录出现第一个 `.md` 的那次提交，顺手删掉它的 `.gitkeep`。

## 命名与路径（硬规则）

- 目录、文件名一律小写 kebab-case（`url-shortener.md`），正文用中文。
- `cases/` 永远扁平。用文件名前缀做领域分组：通用案例不加前缀（`url-shortener.md`、`rate-limiter.md`），垂直领域加前缀（`social-news-feed.md`、`ecommerce-seckill.md`）。常用前缀：`social-` / `ecommerce-` / `media-` / `infra-` / `collab-`，不够再加。
- `refs/` 固定两层，不加层：`refs/cache/consistency.md`。
- 站内链接一律相对路径：case → ref 写 `../refs/<主题域>/<概念>.md`；禁止绝对路径。外部参考资料放在文末，不算站内链接。

## 相互关联（核心纪律）

1. **case 不内联展开共性概念。** 碰到共性概念（缓存一致性、MQ 顺序性、分布式锁……），case 只写"本场景怎么取舍"，并链接对应 ref。在 case 里复述 ref 的内容 = 制造两份会各自腐烂的真相。
2. **ref 必须自包含。** 不依赖任何 case 的语境也能读懂：是什么 / 方案谱系对比 / 各自适用条件和代价 / 常见误区。
3. **先有引用，再写 ref。** 新增 ref 的前提是已经（或正在）有 case 链接它。没有任何 case 引用的 ref 是孤儿词条，删除或合并。
4. **索引同步。** 新增、移动、重命名任何文章，同一次提交里更新 `README.md` 的案例索引 / refs 总览。
5. **移动或重命名 ref**：先全仓库搜旧路径，更新所有引用它的 case，再提交。
6. **修改 ref 的核心结论**：检查所有引用它的 case，其中的取舍结论是否仍然成立；不成立就连带修正，不要只改一半。

## 提交前自检（在仓库根目录，依赖 rg）

```bash
# 死链：cases 里引用的 ref 是否都存在（支持 # 锚点链接）
rg -o -N --no-filename '\]\(\.\./(refs/[^)#\s"]+)(#[^)]*)?\)' -r '$1' cases/ | sort -u |
  while read -r t; do [ -f "$t" ] || echo "dead link: $t"; done

# 孤儿：refs 里没有任何 case 引用的词条
find refs -name '*.md' | while read -r f; do
  rg -q "$f" cases/ || echo "orphan ref: $f"; done
```

两条命令零输出才算干净，才可以提交。后续可以挂 CI（lychee / markdown-link-check + 孤儿检查脚本）。

## 新增一篇 case

1. `cp templates/case.md cases/<命名>.md`
2. 按模板填写；共性概念链接 refs，不展开。
3. 需要尚不存在的 ref？先在本文里写下链接（自检会报死链），紧接着补 ref，再提交。
4. `README.md` 案例索引登记一行（领域 / 案例链接 / 一句话结论）。
5. 跑「提交前自检」。

## 新增一篇 ref

1. 确认至少一个 case 链接了它（先有需求，再动笔）。
2. `cp templates/ref.md refs/<主题域>/<概念>.md`，按模板填写，保持自包含。
3. 跑「提交前自检」。

## 给 AI agent 的额外提醒

- 内容可以粗糙，引用关系必须完整：宁可留一个明确的 TODO 占位并链接 ref，也不要在 case 里硬写一段本该属于 ref 的概念解释。
- 不要顺手"优化"结构：`cases/` 扁平、`refs/` 两层都是刻意设计（扁平保证 case 里的相对链接永不失效），不要加层级、不要移动文件。
- 每次改动结束前运行上面两条自检命令，零输出再提交。
- 提交聚焦一件事：一篇 case、一批配套 refs，或纯索引/模板调整，不要混在一起。
