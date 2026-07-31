# 为 Spring Boot 项目生成 Claude Code 配置文档的 Agent 提示词

---

## 一、文档体系设计原则（读懂再用）

本项目的配置文档采用**主文件 + 辅助规则文件**两级结构：

```
项目根目录/
├── claude.md                  ← 入口总纲，≤1000字，所有会话必读
└── .claude/
    ├── settings.json          ← 工具权限白/黑名单，控制 AI 可执行命令
    └── rules/
        ├── architecture.md    ← 架构详情、业务域、设计模式（按需读）
        ├── building.md        ← 构建命令、私有依赖、常见问题（按需读）
        ├── database.md        ← ORM配置、Mapper规范、变更流程（按需读）
        ├── testing.md         ← 测试框架、运行命令、注意事项（按需读）
        └── deployment.md      ← Docker、K8s、环境变量、镜像仓库（按需读）
```

**分工逻辑：**
- `claude.md`：快速定位关键约定 + 禁止行为 + 执行命令，末尾引导读辅助文档
- `.claude/rules/*.md`：各专项详情，Agent 在涉及该主题时主动阅读
- `.claude/settings.json`：无需 AI 判断，直接白名单放行安全命令、黑名单拦截危险命令

---

## 二、提示词正文（直接复制给 Agent 使用）

---

你是专业的 Claude Code 配置工程师。请对当前 Spring Boot 项目进行探索，生成一套可直接落地的 Claude Code 配置文档。

### 阶段一：探索项目（禁止跳过）

读文件前不得生成任何文档。依次探索以下内容并记录结果：

**基础信息**
- 根目录 `pom.xml` / `build.gradle`：项目名、groupId、版本号、构建工具
- 是否多模块？各模块目录名和职责
- 主启动类路径 → 得到基础包名、`@MapperScan` 路径、`scanBasePackages`

**分层结构**（实际 Java 包路径，不猜测）
- Controller 所在包
- Service 所在包（有无接口+Impl 分离？）
- Repository / DAO 所在包（有无 Repository 层？）
- Mapper 所在包
- Entity / Domain 所在包
- DTO 所在包，VO / Request / Response 所在包

**关键类**（读源码确认，不假设）
- 统一返回类：名称、所在包、核心静态方法签名
- 全局异常处理类：位置、捕获的异常类型
- 错误码枚举/常量类：位置、分段规律（如 1xxxx=系统，2xxxx=业务）
- Entity 基类（如有）：字段、自动填充注解
- MapStruct 转换器：命名规范（`*Adapter` / `*Converter` / `*Transfer`）、所在层

**工程约定**
- 依赖注入方式：`@Autowired` 或 `@RequiredArgsConstructor`
- ID 策略：雪花/UUID/自增
- 枚举序列化方式（`@EnumValue` / `@JsonValue` / 其他）
- API 路径规范（如 `/api/{domain}/{action}`）

**外围配置**
- 数据库：类型、版本、连接池、是否有迁移工具（Flyway/Liquibase）
- 缓存/锁：Redis / Redisson 版本
- MQ：类型、版本、环境隔离方式
- 注册中心/配置中心：Nacos / Eureka / 其他
- 定时任务：XXL-Job / Quartz / Spring Scheduler
- 规则引擎/工作流（如有）

**构建与部署**
- 常用 Maven/Gradle 命令（实际可用的）
- 是否有 Dockerfile，基础镜像是什么
- 是否有 K8s YAML，资源规格、环境变量
- 环境清单（local/dev/test/prod 对应的 Profile 和配置中心 namespace）
- 私有 Maven 仓库（如有 `settings.xml`）

**测试**
- 测试框架版本（JUnit 4/5、Mockito）
- 测试目录结构
- 现有测试以集成测试为主还是单元测试为主

### 阶段二：生成文件

探索完成后，按顺序创建/覆盖以下 7 个文件。**已有文件必须先 Read 再写入。**

---

#### 文件1：`claude.md`（根目录）

字数严格控制在 **600～1000字**，结构固定如下：

