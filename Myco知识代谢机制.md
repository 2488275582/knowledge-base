# Myco 知识代谢机制：默会维度

> *"We can know more than we can tell."*
> — Michael Polanyi, *The Tacit Dimension* (1966)

---

## 核心隐喻：从默会到明述

波兰尼将知识分为两个维度——**默会知识**（tacit knowing）与**明述知识**（explicit knowing）。我们骑自行车时知道如何保持平衡，却无法用公式讲清楚；外科医生的手感、棋手的直觉、程序员读代码时"闻到"的 bad smell——这些都是默会知识。

波兰尼的关键洞见是 **from-to 结构**：

- **近端项**（proximal term）：我们**从**它出发，辅助性地觉知它，却无法直接注视它
- **远端项**（distal term）：我们**朝向**它聚焦，它是注意力的焦点

> 当你用锤子钉钉子时，你**从**手掌的触感**朝向**钉头聚焦。
> 你并不注意手掌——但如果戴上手套，你就"失去了手感"。

Myco 做的事情，正是将散落在代码库中的**默会知识**（近端项），经由消化代谢，转化为可以被 Claude 聚焦阅读的**明述知识**（远端项）。

---

## 知识流转全景

```mermaid
graph TB
    subgraph TACIT ["<b>默会维度</b><br/><i>Tacit Dimension</i>"]
        direction TB
        CODE["<b>代码库</b><br/>gaia-product / wms / fulfillment<br/><br/><i>寓居其中的知识：</i><br/>架构意图 · 设计取舍<br/>命名惯例 · 异常处理模式<br/>跨服务耦合 · 历史包袱"]
        DEV["<b>开发者心智模型</b><br/><br/><i>只可意会的部分：</i><br/>为什么选 EasyRules 而非 Drools<br/>为什么 SLA 用有向图<br/>为什么 Repository 包一层 Mapper"]
    end

    subgraph METABOLISM ["<b>代谢引擎</b><br/><i>Myco Digestive System</i>"]
        direction TB
        EAT["<b>myco_eat</b><br/>即时捕获<br/><i>近端项 → 原子笔记</i>"]
        DIGEST["<b>myco_digest</b><br/>消化提炼<br/><i>raw → extracted</i>"]
        ABSORB["<b>myco_absorb</b><br/>吸收整合<br/><i>extracted → wiki</i>"]
        IMMUNE["<b>myco_immune</b><br/>免疫校验<br/><i>一致性 · 漂移检测</i>"]
    end

    subgraph EXPLICIT ["<b>明述维度</b><br/><i>Explicit Dimension</i>"]
        direction TB
        WIKI["<b>Wiki 知识页</b><br/>wiki/*.md<br/><br/>architecture · gaia-product<br/>gaia-wms · gaia-fulfillment<br/>build-deploy · data-layer"]
        CLAUDE_MD["<b>CLAUDE.md</b><br/>索引入口<br/><br/>热区 · 任务队列<br/>Wiki 索引 · 文档索引"]
        DOCS["<b>辩论记录</b><br/>docs/primordia/*_craft_*.md<br/><br/>架构决策的显式推理过程"]
    end

    subgraph INDWELLING ["<b>寓居</b><br/><i>Indwelling — Claude 的内化</i>"]
        direction TB
        BOOT["<b>会话启动</b><br/>自动加载 CLAUDE.md<br/><i>建立焦点意识</i>"]
        READ["<b>按需深读</b><br/>Read wiki/*.md<br/><i>辅助觉知 → 焦点觉知</i>"]
        WORK["<b>编码工作</b><br/>利用内化知识<br/><i>知识再次成为默会</i>"]
    end

    CODE -->|"观察 · 提取"| EAT
    DEV -->|"决策记录"| EAT
    EAT --> DIGEST
    DIGEST --> ABSORB
    ABSORB --> WIKI
    ABSORB --> CLAUDE_MD
    IMMUNE -.->|"漂移修复"| WIKI
    WIKI --> BOOT
    CLAUDE_MD --> BOOT
    DOCS --> READ
    WIKI --> READ
    BOOT --> READ
    READ --> WORK
    WORK -->|"新发现 · 新决策"| EAT

    style TACIT fill:#1a1a2e,stroke:#e94560,color:#eee,stroke-width:2px
    style METABOLISM fill:#16213e,stroke:#0f3460,color:#eee,stroke-width:2px
    style EXPLICIT fill:#0f3460,stroke:#53a8b6,color:#eee,stroke-width:2px
    style INDWELLING fill:#1b262c,stroke:#bbe1fa,color:#eee,stroke-width:2px
```

