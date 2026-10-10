---
layout: cover
routerMode: hash
theme: '@ktym4a/slidev-theme-ktym4a'
favicon: ./images/dci-agent-icon.svg
lineNumbers: true
fonts: false
title: IHEP 的 AI 辅助 DIRAC 部署与运维（中文对照版）
titleTemplate: '%s - Xiao Han'
mermaid:
  theme: dark
  themeVariables:
    primaryColor: '#313244'
    primaryTextColor: '#cdd6f4'
    primaryBorderColor: '#585b70'
    lineColor: '#94e2d5'
themeConfig:
  baseColor: sky
---

# AI 辅助的 DIRAC 部署<br/>与运维 · IHEP

#### dci-agent · 分阶段自主 · MCP 与技能

<br>

**Xiao Han** 代表 IHEP DCI 组 · <a href="mailto:hanx@ihep.ac.cn">hanx@ihep.ac.cn</a>

<br>

**第 12 届 DIRAC(X) 用户研讨会** · 2026 · *IHEP，北京*

<a href="https://indico.cern.ch/event/1588323" class="ns-c-iconlink"><mdi-link-variant /> Workshop Indico</a>


<!--
Timing: 0:30

Good morning. I am Xiao Han, from the IHEP DCI Group. AI models keep getting stronger — they reason better, and they understand systems like ours better every year. We decided to put that to work: at IHEP we run an operational agent, dci-agent, which helps us deploy and operate DIRAC and the middleware chain around it. This talk has two halves: first the operations story — what dci-agent is and does, including a DIRAC v9 deployment it carried out in the test environment; then the AI infrastructure underneath — the MCP services and the skills that make it useful and safe.
-->
---
layout: default
---

# 更强的 AI 正在参与 DCI 运维

<div class="cols mt-2">
  <div>

## 为什么是现在

- 模型更<strong>强大</strong> — 对配置、日志和代码的推理更好
- 能<strong>跨系统推理</strong> — DIRAC、FTS 与网格中间件
- 工具生态成熟 — <strong>MCP</strong>、agent 框架、技能库

<div class="takeaway compact mt-3">
在 IHEP，我们部署了 <img :src="'./images/dci-agent-icon.svg'" class="dci-agent-icon" alt="" /><strong>dci-agent</strong>，把跨系统的证据引入日常运维。
</div>

  </div>
  <div>

## 我们对它的要求

- <strong>看清链路</strong> — 脱敏后的配置、日志、源码与监控
- <strong>回答与报告</strong> — 聊天按需回答，也可定时执行
- <strong>协助部署</strong> — 基本完成了 DIRAC v9 测试部署
- <strong>保持安全</strong> — 构造上只读；操作只能通过白名单 MCP 函数

<div class="takeaway compact mt-3">
先讲运维，再讲背后的 MCP 与技能。
</div>

  </div>
</div>


<!--
Timing: 0:50

Why this talk now? Models reason better over configs, logs and code, and MCP and skills make that reasoning useful. We deployed dci-agent to bring evidence from across IHEP DCI into everyday operations: see the chain, answer and report, help deploy, and keep actions inside an explicit boundary. First I will show the operations; then I will show the infrastructure behind them.
-->
---
layout: default
---

# DCI 运维横跨多个系统

<div class="cols mt-2">
  <div>

<div class="triage-list">
  <div><small>生产环境</small><strong>DIRAC v8.0.58（JUNO）</strong><span>5 台服务器上 60+ 组件；6 个 SiteDirector 实例（JUNO、CEPC、CMS、BES、LHCb、JUNOCloud）。</span></div>
  <div><small>中间件链</small><strong>FTS3 · grid-data · IHEPDIRAC · T1 CE</strong><span>传输、存储与站点服务 — 各自有配置、日志与源码。</span></div>
  <div><small>准备中</small><strong>v9 迁移基线</strong><span>测试主机上 agent 已基本完成 v9.0.26 部署。</span></div>
</div>

  </div>
  <div>

日常工作分四条线：

- **部署** — 版本升级、组件安装、配置变更
- **监控** — 指标、可用性探测、仪表盘
- **诊断** — 组件日志、传输链路、跨系统追踪
- **沉淀** — 运行手册、安装笔记、下一次值班需要的知识

<div class="takeaway compact mt-4">
故障的现象和根源往往在<strong>不同系统</strong>上 — 这正是只有一个只读视图的 agent 发挥作用的地方。
</div>

  </div>
</div>


<!--
Timing: 0:50

