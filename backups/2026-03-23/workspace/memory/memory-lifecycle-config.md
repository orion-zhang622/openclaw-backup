# 记忆保质期机制配置

## 概述

三层记忆系统已启用 Weibull Decay 自动过期机制。

---

## 🕐 三层记忆保质期

| 层级 | 存储位置 | 半衰期 | 清理策略 |
|------|----------|--------|----------|
| **永久层** | MEMORY.md + Ontology | 永不删除 | 人工审核后归档 |
| **活跃层** | LanceDB (core/working) | 14-30天 | 自动降权+降级 |
| **临时层** | LanceDB (ephemeral) | 7天 | 自动清理 |

---

## 🧠 LanceDB Weibull Decay 参数

### 每日维护 (21:25) 执行规则

```yaml
# 过期标记
14天未访问: importance *= 0.9   # 轻微降权
30天未访问: importance *= 0.7   # 中度降权  
60天未访问: 标记为 stale        # 重度标记

# 层级降级
core → working: 访问<5次 且 >30天
working → ephemeral: 访问<2次 且 >14天

# 自动清理
ephemeral + importance<0.3 + 30天未访问 → 自动删除
```

### 月度深度清理 (每月1日 03:00) 执行规则

```yaml
# 强制清理
stale标记 + 用户确认 → 永久删除
ephemeral + importance<0.3 → 自动删除（无需确认）

# 重复检测
相似度>0.85的记忆对 → 建议合并

# 升级建议
访问>20次 + importance>0.9 → 建议写入MEMORY.md
```

---

## 📊 记忆健康度评分

计算公式：
```
健康度 = (core_count * 1.0 + working_count * 0.7 + ephemeral_count * 0.3) / total_count
       - (stale_count * 0.5) / total_count
```

| 健康度 | 状态 | 建议 |
|--------|------|------|
| 0.8-1.0 | 🟢 优秀 | 无需干预 |
| 0.6-0.8 | 🟡 良好 | 关注即可 |
| 0.4-0.6 | 🟠 警告 | 需要清理 |
| <0.4 | 🔴 危险 | 立即深度清理 |

---

## 🔧 手动操作命令

### 查看记忆统计
```bash
# 使用 memory_stats 工具查看
```

### 手动标记过期
```python
# 对特定记忆执行降权
memory_update(memoryId="xxx", importance=0.5)
```

### 手动删除
```python
# 删除单条记忆
memory_forget(memoryId="xxx")

# 按条件批量删除（谨慎使用）
memory_forget(query=" stale 标记")
```

---

## 📅 定时任务清单

| 时间 | 任务 | 记忆维护内容 |
|------|------|--------------|
| 每日 21:25 | 记忆维护 | Weibull decay、层级降级、健康报告 |
| 每日 08:12 | 每日日报 | 记忆统计摘要 |
| 每月1日 03:00 | 月度深度清理 | Stale清理、重复合并、升级建议 |

---

## 🚨 紧急处理

如果发现过期记忆污染判断：

1. **立即执行** `memory_recall` 检查最近记忆
2. **手动标记** 可疑记忆 importance=0.1
3. **强制遗忘** `memory_forget` 删除污染记忆
4. **更新本配置** 调整 decay 参数

---

*配置更新时间: 2026-03-18*
*下次月度清理: 2026-04-01 03:00*
