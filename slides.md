---
layout: cover
routerMode: hash
theme: '@ktym4a/slidev-theme-ktym4a'
favicon: ./images/dci-agent-icon.svg
lineNumbers: true
fonts: false
title: AI-Assisted DIRAC Deployment and Operations at IHEP
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

# AI-Assisted DIRAC Deployment<br/>and Operations at IHEP

#### dci-agent, staged autonomy, MCP and skills

<br>

**Xiao Han** on behalf of the IHEP DCI Group · <a href="mailto:hanx@ihep.ac.cn">hanx@ihep.ac.cn</a>

<br>

**The 12th DIRAC(X) Users' Workshop** · 2026 · *IHEP, Beijing*

<a href="https://dci-grafana.ihep.ac.cn/" class="ns-c-iconlink"><mdi-view-dashboard-outline /> DCI Grafana</a>
 · <a href="https://github.com/hanx-hep/2026-cepc-dci" class="ns-c-iconlink"><mdi-history /> Monitoring report</a>


<!--
Timing: 0:30

Good morning. I am Xiao Han, from the IHEP DCI Group. AI models keep getting stronger — they reason better, and they understand systems like ours better every year. We decided to put that to work: at IHEP we run an operational agent, dci-agent, which helps us deploy and operate DIRAC and the middleware chain around it. This talk has two halves: first the operations story — what dci-agent is and does, including a DIRAC v9 deployment it carried out in the test environment; then the AI infrastructure underneath — the MCP services and the skills that make it useful and safe.
-->
---
layout: default
---

# A stronger AI now helps operate the DCI

<div class="cols mt-2">
  <div>

## Why now

- Models are <strong>stronger</strong> — better reasoning over configs, logs, and code
- They can <strong>reason across our stack</strong> — DIRAC, FTS, and grid middleware
- Tool ecosystems matured — <strong>MCP</strong>, agent frameworks, skill libraries

<div class="takeaway compact mt-3">
At IHEP we deployed <img :src="'./images/dci-agent-icon.svg'" class="dci-agent-icon" alt="" /><strong>dci-agent</strong> to bring cross-system evidence into everyday operations.
</div>

  </div>
  <div>

## What we ask of it

- <strong>See the chain</strong> — sanitized configs, logs, source, and monitoring
- <strong>Answer and report</strong> — on demand in chat, and on a schedule
- <strong>Help deploy</strong> — it largely completed a DIRAC v9 test deployment
- <strong>Stay safe</strong> — read-only by construction; actions only through whitelisted MCP functions

<div class="takeaway compact mt-3">
Start with operations; then show the MCP and skills behind them.
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

# DCI operations cross multiple systems

<div class="cols mt-2">
  <div>

<div class="triage-list">
  <div><small>PRODUCTION</small><strong>DIRAC v8.0.58 for JUNO</strong><span>60+ components on 5 servers; 6 SiteDirector instances (JUNO, CEPC, CMS, BES, LHCb, JUNOCloud).</span></div>
  <div><small>MIDDLEWARE CHAIN</small><strong>FTS3 · grid-data · IHEPDIRAC · T1 CE</strong><span>Transfers, storage and site services — each with its own configs, logs, and source.</span></div>
  <div><small>IN PREPARATION</small><strong>v9 migration baseline</strong><span>A test host where the agent largely completed a v9.0.24 deployment.</span></div>
</div>

  </div>
  <div>

The daily work is four streams:

- **Deploy** — version upgrades, component installs, config changes
- **Watch** — metrics, availability probes, dashboards
- **Diagnose** — component logs, transfer chains, cross-system traces
- **Record** — runbooks, install notes, the knowledge the next shift needs

<div class="takeaway compact mt-4">
A fault's symptom and its cause often live on <strong>different systems</strong> — exactly where an agent with one read-only view helps.
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

# 1 · Operate: the dci-agent


<!--
Timing: 0:10

Part one: dci-agent — how an AI operations assistant should grow, what it is, and what it has already done.
-->
---
layout: default
---

# Autonomy grows in four controlled stages

