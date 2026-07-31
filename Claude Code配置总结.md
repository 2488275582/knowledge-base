# Claude Code 当前完整配置清单

*更新时间: 2026-04-13*

## 一、已启用插件 (Plugins)

| 插件 | 版本 | 来源 |
|------|------|------|
| `superpowers@claude-plugins-official` | 5.0.7 | claude-plugins-official |
| `code-review@claude-plugins-official` | b091cb4179d3 | claude-plugins-official |
| `agent-sdk-dev@claude-plugins-official` | b091cb4179d3 | claude-plugins-official |
| `playwright@claude-plugins-official` | b091cb4179d3 | claude-plugins-official |
| `oh-my-claudecode@omc` | 4.11.5 | [github.com/Yeachan-Heo/oh-my-claudecode](https://github.com/Yeachan-Heo/oh-my-claudecode) |
| `ecc@ecc` | 1.10.0 | [github.com/affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) |

## 二、已配置 MCP 服务器

| 名称                               | 状态   | 用途                                                     |
| -------------------------------- | ---- | ------------------------------------------------------ |
| **GitHub MCP Server**            | ✅ 启用 | GitHub 仓库操作 (issues, PRs, 文件读写)                        |
| **Alibaba Cloud Yunxiao DevOps** | ✅ 启用 | 阿里云云效 DevOps 集成 (项目管理、代码、流水线、应用交付)                     |
| **Filesystem**                   | ✅ 启用 | 文件系统访问 (限定 `/Users/liran/workspace/IdeaProjects/gaia`) |
| **Fetch**                        | ✅ 启用 | 网页内容获取                                                 |
| **Sequential Thinking**          | ✅ 启用 | 结构化顺序思考                                                |
| **Memory**                       | ✅ 启用 | 知识图谱记忆                                                 |

## 三、全局规则 (Rules)

### 规则分类

- **common/** (10个) - 通用原则
  - [`coding-style.md`](file:///Users/liran/.claude/rules/common/coding-style.md) - 编码风格 (不可变性、KISS、DRY、YAGNI、错误处理、输入验证)
  - [`git-workflow.md`](file:///Users/liran/.claude/rules/common/git-workflow.md) - Git 工作流 (约定式提交)
  - [`testing.md`](file:///Users/liran/.claude/rules/common/testing.md) - 测试要求 (最低 80% 覆盖率、TDD)
  - [`performance.md`](file:///Users/liran/.claude/rules/common/performance.md) - 性能优化 (模型选择、上下文管理)
  - [`patterns.md`](file:///Users/liran/.claude/rules/common/patterns.md) - 常用设计模式 (仓储模式、API 响应格式)
  - [`hooks.md`](file:///Users/liran/.claude/rules/common/hooks.md) - 钩子系统 (Pre/Post/Stop)
  - [`agents.md`](file:///Users/liran/.claude/rules/common/agents.md) - 代理编排 (并行执行、多视角分析)
  - [`security.md`](file:///Users/liran/.claude/rules/common/security.md) - 安全指南 (密钥管理、强制检查)
  - [`code-review.md`](file:///Users/liran/.claude/rules/common/code-review.md) - 代码审查标准 (检查清单、严重级别)
  - [`development-workflow.md`](file:///Users/liran/.claude/rules/common/development-workflow.md) - 功能开发工作流 (研究 → 规划 → TDD → 审查 → 提交)

- **zh/** (10个) - 中文翻译版本

- **web/** (9个) - Web 前端特定规则
  - [`coding-style.md`](file:///Users/liran/.claude/rules/web/coding-style.md) - 文件组织、CSS 变量、动画、语义 HTML
  - [`testing.md`](file:///Users/liran/.claude/rules/web/testing.md) - 视觉回归、可访问性、性能、跨浏览器测试
  - [`performance.md`](file:///Users/liran/.claude/rules/web/performance.md) - Core Web Vitals 目标、分块预算、图片/字体优化
  - [`patterns.md`](file:///Users/liran/.claude/rules/web/patterns.md) - 组件组合、状态管理、URL 作为状态
  - [`hooks.md`](file:///Users/liran/.claude/rules/web/hooks.md) - PostToolUse 钩子 (format/lint/type-check)
  - [`design-quality.md`](file:///Users/liran/.claude/rules/web/design-quality.md) - 设计质量标准 (避免模板外观，要求层级/节奏/深度)
  - [`security.md`](file:///Users/liran/.claude/rules/web/security.md) - CSP、XSS 防护、第三方脚本、安全头

- **语言特定规则**: 
  - [`cpp/coding-style.md`](file:///Users/liran/.claude/rules/cpp/coding-style.md), [`cpp/testing.md`](file:///Users/liran/.claude/rules/cpp/testing.md), [`cpp/patterns.md`](file:///Users/liran/.claude/rules/cpp/patterns.md), [`cpp/hooks.md`](file:///Users/liran/.claude/rules/cpp/hooks.md), [`cpp/security.md`](file:///Users/liran/.claude/rules/cpp/security.md)
  - [`csharp/coding-style.md`](file:///Users/liran/.claude/rules/csharp/coding-style.md), [`csharp/testing.md`](file:///Users/liran/.claude/rules/csharp/testing.md), [`csharp/patterns.md`](file:///Users/liran/.claude/rules/csharp/patterns.md), [`csharp/hooks.md`](file:///Users/liran/.claude/rules/csharp/hooks.md), [`csharp/security.md`](file:///Users/liran/.claude/rules/csharp/security.md)
  - [`dart/coding-style.md`](file:///Users/liran/.claude/rules/dart/coding-style.md), [`dart/testing.md`](file:///Users/liran/.claude/rules/dart/testing.md), [`dart/patterns.md`](file:///Users/liran/.claude/rules/dart/patterns.md), [`dart/hooks.md`](file:///Users/liran/.claude/rules/dart/hooks.md), [`dart/security.md`](file:///Users/liran/.claude/rules/dart/security.md)
  - [`golang/coding-style.md`](file:///Users/liran/.claude/rules/golang/coding-style.md), [`golang/testing.md`](file:///Users/liran/.claude/rules/golang/testing.md), [`golang/patterns.md`](file:///Users/liran/.claude/rules/golang/patterns.md), [`golang/hooks.md`](file:///Users/liran/.claude/rules/golang/hooks.md), [`golang/security.md`](file:///Users/liran/.claude/rules/golang/security.md)
  - [`java/coding-style.md`](file:///Users/liran/.claude/rules/java/coding-style.md), [`java/testing.md`](file:///Users/liran/.claude/rules/java/testing.md), [`java/patterns.md`](file:///Users/liran/.claude/rules/java/patterns.md), [`java/hooks.md`](file:///Users/liran/.claude/rules/java/hooks.md), [`java/security.md`](file:///Users/liran/.claude/rules/java/security.md)
  - [`kotlin/coding-style.md`](file:///Users/liran/.claude/rules/kotlin/coding-style.md), [`kotlin/testing.md`](file:///Users/liran/.claude/rules/kotlin/testing.md), [`kotlin/patterns.md`](file:///Users/liran/.claude/rules/kotlin/patterns.md), [`kotlin/hooks.md`](file:///Users/liran/.claude/rules/kotlin/hooks.md), [`kotlin/security.md`](file:///Users/liran/.claude/rules/kotlin/security.md)
  - [`perl/coding-style.md`](file:///Users/liran/.claude/rules/perl/coding-style.md), [`perl/testing.md`](file:///Users/liran/.claude/rules/perl/testing.md), [`perl/patterns.md`](file:///Users/liran/.claude/rules/perl/patterns.md), [`perl/hooks.md`](file:///Users/liran/.claude/rules/perl/hooks.md), [`perl/security.md`](file:///Users/liran/.claude/rules/perl/security.md)
  - [`php/coding-style.md`](file:///Users/liran/.claude/rules/php/coding-style.md), [`php/testing.md`](file:///Users/liran/.claude/rules/php/testing.md), [`php/patterns.md`](file:///Users/liran/.claude/rules/php/patterns.md), [`php/hooks.md`](file:///Users/liran/.claude/rules/php/hooks.md), [`php/security.md`](file:///Users/liran/.claude/rules/php/security.md)
  - [`python/coding-style.md`](file:///Users/liran/.claude/rules/python/coding-style.md), [`python/testing.md`](file:///Users/liran/.claude/rules/python/testing.md), [`python/patterns.md`](file:///Users/liran/.claude/rules/python/patterns.md), [`python/hooks.md`](file:///Users/liran/.claude/rules/python/hooks.md), [`python/security.md`](file:///Users/liran/.claude/rules/python/security.md)
  - [`rust/coding-style.md`](file:///Users/liran/.claude/rules/rust/coding-style.md), [`rust/testing.md`](file:///Users/liran/.claude/rules/rust/testing.md), [`rust/patterns.md`](file:///Users/liran/.claude/rules/rust/patterns.md), [`rust/hooks.md`](file:///Users/liran/.claude/rules/rust/hooks.md), [`rust/security.md`](file:///Users/liran/.claude/rules/rust/security.md)
  - [`swift/coding-style.md`](file:///Users/liran/.claude/rules/swift/coding-style.md), [`swift/testing.md`](file:///Users/liran/.claude/rules/swift/testing.md), [`swift/patterns.md`](file:///Users/liran/.claude/rules/swift/patterns.md), [`swift/hooks.md`](file:///Users/liran/.claude/rules/swift/hooks.md), [`swift/security.md`](file:///Users/liran/.claude/rules/swift/security.md)
  - [`typescript/coding-style.md`](file:///Users/liran/.claude/rules/typescript/coding-style.md), [`typescript/testing.md`](file:///Users/liran/.claude/rules/typescript/testing.md), [`typescript/patterns.md`](file:///Users/liran/.claude/rules/typescript/patterns.md), [`typescript/hooks.md`](file:///Users/liran/.claude/rules/typescript/hooks.md), [`typescript/security.md`](file:///Users/liran/.claude/rules/typescript/security.md)

## 四、已安装技能 (Skills)

总计 **962** 个技能目录。

### 顶级技能清单

#### gstack 原生 slash 命令

| 命令 | 用途 |
|------|------|
| `/office-hours` | YC 风格产品审问 |
| `/plan-ceo-review` | CEO 重新思考产品范围 |
| `/plan-eng-review` | 工程经理锁定架构 |
| `/plan-design-review` | 设计师评估设计维度 |
| `/plan-devex-review` | DX 负责人审计开发者体验 |
| `/design-consultation` | 设计合伙人从头构建完整设计系统 |
| `/design-shotgun` | 生成多个设计变体供选择 |
| `/design-html` | 将原型转换为生产 HTML |
| `/review` | 员工工程师查找 CI 遗漏的生产 bug |
| `/investigate` | 系统化根因分析调试 |
| `/design-review` | 设计师审查设计问题 |
| `/devex-review` | 开发者体验审计 |
| `/qa` | 在真实浏览器测试，发现 bug 并修复 |
| `/qa-only` | 仅 QA 报告 |
| `/pair-agent` | 多代理协调 |
| `/cso` | 首席安全官 - OWASP Top 10 + STRIDE 安全审计 |
| `/ship` | 运行测试，推送，打开 GitHub PR |
| `/land-and-deploy` | 合并 PR，等待 CI，部署，验证生产 |
| `/canary` | 发布后监控 |
| `/benchmark` | 测量页面加载时间，比较前后 |
| `/document-release` | 更新文档匹配已发布内容 |
| `/retro` | 每周回顾 |
| `/browse` | QA 工程师真实 Chrome 测试 |
| `/open-gstack-browser` | 启动带侧边栏的 GStack 浏览器 |
| `/setup-browser-cookies` | 从浏览器导入 cookie 用于认证测试 |
| `/setup-deploy` | 一次性设置部署 |
| `/autoplan` | 全自动规划 (CEO → 设计 → 工程评审) |
| `/codex` | OpenAI Codex 第二意见 |
| `/careful` | 安全护栏 |
| `/freeze` | 编辑锁定 - 限制编辑到一个目录 |
| `/guard` | 全面安全 (/careful + /freeze 组合) |
| `/unfreeze` | 解锁 - 移除 /freeze 边界 |
| `/gstack-upgrade` | 自升级 gstack |

### oh-my-claudecode 技能

| 技能 | 用途 |
|------|------|
| `oh-my-claudecode:ask` | 提问 |
| `oh-my-claudecode:autopilot` | 自动驾驶模式 |
| `oh-my-claudecode:browse` | 浏览 |
| `oh-my-claudecode:cancel` | 取消 |
| `oh-my-claudecode:ccg` |  |
| `oh-my-claudecode:debug` | 调试 |
| `oh-my-claudecode:deep-dive` | 深入研究 |
| `oh-my-claudecode:deep-init` | 深度初始化 |
| `oh-my-claudecode:deep-interview` | 深度访谈 |
| `oh-my-claudecode:hud` | 状态栏 |
| `oh-my-claudecode:learner` | 学习 |
| `oh-my-claudecode:mcp-setup` | MCP 设置 |
| `oh-my-claudecode:memory` | 记忆 |
| `oh-my-claudecode:omc-doctor` | OMC 诊断 |
| `oh-my-claudecode:omc-reference` | OMC 参考 |
| `oh-my-claudecode:omc-setup` | OMC 设置 |
| `oh-my-claudecode:omc-teams` | OMC 团队 |
| `oh-my-claudecode:plan` | 规划 |
| `oh-my-claudecode:project-session-manager` | 项目会话管理 |
| `oh-my-claudecode:ralph` | Ralph |
| `oh-my-claudecode:ralplan` | Ralph 规划 |
| `oh-my-claudecode:release` | 发布 |
| `oh-my-claudecode:remember` | 记忆 |
| `oh-my-claudecode:review` | 审查 |
| `oh-my-claudecode:skill` | 技能 |
| `oh-my-claudecode:skillify` | 技能化 |
| `oh-my-claudecode:team` | 团队 |
| `oh-my-claudecode:trace` | 追踪 |
| `oh-my-claudecode:ultraqa` | 超高质量保证 |
| `oh-my-claudecode:ultrawork` | 超高效工作 |
| `oh-my-claudecode:verify` | 验证 |
| `oh-my-claudecode:visual-verdict` | 视觉裁决 |
| `oh-my-claudecode:wiki` | Wiki |
| `oh-my-claudecode:writer-memory` | 写入记忆 |
| `oh-my-claudecode:ai-slop-cleaner` | AI 垃圾清理 |
| `oh-my-claudecode:configure-notifications` | 配置通知 |
| `oh-my-claudecode:external-context` | 外部上下文 |
| `oh-my-claudecode:sciomc` |  |
| `oh-my-claudecode:self-improve` | 自我改进 |

### superpowers 核心技能

| 技能 | 用途 |
|------|------|
| `superpowers:using-superpowers` | 开始任何对话时使用，建立技能调用流程 |
| `superpowers:brainstorming` | 头脑风暴 |
| `superpowers:writing-plans` | 编写计划 |
| `superpowers:executing-plans` | 执行计划 |
| `superpowers:subagent-driven-development` | 子代理驱动开发 |
| `superpowers:dispatching-parallel-agents` | 分发并行代理 |
| `superpowers:systematic-debugging` | 系统化调试 |
| `superpowers:test-driven-development` | 测试驱动开发 |
| `superpowers:requesting-code-review` | 请求代码审查 |
| `superpowers:receiving-code-review` | 接收代码审查 |
| `superpowers:finishing-a-development-branch` | 完成开发分支 |
| `superpowers:verification-before-completion` | 完成前验证 |
| `superpowers:using-git-worktrees` | 使用 Git worktrees |

## 五、ecc (everything-claude-code) 技能分类

### 开发流程
`ecc:plan` - 复杂功能实现规划  
`ecc:feature-dev` - 新功能开发工作流  
`ecc:tdd` - 测试驱动开发  
`ecc:tdd-workflow` - TDD 完整工作流  
`ecc:code-review` - 代码审查  
`ecc:security-review` - 安全审查  
`ecc:verify` - 完成验证

### 语言/框架特定
`ecc:go-review` - Go 代码审查  
`ecc:go-build` - Go 构建错误修复  
`ecc:go-test` - Go 测试  
`ecc:rust-review` - Rust 代码审查  
`ecc:rust-build` - Rust 构建错误修复  
`ecc:rust-test` - Rust 测试  
`ecc:python-review` - Python 代码审查  
`ecc:python-testing` - Python 测试  
`ecc:typescript-review` - TypeScript 代码审查  
`ecc:kotlin-review` - Kotlin 代码审查  
`ecc:kotlin-build` - Kotlin 构建错误修复  
`ecc:kotlin-testing` - Kotlin 测试  
`ecc:cpp-review` - C++ 代码审查  
`ecc:cpp-build` - C++ 构建错误修复  
`ecc:cpp-testing` - C++ 测试  
`ecc:csharp-review` - C# 代码审查  
`ecc:csharp-testing` - C# 测试  
`ecc:java-review` - Java 代码审查  
`ecc:java-build` - Java 构建错误修复  
`ecc:flutter-review` - Flutter/Dart 代码审查  
`ecc:flutter-build` - Flutter 构建错误修复  
`ecc:flutter-test` - Flutter 测试  
`ecc:laravel-patterns` - Laravel 开发模式  
`ecc:laravel-tdd` - Laravel TDD  
`ecc:laravel-security` - Laravel 安全  
`ecc:django-patterns` - Django 开发模式  
`ecc:django-tdd` - Django TDD  
`ecc:django-security` - Django 安全  
`ecc:nestjs-patterns` - NestJS 开发模式  
`ecc:nextjs-turbopack` - Next.js Turbopack 优化  
`ecc:nuxt4-patterns` - Nuxt 4 开发模式

### 架构与设计
`ecc:architect` - 架构设计  
`ecc:hexagonal-architecture` - 六边形架构  
`ecc:blueprint` - 蓝图设计  
`ecc:design-system` - 设计系统  
`ecc:frontend-design` - 前端设计  
`ecc:frontend-patterns` - 前端模式  
`ecc:liquid-glass-design` - 液态玻璃设计  
`ecc:deployment-patterns` - 部署模式  
`ecc:database-migrations` - 数据库迁移

### 测试与质量
`ecc:e2e` - E2E 测试  
`ecc:e2e-testing` - E2E 测试开发  
`ecc:browser-qa` - 浏览器 QA 测试  
`ecc:prp-pr` - PR 准备  
`ecc:review-pr` - PR 审查  
`ecc:prp-implement` - PRP 实现  
`ecc:prp-plan` - PRP 规划  
`ecc:safety-guard` - 安全防护  
`ecc:plankton-code-quality` - 代码质量检查

### 运营与运维
`ecc:prompt-optimizer` - Prompt 优化  
`ecc:evolve` - 代码演进优化  
`ecc:prune` - 死代码清理  
`ecc:docs` - 文档更新  
`ecc:opensource-pipeline` - 开源项目流水线  
`ecc:repository-scan` - 代码库扫描  
`ecc:codebase-onboarding` - 代码库入门指南

### 特定领域
`ecc:product-capability` - 产品能力梳理  
`ecc:click-path-audit` - 点击路径审计  
`ecc:content-hash-cache-pattern` - 内容哈希缓存模式  
`ecc:evm-token-decimals` - EVM 代币小数处理  
`ecc:defi-amm-security` - DeFi AMM 安全审查  
`ecc:healthcare-emr-patterns` - 医疗 EMR 模式  
`ecc:healthcare-phi-compliance` - 医疗 PHI 合规  
`ecc:hipaa-compliance` - HIPAA 合规  
`ecc:postgres-patterns` - PostgreSQL 模式  
`ecc:clickhouse-io` - ClickHouse 输入输出  
`ecc:github-ops` - GitHub 操作  
`ecc:api-design` - API 设计  
`ecc:connections-optimizer` - 连接优化  
`ecc:project-flow-ops` - 项目流程操作  
`ecc:autonomous-loops` - 自主循环  
`ecc:continuous-agent-loop` - 持续代理循环  
`ecc:agent-eval` - 代理评估  
`ecc:agent-harness-construction` - 代理 harness 构建  
`ecc:seed:api-connector-builder` - API 连接器构建器

### 钩子与配置
`ecc:hookify` - Hook 配置  
`ecc:hookify-list` - 列出 Hook  
`ecc:hookify-help` - Hook 帮助  
`ecc:hookify-configure` - Hook 配置  
`ecc:configure-ecc` - 配置 ecc  
`ecc:rules-distill` - 规则提炼

### 研究与内容
`ecc:exa-search` - Exa 网络搜索  
`ecc:deep-research` - 深度研究  
`ecc:article-writing` - 文章写作  
`ecc:video-editing` - 视频编辑  
`ecc:remotion-video-creation` - Remotion 视频创作

### ecc 子代理类型
| 子代理 | 用途 |
|--------|------|
| `ecc:architect` | 软件架构专家 |
| `ecc:build-error-resolver` | 构建错误解决专家 |
| `ecc:code-reviewer` | 代码审查专家 |
| `ecc:database-reviewer` | 数据库审查专家 |
| `ecc:doc-updater` | 文档更新专家 |
| `ecc:e2e` | E2E 测试专家 |
| `ecc:explore` | 代码探索专家 |
| `ecc:gan-evaluator` | GAN 评估器 |
| `ecc:gan-generator` | GAN 生成器 |
| `ecc:gan-planner` | GAN 规划器 |
| `ecc:performance-optimizer` | 性能优化专家 |
| `ecc:planner` | 规划专家 |
| `ecc:refactor-cleaner` | 重构清理专家 |
| `ecc:security-reviewer` | 安全审查专家 |
| `ecc:tdd-guide` | TDD 指南 |
| `product-requirements-analyst` | 产品需求分析师 |

## 六、已配置钩子 (Hooks)

```json
{
  "Stop": [
    {
      "command": "python3 ~/.claude/scripts/sync-session-log.py",
      "timeout": 30,
      "description": "Syncing session log..."
    }
  ],
  "SessionEnd": [
    {
      "command": "python3 ~/.claude/scripts/sync-session-log.py --merge",
      "timeout": 60,
      "description": "Merging session logs..."
    }
  ]
}
```

**会话日志自动归档**：
- 根目录：`~/workspace/claudecode/{YYYY-MM-DD}/{project}-{timestamp}/session-log.md`
- 合并日志：`~/workspace/claudecode/merged/{project}-session-log.md`
- 5MB 滚动归档机制

## 七、环境变量

```
ANTHROPIC_AUTH_TOKEN=a39e6d0a-9016-4ce7-94ae-2029dbadfbb7
ANTHROPIC_BASE_URL=https://ark.cn-beijing.volces.com/api/coding
ANTHROPIC_DEFAULT_SONNET_MODEL=ark-code-latest
ANTHROPIC_DEFAULT_OPUS_MODEL=ark-code-latest
ANTHROPIC_DEFAULT_HAIKU_MODEL=ark-code-latest
CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
API_TIMEOUT_MS=3000000
```

**语言**: 中文

**会话录制**: 启用，输出到 `~/Desktop/claude-sessions`

## 八、额外市场

| 名称 | 来源 |
|------|------|
| `anthropic-agent-skills` | github.com/anthropics/skills |
| `omc` | github.com/Yeachan-Heo/oh-my-claudecode |
| `claude-code-plugins-plus` | github.com/jeremylongshore/claude-code-plugins |
| `ecc` | github.com/affaan-m/everything-claude-code |

## 统计

| 项目 | 数量 |
|------|------|
| 插件 | 6 |
| MCP 服务器 | 6 |
| 规则文件 | 45+ |
| 技能目录 | 962 |
| 顶级技能 | 150+ |