---

## 波兰尼四层映射

```mermaid
graph LR
    subgraph POLANYI ["波兰尼知识论"]
        P1["<b>辅助觉知</b><br/><i>Subsidiary Awareness</i><br/>从它出发，不直接注视"]
        P2["<b>焦点觉知</b><br/><i>Focal Awareness</i><br/>注意力聚焦之处"]
        P3["<b>寓居</b><br/><i>Indwelling</i><br/>工具成为身体延伸"]
        P4["<b>涌现</b><br/><i>Emergence</i><br/>整体大于部分之和"]
    end

    subgraph MYCO ["Myco 知识系统"]
        M1["<b>L3 代码层</b><br/>src/ 实际代码<br/><i>数千个文件中的隐含模式</i>"]
        M2["<b>L1.5 Wiki 层</b><br/>wiki/*.md<br/><i>提炼后的可读知识</i>"]
        M3["<b>L1 索引层</b><br/>CLAUDE.md<br/><i>内化为工作记忆</i>"]
        M4["<b>工作产出</b><br/>代码修改 · 架构决策<br/><i>知识转化为行动</i>"]
    end

    P1 --- M1
    P2 --- M2
    P3 --- M3
    P4 --- M4

    style POLANYI fill:#2d132c,stroke:#ee4540,color:#eee,stroke-width:2px
    style MYCO fill:#0d1137,stroke:#e8d21d,color:#eee,stroke-width:2px
```

---

## 单次会话的知识生命周期

```mermaid
sequenceDiagram
    participant U as 开发者
    participant C as Claude Agent
    participant M as Myco 代谢引擎
    participant K as 知识基底<br/>(wiki + CLAUDE.md)

    Note over C,K: ── 会话启动：寓居阶段 ──

    C->>M: myco_pulse (含 hunger)
    M-->>C: 健康信号 + 反射指令
    C->>K: 自动加载 CLAUDE.md
    K-->>C: 索引 · 热区 · 任务队列
    Note right of C: 建立焦点觉知<br/>知道"有什么"

    Note over U,K: ── 工作阶段：from-to 结构 ──

    U->>C: 提出任务
    C->>K: 按需读取 wiki 页面
    Note right of C: 辅助觉知(wiki) →<br/>焦点觉知(当前任务)
    K-->>C: 结构化领域知识
    C->>C: 读代码 · 编码 · 决策

    opt 遇到关键决策
        C->>M: myco_eat (即时捕获)
        Note right of M: 默会知识 → 原子笔记<br/>近端项被显式化
    end

    opt 发现自己说错
        C->>M: myco_eat + on-self-correction
        Note right of M: 摩擦必捕<br/>错误也是知识
    end

    Note over C,K: ── 会话结束：代谢反思 ──

    C->>M: myco_hunger
    M-->>C: session_end_drift advisory
    C->>M: myco_reflect (执行学习)
    Note right of M: 涌现：<br/>工作经验沉淀为<br/>可复用的模式知识
    C->>K: 更新任务队列 · 追加 log.md
```

---

## 代谢流水线：原子笔记的一生

```mermaid
stateDiagram-v2
    [*] --> raw : myco_eat<br/>即时捕获

    state "默会 → 明述 转化" as transform {
        raw --> digesting : myco_digest<br/>开始消化
        digesting --> extracted : 提取结构<br/>tag · summary · links
        extracted --> integrated : myco_absorb<br/>写入 wiki 页面
    }

    integrated --> [*] : 知识可被检索

    state "免疫系统" as immune {
        integrated --> drift_detected : myco_immune<br/>发现漂移
        drift_detected --> integrated : 修复一致性
    }

    state "代谢废物" as waste {
        raw --> excreted : 低价值 / 过时
        digesting --> excreted : 消化失败
        excreted --> [*] : 排出系统
    }

    note left of raw
        波兰尼的"近端项"
        ──────────────
        代码中的观察
        开发者的口述
        错误的修正
        模糊的直觉
    end note

    note right of integrated
        波兰尼的"远端项"
        ──────────────
        结构化的 wiki
        可检索的知识
        可共享的理解
    end note
```

---

## Claude 如何"寓居"于知识

波兰尼的 **indwelling**（寓居）概念是理解 Claude 与 Myco 关系的关键。

盲人用拐杖探路时，拐杖的触感不再是"手掌上的压力"，而变成了"前方地面的形状"——拐杖成为身体的延伸。同样地：