<div class="four-cards mt-4">
  <div class="story-card">
    <h2>1 · Analyzes</h2>
    <p>Completely read-only: advice, alarms, first-pass diagnosis — evidence cited, humans decide.</p>
  </div>
  <div class="story-card">
    <h2>2 · Assists</h2>
    <p>Executes harmless operations — the routine, reversible work that burdens administrators.</p>
  </div>
  <div class="story-card">
    <h2>3 · Acts, approved</h2>
    <p>Critical operations prepared and executed only with the administrator's consent.</p>
  </div>
  <div class="story-card">
    <h2>4 · Autopilots</h2>
    <p>Full automation across operations — a future goal, not today's claim.</p>
  </div>
</div>

<div class="takeaway mt-6">
The learning loop: Hermes can distill conversations and operations into <strong>reviewed skills</strong>. Greater model capability and accumulated knowledge make higher autonomy possible — <strong>not automatic</strong>.
</div>


<!--
Timing: 1:20

How should an AI assistant grow into an operations role? Four stages: read-only analysis and alarms; harmless, reversible actions; critical actions with administrator approval; and ultimately full automation. The last stage is a goal, not today's reality. Hermes can summarize conversations and operations into skills for human review. Better models and accumulated knowledge help us move upward, but each stage also needs stronger controls and evidence — learning alone never grants authority.
-->
---
layout: default
---

# dci-agent brings the DCI chain into one view

<div class="architecture-map mt-2">
  <div class="architecture-source"><strong>READ-ONLY NFS</strong><span>configs · logs · source code · credentials sanitized</span></div>
  <div class="architecture-systems">
    <span>DIRAC</span><span>IHEPDIRAC</span><span>FTS3</span><span>grid-data</span><span>ihep-T1-ce</span>
  </div>
  <div class="architecture-down"><span>read-only evidence</span><i aria-hidden="true"></i></div>
  <div class="architecture-flow">
    <div class="architecture-node"><small>MONITORING</small><strong>dci-grafana MCP</strong><span>metrics &amp; dashboards</span></div>
    <b aria-hidden="true">→</b>
    <div class="architecture-node architecture-agent"><small>HERMES-BASED</small><strong>dci-agent</strong><span>correlate · analyze · report</span></div>
    <b aria-hidden="true">→</b>
    <div class="architecture-node"><small>OUTPUT</small><strong>Status reports</strong><span>checks &amp; findings</span></div>
  </div>
  <div class="architecture-triggers">
    <div class="architecture-chat"><i aria-hidden="true"></i><strong>Feishu · Mattermost · WebUI</strong><span>administrator chat</span></div>
    <div class="architecture-schedule"><strong>cronjob</strong> → scheduled checks</div>
  </div>
</div>

<div class="takeaway compact mt-2">The evidence mounts are <strong>read-only</strong>. Any operational write uses a separate, scoped MCP action path.</div>


<!--
Timing: 1:25

The architecture. Read-only NFS mounts bring sanitized configurations, logs and source from DIRAC, IHEPDIRAC, FTS3, grid-data and ihep-T1-ce. The dci-grafana MCP provides metrics and dashboard queries. Administrators reach the Hermes-based dci-agent through Feishu, Mattermost or WebUI; a cronjob triggers scheduled checks, and the agent produces status reports. Notice that this diagram is the evidence path: operational writes do not flow back through the NFS mounts. They use the separate, scoped MCP functions shown later.
-->
---
layout: default
---

# Evidence is read-only; actions have a separate path

<div class="cols mt-2">
  <div>

## Read-only is a property, not a policy

- NFS mounts are <strong>read-only</strong> — configs, logs, and source cannot be written
- The evidence plane simply has <strong>no write path</strong> to the middleware

<div class="takeaway compact mt-3">
The NFS evidence path cannot modify mounted middleware files.
</div>

  </div>
  <div>

## Sanitized before the model

- <strong>All credentials sanitized</strong> — tokens, keys and passwords masked out of configs and logs
- Sensitive values are filtered before reaching the model
- Logs and source remain <strong>untrusted input</strong>, even after filtering

