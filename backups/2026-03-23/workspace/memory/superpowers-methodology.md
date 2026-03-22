# Superpowers 方法论萃取

> 从 LunarTech Superpowers 项目提取的 AI Agent 工作流最佳实践  
> 来源: https://github.com/LUNARTECH-X/superpowers  
> 适配: OpenClaw 个人助理场景

---

## 核心理念

**Superpowers** 是一套完整的 AI 编程 Agent 工作流系统，核心思想是：

> **通过标准化流程和可组合技能，让 AI Agent 遵循工程最佳实践，而非随意编码。**

---

## 五大核心技能

### 1. 🧠 Brainstorming（需求澄清）

**触发时机**: 任何创造性工作之前（新功能、组件、修改行为）

**核心流程**:
```
检查项目状态 → 提问澄清（一次一个）→ 探索多种方案 → 分段呈现设计 → 确认保存
```

**关键原则**:
- **一次一问** - 不要同时抛多个问题
- **选择题优先** - 比开放式问题更容易回答
- **YAGNI** - 无情地砍掉不必要的功能
- **探索替代方案** - 总是提出 2-3 种方案并比较
- **增量验证** - 每 200-300 字确认一次方向

**输出**: `docs/plans/YYYY-MM-DD-<topic>-design.md`

---

### 2. 📝 Writing Plans（制定计划）

**触发时机**: 有明确需求后，动手写代码之前

**核心原则**:
> 编写详细的实现计划，假设执行者**对代码库零上下文**且品味堪忧。

**任务粒度**:
- 每个任务 = 一个动作 = **2-5 分钟**
- 示例: "写失败测试" → "运行看失败" → "写最小实现" → "运行看通过" → "提交"

**计划文档结构**:
```markdown
# [功能名] Implementation Plan
> **For Claude:** REQUIRED SUB-SKILL: Use executing-plans to implement this plan task-by-task.

**Goal:** [一句话描述]
**Architecture:** [2-3句话架构概述]
**Tech Stack:** [关键技术/库]

---

### Task N: [组件名]
**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: 写失败测试**
```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: 运行测试验证失败**
```bash
pytest tests/path/test.py::test_name -v
# Expected: FAIL with "function not defined"
```

**Step 3: 写最小实现**
```python
def function(input):
    return expected
```

**Step 4: 运行测试验证通过**
```bash
pytest tests/path/test.py::test_name -v
# Expected: PASS
```

**Step 5: 提交**
```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
```

**重要规则**:
- 始终使用**精确文件路径**
- 计划中包含**完整代码**（不是"添加验证"这种模糊描述）
- 包含**确切命令和预期输出**
- DRY、YAGNI、TDD、频繁提交

---

### 3. 🚀 Subagent-Driven Development（子代理开发）

**触发时机**: 在当前会话中执行独立任务的实现计划

**核心理念**:
> Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration

**工作流程**:
```
读取计划 → 提取所有任务 → 创建 TodoWrite

对每个任务:
  ├─ 派遣实现子代理（./implementer-prompt.md）
  ├─ 实现者提问？→ 回答 → 继续
  ├─ 实现者编码、测试、提交、自审
  ├─ 派遣 spec reviewer（验证符合规范）
  │   └─ 不通过 → 实现者修复 → 重新审查
  ├─ 派遣 code quality reviewer（代码质量）
  │   └─ 不通过 → 实现者修复 → 重新审查
  └─ 标记任务完成

全部完成后:
  ├─ 派遣最终代码审查
  └─ 使用 finishing-a-development-branch 完成
```

**两阶段审查**:
1. **Spec Compliance** - 代码是否符合需求（不多不少）
2. **Code Quality** - 代码质量（设计、测试、风格）

**红线（Never）**:
- 跳过任一阶段审查
- 并行派遣多个实现子代理
- 让子代理读取计划文件（应提供完整文本）
- 忽略子代理的问题
- 在审查有未解决问题时继续下一任务

---

### 4. 🔄 Test-Driven Development（测试驱动开发）

**触发时机**: 实现任何功能或修复任何 Bug 之前

**铁律**:
```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

**Red-Green-Refactor 循环**:

| 阶段 | 动作 | 验证 |
|------|------|------|
| **RED** | 写失败测试 | 运行测试，确认失败且失败原因正确 |
| **GREEN** | 写最小代码通过测试 | 运行测试，确认通过 |
| **REFACTOR** | 清理代码（不改行为） | 保持测试通过 |

**关键规则**:
- 先写代码再补测试？**删除重写**
- 测试立即通过？说明测试的是已有行为，修复测试
- 最小代码 = 刚好通过，不多不少
- 不重构其他代码，不"改进"超出测试范围

**常见借口与真相**:

| 借口 | 真相 |
|------|------|
| "太简单不用测" | 简单代码也会坏，测试只需30秒 |
| "先写代码再补测试验证" | 事后测试立即通过，证明不了什么 |
| "已经手动测过所有边界" | 手动=临时，无法复现，容易遗漏 |
| "删X小时工作太浪费" | 沉没成本谬误。留着不信任的代码才是债务 |
| "TDD太教条，我务实" | TDD就是务实：提前发现bug，防止回归，支持重构 |

**测试质量**:
- 一个测试 = 一个行为（名字里有"and"就拆分）
- 名字清晰描述行为（不是 `test1`）
- 展示期望的 API（不是测试实现细节）

---

### 5. 🔍 Systematic Debugging（系统调试）

**触发时机**: 任何技术问题（测试失败、Bug、异常行为、性能问题）

**铁律**:
```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