Context. In production we run DIRAC v8.0.58 for JUNO — sixty-plus components, six SiteDirectors — inside a middleware chain of FTS3, grid-data, IHEPDIRAC and the T1 computing element. Daily work is four streams: deploy, watch, diagnose, record. And the pain point: symptoms and causes live on different systems. That is the gap dci-agent fills. Before the architecture, one slide on how we think an AI assistant should grow into this job.
-->
---
layout: section
---

# 1 · 运维：dci-agent


<!--
Timing: 0:10

Part one: dci-agent — how an AI operations assistant should grow, what it is, and what it has already done.
-->
---
layout: default
---

# 自主能力分四个受控阶段

<div class="four-cards mt-4">
  <div class="story-card">
    <h2>1 · 只读分析</h2>
    <p>完全只读：建议、告警、初步诊断 — 给出依据，人来决策。</p>
  </div>
  <div class="story-card">
    <h2>2 · 协助操作</h2>
    <p>执行无害操作 — 重复、可逆、困扰管理员的日常工作。</p>
  </div>
  <div class="story-card">
    <h2>3 · 经批准执行</h2>
    <p>关键操作先准备，必须在管理员同意后才执行。</p>
  </div>
  <div class="story-card">
    <h2>4 · 自动驾驶</h2>
    <p>全面自动化 — 未来目标，不是今天的现状。</p>
  </div>
</div>

<div class="takeaway mt-6">
学习闭环：Hermes 可以把对话和操作沉淀为<strong>经审核的技能</strong>。更强的模型和积累的知识让更高自主成为可能 — <strong>不是自动获得</strong>。
</div>


<!--
Timing: 1:20

How should an AI assistant grow into an operations role? Four stages: read-only analysis and alarms; harmless, reversible actions; critical actions with administrator approval; and ultimately full automation. The last stage is a goal, not today's reality. Hermes can summarize conversations and operations into skills for human review. Better models and accumulated knowledge help us move upward, but each stage also needs stronger controls and evidence — learning alone never grants authority.
-->
---
layout: default
---

# dci-agent 把 DCI 链路纳入一个视图

<div class="architecture-map mt-2">
  <div class="architecture-source"><strong>只读 NFS</strong><span>配置 · 日志 · 源码 · 凭据已脱敏</span></div>
  <div class="architecture-systems">
    <span>DIRAC</span><span>IHEPDIRAC</span><span>FTS3</span><span>grid-data</span><span>ihep-T1-ce</span>
  </div>
  <div class="architecture-down"><span>只读证据</span><i aria-hidden="true"></i></div>
  <div class="architecture-flow">
    <div class="architecture-node"><small>监控</small><strong>dci-grafana MCP</strong><span>指标与仪表盘</span></div>
    <b aria-hidden="true">→</b>
    <div class="architecture-node architecture-agent"><small>基于 Hermes</small><strong>dci-agent</strong><span>关联 · 分析 · 报告</span></div>
    <b aria-hidden="true">→</b>
    <div class="architecture-node"><small>输出</small><strong>状态报告</strong><span>检查与发现</span></div>
  </div>
  <div class="architecture-triggers">
    <div class="architecture-chat"><i aria-hidden="true"></i><strong>飞书 · Mattermost · WebUI</strong><span>管理员对话</span></div>
    <div class="architecture-schedule"><strong>cronjob</strong> → 定时检查</div>
  </div>
</div>

<div class="takeaway compact mt-2">证据挂载是<strong>只读</strong>的。任何运维写入都走单独的、受限的 MCP 操作路径。</div>


<!--
Timing: 1:25

The architecture. Read-only NFS mounts bring sanitized configurations, logs and source from DIRAC, IHEPDIRAC, FTS3, grid-data and ihep-T1-ce. The dci-grafana MCP provides metrics and dashboard queries. Administrators reach the Hermes-based dci-agent through Feishu, Mattermost or WebUI; a cronjob triggers scheduled checks, and the agent produces status reports. Notice that this diagram is the evidence path: operational writes do not flow back through the NFS mounts. They use the separate, scoped MCP functions shown later.
-->
---
layout: default
---

# 证据只读；操作走单独路径

<div class="cols mt-2">
  <div>

## 只读是构造属性，不是口头承诺

- NFS 挂载<strong>只读</strong> — 配置、日志、源码不可写
- 证据面到中间件<strong>没有写路径</strong>

<div class="takeaway compact mt-3">
NFS 证据路径无法修改挂载的中间件文件。
</div>

  </div>
  <div>

## 模型看到之前先脱敏

- <strong>所有凭据脱敏</strong> — token、密钥、密码从配置与日志中屏蔽
- 敏感值在到达模型前被过滤
- 即使过滤后，日志与源码仍是<strong>不可信输入</strong>