```mermaid
graph TD
    subgraph BEFORE ["<b>无 Myco 时</b>"]
        C1["Claude"] -->|"每次都要"| R1["从头读代码"]
        R1 -->|"逐文件拼凑"| U1["理解架构"]
        U1 -->|"费力且不完整"| W1["开始工作"]
    end

    subgraph AFTER ["<b>有 Myco 后</b>"]
        C2["Claude"] -->|"自动加载"| I2["CLAUDE.md 索引"]
        I2 -->|"已内化"| F2["焦点直达任务"]
        F2 -->|"按需深读 wiki"| W2["精准工作"]
        W2 -->|"即时沉淀"| I2
    end

    style BEFORE fill:#3d0000,stroke:#ff6b6b,color:#eee,stroke-width:2px
    style AFTER fill:#003d00,stroke:#6bff6b,color:#eee,stroke-width:2px
```

| 无 Myco | 有 Myco |
|---------|---------|
| 每次会话从零开始 | 知识在会话间持续积累 |
| 代码即文档（但代码不会自我解释） | 结构化 wiki 提供"为什么" |
| 默会知识困在开发者脑中 | 默会知识被外化、可传递 |
| Claude 是无记忆的工具 | Claude **寓居**于知识基底之中 |

---

## 知识加载层级

```mermaid
graph TD
    subgraph LAYERS ["知识金字塔"]
        L1["<b>L1 · CLAUDE.md</b><br/>━━━━━━━━━━━━━━<br/>自动加载 · 每次会话<br/><br/>项目摘要 · Wiki 索引<br/>任务队列 · 热区<br/><br/><i>≈ 波兰尼的「焦点」</i>"]

        L15["<b>L1.5 · Wiki</b><br/>━━━━━━━━━━━━━━<br/>按需读取 · wiki/*.md<br/><br/>架构 · 各服务详情<br/>构建部署 · 数据层<br/><br/><i>≈ 波兰尼的「工具延伸」</i>"]

        L2["<b>L2 · Docs</b><br/>━━━━━━━━━━━━━━<br/>按需读取 · docs/*.md<br/><br/>工作流手册 · 辩论记录<br/>Craft Protocol 产物<br/><br/><i>≈ 波兰尼的「推理链」</i>"]

        L3["<b>L3 · Code</b><br/>━━━━━━━━━━━━━━<br/>按需读取 · src/**<br/><br/>Java 源码 · 配置文件<br/>SQL · 测试<br/><br/><i>≈ 波兰尼的「近端项」</i>"]

        L1 --> L15
        L15 --> L2
        L2 --> L3
    end

    style L1 fill:#1b4332,stroke:#95d5b2,color:#eee,stroke-width:3px
    style L15 fill:#2d6a4f,stroke:#95d5b2,color:#eee,stroke-width:2px
    style L2 fill:#40916c,stroke:#95d5b2,color:#eee,stroke-width:1px
    style L3 fill:#52b788,stroke:#95d5b2,color:#1a1a1a,stroke-width:1px
```

越往上，知识越**显式、浓缩、可直接使用**；
越往下，知识越**隐含、丰富、需要解读**。

Myco 的代谢方向始终是 **L3 → L1**：从代码的默会知识中提炼出可传递的明述知识。

---

## 附：波兰尼核心概念速查

| 概念 | 原文 | Myco 对应 |
|------|------|-----------|
| 默会知识 | Tacit Knowing | 代码库中的架构意图、设计取舍、命名惯例 |
| 明述知识 | Explicit Knowing | Wiki 页面、CLAUDE.md、辩论记录 |
| 辅助觉知 | Subsidiary Awareness | 背景性的代码理解（不直接注视但依赖） |
| 焦点觉知 | Focal Awareness | 当前任务聚焦的具体问题 |
| from-to 结构 | From-To Structure | 从代码(近端) → 到 wiki(远端) → 到工作产出 |
| 寓居 | Indwelling | Claude 将 CLAUDE.md 内化为工作记忆 |
| 涌现 | Emergence | 结构化知识使 Agent 能力超越单次代码阅读 |
| 个人知识 | Personal Knowledge | 开发者独有的上下文，通过 myco_eat 外化 |

---

> *工具最好的状态，是你忘了它的存在。*
> *Myco 最好的状态，是 Claude 不再需要"从头读代码"。*
> *这就是波兰尼所说的寓居——知识成为了身体的延伸。*