**四阶段流程**:

#### Phase 1: 根因调查（必须完成才能进入下一步）

1. **仔细阅读错误信息** - 不要跳过，通常包含解决方案
2. **稳定复现** - 能可靠触发吗？具体步骤？
3. **检查近期变更** - Git diff、新依赖、配置变化
4. **多组件系统收集证据** - 在每个边界添加日志
5. **追踪数据流** - 坏值从哪来？层层向上直到源头

#### Phase 2: 模式分析

1. **找工作的例子** - 类似功能中什么是正常的？
2. **对比参考实现** - 完整阅读，不跳过
3. **识别差异** - 列出所有不同，无论多小
4. **理解依赖** - 需要哪些组件、配置、环境？

#### Phase 3: 假设与验证

1. **形成单一假设** - "我认为X是根因，因为Y"
2. **最小化测试** - 最小改动验证假设
3. **验证后继续** - 有效→Phase 4，无效→新假设

#### Phase 4: 实施修复

1. **创建失败测试** - 最简单的复现
2. **单一修复** - 针对根因，一次一改
3. **验证修复** - 测试通过？没破坏其他？
4. **修复无效？**
   - < 3次失败 → 返回 Phase 1 重新分析
   - **≥ 3次失败 → 质疑架构**（不是继续修）

**红线（STOP）**:
- "先快速修复，以后再调查"
- "试试改X看有没有用"
- "一次改多处，跑测试"
- "大概知道问题，我来修"
- 3次以上失败仍继续尝试修复

---

## OpenClaw 适配建议

### 可直接借鉴的

| 技能 | 适配方式 |
|------|----------|
| **Brainstorming** | 复杂任务前先澄清需求，一次一问，分段确认 |
| **Writing Plans** | 多步骤任务前写计划，每个子任务 2-5 分钟粒度 |
| **TDD** | 写代码前写测试/验证步骤，RED-GREEN-验证 |
| **Systematic Debugging** | 遇问题先调查根因，禁止"试试这个" |

### 需调整的

| 原技能 | 调整原因 | OpenClaw 替代 |
|--------|----------|---------------|
| **Subagent-Driven** | OpenClaw 用 `sessions_spawn` 而非内置子代理 | 主会话控制 + `sessions_spawn` 执行子任务 |
| **Git Worktrees** | 技能本身有用，但实现方式不同 | 使用 `cwd` 参数隔离工作目录 |
| **Code Reviewer 子代理** | 架构差异 | 主代理审查 + `subagents list` 监控 |

### 核心工作流（OpenClaw 版）

```
主人提出复杂任务
    ↓
Brainstorming: 澄清需求、探索方案、确认设计
    ↓
Writing Plans: 制定详细计划（任务粒度 2-5 分钟）
    ↓
主人确认计划
    ↓
执行方式选择:
  ├─ 方式A: 当前会话执行（适合短任务）
  │   └─ 逐任务执行 + TDD + 自审
  └─ 方式B: 子代理执行（适合长任务）
      └─ sessions_spawn 子代理
      └─ 主代理审查结果
    ↓
完成后验证 + 总结
```

---

## 关键原则总结

1. **先理解再动手** - Brainstorming 不是浪费时间
2. **计划先于编码** - 写代码前写计划，假设执行者零上下文
3. **小步快跑** - 每个任务 2-5 分钟，频繁验证
4. **测试先行** - RED-GREEN-验证，不看测试失败不算数
5. **系统调试** - 先找根因再修复，禁止猜测
6. **YAGNI** - 无情砍掉不需要的功能
7. **DRY** - 不重复自己

---

## 参考资源

- **原项目**: https://github.com/LUNARTECH-X/superpowers
- **技能目录**: `skills/` 下 40+ 个详细技能
- **核心技能**:
  - `brainstorming/SKILL.md` - 需求澄清
  - `writing-plans/SKILL.md` - 计划制定
  - `subagent-driven-development/SKILL.md` - 子代理开发
  - `test-driven-development/SKILL.md` - 测试驱动
  - `systematic-debugging/SKILL.md` - 系统调试

---

*萃取时间: 2026-03-16*  
*萃取者: Claude (OpenClaw)*