<div class="takeaway compact mt-3">
脱敏降低暴露；不能替代审批和 MCP 侧控制。
</div>

  </div>
</div>


<!--
Timing: 1:00

The evidence mounts are read-only, so reading a log or comparing a config cannot alter those mounted files. We sanitize credentials before the model sees them. But read-only mounts are not a universal safety guarantee: logs and source can still contain misleading instructions, and the agent has a separate MCP action path. We treat those inputs as untrusted and enforce write permissions at that separate boundary.
-->
---
layout: default
---

# 聊天与定时检查共享同一证据面

<div class="cols mt-2">
  <div>

## 两种启动方式

<div class="triage-list">
  <div><small>对话</small><strong>管理员提问</strong><span>用 FTS3、DIRAC 等证据检查传输或诊断故障；回答附来源。</span></div>
  <div><small>定时</small><strong>cronjob 定期检查</strong><span>汇总服务状态，把异常发现交给管理员复核。</span></div>
</div>

<div class="takeaway compact mt-3">
可通过飞书、Mattermost 和 Web UI 访问 — 同一个 agent，同一个证据面；区别只是<strong>触发方式</strong>：一条消息，或时钟。
</div>

  </div>
  <div>

<div class="agent-demo">
  <video :src="'./dci-agent-demo-trimmed.mp4'" autoplay loop muted playsinline controls preload="metadata" aria-label="DCI 运维助手仪表盘与 AI 对话的屏幕录制"></video>
  <div class="agent-demo-caption"><mdi-monitor-screenshot /> dci-agent 仪表盘与对话 · 屏幕录制（静音）</div>
</div>

  </div>
</div>


<!--
Timing: 1:05

Two ways to drive it. Interactively: an administrator asks, in Feishu, Mattermost or the web UI — what is the transfer status for this site, help me triage this failure — and the agent answers with sources it can cite, or drafts a diagnosis for review. And on a schedule: a cronjob triggers periodic status checks that aggregate across the chain and summarize the running state; anomalies get a drafted analysis flagged for a human to judge. Same agent, same read-only evidence plane — only the trigger differs: a message, or the clock.
-->
---
layout: default
---

# agent 在测试环境部署了 v9.0.26

<div class="cols mt-2">
  <div>

## agent 做了什么

- 仅在<strong>测试环境获得操作权限</strong>
- 安装 <strong>DIRAC v9.0.26</strong>，基本完成部署
- 速度来自理解：按需对整个 DCI 栈做<strong>配置对比</strong>和<strong>日志检查</strong>
- 每一步都记录为运行手册式笔记

<div class="takeaway compact mt-1">
生产保持 <strong>v8.0.58</strong> — 未受影响。
</div>

  </div>
  <div>

## 为什么可行

- agent 本来就<strong>熟悉 DCI 系统</strong>：配置、日志、源码是它的主场
- v8 对比 v9 的<strong>配置比较</strong>正是它擅长的跨文件推理
- 安装的坑变成<strong>文档化步骤</strong>，不再重复调试

<div class="status-stack tight mt-1">
  <div><mdi-check-circle-outline /><span><strong>测试部署基本完成</strong><br/>v9.0.26（隔离测试主机）</span></div>
  <div><mdi-check-circle-outline /><span><strong>可复用运行手册</strong><br/>每个坑都是下一台主机的文档化步骤</span></div>
</div>

  </div>
</div>


<!--
Timing: 1:20

In the isolated test environment we gave the agent operation permission. It installed DIRAC v9.0.26 and largely completed the deployment; production on v8.0.58 was not touched. Comparing v8 and v9 configuration and tracing startup problems through logs were particularly effective because the agent already had the DCI evidence in view. The pitfalls became reusable notes. Next, the infrastructure that defines which actions an agent may take.
-->
---
layout: section
---

# 2 · AI 基础设施：MCP 与技能


<!--
Timing: 0:10

Part two: the infrastructure underneath — MCP services on the read and write sides, and the skills that make the agent useful to administrators and users alike.
-->
---
layout: default
---

# 监控证据通过 MCP 到达 agent

<div class="cols mt-2">
  <div>

## 一个可查询的证据层

- **dci-grafana MCP** — agent 搜索仪表盘、读取面板查询、执行 Prometheus 查询、返回渲染结果
- **31 个仪表盘以 Git JSON 存储** — 5 个 provisioning provider，30 秒同步；agent 可组合，人来审核 diff
- **DIRAC 集中日志** — 一条后端线路把 60+ 组件经 ActiveMQ → Logstash → Elasticsearch 汇聚

<div class="takeaway compact mt-2">
人和 agent 读<strong>同一份证据</strong> — 模型没有私有数据通道。
</div>

  </div>
  <div>

