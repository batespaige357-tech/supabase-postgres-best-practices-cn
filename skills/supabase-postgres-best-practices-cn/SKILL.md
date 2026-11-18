---
name: supabase-postgres-best-practices-cn
description: "中文适配：Postgres 查询、索引、RLS、连接与锁诊断。Postgres best practices originally maintained by Supabase, for Postgres running anywhere. Load this skill BEFORE writing or changing anything that lives in a Postgres database: creating or altering tables and columns (including choosing column types), schema design, migrations and declarative schema files, RLS policies and the tests that verify them, indexes, triggers, database functions, queues and scheduled jobs (pg_cron, pgmq), vector/semantic search (pgvector), and restoring dumps (pg_restore) or importing data. Also load it when diagnosing slow queries, high CPU, timeouts, EXPLAIN plans, connection exhaustion, locking, bloat, or rows visible to the wrong user or tenant. This is not just a performance guide — schema, migration, security, and SQL authoring tasks need these rules too, even for a one-column change or a single query."
license: MIT
metadata:
  author: supabase
  version: "1.1.1"
  organization: Supabase
  date: January 2026
  abstract: Comprehensive Postgres performance optimization guide for developers using Supabase and Postgres. Contains performance rules across 8 categories, prioritized by impact from critical (query performance, connection management) to incremental (advanced features). Each rule includes detailed explanations, incorrect vs. correct SQL examples, query plan analysis, and specific performance metrics to guide automated optimization and code generation.
---

# Supabase Postgres Best Practices

Comprehensive performance optimization guide for Postgres, maintained by Supabase. Contains rules across 8 categories, prioritized by impact to guide automated query optimization and schema design.

## When to Apply

Reference these guidelines when:
- Writing SQL queries or designing schemas
- Implementing indexes or query optimization
- Reviewing database performance issues
- Configuring connection pooling or scaling
- Optimizing for Postgres-specific features
- Working with Row-Level Security (RLS)

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | Query Performance | CRITICAL | `query-` |
| 2 | Connection Management | CRITICAL | `conn-` |
| 3 | Security & RLS | CRITICAL | `security-` |
| 4 | Schema Design | HIGH | `schema-` |
| 5 | Concurrency & Locking | MEDIUM-HIGH | `lock-` |
| 6 | Data Access Patterns | MEDIUM | `data-` |
| 7 | Monitoring & Diagnostics | LOW-MEDIUM | `monitor-` |
| 8 | Advanced Features | LOW | `advanced-` |

## How to Use

Read individual rule files for detailed explanations and SQL examples:

```
references/query-missing-indexes.md
references/query-partial-indexes.md
references/_sections.md
```

Each rule file contains:
- Brief explanation of why it matters
- Incorrect SQL example with explanation
- Correct SQL example with explanation
- Optional EXPLAIN output or metrics
- Additional context and references
- Supabase-specific notes (when applicable)

## References

- https://www.postgresql.org/docs/current/
- https://supabase.com/docs
- https://wiki.postgresql.org/wiki/Performance_Optimization
- https://supabase.com/docs/guides/database/overview
- https://supabase.com/docs/guides/auth/row-level-security


## 中文任务导航 — 社区适配补充

保留上游八类规则和优先级。

### 先确认项目事实

先看实际 SQL、表定义、索引、Postgres 版本、连接方式和已有查询计划；遵守项目约束。不要只凭“接口慢”就新建索引或换连接方式。上游示例中的业务表名、指标和规模是参考，不是当前项目的实测。

| 中文问题 | 优先读取 |
|---|---|
| 查询慢、过滤排序慢 | [执行计划](references/monitor-explain-analyze.md)、[缺少索引](references/query-missing-indexes.md)、[组合索引](references/query-composite-indexes.md) |
| 连接耗尽或大量空闲连接 | [连接限制](references/conn-limits.md)、[连接池](references/conn-pooling.md)、[空闲超时](references/conn-idle-timeout.md) |
| 用户能看到别人的数据 | [RLS基础](references/security-rls-basics.md)、[权限](references/security-privileges.md) |
| RLS开启后慢 | [RLS性能](references/security-rls-performance.md)、[执行计划](references/monitor-explain-analyze.md) |
| 写入、分页或逐行调用慢 | [批量插入](references/data-batch-inserts.md)、[分页](references/data-pagination.md)、[N+1](references/data-n-plus-one.md) |
| 死锁或锁等待 | [死锁预防](references/lock-deadlock-prevention.md)、[短事务](references/lock-short-transactions.md) |

### SQL与证据保持可核对

中文解释现象、证据、建议与适用条件，保留标识符、SQL、计划节点、数值和报错原文。区分计划估算与实际执行结果；没有跑过的优化不能声称已提速。

`EXPLAIN ANALYZE` 会执行目标语句；不能把它一概当作无副作用的只读检查。先确认语句和环境，在获准的测试或安全检查范围内使用；未获准的写入、迁移、索引构建或线上配置变更先形成可审阅方案。不能仅因技能建议就执行生产操作。

外键规则在已有或明确采用外键时使用；项目要求尽量不用外键时，不擅自加外键，也不删除已有约束。说明所选关联与一致性方案、现有约束和实际查询需要的索引，保留数据完整性要求。

### 验证边界

对照同一查询、参数、数据规模和环境，记录前后证据；缺少数据库访问或真实负载时列为待验证。不要通过关闭权限校验或 RLS 换取表面性能。涉及版本特性先查对应版本的官方文档。用户未要求前端编译时不运行前端 build。
