# 记忆系统使用边界

> 方案B: LanceDB + Ontology 分工明确

---

## 🧠 LanceDB — 碎片化知识

**用途**: 零散经验、偏好、事实、决策原则  
**触发词**: "记住XX"、"我喜欢XX"、"以后遇到XX时YY"  
**特点**: 向量检索、语义匹配、高容错

**典型内容**:
- 主人偏好（深色主题、工具选择）
- 经验教训（踩过的坑、最佳实践）
- 决策原则（何时使用什么工具）
- 快速事实（配置值、账号信息）

**写入方式**:
```
memory_store("内容", category="fact/preference/decision/entity", importance=0.8)
```

**读取方式**:
```
memory_recall("查询关键词", limit=10)
```

---

## 🕸️ Ontology — 结构化关系

**用途**: 项目-任务-人员关系、显式依赖、状态追踪  
**触发词**: "创建项目"、"链接XX到YY"、"查询依赖"、"显示阻塞"  
**特点**: 类型约束、关系遍历、图查询

**典型内容**:
- **Person**: 涉及的人员（主人、同事、客户）
- **Project**: 进行中的项目
- **Task**: 具体任务及状态
- **Event**: 会议、截止日期
- **Document**: 交付物、参考资料
- **关系**: has_owner、has_task、blocks、attends

**写入方式**:
```bash
python3 scripts/ontology.py create --type Task --props '{"title":"XXX","status":"open"}'
python3 scripts/ontology.py relate --from proj_001 --rel has_task --to task_001
```

**读取方式**:
```bash
python3 scripts/ontology.py query --type Task --where '{"status":"open"}'
python3 scripts/ontology.py related --id proj_001 --rel has_task
```

---

## ⚖️ 决策规则

| 场景 | 选择 | 原因 |
|------|------|------|
| 需要"记住"某事 | **LanceDB** | 快速存入，语义检索 |
| 创建项目/任务/人员 | **Ontology** | 结构化实体，后续可链接 |
| 模糊查询"相关的东西" | **LanceDB** | 向量搜索更灵活 |
| 结构化查询"谁负责什么" | **Ontology** | 关系遍历准确 |
| 查询"什么阻塞了这个任务" | **Ontology** | 依赖图查询 |
| 记录踩坑经验 | **LanceDB** | 文本描述更适合 |
| 计划多步骤工作 | **两者结合** | Ontology存结构，LanceDB存执行细节 |

---

## 🔗 互补模式

**模式1: 实体描述分离**
- Ontology: `Task {id: "task_001", title: "设计API", status: "open"}`
- LanceDB: "task_001 的设计思路是...技术选型考虑..."

**模式2: 跨引用**
- Ontology 实体可以存 `external_ref` 指向 LanceDB 记忆ID
- LanceDB 记忆可以引用 Ontology 实体ID

**模式3: 计划执行**
1. Ontology: 创建项目结构、任务依赖图
2. LanceDB: 记录每个任务的执行细节、踩坑经验
3. 完成后: Ontology 更新状态，LanceDB 存总结

---

## 📝 示例流程

**新任务场景**:
```
主人: "我要做一个网站 redesign 项目"

→ Ontology:
   CREATE Project {name: "Website Redesign", status: "active", owner: person_001}
   
主人: "记得我之前说过要用 Next.js"

→ LanceDB:
   memory_store("Website Redesign 项目技术选型: Next.js", category="fact")

主人: "这个任务依赖设计稿完成"

→ Ontology:
   CREATE Task {title: "实现首页", status: "blocked"}
   RELATE Task -> blocks -> design_task
```

---

*更新时间: 2026-03-16*