<div class="dashboard-frame snapshot-frame">
  <img :src="'images/component-logs-snapshot.png'" alt="Component Logs 历史快照：日志等级、速率与示例条目" />
</div>

<div class="text-center mt-2">
  <span class="muted">历史快照 · 2026 年 9 月</span> · <a href="https://dci-grafana.ihep.ac.cn/d/bfgu666p30xdsb/component-logs?orgId=1&from=1788912000000&to=1788998400000&timezone=browser&var-Category=$__all&var-Name=$__all&var-Level=$__all&kiosk"><mdi-open-in-new /> 打开仪表盘（需登录）</a>
</div>

  </div>
</div>


<!--
Timing: 1:05

The read side. The dci-grafana MCP lets the agent search dashboards and query monitoring. Our dashboard definitions are JSON in Git; human reviewers decide whether a proposed change is accepted. DIRAC logs are centralized through ActiveMQ, Logstash and Elasticsearch. On the right is a historical snapshot from September 2026, not a live view. The link opens the restricted dashboard for people with Grafana access; a public viewer can still see the evidence on this slide.
-->
---
layout: default
---

# MCP 函数定义了 agent 能执行什么

<div class="action-flow mt-2">
  <div><small>请求</small><strong>dci-agent</strong><span>运维意图</span></div><b aria-hidden="true">→</b>
  <div><small>身份</small><strong>受限 token</strong><span>仅允许的操作</span></div><b aria-hidden="true">→</b>
  <div><small>边界</small><strong>MCP 函数</strong><span>预定义参数</span></div><b aria-hidden="true">→</b>
  <div><small>目标</small><strong>DIRAC / FTS</strong><span>具体服务</span></div>
</div>

<div class="action-examples"><span>例：重启选定的 DIRAC 组件</span><span>例：重启 FTS 服务</span></div>

<div class="rule-list mt-3">
  <div><mdi-key-outline /><span><strong>受限 token</strong> — agent 用专用 token 认证，其 scope 明确列出可执行的内容；其余均不可达。</span></div>
  <div><mdi-cube-outline /><span><strong>操作是代码，不是提示词</strong> — "重启组件 X" 是 MCP 服务内的内部函数；agent 无法拼出任意的 shell 命令。</span></div>
  <div><mdi-book-open-variant-outline /><span><strong>技能引导决策</strong> — 技能描述何时调用函数；真正能否执行由 token 范围和 MCP 实现强制。</span></div>
</div>


<!--
Timing: 1:20

The write side is separate. A dedicated token limits which MCP functions are callable; the service exposes predefined operations such as restarting a selected DIRAC component or FTS service rather than an arbitrary shell interface. Skills advise the model when a call is appropriate, but they are not an authorization gate: the token and MCP implementation enforce that boundary. For a critical operation we would add explicit administrator approval; that is the next stage, not something to infer from today's function catalog.
-->
---
layout: default
---

# 经审核的技能让经验可复用

<div class="skill-flow mt-2">
  <div><small>01 · 做</small><strong>对话<br/>或操作</strong></div><b aria-hidden="true">→</b>
  <div><small>02 · 提炼</small><strong>agent 总结<br/>有效做法</strong></div><b aria-hidden="true">→</b>
  <div><small>03 · 审核</small><strong>人工检查<br/>成文内容</strong></div><b aria-hidden="true">→</b>
  <div><small>04 · 复用</small><strong>发布为<br/>技能</strong></div>
</div>

<div class="two-notes compact-notes mt-3">
  <div><strong>面向运维</strong><br/>DIRAC 与作业操作技能、每仓库 agent 指南、v9 升级笔记 — 每次修复都成为下一次会话的文档化起点。</div>
  <div><strong>面向用户 — 已可用</strong><br/>JUNO DIRAC 基础打包为技能：用户可以让 agent <strong>提交作业</strong>、<strong>查询文件</strong>，不用记住每条命令。</div>
</div>

<div class="takeaway mt-3">
下一次会话直接查阅经审核的技能；agent 不必重新摸索同一流程。
</div>


<!--
Timing: 1:10

Skills are how the system compounds. After a conversation or an operation, the agent summarizes what worked; a human reviews the write-up; approved summaries are published as skills. Two audiences benefit. For operations: DIRAC and job-operations skills, repository guides, the v9 upgrade notes — every fix becomes a documented step. And for users, this is already live: the JUNO DIRAC basics are packaged as skills, so a user can ask the agent to submit a job or query files without remembering every DIRAC command by heart. The same loop — do, summarize, review, publish — powers both.
-->
---
layout: default
---