<div class="takeaway compact mt-3">
Sanitization reduces exposure; it does not replace approval and MCP-side controls.
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

# Chat and scheduled checks share one evidence plane

<div class="cols mt-2">
  <div>

## Two ways to start a task

<div class="triage-list">
  <div><small>CHAT</small><strong>An administrator asks</strong><span>Check a transfer or diagnose a failure using FTS3, DIRAC and other evidence; answer with sources.</span></div>
  <div><small>SCHEDULE</small><strong>A cronjob starts regular checks</strong><span>Summarize service status and flag unusual findings for an administrator to review.</span></div>
</div>

<div class="takeaway compact mt-3">
Reachable through Feishu, Mattermost and the web UI — the same agent, the same evidence plane; only the <strong>trigger</strong> differs: a message, or the clock.
</div>

  </div>
  <div>

<div class="agent-demo">
  <video :src="'./dci-agent-demo-trimmed.mp4'" autoplay loop muted playsinline controls preload="metadata" aria-label="Screen recording of the DCI operations assistant dashboard and AI chat"></video>
  <div class="agent-demo-caption"><mdi-monitor-screenshot /> dci-agent dashboard and chat · screen recording (muted)</div>
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

# The agent helped deploy v9.0.24 in the test bed

<div class="cols mt-2">
  <div>

## What the agent did

- Given <strong>operation permission in the test environment only</strong>
- Installed <strong>DIRAC v9.0.24</strong> and basically completed the deployment
- The speed came from understanding: <strong>config comparison</strong> and <strong>log inspection</strong> across the DCI stack, on demand
- Every step recorded as runbook-style notes

<div class="takeaway compact mt-1">
Production stays on <strong>v8.0.58</strong> — untouched.
</div>

  </div>
  <div>

## Why it worked

- The agent already <strong>knows the DCI system</strong>: configs, logs, and source are its home turf
- v8-vs-v9 <strong>config comparison</strong> is exactly its kind of cross-file reasoning
- Install pitfalls became <strong>documented steps</strong>, not repeated debugging

<div class="status-stack tight mt-1">
  <div><mdi-check-circle-outline /><span><strong>Test deployment largely complete</strong><br/>v9.0.24 on the isolated test host</span></div>
  <div><mdi-check-circle-outline /><span><strong>Reusable runbook</strong><br/>Each pitfall is a documented step for the next host</span></div>
</div>

  </div>
</div>


<!--
Timing: 1:20

In the isolated test environment we gave the agent operation permission. It installed DIRAC v9.0.24 and largely completed the deployment; production on v8.0.58 was not touched. Comparing v8 and v9 configuration and tracing startup problems through logs were particularly effective because the agent already had the DCI evidence in view. The pitfalls became reusable notes. Next, the infrastructure that defines which actions an agent may take.
-->
---
layout: section
---

# 2 · AI infrastructure: MCP and skills


<!--
Timing: 0:10

Part two: the infrastructure underneath — MCP services on the read and write sides, and the skills that make the agent useful to administrators and users alike.
-->
---
layout: default
---

# Monitoring evidence reaches the agent through MCP

<div class="cols mt-2">
  <div>

## One queryable evidence layer

- **dci-grafana MCP** — the agent searches dashboards, reads panel queries, runs Prometheus queries, returns rendered panels
- **31 dashboards as Git JSON** — five provisioning providers, 30 s sync; the agent can compose them, humans review the diff
- **Central DIRAC logs** — one backend line sends 60+ components through ActiveMQ → Logstash → Elasticsearch

<div class="takeaway compact mt-2">
People and agents read the <strong>same evidence</strong> — no private data path for the model.
</div>

  </div>
  <div>

<div class="dashboard-frame snapshot-frame">
  <img :src="'images/component-logs-snapshot.png'" alt="Historical Component Logs snapshot showing log levels, rates and example entries" />
</div>

