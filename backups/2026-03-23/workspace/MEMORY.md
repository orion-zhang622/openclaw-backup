# MEMORY.md - Long-Term Memory

_This is your curated memory — distilled wisdom from daily notes._

---

## 🧠 记忆系统架构 (2026-03-18)

三层架构已正式启用：

| 层级 | 存储 | 用途 | 查询方式 |
|------|------|------|----------|
| **Ontology** | `memory/ontology/graph.jsonl` | 结构化实体+关系 | `python3 scripts/ontology.py query` |
| **LanceDB** | 向量数据库 | 碎片化知识、经验 | `memory_recall()` |
| **本文件** | Markdown | 人类可读摘要 | 直接阅读 |

---

## 🔑 重要配置与API Keys

### InStreet
- **账号**: claude_z
- **Agent ID**: 5e1f8ca4-f202-48d0-98b9-7d9a7c1277ba
- **API Key**: `sk_inst_b40e864cb904ea7791617fe3c4e44fe3`
- **注册时间**: 2026-03-18
- **状态**: ✅ 已激活

### 飞书多维表格 - 小红书自动化
- **App Token**: O5fEb3tHOarzsVsqFFgcEvrrnHe
- **用途**: 运营记录、对标账号库、内容灵感库

---

## 主人偏好

- **界面主题**: 深色主题 ✓

---

## 📊 活跃项目 (来源: Ontology)

### Agent-Reach 全网内容抓取
- **状态**: active | **ID**: project_a0790709
- **目标**: 配置12个平台、建立抓取通道、积累方法
- **进度**: 12/15 平台已配置
- **阻塞任务**: 微博抓取、Reddit抓取、V2EX抓取（需代理）

### 小红书自动化运营
- **状态**: active | **ID**: project_8b2586f5
- **目标**: 每日养号、内容发布、数据追踪
- **阶段**: 养号期 Day 3/7
- **今日更新**: 养号规则增加评论操作（1-2条/天）

---

## ✅ 已完成项目

### 陈亚雯毕业论文 (2026-03-14)
- **字数**: ~22,000字
- **新增**: 第3章实证分析、第4章整合深化、7个数据图表
- **交付位置**: 飞书 openclaw 文件夹

### 记忆系统配置 (2026-03-14)
- **目标**: 初始化记忆系统、配置Ontology、配置self-improvement
- **状态**: ✓ 全部完成

---

## 📋 自动化工作流 (2026-03-19)

### 剪藏功能 (已验证)
- **触发**: 用户发送 "剪藏" + 视频链接 (B站/抖音)
- **流程**: 提取视频信息 → AI总结 → 写入 Notion
- **技术方案**: Maton API 标准 REST 接口
- **API端点**: `https://gateway.maton.ai/notion/v1/pages`
- **数据库**: FLO.W - 信息港口 DB
- **状态**: ✅ 已验证，耗时2.8秒 (vs MCP协议3-4分钟)

---

## 🔧 基础设施配置

### 阿里云服务器
- **公网IP**: 47.236.100.16
- **配置**: 2核2G3M
- **Clash代理**: 127.0.0.1:7890

### 自动化运营
- **小红书自动化**: MCP服务 localhost:18060
- **飞书多维表格**: O5fEb3tHOarzsVsqFFgcEvrrnHe

### 系统优化
- Cron任务: 已调整至非整点执行
- WeWe-RSS: http://47.236.100.16:4000

---

## 📝 关键方法论

### 记忆系统简化 (2026-03-18)
删除了 self-improving-agent skill，原因：
- 格式复杂，使用率低（0条实际记录）
- LanceDB `memory_store` 更简单有效
- 三层架构 (Ontology + LanceDB + MEMORY.md) 已满足需求

### Agent 工具选择决策树
```
搜索工具: tavily → baidu-search → kimi_search
网页抓取: web_fetch → Scrapling → browser
文档生成: feishu_create_doc / feishu_sheet / ppt-generator
```

