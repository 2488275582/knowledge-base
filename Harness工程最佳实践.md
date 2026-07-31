# Harness Engineering 最佳实践指南

> **Harness Engineering**（线束工程）是 2026 年初兴起的新兴工程学科。  
> 核心理念：**Agent = Model + Harness**  
> 软件工程师的角色从"写代码"转变为"设计让 AI Agent 可靠写代码的环境"。

---

## 一、什么是 Harness Engineering

### 1.1 起源

2026 年 2 月，OpenAI 的 Ryan Lopopolo 发表了一篇关于 Codex agent 开发实践的文章。他的团队用 Codex agent 在 5 个月内生成了约 100 万行代码的生产级产品，**人类没有直接写任何一行代码**。

> Humans steer. Agents execute.（人类引导，Agent 执行。）

### 1.2 "Harness" 的含义

借用马术比喻——马（AI 模型）跑得快但不知道去哪里，**缰绳（Harness）是你用来引导它的一切**：

- 约束规则
- 反馈循环
- 文档结构
- Lint 规则
- 可观测性管道
- 生命周期管理系统

### 1.3 稀缺性反转

| 传统开发 | Agent 优先开发 |
|----------|----------------|
| 计算资源便宜 | 代码生产力过剩 |
| 人类注意力适度稀缺 | **人类时间和注意力成为最稀缺资源** |
| 写代码是瓶颈 | **等待是昂贵的，修正是廉价的** |

---

## 二、核心概念框架

### 2.1 前馈 + 反馈控制系统（Martin Fowler 框架）

| 控制类型 | 方向 | 说明 |
|----------|------|------|
| **Guides（前馈）** | 行动前 | 预期 Agent 行为并提前引导（AGENTS.md、Skills） |
| **Sensors（反馈）** | 行动后 | 观察 Agent 输出并帮助自我纠正（Linter、测试、AI 审查） |

每种又分为两类执行方式：

| 执行类型 | 特点 | 示例 |
|----------|------|------|
| **计算型** | 确定性、快速、CPU 执行 | 测试、Linter、类型检查、结构分析 |
| **推理型** | 语义分析、非确定性、GPU/NPU 执行 | AI 代码审查、LLM-as-judge |

### 2.2 控制组合矩阵

| 方向 | 计算型 / 推理型 | 示例实现 |
|------|-----------------|----------|
| 编码规范（前馈） | 推理型 | AGENTS.md / CLAUDE.md / Skills |
| 新项目初始化指引（前馈） | 两者结合 | Skill + 引导脚本 |
| Code mods（前馈） | 计算型 | OpenRewrite recipes |
| 结构测试（反馈） | 计算型 | ArchUnit 检查模块边界 |
| 审查指引（反馈） | 推理型 | Skills |

---

## 三、仓库知识即真相源

### 3.1 AGENTS.md / CLAUDE.md 作为入口

核心原则：**做目录，不做百科全书**。保持 ~100 行。

```markdown
# CLAUDE.md

## 构建与测试
pnpm install && pnpm build && pnpm test

## 架构
见 docs/ARCHITECTURE.md

## 编码规范
见 docs/CONVENTIONS.md

## 领域知识
见 docs/domain/（每个业务模块一个文件）
```

巨型说明书失败的原因：

- **上下文挤占**：大型指令文件挤掉了任务、代码和相关文档的空间
- **过度指导等于无指导**：当一切都"重要"时，Agent 开始局部模式匹配
- **即时腐烂**：单体手册无法被机械化验证，变成过时规则的坟场
- **不可检测的漂移**：没有结构化验证就无法发现偏差

### 3.2 推荐的文档结构

```
AGENTS.md / CLAUDE.md         ← 入口目录（~100行）
ARCHITECTURE.md                ← 顶层架构图
docs/
├── design-docs/               ← 索引化的架构决策
│   ├── index.md
│   ├── core-beliefs.md
│   └── ...
├── exec-plans/                ← 执行计划
│   ├── active/                ← 进行中的计划
│   ├── completed/             ← 已完成的计划
│   └── tech-debt-tracker.md   ← 技术债追踪
├── generated/                 ← 自动生成的文档
│   └── db-schema.md
├── product-specs/             ← 产品规格
│   ├── index.md
│   └── ...
├── references/                ← 外部库文档（LLM友好格式）
├── domain/                    ← 领域知识
│   ├── fulfillment.md
│   ├── wms.md
│   └── aftersale.md
├── CONVENTIONS.md             ← 编码规范
├── DESIGN.md                  ← 设计指南
├── QUALITY_SCORE.md           ← 质量评分
├── RELIABILITY.md             ← 可靠性要求
└── SECURITY.md                ← 安全要求
```

### 3.3 渐进式披露

Agent 从小而稳定的入口开始，按需深入查找，而不是一次性被大量信息淹没。

---

## 四、架构约束机械化执行

### 4.1 核心原则