<div class="text-center mt-2">
  <span class="muted">Historical snapshot · Sep 2026</span> · <a href="https://dci-grafana.ihep.ac.cn/d/bfgu666p30xdsb/component-logs?orgId=1&from=1788912000000&to=1788998400000&timezone=browser&var-Category=$__all&var-Name=$__all&var-Level=$__all&kiosk"><mdi-open-in-new /> Open dashboard (login required)</a>
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

# MCP functions define what the agent can execute

<div class="action-flow mt-2">
  <div><small>REQUEST</small><strong>dci-agent</strong><span>operational intent</span></div><b aria-hidden="true">→</b>
  <div><small>IDENTITY</small><strong>scoped token</strong><span>allowed actions only</span></div><b aria-hidden="true">→</b>
  <div><small>BOUNDARY</small><strong>MCP function</strong><span>predefined parameters</span></div><b aria-hidden="true">→</b>
  <div><small>TARGET</small><strong>DIRAC / FTS</strong><span>specific service</span></div>
</div>

<div class="action-examples"><span>e.g. restart a selected DIRAC component</span><span>e.g. restart an FTS service</span></div>

<div class="rule-list mt-3">
  <div><mdi-key-outline /><span><strong>Scoped tokens</strong> — the agent authenticates with a dedicated token whose scope names exactly what may be executed; nothing else is reachable.</span></div>
  <div><mdi-cube-outline /><span><strong>Operations are code, not prompts</strong> — "restart component X" is an internal function inside the MCP service; the agent cannot compose arbitrary shell commands.</span></div>
  <div><mdi-book-open-variant-outline /><span><strong>Skills guide the decision</strong> — they describe when to call a function; the token scope and MCP implementation enforce what is actually executable.</span></div>
</div>


<!--
Timing: 1:20

The write side is separate. A dedicated token limits which MCP functions are callable; the service exposes predefined operations such as restarting a selected DIRAC component or FTS service rather than an arbitrary shell interface. Skills advise the model when a call is appropriate, but they are not an authorization gate: the token and MCP implementation enforce that boundary. For a critical operation we would add explicit administrator approval; that is the next stage, not something to infer from today's function catalog.
-->
---
layout: default
---

# Reviewed skills make experience reusable

<div class="skill-flow mt-2">
  <div><small>01 · DO</small><strong>Conversation<br/>or operation</strong></div><b aria-hidden="true">→</b>
  <div><small>02 · DISTILL</small><strong>Agent summarizes<br/>what worked</strong></div><b aria-hidden="true">→</b>
  <div><small>03 · REVIEW</small><strong>Human checks<br/>the write-up</strong></div><b aria-hidden="true">→</b>
  <div><small>04 · REUSE</small><strong>Publish as<br/>a skill</strong></div>
</div>

<div class="two-notes compact-notes mt-3">
  <div><strong>For operations</strong><br/>DIRAC and job-operations skills, per-repository agent guides, and the v9 upgrade notes — each fix becomes a documented step the next session starts from.</div>
  <div><strong>For users — already available</strong><br/>JUNO DIRAC basics packaged as skills: users can ask the agent to <strong>submit jobs</strong> and <strong>query files</strong>, without remembering every command.</div>
</div>

<div class="takeaway mt-3">
The next session consults the reviewed skill; the agent need not rediscover the same procedure.
</div>


<!--
Timing: 1:10

Skills are how the system compounds. After a conversation or an operation, the agent summarizes what worked; a human reviews the write-up; approved summaries are published as skills. Two audiences benefit. For operations: DIRAC and job-operations skills, repository guides, the v9 upgrade notes — every fix becomes a documented step. And for users, this is already live: the JUNO DIRAC basics are packaged as skills, so a user can ask the agent to submit a job or query files without remembering every DIRAC command by heart. The same loop — do, summarize, review, publish — powers both.
-->
---
layout: default
---

# Five boundaries limit operational risk

