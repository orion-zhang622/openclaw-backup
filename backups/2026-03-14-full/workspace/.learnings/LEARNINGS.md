# LEARNINGS.md - 经验与学习记录

记录从错误中学到的经验、用户的纠正、最佳实践。

## 格式模板

```markdown
## [LRN-YYYYMMDD-XXX] 类别

**Logged**: ISO-8601 时间戳
**Priority**: low | medium | high | critical
**Status**: pending | in_progress | resolved | promoted
**Area**: frontend | backend | infra | tests | docs | config | memory

### Summary
一句话概括学到了什么

### Details
详细上下文：发生了什么、哪里错了、正确的做法是什么

### Suggested Action
具体的改进建议

### Metadata
- Source: conversation | error | user_feedback
- Related Files: 相关文件路径
- Tags: 标签
- See Also: 相关记录ID（如果有）

---
```

## 记录列表

## [LRN-20250314-001] memory-data-vs-knowledge

**Logged**: 2026-03-14T01:35:00+08:00
**Priority**: high
**Status**: resolved
**Area**: memory

### Summary
配置清单（装了哪些 skills、仓库地址等）是数据，不是认知，不该存入记忆系统。

### Details
用户询问记忆系统中存储了哪些记忆时，我展示了一条关于 OpenClaw 配置的记忆（"2026-03-14 完成 OpenClaw 全面配置：安装14个skills..."）。

用户质疑这种信息是否有必要存储。我反思后认同：这类信息是容易过时的**数据**（可通过 ls/config 实时查询），而非**认知**（决策原因、失败教训、用户偏好）。

记忆系统应该存的是：
- 决策原因（"为什么选 OpenRouter 而不是直接买 API Key"）
- 失败教训（"skills 之间的依赖关系问题"）
- 用户偏好（"不要在群聊中暴露我的日程细节"）

### Suggested Action
1. ✅ 已清理 LanceDB 中的数据型记忆
2. 未来存入记忆前，先问自己：这是数据还是认知？
3. 数据型信息通过实时查询获得，不占用记忆空间

### Metadata
- Source: user_feedback
- Related Files: LanceDB memory store
- Tags: memory-hygiene, data-vs-knowledge, decision-principle
- Pattern-Key: memory.store-cognitive-only

---

## [LRN-20250314-002] context-check-before-long-tasks

**Logged**: 2026-03-14T02:01:00+08:00
**Priority**: high
**Status**: promoted
**Area**: workflow

### Summary
用户要求：在开始长任务前必须先检查上下文使用情况，如果可能不足要提醒用户。

### Details
用户担心在长任务执行到一半时上下文爆了导致失忆。要求建立工作规范：
1. 长任务开始前运行 `session_status` 检查上下文
2. 根据剩余空间评估：
   - > 50% -> 正常执行
   - 40-50% -> 提醒用户"可能不够，要分批吗？"
   - < 40% -> 强烈建议"开新会话"
3. 长任务中定期汇报进度

这是 proactive workflow 的重要组成部分，避免任务执行到一半被迫中断。

### Suggested Action
1. 已更新 AGENTS.md 添加 "Long Task Context Check" 章节
2. 每次长任务前自动执行检查
3. 形成本能反应，不需要用户提醒

### Metadata
- Source: user_feedback
- Related Files: AGENTS.md, proactive-agent skill
- Tags: workflow, context-management, proactive-behavior
- Pattern-Key: workflow.check-context-before-long-tasks
- Promoted to: AGENTS.md

### Resolution
- **Resolved**: 2026-03-14T02:01:00+08:00
- **Promoted**: AGENTS.md

---
