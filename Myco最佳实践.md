# Myco 最佳实践指南

## 核心概念：生物循环的知识管理

Myco 将知识管理类比为一个生物系统：
- **入摄** (eat) = 摄取营养
- **整合** (assimilate) = 消化吸收
- **蒸馏** (sporulate) = 繁殖遗传
- **免疫** (immune) = 体质检查
- **新陈代谢** = raw → integrated → doctrine

---

## 🔄 第一章：每日工作节奏（四个必做步骤）

```
会话开始 → 工作中摄入 → 查证已知 → 会话结束整理
```

### 步骤 1：会话开始（R1 规则）

```bash
myco hunger --execute
```

| 作用 | 详情 |
|------|------|
| **启动仪式** | 必须第一步执行 |
| **系统检查** | 告诉你基底缺什么 |
| 生成简报内容 | 有多少 raw 笔记等待消化；canon 缺失字段；上次免疫遗留问题 |

### 步骤 2：工作过程中 - 记录洞察（R4 规则）

| 命令 | 用途 | 示例 |
|------|------|------|
| `myco eat --content "..."` | 记录文字洞察 | `myco eat --content "calculated_price 溢出根因是 double scaling" --tags v4.13,bug,price` |
| `myco eat --path <文件>` | 吞入单个文件 | `myco eat --path ./PRD_V1.1.md --tags v4.12,prd` |
| `myco eat --path <目录>` | 批量吞入目录 | `myco eat --path ./project-docs/604/` |
| `myco eat --url <URL>` | 吞入网页内容 | 需要安装 `myco[adapters]` |
| `myco forage <路径>` | 预览可吞入文件 | `myco forage ./gaia-product/service/` |

**⚠️ 黄金法则（R4）**：决策产生的那一秒就 eat，不要攒到会话结束。丢失一条洞察的代价远大于多记一条废话的成本。

### 步骤 3：工作过程中 - 查证已知（R3 规则）

| 命令 | 用途 | 示例 |
|------|------|------|
| `myco sense "关键词"` | 搜索基底中的已有知识 | `myco sense "价格计算"` |

**⚠️ 黄金法则（R3）**：要引用基底中的事实（版本号、公式、决策）之前，先 sense 确认，不要凭记忆。

### 步骤 4：会话结束（R2 规则）

```bash
myco senesce
```

| 作用 | 详情 |
|------|------|
| **收尾仪式** | 自动执行 assimilate + immune --fix |
| 提升 raw 笔记 | 从具体碎片 → 结构化笔记 |
| 修复可修的问题 | 自动修复冗余、悬空引用等 |

---

## 📚 第二章：17 个命令分场景速查

### 场景 A：初始化（仅一次）

| 命令 | 用途 | 示例 |
|------|------|------|
| `myco germinate` | 在项目中创建基底 | `myco germinate . --substrate-id gaia` |

### 场景 B：每次会话开头 ✅

| 命令 | 用途 | 说明 |
|------|------|------|
| `myco hunger` | 检查缺什么 + 生成启动简报 | 加 `--execute` 写入；不加则只预览 |

### 场景 C：工作过程中 ✅ （核心高频）

| 命令 | 用途 | 示例 |
|------|------|------|
| `myco eat --content` | 记录洞察 | 见上面步骤 2 |
| `myco eat --path` | 吞入文件/目录 | 见上面步骤 2 |
| `myco sense` | 查证已知 | 见上面步骤 3 |
| `myco forage` | 预览可吞文件 | 见上面步骤 2 |

### 场景 D：定期整理 - 知识提升

| 命令 | 用途 | 频率 |
|------|------|------|
| `myco assimilate` | 批量提升 raw → integrated | 每次会话结束 |
| `myco digest <note-id>` | 单条笔记精准提升 | 需要特别处理时 |
| `myco sporulate <slug>` | 多条笔记蒸馏为教义 | 同一主题 3+ 条笔记时 |

**知识管道流向**：
```
eat 吞入 → raw/
     ↓
assimilate 整合 → integrated/
     ↓
sporulate 蒸馏 → docs/doctrine/

具体碎片 → 结构化笔记 → 浓缩教义
```

**实例**：gaia 价格相关的 bug 记了 5 条笔记后，执行蒸馏：
```bash
myco sporulate "gaia-price-system-rules"
```
Agent 会从 integrated 笔记中提炼出一份教义文档。

### 场景 E：健康检查

| 命令 | 用途 | 说明 |
|------|------|------|
| `myco immune` | 运行 10 维 lint 检查 | 查出问题 |
| `myco immune --fix` | 自动修复可修复的问题 | 解决问题 |
| `myco immune --list` | 列出所有检查维度 | 查看有哪些检查项 |
| `myco immune --explain <ID>` | 解释某个维度 | 如 `myco immune --explain SE2` |
| `myco traverse` | 检查知识图谱连通性 | `myco traverse --scope notes` |

#### 10 个免疫维度速查

| ID | 检查什么 | 严重度 |
|----|---------|--------|
| **M1** | _canon.yaml 身份字段完整性 | 🔴 CRITICAL |
| **M2** | 入口文件 MYCO.md 是否存在 | 🔴 CRITICAL |
| **SH1** | 包版本引用是否一致 | 🔴 CRITICAL |
| **M3** | 写入面声明是否完整 | 🟠 HIGH |
| **MF1** | 声明的子系统目录是否真的存在 | 🟠 HIGH |
| **MF2** | 本地插件是否健康 | 🟠 HIGH |
| **SE1** | 悬空引用（指向不存在的文件） | 🟠 HIGH |
| **MB1** | raw 笔记积压量（代谢信号） | 🟡 MEDIUM |
| **MB2** | integrated 层是否为空（代谢信号） | 🟡 MEDIUM |
| **SE2** | 孤儿笔记（没有任何节点引用） | 🟡 MEDIUM |