<div class="rule-list mt-3">
  <div><mdi-check-circle-outline /><span><strong>The NFS evidence plane is read-only.</strong> Configs, logs and source are mounted without a write path; actions use separate MCP functions.</span></div>
  <div><mdi-check-circle-outline /><span><strong>Credentials are sanitized before the model.</strong> Tokens, keys and passwords are masked; source and logs remain untrusted input.</span></div>
  <div><mdi-check-circle-outline /><span><strong>Actions are whitelisted functions behind scoped tokens.</strong> The agent invokes predefined operations — it cannot compose arbitrary commands.</span></div>
  <div><mdi-check-circle-outline /><span><strong>Skills guide when to invoke an action.</strong> Authorization is enforced by the MCP scope and function, not by the skill text.</span></div>
  <div><mdi-check-circle-outline /><span><strong>Knowledge is written to be consulted.</strong> Notes and skills are part of the workflow — they are what the next session, or the next user, reads first.</span></div>
</div>


<!--
Timing: 1:00

Five boundaries hold the story together: read-only evidence mounts, credential sanitization, scoped tokens, predefined functions, and reviewed procedural knowledge. The skill tells the agent when to consider an action, but authorization lives in the MCP service; approval for dangerous actions remains a separate requirement. These are distinct controls, not five interchangeable promises.
-->
---
layout: default
---

# Roadmap: earn autonomy with evidence

<div class="cols mt-2">
  <div>

## Horizontal — widen the view

<div class="status-stack tight">
  <div><mdi-arrow-right-circle-outline /><span><strong>More sources, same contract</strong><br/>More middleware mounts; scheduled analyses over all of them</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>Log early warning</strong><br/>Error patterns and log-rate anomalies from the centralized logs</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>RAG over runbooks</strong><br/>Natural-language query over accumulated notes, on our own infra</span></div>
</div>

  </div>
  <div>

## Vertical — widen the whitelist

<div class="status-stack next tight">
  <div><mdi-arrow-right-circle-outline /><span><strong>More MCP functions</strong><br/>Each new operation enters as a reviewed internal function with its own scope</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>More user-facing skills</strong><br/>Job submission and file queries for non-experts, beyond the JUNO basics</span></div>
  <div><mdi-arrow-right-circle-outline /><span><strong>Audit + approval flows</strong><br/>Every action persisted; stage-3 critical operations behind explicit consent</span></div>
</div>

  </div>
</div>

<div class="takeaway compact mt-3">
Each new action is a deliberate <strong>code review</strong>, not a prompt tweak. Autonomy is earned by evidence, not by confidence.
</div>


<!--
Timing: 1:00

The roadmap. Horizontal: widen the view — more middleware mounts, log early warning, retrieval over our own runbooks. Vertical: widen the whitelist — each new operation enters as a reviewed internal function with its own scope; more user-facing skills beyond the JUNO basics; and audit plus approval flows, which is how stage three — critical operations with explicit consent — gets earned. Each new action is a deliberate code review, not a prompt tweak. Autonomy is earned by evidence, not by confidence.
-->
---
layout: default
---

# One view, bounded actions, reusable knowledge

<div class="three-cards takeaway-cards mt-6">
  <div class="story-card"><strong>1</strong><h2>One view</h2><p>dci-agent sees the whole chain — configs, logs, source, monitoring — read-only and sanitized, in chat or on a schedule.</p></div>
  <div class="story-card"><strong>2</strong><h2>Staged autonomy</h2><p>From read-only analysis to approved actions: each step needs stronger controls and evidence.</p></div>
  <div class="story-card"><strong>3</strong><h2>Compounding</h2><p>Skills serve administrators and users alike — every conversation and fix makes the next task easier.</p></div>
</div>

<div class="closing-line mt-10">
Not an autopilot today.<br/>
A <strong>longer reach for operators and users</strong> — with a path toward earned autonomy.
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


# Thank you. Questions?

**Xiao Han · IHEP, CC**<br/>
IHEP DCI Group

<a href="https://dci-grafana.ihep.ac.cn/"><mdi-view-dashboard-outline /> dci-grafana.ihep.ac.cn</a>
 · <a href="https://github.com/hanx-hep/2026-cepc-dci"><mdi-github /> monitoring report</a>

<!--
Timing: 0:15

That is the end of my talk. Thank you — I am happy to take questions.
-->