> **文档化还不够，必须用 Linter/CI 机械化执行。**

Agent 会忠实复制代码库中的模式——包括坏模式。如果代码库有架构漂移问题，Agent 会忠实地复制并放大这种漂移。

### 4.2 分层依赖规则示例

```
每个业务领域内的依赖方向（单向）：

Types → Config → Repo → Service → Runtime → UI

横切关注点（auth、telemetry、feature flags）
只能通过 Providers 接口进入。
其他依赖路径在 CI 中被禁止。
```

### 4.3 Java ArchUnit 结构测试

```java
@ArchTest
static final ArchRule service_should_not_depend_on_controller =
    noClasses().that().resideInAPackage("..service..")
        .should().dependOnClassesThat().resideInAPackage("..controller..");

@ArchTest
static final ArchRule repo_should_not_depend_on_service =
    noClasses().that().resideInAPackage("..repo..")
        .should().dependOnClassesThat().resideInAPackage("..service..");
```

### 4.4 机械化执行清单

- [ ] 架构边界用 Linter/结构测试强制执行
- [ ] 格式化规则自动应用
- [ ] 数据验证在外部边界强制执行
- [ ] 依赖规则有 CI 检查
- [ ] 命名规范有 lint 规则
- [ ] 文件大小限制有检查

---

## 五、让应用对 Agent 可读

### 5.1 技术手段

| 技术 | 目的 |
|------|------|
| **Per-worktree 启动** | 每个任务一个隔离的应用实例，防止跨任务污染 |
| **Chrome DevTools Protocol 集成** | Agent 可以截图、操作 DOM、复现 Bug |
| **临时可观测性栈** | 每个 worktree 有独立的日志/指标/追踪，任务完成后销毁 |
| **结构化日志 + 查询** | Agent 可用 LogQL/PromQL 直接查询运行时数据 |

### 5.2 可解锁的高级能力

当应用对 Agent 可读后，以下指令变得可行：

- "确保服务启动在 800ms 内完成"
- "这四个关键用户旅程中没有 span 超过 2 秒"
- "复现这个 bug 并录制视频证明修复效果"

---

## 六、反馈传感器（Hooks）配置

### 6.1 PostToolUse Hooks（写/编辑后自动触发）

```jsonc
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "command": "pnpm prettier --write \"$FILE_PATH\"",
        "description": "格式化编辑过的文件"
      },
      {
        "matcher": "Write|Edit",
        "command": "pnpm eslint --fix \"$FILE_PATH\"",
        "description": "Lint 检查编辑过的文件"
      },
      {
        "matcher": "Write|Edit",
        "command": "pnpm tsc --noEmit --pretty false",
        "description": "类型检查"
      }
    ]
  }
}
```

### 6.2 PreToolUse Hooks（写入前拦截）

```jsonc
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "command": "检查文件行数是否超过 800 行，超过则阻止写入",
        "description": "阻止过大文件写入"
      }
    ]
  }
}
```

### 6.3 Stop Hooks（会话结束时验证）

```jsonc
{
  "hooks": {
    "Stop": [
      {
        "command": "pnpm build",
        "description": "会话结束时验证构建"
      }
    ]
  }
}
```

### 6.4 执行顺序

1. 格式化 → 2. Lint → 3. 类型检查 → 4. 构建验证

---

## 七、"垃圾回收"对抗 AI Slop

### 7.1 问题

Agent 会忠实复制代码库中已有的模式，包括不良模式。随时间推移，这导致漂移和不一致——即 "AI slop"。

OpenAI 团队曾每周五花 **20% 时间**手动清理。这不可持续。

### 7.2 解决方案：Golden Principles（黄金原则）

编码进仓库的机械化规则：

| 原则 | 说明 |
|------|------|
| 共享工具包优于手写 helper | 保持不变量集中化 |
| 在边界验证数据 | 不要 YOLO 式探测数据形状 |
| 使用团队内部的并发工具 | 而非引入行为不透明的第三方库 |

### 7.3 自动化清理流程

1. 后台 Agent 定期扫描代码库偏差
2. 更新质量评分（QUALITY_SCORE.md）
3. 开针对性重构 PR
4. 大多数 PR 可在 1 分钟内审查并自动合并

> 技术债像高利贷：持续小额偿还，远好于让它积累后痛苦地集中还债。

---

## 八、需求开发实战流程

### 8.1 前提：Harness 已搭建

确保已完成第三~六节的基础建设。

### 8.2 Step 1 — 写需求计划（前馈控制）

```markdown
# docs/plans/active/batch-inspection.md

## 需求背景
仓储场景下需要支持一次质检多个SKU，减少操作次数

## 验收标准
- [ ] 支持选择多个待检商品
- [ ] 每个商品独立记录质检结果
- [ ] 质检完成后自动更新库存状态
- [ ] 异常商品进入售后流程

## 架构约束
- 复用现有 InspectionService，不新建 Service
- 数据库操作必须在事务内
- 批量操作上限 100 条

## 非目标
- 不改变单个质检的现有逻辑
- 不涉及 UI 改动（本期仅 API）
```

