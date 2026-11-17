# Supabase Postgres Best Practices CN

**用中文定位查询、连接与权限问题，保留 SQL 证据。**

基于 [supabase/agent-skills 的 supabase-postgres-best-practices](https://github.com/supabase/agent-skills/tree/main/skills/supabase-postgres-best-practices) 的小幅中文适配。独立维护、非官方。

## 保留上游核心

八类优先级：查询性能、连接管理、安全与 RLS、Schema、并发与锁、数据访问、监控诊断、进阶特性。原入口正文与 34 份参考文件保留，SQL 示例和来源不另造。

## 小幅补充

- 中文问题到相关规则的导航，按查询、连接、RLS、分页和锁问题定位。
- SQL、计划节点与指标保持原文，中文说明区分估算与实测。
- 提醒 EXPLAIN ANALYZE 的执行性质与数据库变更范围。
- 尊重项目的外键策略，不擅自添加或移除已有约束。

## 安装与调用

```bash
npx skills add batespaige357-tech/supabase-postgres-best-practices-cn --skill supabase-postgres-best-practices-cn
```

对助手说“用 supabase-postgres-best-practices-cn 分析这条查询，先看证据再给建议”；Codex 可使用 `$supabase-postgres-best-practices-cn`。也可将 `skills/supabase-postgres-best-practices-cn` 整个目录复制到助手技能目录，保留 references。

这是规则与工作流包，不自带数据库、凭据或执行器。适用于 Postgres 项目；Supabase 特有内容按实际环境使用。安装不自动连接数据库或执行 SQL，没有示例网页或效果截图。

## 来源与验证

MIT，保留 Supabase 版权和完整许可证。[CHANGES.md](CHANGES.md) 记录来源提交及具体变化。

已检查原正文、全部参考、UTF-8、本地链接和技能格式。未连接真实数据库、执行 SQL 或进行性能基准测试；不承诺性能收益。未运行前端编译。