# 五条边界控制运维风险

<div class="rule-list mt-3">
  <div><mdi-check-circle-outline /><span><strong>NFS 证据面只读。</strong>配置、日志、源码挂载时没有写路径；操作使用单独的 MCP 函数。</span></div>
  <div><mdi-check-circle-outline /><span><strong>凭据在模型之前脱敏。</strong>token、密钥、密码被屏蔽；源码与日志仍是不可信输入。</span></div>
  <div><mdi-check-circle-outline /><span><strong>操作是受限 token 后面的白名单函数。</strong>agent 调用预定义操作 — 无法拼出任意命令。</span></div>
  <div><mdi-check-circle-outline /><span><strong>技能指导何时调用操作。</strong>授权由 MCP 范围和函数实现强制，而不是技能文本。</span></div>
  <div><mdi-check-circle-outline /><span><strong>知识写成供查阅的形式。</strong>笔记和技能是工作流的一部分 — 是下一次会话或用户首先读的东西。</span></div>
</div>


<!--
Timing: 1:00

Five boundaries hold the story together: read-only evidence mounts, credential sanitization, scoped tokens, predefined functions, and reviewed procedural knowledge. The skill tells the agent when to consider an action, but authorization lives in the MCP service; approval for dangerous actions remains a separate requirement. These are distinct controls, not five interchangeable promises.
-->
---
layout: default
---

# 路线图：用证据赢得自主

<div class="cols mt-2">
  <div>

## 横向 — 拓宽视野

<div class="status-stack tight">
  <div><mdi-arrow-right-circle-outline /><span><strong>更多数据源，同一契约</strong><br/>更多中间件挂载；对全部数据源做定时分析</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>日志早期预警</strong><br/>集中日志中的错误模式与日志率异常</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>运行手册 RAG</strong><br/>在自建基础设施上用自然语言查询积累的笔记</span></div>
</div>

  </div>
  <div>

## 纵向 — 扩大白名单

<div class="status-stack next tight">
  <div><mdi-arrow-right-circle-outline /><span><strong>更多 MCP 函数</strong><br/>每个新操作都以经审核的内部函数加独立范围进入</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>更多面向用户的技能</strong><br/>超越 JUNO 基础的作业提交与文件查询</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>审计 + 审批流</strong><br/>每个操作持久化；第三阶段关键操作需明确同意</span></div>
</div>

  </div>
</div>

<div class="takeaway compact mt-3">
每个新操作都是一次认真的<strong>代码评审</strong>，不是改提示词。自主靠证据赢得，不是靠自信。
</div>


<!--
Timing: 1:00

The roadmap. Horizontal: widen the view — more middleware mounts, log early warning, retrieval over our own runbooks. Vertical: widen the whitelist — each new operation enters as a reviewed internal function with its own scope; more user-facing skills beyond the JUNO basics; and audit plus approval flows, which is how stage three — critical operations with explicit consent — gets earned. Each new action is a deliberate code review, not a prompt tweak. Autonomy is earned by evidence, not by confidence.
-->
---
layout: default
---

# 一个视图、有界操作、可复用知识

<div class="three-cards takeaway-cards mt-6">
  <div class="story-card"><strong>1</strong><h2>一个视图</h2><p>dci-agent 看到整条链 — 配置、日志、源码、监控 — 只读且脱敏，聊天或定时可用。</p></div>
  <div class="story-card"><strong>2</strong><h2>分阶段自主</h2><p>从只读分析到经批准的操作：每一步都需要更强的控制与证据。</p></div>
  <div class="story-card"><strong>3</strong><h2>复利效应</h2><p>技能同时服务管理员与用户 — 每次对话和修复都让下一个任务更容易。</p></div>
</div>

<div class="closing-line mt-10">
今天不是自动驾驶。<br/>
是给操作者和用户<strong>更长的手臂</strong> — 有一条靠证据赢得的自主之路。
</div>


<!--
Timing: 0:50

Three takeaways. One evidence view across the DCI, available in chat and on a schedule. Bounded actions, with stronger controls before advancing to more autonomy. And reviewed skills serving both administrators and users. Today this is not an autopilot; it gives operators and users a longer reach and a deliberate path toward more automation. Thank you.
-->
---
layout: cover
loop: true
title: Questions
---


# 谢谢！欢迎提问

**Xiao Han · IHEP, CC**<br/>
IHEP DCI 组

<a href="https://indico.cern.ch/event/1588323" class="ns-c-iconlink"><mdi-link-variant /> Workshop Indico</a>

<!--
Timing: 0:15

That is the end of my talk. Thank you — I am happy to take questions.
-->