```
# {项目名} — {一句话业务描述}

## 项目概述
{2-3行：业务定位、所属系统生态、核心功能域}

## 技术栈
{列表形式，每行：技术名 版本 — 用途说明}

## 项目结构
{代码块，展示模块/包层级，每行注释职责}
依赖关系：{一行说明模块间依赖方向}

## 核心工作流

### 构建
{bash 代码块，完整构建 + 常用单模块构建命令}

### 本地运行
{bash 代码块，含启动类路径注释、端口、外部依赖说明}

### 测试
{bash 代码块，运行全量/模块/单类测试}

## 关键约定
{短列表，每条格式：约定项：具体值或类名}
例：
- 基础包名：`com.example.app`
- 统一返回：`ResultVO<T>`，用 `ResultVO.success()` / `ResultVO.error(code, msg)`
- 分页：内部 `PageDTO<T>`，响应 `PageVO<T>`
- ID 策略：雪花算法，`@TableId(type = IdType.ASSIGN_ID)`
- 自动填充：`createTime`/`updateTime`/`createBy` 由 MetaHandler 处理，禁止手动赋值
- 异常：业务异常用 `BusinessException(ErrorCode.XXX)`，平台异常用 `XxxException`
- 注入：`@RequiredArgsConstructor` 构造器注入，禁止 `@Autowired`
- 敏感配置：不入代码库，由 {配置中心} 下发

## 禁止行为
{编号列表，≤10条，只写项目特有的，不写通用常识}
1. 禁止 ...
...

## 辅助文档
在开始工作前，如需了解细节，请阅读 `.claude/rules/` 中对应文档：
- `.claude/rules/architecture.md` — 分层架构、业务域划分、设计模式
- `.claude/rules/building.md` — 构建流程、私有依赖、常见问题
- `.claude/rules/database.md` — ORM配置、Mapper规范、数据变更流程
- `.claude/rules/testing.md` — 测试规范与运行命令
- `.claude/rules/deployment.md` — Docker/K8s 部署、环境变量
```

---

#### 文件2：`.claude/rules/architecture.md`

包含以下小节（全部基于实际探索结果，不补充猜测内容）：

1. **分层架构**：每层对应的 Maven 模块、Java 包路径、职责、关键子目录及含义
2. **业务领域**：表格（领域 | 说明 | 关键实体类名）
3. **外部服务依赖**：通过 Feign/gRPC 调用的服务名 + 用途（如有）
4. **设计模式**：项目中实际使用的模式（Repository、责任链、规则引擎、事件驱动等）+ 对应包路径

---

#### 文件3：`.claude/rules/building.md`

包含以下小节：

1. **前置要求**：JDK 版本、构建工具版本、私有仓库配置文件路径
2. **构建命令**：bash 代码块，含注释说明
3. **模块依赖链**：文字或图示，说明构建顺序
4. **输出产物**：JAR 路径、用途
5. **内部私有依赖**：groupId:artifactId 列表 + 说明
6. **常见问题**：实际可能遇到的问题 + 解决命令（如 MapStruct 重新编译、SNAPSHOT 强制更新）

---

#### 文件4：`.claude/rules/database.md`

包含以下小节：

1. **基本信息**：数据库类型/版本、连接池、连接配置来源
2. **ORM 配置**：分页插件、ID 策略配置类路径、自动填充处理器路径 + 填充字段清单、自定义 TypeHandler 列表
3. **Mapper 规范**：接口位置（按业务域分包规律）、XML 位置、简单 CRUD vs 复杂查询的边界
4. **Repository 层**：接口位置、实现位置、继承关系
5. **数据库变更**：有无迁移工具、新增实体的完整步骤（编号列表）

---

#### 文件5：`.claude/rules/testing.md`

包含以下小节：

1. **测试框架**：JUnit 版本、Mock 框架、测试依赖所在模块
2. **测试位置**：实际目录路径（Controller/Service 测试分别在哪）
3. **运行命令**：bash 代码块（全量、指定模块、指定类、指定方法）
4. **编写指南**：集成测试 vs 单元测试的适用场景、Mock 方式示例、测试数据构造建议
5. **注意事项**：项目特有的坑（如需要外部服务、MapStruct 需重编译、无迁移工具等）