### 场景 F：会话结束 ✅

| 命令 | 用途 |
|------|------|
| `myco senesce` | 一键收尾：assimilate + immune --fix |

### 场景 G：跨项目/进化（低频）

| 命令 | 用途 | 示例 |
|------|------|------|
| `myco propagate --dst <路径>` | 推送知识到另一个基底 | `myco propagate --dst ~/substrates/gaia-wms` |
| `myco fruit <主题>` | 创建改进提案文档 | `myco fruit "add-version-awareness" --kind design` |
| `myco molt <版本>` | 基底合约版本升级 | `myco molt v0.6.0` |
| `myco winnow <提案>` | 验证提案符合协议 | `myco winnow docs/primordia/xxx_craft.md` |
| `myco ramify` | 脚手架：新建动词/维度/适配器 | `myco ramify --dimension VER1 --category semantic` |
| `myco graft --list` | 列出本地插件 | 查看有哪些自定义扩展 |

---

## 🎯 第三章：实战示例（gaia v4.13 需求）

### 完整会话流程

```bash
# 1️⃣ 开始会话
myco hunger --execute

# 2️⃣ 吞入 PRD 需求文档
myco eat --path ~/Documents/卡谷电商/PRD/市场参考价和系数修改功能PRD_V4.13.md \
  --tags v4.13,prd,price,coefficient

# 3️⃣ 开发过程中发现 calculated_price 溢出 bug
myco eat --content "ProductPriceProcessService:109 对采购价做了双重×10000缩放，\
PurchasePriceInfoDTO 传入的值已经是×10000格式，导致 calculated_price 溢出 BIGINT UNSIGNED" \
  --tags v4.13,bug,price,calculated-price

# 4️⃣ 确认修复方案前，先查下是否有类似问题
myco sense "价格溢出"
myco sense "calculated_price"

# 5️⃣ 记录修复决策
myco eat --content "修复方案：ProductPriceProcessService:109 不再乘10000，\
因为 PurchasePriceInfoDTO.newPurchasePrice 已经是实际值×10000格式" \
  --tags v4.13,fix,price

# 6️⃣ 吞入关键代码文件作为上下文
myco eat --path gaia-product/service/src/main/java/com/caguuu/product/app/service/price/PriceCalculator.java \
  --tags price,core-logic

# 7️⃣ 会话结束
myco senesce
```

### 知识蒸馏示例

当 price 相关的 integrated 笔记积累到 5+ 条时：

```bash
myco sporulate "gaia-price-system"
```

Agent 会生成一份教义文档，包含：
- ✓ 价格存储规范（Long, ×10000）
- ✓ 计算公式
- ✓ 已知坑（双重缩放、UNSIGNED 限制）
- ✓ 版本演进（v4.12 → v4.13 的变更）

---

## ⚡ 第四章：7 条硬规则速记

| 规则 | 一句话 | 对应命令 |
|------|--------|---------|
| **R1** | 会话开始必须 hunger | `myco hunger --execute` |
| **R2** | 会话结束必须 senesce | `myco senesce` |
| **R3** | 断言前先搜索 | `myco sense "..."` |
| **R4** | 洞察产生立即吃 | `myco eat --content "..."` |
| **R5** | 创建文件必须加引用 | 写笔记时加 cross-ref |
| **R6** | 只写允许的路径 | _canon.yaml 白名单控制 |
| **R7** | 上层文档 > 下层实现 | L0 > L1 > L2 > L3 > L4 |

---

## 🔥 第五章：命令频率热力图

```
每次会话（日常核心）    定期（本周内）       偶尔（项目演进）
════════════════════  ════════════════   ════════════════
✅ hunger   ████████  immune   ████      propagate  ██
✅ eat      ████████  traverse ████      fruit      ██
✅ sense    ████████  sporulate ███      molt       █
✅ senesce  ████████  digest   ███       winnow     █
  assimilate ███████                     ramify     █
  forage    ████                        graft      █
```

### 记住这个

**最核心的日常循环只有 4 个命令**：

```
🔄 hunger → (eat + sense 循环) → senesce
```

其他所有命令都是在这个基础上的增强。

---

## 📋 速查表：我现在应该用什么命令？

| 我想... | 用这个命令 |
|--------|----------|
| 开始一天的工作 | `myco hunger --execute` |
| 记录一个 bug 发现 | `myco eat --content "..."` |
| 查一下之前有没有提过某个话题 | `myco sense "关键词"` |
| 导入一个文件/目录 | `myco eat --path <路径>` |
| 看看知识库健康不健康 | `myco immune` |
| 自动修复问题 | `myco immune --fix` |
| 把一类知识浓缩成教义 | `myco sporulate "主题"` |
| 结束一天的工作 | `myco senesce` |
| 把知识推送到另一个项目 | `myco propagate --dst <路径>` |

---

## 🧠 本质理解

Myco 的设计哲学：

> **不是记笔记系统，是自我消化系统**

- 每条 eat 的内容都是「当下的洞察」
- assimilate 是「反复思考的过程」
- sporulate 是「提炼通用规则」
- immune 是「自我纠正的能力」

最终目标：**让分散的工作日记自动演进成可复用的知识教义**。