### 8.3 Step 2 — 给 Agent 提示

```
基于 docs/plans/active/batch-inspection.md 的计划，
在 InspectionService 中实现批量质检功能。

约束：
- 先写测试再写实现
- 遵循现有代码模式
- 事务边界在 Service 层
```

### 8.4 Step 3 — 反馈传感器自动运行

```
Agent 编码完成
  ↓
PostToolUse Hook → lint + type check（计算型反馈）
  ↓
运行测试 → pnpm test / mvn test（计算型反馈）
  ↓
ArchUnit 结构测试 → 检查依赖方向（计算型反馈）
  ↓
Agent 自我审查 → 对比计划中的验收标准（推理型反馈）
```

### 8.5 Step 4 — 人类只做高价值判断

你只需要审查：

- 业务逻辑是否正确（Agent 不懂你的业务细微差别）
- 边界 case 是否覆盖
- 性能是否满足要求

### 8.6 Step 5 — 转动飞轮

每次发现 Agent 犯了重复错误，不要手动修——**改 harness**：

| Agent 犯的错 | Harness 修复方式 |
|---|---|
| 总是忘记加事务注解 | 加 ArchUnit 测试：Service 方法必须有 `@Transactional` |
| 生成的代码风格不统一 | 更新 CONVENTIONS.md + 加 lint 规则 |
| 不了解领域概念 | 补充 `docs/domain/` 文档 |
| 引入不该用的依赖 | 加 CI 检查：dependency allowlist |
| 同样的工具类重复造轮子 | 在 CLAUDE.md 中指向已有 utils |

---

## 九、最小可行 Harness 清单

- [ ] 小型 `AGENTS.md` / `CLAUDE.md` 入口指向更深文档
- [ ] 可复现的开发环境（一条命令启动）+ per-worktree 隔离
- [ ] CI 中的机械化不变量（架构边界、格式化、数据验证、依赖规则）
- [ ] Agent 可读性钩子（结构化日志 + 可查询的追踪/指标）
- [ ] 清晰的评估门槛（"完成"标准、回归测试、安全检查）
- [ ] 安全护栏（最小权限凭证、受控出站、审计日志、回滚预案）

---

## 十、度量体系

| 维度 | 指标 |
|------|------|
| **吞吐量** | PR 首次创建时间、合并时间、每天完成任务数、平均迭代次数 |
| **质量** | CI 通过率、缺陷逃逸率、回滚频率、回归检测平均时间 |
| **人类注意力** | 每 PR 审查分钟数、升级次数、需要人类判断的任务占比 |
| **Harness 健康度** | 文档新鲜度违规、架构边界违规、测试 Flake 率、工具/运行时错误率 |
| **安全** | 出站拦截次数、权限拒绝次数、密钥扫描命中、需审批的依赖变更 |

> 关键：让这些指标对 Agent 也可见（通过工具和仪表盘），而不只是人类能看到。

---

## 十一、速查流程图

```
需求来了
  │
  ├─ 1. 写计划文件 → docs/plans/active/xxx.md（前馈）
  │     定义验收标准、架构约束、非目标
  │
  ├─ 2. 给 Agent 提示 → 引用计划文件 + 约束
  │     "先写测试、遵循现有模式、读计划文件"
  │
  ├─ 3. Agent 编码 → Harness 自动检查
  │     hooks（lint/type）→ 测试 → 结构测试
  │
  ├─ 4. 人类审查 → 只看业务逻辑和边界
  │     不看格式、不看架构合规（机器已检查）
  │
  └─ 5. 回顾 → Agent 哪里做错了？
        更新 docs/ 或加检查规则 → harness 变强
```

---

## 十二、核心公式

> **每个重复的人工修正 → 编码为 Guide 或 Sensor → Agent 下次自动做对**

> 把你脑子里的知识写进仓库，把你反复纠正的事情编码为规则，让机器替你看守。
> 你的人类注意力只花在真正需要判断力的地方。

---

## 参考来源

- [OpenAI - Harness Engineering](https://openai.com/index/harness-engineering/) — Ryan Lopopolo, 2026-02-11
- [Martin Fowler - Harness Engineering for Coding Agent Users](https://martinfowler.com/articles/harness-engineering.html) — Birgitta Böckeler, 2026-04-02
- [GTCode - Harness Engineering Deep Dive](https://gtcode.com/articles/harness-engineering/) — Ekewaka Lono, 2026-03-04
- [Harness Engineering AI - Agent Harness Complete Guide](https://harness-engineering.ai/blog/agent-harness-complete-guide/) — Dr. Sarah Chen, 2026-03-05
- [AGENTS.md Standard](https://agents.md/) — 社区标准