---

#### 文件6：`.claude/rules/deployment.md`

包含以下小节：

1. **容器化**：Dockerfile 位置、基础镜像、构建产物路径、容器内端口
2. **构建镜像命令**：bash 代码块
3. **Kubernetes**：部署文件路径、资源规格、关键环境变量表格、模板变量说明
4. **镜像仓库**：地址、地域（如有）
5. **环境清单**：表格（环境 | Profile | 配置中心Namespace | 说明）
6. **运行时外部依赖**：服务名 + 是否必须 + 配置来源

---

#### 文件7：`.claude/settings.json`

根据实际项目的构建工具和常用命令生成，Maven 项目参考如下，Gradle 项目将 `mvn` 替换为 `./gradlew`：

```json
{
  "permissions": {
    "allow": [
      "Bash(mvn clean compile)",
      "Bash(mvn clean package -DskipTests)",
      "Bash(mvn clean package -pl*)",
      "Bash(mvn clean install -DskipTests)",
      "Bash(mvn test*)",
      "Bash(git status*)",
      "Bash(git diff*)",
      "Bash(git log*)",
      "Bash(git branch*)",
      "Bash(git add*)",
      "Read(*)",
      "Glob(*)",
      "Grep(*)"
    ],
    "deny": [
      "Bash(rm -rf*)",
      "Bash(git push --force*)",
      "Bash(git reset --hard*)",
      "Bash(docker push*)"
    ]
  }
}
```

---

### 写作约束（所有文件强制执行）

- **只写项目特有信息**：AI 能从代码推断的通用常识（如"Controller 处理 HTTP 请求"）不写
- **路径和类名必须真实**：来自探索结果，不猜测、不补全
- **命令必须可直接执行**：不写 `<your-module>` 这类占位符
- **参考文件用相对路径**：从项目根目录起，如 `api/src/main/.../ReceiptController.java`
- **信息密度优先**：表格 > 列表 > 段落；短句 > 长句
- **`claude.md` 字数上限 1000 字**；各 rules 文件不超过 150 行
- **不重复**：`claude.md` 已有的内容，rules 文件不再复述，只做深化

### 完成标准

所有 7 个文件写入完毕后，输出一行确认：
`完成：claude.md + architecture.md + building.md + database.md + testing.md + deployment.md + settings.json`

---

## 三、gaia-wms 项目实际生成结果（参考样本）

### `claude.md` 结构要点

- 项目概述：2行定位（跨境电商WMS + 微服务生态角色）
- 技术栈：列表，每项含版本
- 项目结构：4模块代码块 + 依赖方向一行
- 核心工作流：构建/运行/测试各一个 bash 块
- 关键约定：10条短列表，每条写明具体类名或值
- 禁止行为：无（本项目将禁止行为放在 claude.md 另一版本中）
- 辅助文档：引导读 `.claude/rules/` 5个文件

### `.claude/rules/` 各文件要点

| 文件 | 核心内容 | 特色 |
|---|---|---|
| `architecture.md` | 4层模块包路径 + 8个业务域表格 + 7个外部服务 + 5种设计模式 | 业务域表含关键实体类名 |
| `building.md` | 4条构建命令 + 模块依赖链 + 5条内部私有依赖 + 3个常见问题 | 含 SNAPSHOT 强制更新命令 |
| `database.md` | MyBatis-Plus 完整配置路径 + 3个自定义TypeHandler + Repository层规范 + 新增实体4步流程 | 无迁移工具的变更说明 |
| `testing.md` | JUnit5 + @SpringBootTest + Mockito + 4条运行命令 + 集成/单元测试选择建议 | 明确指出集成测试需外部Nacos |
| `deployment.md` | Dockerfile基础镜像 + K8s两套规格 + 4环境表格 + 5个运行时依赖 + 镜像仓库地址 | 含 CI 模板变量说明 |

### `.claude/settings.json` 要点

- allow：13条（mvn 5条 + git 5条 + 读操作 3条）
- deny：4条（rm -rf / git push --force / git reset --hard / docker push）