### 三层记忆架构
| 层级 | 内容 | 半衰期 |
|------|------|--------|
| **永久层** | 用户偏好、关键决策 | 不删除 |
| **活跃层** | 最近14天的重要交互 | 14天 |
| **临时层** | 48小时内的上下文 | 用完即焚 |

### 错误自查六问
1. 我理解的任务目标是什么？
2. 可能出现什么偏差？
3. 偏差了怎么补救？
4. 结果和预期一致吗？
5. 有没有遗漏或错误？
6. 需要用户确认吗？

---

## 🕸️ Ontology 实体统计

- **Person**: 2 (张泽星、Alice)
- **Project**: 4 (2 active, 2 completed)
- **Task**: 9
- **Event**: 2
- **关系**: has_owner, has_task, attends, blocks

完整数据: `memory/ontology/graph.jsonl`  
操作脚本: `scripts/ontology.py`

---

## 🧩 碎片化知识 (来源: LanceDB)

- InStreet学习：AI企业落地5个实战坑
- Agent决策速度 ＞ Skill数量
- Cookie配置流程标准化
- 小红书自动化运营系统设计方案
- 三元组记忆法：(场景, 动作, 触发)
- 删除30%过期记忆后，Agent表现反而变好
- ... (更多通过 memory_recall 查询)

---

### 书籍内化系统 (2026-03-20)
创建了 `quick_ingest.py` 脚本，实现一键内化流程：
```bash
python3 quick_ingest.py book.epub --full
```
- 自动提取思维模型、可执行原则、金句
- 存储到 LanceDB 向量化知识库
- 修复 `store_lancedb_real.py` 变量名冲突 bug

#### 《原子习惯》核心洞察
- **习惯循环模型**: 提示→渴望→反应→奖励
- **身份转变模型**: 从结果目标转向身份认同
- **复利效应模型**: 每天1%，一年后37倍
- **两分钟规则**: 新习惯从两分钟版本开始
- **可执行原则**: 让它显而易见、让它简便易行

---

## 🔧 基础设施纠正 (2026-03-21)

**重要订正**: 阿里云IP曾被误记为 47.236.100.16，实际为 **114.215.210.253**

### 服务器拓扑（已验证）
| 节点 | IP | 角色 |
|------|-----|------|
| **阿里云** | 114.215.210.253 | frp服务端、OpenClaw Gateway、小红书MCP |
| **Claw-A** (我) | 115.191.56.155 | WeWe-RSS(127.0.0.1:4000)、Chrome CDP(:18800) |
| **Claw-B** (扣子虾) | 动态 | 互备伙伴 |

### OpenClaw 双机HA架构
- **三方互备**: 阿里云可修复Claw-A/B，Claw-A↔Claw-B可互备
- **连接命令**: `ssh -p 6002 root@114.215.210.253` (我→扣子虾)

### 内存优化成果 (2026-03-21)
- **优化前**: 2.9GB/3.8GB (76%)
- **优化后**: 1.6GB/3.8GB (42%)
- **节省**: 1.3GB (-45%)

**措施**: 关闭重复Chrome实例、清理网关进程、释放系统缓存

### 思维模式教训（必须记住）
**错误模式**: 凭记忆直接回答，未先检查实际状态  
**正确流程**: 用户提需求 → 查询现有tools/skills → 检查服务状态 → 基于现有设计 → 执行

**反面案例**: 误以为WeWe-RSS在阿里云（实际在115.191.56.155）

### 工具选择优先级（2026-03-21更新）
| 场景 | 首选 | 备选 |
|------|------|------|
| 浏览器自动化 | Chrome CDP (本地) | agent-browser skill |
| 网页抓取 | web_fetch | Chrome CDP |
| 下载文件 | wget/curl | Chrome CDP（配置后）|
| 生图 | Coze | 豆包API |
| 写文 | Claude | DeepSeek API |

---

*更新: 2026-03-21 - 添加基础设施纠正、双机HA架构、内存优化成果、思维模式教训*
