# AI Agent 面试学习材料深度调研报告

调研日期: 2026-09-15 | 对象: 6 个开源项目 + 2 门名校课程 + 1 本出版书(含其官方仓库) | 方式: GitHub API 元数据 + 仓库树逐文件取证 + 一手页面/规范原文比对 + 本地实跑复现
决策者画像: 7 年研发(Java 后端 → NLP/LLM 工程化 → 代码智能 & Agent 工具链), 主线投 AI Agent 应用/平台岗; 靶心岗位: 腾讯混元 Agent Harness(深圳) / 蚂蚁 AI 应用(成都) / 鼎桥终端 AI Agent(成都) / MiniMax Agent 服务端(上海); 时间窗口 2026-08-28 → 09-28

---

## 结论(TL;DR)

| 分档 | 材料 | 投入 | 用途 |
|---|---|---|---|
| **主选** | **CMU 11-768「AI Agents」** | 20-25 h | 讲次表逐条对应靶心 JD: 沙箱/可观测性/Harness/LangGraph/Rerank/MCP; 公开 slides + A1 可动手 |
| **主选** | **bojieli/ai-agent-book(李博杰)** | 12-16 h | 唯一写透「可观测性 span 树 + Eval Harness + 混合检索 + 沙箱三级隔离」的正文体系; Apache-2.0 |
| **次选** | **ranxi2001/zero2Agent** | 12-16 h | 693 题一手面经 + 新手答/高手答分层 + 可跑实验; 但 A2A 段有规范级错误, 需纠偏后使用 |
| **速查** | Zchary1106/agent-interview-hub | 6-8 h | 69 题八股 + `data/question_signals.json` 证据表; 用于当日抽检与话术校准 |
| **补协议** | datawhalechina/hello-agents | 10 h(选读 4-6 章) | 唯一有 MCP + A2A + ANP 三协议专章 + 可跑代码; 补 A2A 动手最直接 |
| **防守弹药** | rasbt/LLMs-from-scratch(书) | 12-14 h | 只读 Ch3/Ch4/附录 D/附录 E + KV cache bonus; 被问 transformer 原理时不露怯 |
| **不读** | Stanford CS336 | ≤4 h | 训练侧课程, 19 讲零 Agent 覆盖; 且明令禁止用 coding agent 写作业 |
| **不读** | datawhalechina/happy-llm | ≤3 h | 偏好对齐章 `[WIP]` 且全文无 DPO, 近 3 月仅 3 次提交, 基础章 bug 未修 |
| **不读** | Lau-Jonathan/LLM-Agent-Interview-Guide | 1-1.5 h | 单次提交停更 6.5 个月; A2A/Harness/K8s/OTel/Tracing 命中 **0** |

**一句话决策**: 时间只有 1-2 周, 不要"学一套教程", 要**对着 8 项缺口清单按需取材**。主线用 **CMU 11-768 的 L1-L6 slides + A1 作业**搭骨架, 用 **ai-agent-book 的第二/三/七章**填可观测性与评测的血肉, 用 **zero2Agent 的 `16-agent-infra` 与 `06-multi-agent-collab`** 刷面经话术, 用 **hello-agents 第十章**补 A2A 动手。其余 5 个对象合计投入不超过 8 小时。

### 时间现实校准(必读)

30 天计划里 **第 3 周(9/14-9/20)已经是「面试强化 + 面经」阶段, 第 4 周就进真实面试**。也就是说"突击窗口"不是一个月, 而是 **7-10 天、每天 1-2 小时的净投入(约 14-20 小时)**, 且面试随时可能插入。本报告给出的所有路线都按这个预算裁剪; 任何"通读一遍"的方案在这一窗口内都不成立。

---

## 决策框架与评分

### 维度与权重

| 维度 | 权重 | 判据 |
|---|:--:|---|
| 缺口覆盖度 | 35% | 对照 `job-search/01-岗位调研/A级岗位精读.md` 的「面试前必补」8 项 + `04-技术复习清单.md` 的 P0/P1 |
| 可信度/准确性 | 20% | 作者可核实背景、是否引一手规范、抽查是否有事实错误、许可是否允许个人使用 |
| 时效性 | 15% | 内容是否 2025-2026 范式(MCP 2024-11 / A2A 2025-04 之后)、维护是否仍在进行 |
| 学习成本(可切削性) | 15% | 通读总时长 vs 14-20 h 窗口; 能否只取 3-5 章拿到 80% 收益 |
| 与简历互补性 | 15% | 是否重复他已具备的 MCP/RAG/DPO/BERT/静态分析经验(重叠=反向扣分) |

### 加权总评分

| 材料 | 覆盖 35% | 可信 20% | 时效 15% | 成本 15% | 互补 15% | **加权** |
|---|:--:|:--:|:--:|:--:|:--:|:--:|
| **CMU 11-768** | 5.0 | 5.0 | 5.0 | 4.0 | 5.0 | **4.9** |
| **ai-agent-book** | 4.5 | 4.5 | 5.0 | 4.0 | 5.0 | **4.6** |
| **zero2Agent** | 5.0 | 3.0 | 4.5 | 3.0 | 4.5 | **4.2** |
| hello-agents | 3.5 | 4.0 | 4.0 | 4.0 | 4.0 | **3.8** |
| agent-interview-hub | 4.0 | 3.0 | 3.5 | 3.5 | 4.0 | **3.7** |
| Stanford CS336 | 1.5 | 5.0 | 4.5 | 2.5 | 1.5 | **2.8** |
| rasbt 书(含仓库) | 1.0 | 5.0 | 2.0 | 4.0 | 2.0 | **2.6** |
| Lau-Jonathan guide | 0.5 | 3.5 | 1.5 | 5.0 | 2.0 | **2.2** |
| happy-llm | 0.5 | 3.0 | 1.5 | 4.0 | 1.5 | **1.8** |

> 评分说明: 「可信」一项对 zero2Agent(4.2)与 agent-interview-hub(3.7)的扣分均来自**实测到的具体事实错误**(见「已知材料错误清单」), 不是印象分。

---

## 对象总览(硬指标)

取值时间均为 2026-09-15, 来源: GitHub REST API `/repos`、`/contributors`、`/commits` 的 Link 头 last page、`/git/trees?recursive=1`。

| 对象 | Stars | 最近提交 | 许可 | 规模 | 学习成本 | 评分 |
|---|--:|---|---|---|---|:--:|
| [CMU 11-768](https://www.cmu-agents.com/) | —(课程) | L1-L6 slides / L1-L5 录像已公开 | 无 LICENSE | 23 讲 / 3 作业 + 团队项目 / A1 仓库 384 行规格 | 20-25 h(选读) | **4.9** |
| [bojieli/ai-agent-book](https://github.com/bojieli/ai-agent-book) | 47,449 | 2026-09-15 | **Apache-2.0** | 10 章 24.98 万 CJK 字 / 109 实验 / 780 MB(正文仅 ~1 MB) | 12-16 h(选读) / 30 h 全量 | **4.6** |
| [ranxi2001/zero2Agent](https://github.com/ranxi2001/zero2Agent) | 462 | 2026-09-15 | MIT(正文/代码) + PDF CC BY-NC-SA | 299 md / 243 万字符 / 693 题 | 12-16 h(选读) / 243 h 全库 | **4.2** |
| [datawhalechina/hello-agents](https://github.com/datawhalechina/hello-agents) | 79,079 | 2026-09-04 | CC BY-NC-SA 4.0 | 16 章 22.59 万 CJK 字 / 231 MB | 10 h(选读 6 章) / 30 h 全量 | **3.8** |
| [Zchary1106/agent-interview-hub](https://github.com/Zchary1106/agent-interview-hub) | 489 | 2026-09-14 | MIT | 92 md / 62.5 万字符 / 717 问句式标题 | 6-8 h(选读) / 62 h 全库 | **3.7** |
| [Stanford CS336](https://cs336.stanford.edu/) | 3,760(讲义仓库) | 2026-09-12 | A1-A4 MIT; 讲义与 A5 **无 LICENSE** | 19 讲 / 5 作业 | ≤4 h(速读) / 40 h+ 跟课 | **2.8** |
| [rasbt/LLMs-from-scratch](https://github.com/rasbt/LLMs-from-scratch) | 105,005 | 2026-09-10 | Apache-2.0(`LICENSE.txt`) | 书 368 页 7 章 / 67 ipynb | 12-14 h(选读) / 40-50 h 全书 | **2.6** |
| [Lau-Jonathan guide](https://github.com/Lau-Jonathan/LLM-Agent-Interview-Guide) | 858 | **2026-02-28(仅 1 次提交)** | Apache-2.0 | 17 md / 5.27 万字符 / 97 题 | 1-1.5 h(速查) | **2.2** |
| [datawhalechina/happy-llm](https://github.com/datawhalechina/happy-llm) | 33,823 | 2026-08-08 | CC BY-NC-SA 4.0 | 8 章 8.42 万 CJK 字 | ≤3 h | **1.8** |

注: 清单里 `datawhalechina/hello-agents` 出现两次, 是同一仓库的重复项, 本报告只评一次。

---

## 逐项取证要点

### 1. CMU 11-768「AI Agents」 · 讲次表就是靶心 JD 的目录 ★主选

**课程号更正**: AI Agents 是 **11-768**(LTI, 12 units, Fall)。**11-668 不存在** —— LTI 官方课程目录(自述收录"所有曾开设课程")中无此条目; 检索敏感性对照: `site:cmu.edu "11-667"` 能命中 11-667, 而 `"11-668"` 只返回 15-668(图形学)。报课时写 11-768。

- 学期/讲师: Fall 2026(Tue/Thu 15:30-16:50, Porter Hall 100); **Daniel Fried + Graham Neubig**; 8 名 TA。来源: https://www.cmu-agents.com/
- 23 场授课, 2 场 guest(Karthik Narasimhan / Sasha Rush)。

**关键的讲次**(逐条抄自官方 schedule, 经我抓取站点 SPA 的 JS 包验证原文):

| 讲次 | 标题(原文) | 对应缺口 |
|---|---|---|
| L2 | Agent Capabilities 1: Tool Use | **MCP 协议**(鼎桥「精通 MCP 或 A2A」) |
| L3 | Agent Capabilities 2: Context Management for Long-Context Agents | 上下文管理(MiniMax「上下文管理」) |
| L4 | Agent Capabilities 3: Skills and Memory | 记忆/Agent Skills |
| L5 | Agent Capabilities 4: Planning, Task Decomposition, and Multi-Agent Coordination | Multi-Agent 编排 |
| L6 | Domains 1: Coding Agents | **Agent Harness / SWE-bench 评测链**(腾讯混元) |
| L7 | Domains 2: GUI Agents | Computer Use |
| L8-L12 | SFT / RL Basics / Advanced RL / RL Systems | 与简历重叠(可跳) |
| **L13** | **Safety 1: Sandboxing and Credential Management** | **Agent 沙箱**(蚂蚁) |
| L14 / L15 | Frameworks 1: OpenHands / Frameworks 2: LangGraph | **编排框架**(鼎桥加分项) |
| **L16** | **Safety 2: Observability and Monitoring**(Eric Wallace) | **Tracing/可观测**(腾讯混元) |
| L18 / L19 | Multi-Agent Interaction / Human-Agent Interaction | Human on the Loop(鼎桥) |
| **L20** | **Search 1: Reranking and Critic Models** | **混合检索/Rerank**(鼎桥) |

**独立复核(我亲自做的, 不是转述)**:
- 下载 `lecture-02-tool-use.pdf`(60 页)与 `lecture-06-coding-agents.pdf`(82 页), 剥取文本: L2 内 `MCP` 出现 **44 次**、`FastMCP` **13 次**; L6 内 `MCP` **0 次**。
- **L6 逐字证据**(slides 第 53 页原文): `Harness = prompts + tools + agent loop + context management` —— 这就是腾讯混元 Agent Harness 岗的岗位定义原句, 可直接用作面试口径。
- **重要限制**: L13/L16/L20 的 slides **目前 404 未公开**(实测 `lecture-13-safety.pdf`、`lecture-16-observability.pdf`、`lecture-20-reranking.pdf` 全部返回 404)。所以**沙箱/可观测性/Rerank 三块只能拿到标题, 拿不到内容** —— 这三块必须转由 `ai-agent-book` 第二/三/七章与 hello-agents 补。这一点必须清楚, 否则计划会挂空。
- 已公开: L1-L6 slides + L1-L5 录像(YouTube `PLSN0qpDfUvTM`)。
- A1 作业仓库 `cmu-agents/assignment-1` 公开: `ASSIGNMENT.md` **384 行 / 30.7 KB**(实测), 47 stars, pushed 2026-09-11, **无 LICENSE**。含 6 个测试文件与 100 分制 rubric(compaction 事件 4 分、SWE-bench patch 通过 FAIL_TO_PASS/PASS_TO_PASS 8 分、sandbox 执行 6 分、trajectory replay 6 分)。
- A1 任务: Part 1 在 ReAct 框架下从零实现 agent harness(ReAct 循环 / `build_prompt` / `execute_tool_calls` / **坏 JSON 与未知工具必须变成可恢复 observation 而非异常**) + 在 Modal sandbox 执行模型写的代码; Part 2 实现 `compact_context`(6000 token 阈值下做 compaction A/B, 交 `artifacts/token-usage-analysis.md`); Part 3 ChessAgent 工具。

**硬伤**: A2/A3 作业仓库与 handout **未公开**(自学者无法复现 Eval / Training 两个作业); A1 需自付 Modal + LLM API(课程 credits 只给注册学生); 课程站与 A1 仓库**均无 LICENSE**; lecture highlights 占 10% 且**禁止用 AI 写**(但作业允许用 AI, 只要求能解释与捍卫 —— 与你 134-commits 的 agentic 工作流兼容)。

**读法**: Day1-4 精读 L1-L6 slides(重点 L2 的 MCP 段、L3 长上下文、L4 memory/skills、L5 planning/multi-agent、L6 harness 与 SWE-bench 评测链); Day5-11 做 A1 Part1+Part2(跳 Part3); Day12-14 对 L13/L16/L20 按标题自建概念卡, 内容从 `ai-agent-book` 取。**明确不做**: 团队项目、lecture highlights、L8-L12 训练部分、L17-L19、L21-L23。

**评分 4.9/5** —— 唯一一门讲次表与靶心 JD 逐条对齐、且材料已公开到可直接动手的课程。

---

### 2. bojieli/ai-agent-book(李博杰) · 唯一写透可观测性与 Eval Harness 的正文体系 ★主选

- 47,449 stars / **2026-09-15 当天仍在提交** / 近 3 个月 **1,626 次提交**(是 hello-agents 的 34 倍) / 贡献者 83 / open issues 仅 28。
- **唯一 Apache-2.0**(LICENSE 11,338 字节标准全文)—— 可商用、可引用进公司材料, 另外两个 datawhale 项目是 CC BY-NC-SA(禁商用)。
- 正文字数: 10 章 **249,796 CJK 字符**(剔除代码块 248,220, 说明正文几乎不塞长代码); 每章有「本章小结 + 思考题」, 且根目录有 `book/reference-answers.md`(68,658 B)—— 三份教材中**唯一给出习题答案**的。
- **作者可核实**: `book/introduction.md` 自述 Pine AI 首席科学家、2025 年图灵《AI Agent 实战营》、2024-2026 中科院大学授课; GitHub 账号 `bojieli` 的 `blog: 01.me` 与站点 title「Bojie Li (李博杰)」一致。纸质出版信息未获一手确认(图灵站点为 JS 渲染, 未取得书目页) `[INFERENCE]`。

**体积陷阱(必须知道)**: 仓库 780 MB, 其中 **691.6 MB 是实验 JSON 轨迹**(单文件最大 47.3 MB)、106 MB 是 15 种语言译本、160 MB 图片、73 MB 视频。**中文正文就是 `book/chapter1.md` ~ `book/chapter10.md`, 合计约 1 MB**。
→ 禁止 `git clone` 全仓。用 `git clone --depth 1 --filter=blob:none --sparse` + `git sparse-checkout set book`, 或直接下 Releases 的 PDF。

**缺口命中(7/8, 关键词实测计数)**:
- **可观测性/Tracing**: 第七章「Agent 的可观测性」直接给出数据结构 —— 「一次任务执行对应一条 **trace**, 每个 LLM 调用、每次工具调用、每次检索都是一个 **span**……**OpenTelemetry** 是通用的分布式追踪标准, **OpenInference** 在其上定义 LLM 应用语义约定」, 并点名 LangSmith / Langfuse / Arize Phoenix。**这是腾讯混元岗最硬的一段材料。**
- **Eval Harness**: 第七章整章 —— τ²-bench 任务解剖、评估指标「成功的定义」、评估数据集设计、评估驱动的模型选型、**统计显著性**(McNemar/bootstrap/Elo)、「从外部评估到内部评估: 生产级 Agent 的评估基础设施」。
- **混合检索**: 第三章给出 BM25 完整公式(`Score(Q,D)=Σ IDF_BM25(q_i)·TF(q_i,D)(k1+1)/(TF(q_i,D)+k1(1-b+b·|D|/avgdl))`) + 手算复现日志 + 从零实现 BM25, 并有 4 嵌入 × 3 reranker × 2 主模型 × 60 用例 = **1,440 条真实轨迹**的检索矩阵(记为 Complete)。
- **沙箱**: 第四章「执行工具的安全机制」讲进程级/容器/microVM 三级隔离, 明确「容器与宿主机共享内核, 内核漏洞仍可能逃逸」。
- **A2A**: 第十章「跨组织协作: A2A 协议」一节(12 次命中, 概念级, 无 SDK 实操)。
- **Multi-Agent**: 第十章专章(分类框架 + 六种失败模式 + 何时真正优于单 Agent)。
- **上下文工程**: 第二章整章(KV Cache 友好设计、Agent Skills 按需加载、状态栏、上下文压缩)—— **这是「Harness 工程」最硬的一章, 也是你简历里没有系统化过的内功**。
- 未覆盖: **K8s 命中 0 次**(仅 Docker 级概念)、vLLM 仅 5 处提及无专章、CrewAI/AutoGen 具体 API、A2A SDK。

**读法(10 小时核心)**: 第七章 3.5h(评估+可观测性) → 第二章 3.0h(上下文工程) → 第三章 1.5h(BM25/混合检索) → 第十章 1.0h(多 Agent + A2A) → 第四章 0.5h(MCP 定位 + 隔离分级) → 第一章 0.5h(取「Agent = Model + Harness」统一表述)。**建议暂不读**: 第五章 Coding Agent(你的 134-commits 重写就是这一章的活样本)、第六章 交互、第八章 后训练(与 BERT/DPO 重叠)、第九章 持续进化。

**评分 4.6/5** —— 「验收 harness 73/73」→「生产级 eval pipeline + trace 模型」的术语与体系升级, 全靠这一本。

---

### 3. ranxi2001/zero2Agent · 693 题一手面经 + 可跑实验 ★次选(需纠偏)

- 462 stars / **2026-09-15 当天仍在提交** / 204 commits / 近 3 月 58 次 / 贡献者 2。
- 许可: 正文与代码 MIT; **PDF 绿皮书单独为 CC BY-NC-SA 4.0**(`publish-pdf/templates/metadata.yaml`), 仓库 MIT 不等于 PDF 可商用。

**取证亮点(实测, 不采信宣称)**:
- **题量对上**: `^#{2,3} Q：` 实测 **693 条**, 与宣称的 693 道**逐条一致**; 每题配 `**新手答**`/`**高手答**` 分层(实测 702/706 处)与 `**差距在哪**` 评语。
- **溯源率高**: 693 题对应 **735 条 `> 来源：`**(含公司+轮次+一手链接), `nowcoder.com` 链接 1,040 条(**去重后 251 条**), 抽查返回 HTTP 200 非死链。
- **引一手规范**: `02-tool-management` 明确引「MCP 2025-11-25 Transport 规范」并纠正「HTTP/SSE 不是并列新传输」这一常见错误。
- **代码真能跑**: `examples/agent-api-lab/` 是无 API Key 的确定性 FakeProvider 实验; 实测 `python3 examples/agent-api-lab/run_lab.py` 退出码 0, `python -m unittest discover` **Ran 18 tests, OK**。含 11 个协议场景 + 4 种上下文消融(`drop_assistant_call`/`mismatch_call_id`/`flatten_roles`/`sliding_window`)。
- 缺口覆盖 **8/8**: A2A 100 次、Harness 487、沙箱 226、Kubernetes 87、混合检索 58、RRF 37、Rerank 172、BM25 157、LangGraph 271、AutoGen 41、vLLM 39、OpenTelemetry 13(全库口径)。
- 有工程化纪律: `AGENTS.md` 要求「对快速变化的工具/API 先验证一手来源并区分事实与推断」; `THIRD_PARTY_NOTICES.md` 逐个列上游仓库+commit+许可(含 `bojieli/ai-agent-book@e3883f8c`)。

**硬伤(我独立复核确认)**:
- **A2A 段含规范级错误** —— 我去 A2A 官方仓库核对: 规范全文 `BLOCKED` 出现 **0 次**; 而 `/.well-known/agent-card.json` 出现 **5 次**、`/.well-known/agent.json` **0 次**。zero2Agent 那两处(`TaskState.BLOCKED`、`agent.json` 路径)都是错的。**照抄会在面试里被打脸。**
- 语料偏实习/校招: 「实习」198 次、「秋招」29、「校招」9, **「社招」仅 10 次**; 归档 120 篇里标题含社招的仅 3 篇。你是在职社招 7 年, 项目深挖样本少于实习框架题。
- 693 题含大量同题变体(如 `16-agent-infra` 28 题中 Kubernetes 相关 5 题), 去重后独特考点估计 300-400 个。
- 全库 243 万字符 ≈ 243 小时, **必须切片读**。
- `nowcoder` 抓取归档 117 篇属平台内容再分发, 许可灰色地带。

**读法(12-16 h)**: 精读 `learn-agent-interview/` 的 `16-agent-infra`(Runtime/Sandbox/K8s/Checkpoint 恢复/fencing token/CRD/Ray 调度)、`06-multi-agent-collab`(**先对照 A2A 现行规范纠错再读**)、`05-eval-and-vision`、`02-tool-management`、`09-rag-retrieval`(只读 RRF/混合检索/重排章节切片, 不读 203 KB 全文); 跑一遍 `examples/agent-api-lab`(含 `--all-ablations`); 读 `final-project/02-architecture` + `09-sub-agent` 当项目叙述模板。**不读**: `learn-agent-training`、`learn-agent-basic`、`learn-agent-training`、`learn-pi`/`learn-codex`(只在被问具体框架时速查)。

**评分 4.2/5** —— 面经密度与可跑实验是同类最强, 但必须在纠偏后使用。

---

### 4. datawhalechina/hello-agents · 唯一有 MCP+A2A+ANP 三协议专章 + 可跑代码

- 79,079 stars / 2026-09-04 / 贡献者 106 / 近 3 月仅 **48 次提交** / open issues 204(积压明显)。
- 许可: **CC BY-NC-SA 4.0**(LICENSE.txt 与 happy-llm 的 LICENSE.txt **md5 完全相同**: `fb5d051e53001fdff7fec0f368f47190`)—— 个人学习可用, **禁止商用**。
- 16 章全部有真实正文, 共 225,937 CJK 字符。
- **A2A 是最直接的动手材料**: 第十章「智能体通信协议」10.2/10.3/10.4 分别为 MCP/A2A/ANP, `code/chapter10/` 下有 6 个 A2A 相关脚本(`09_A2A_Server.py`、`09_A2A_WithAgent.py`、`10_A2ATool_Simple.py`), 并含 `weather-mcp-server/`(带 Dockerfile 与 smithery.yaml)。
- 第十二章 Eval 有真实工具链: 12.2 BFCL(含官方 `bfcl-eval` 包与 numpy 冲突的实操提示)、12.3 GAIA, 并附实际评估报告样例。
- 第六章有 AutoGen / AgentScope / CAMEL / **LangGraph** 四套各自带 requirements 的 demo。

**硬伤**: 12 章有「习题」但**没有章节习题答案**(`Extra-Chapter/Extra01-参考答案.md` 是**面试题**答案, 248 KB, 82 个标题); 第一章示例代码有未修 bug(2026-09-10 的 issue 与 PR 至 9-15 仍 open); 第五章是 Coze/Dify/FastGPT/n8n 平台截图教程(21,498 字 + 87 张截图), 时效最差且对工程岗无用; 无可观测性专章(6 处均为形容词式描述, 无 span/trace 数据模型)、无 K8s、混合检索仅提及。

**读法(10 h)**: 第十章 3.0h(A2A 精读 + 跑通 `09_A2A_WithAgent.py`) → 第十二章 2.0h(BFCL/GAIA, 把 73/73 改写成 Eval Harness 口径) → 第九章 1.5h(上下文工程) → 第四章 1.5h(ReAct/Plan-and-Solve/Reflection 手写一遍保证能白板画) → 第六章 1.0h(三框架编排范式差异) → 第八章 1.0h(RAG 三阶段对齐术语)。**跳过**: 第二/三章(发展史/LLM 基础)、第五章、第七/十一/十三至十六章。

**评分 3.8/5** —— A2A 动手 + 评估实战值这个分; 其余章节对你是纯消耗。

---

### 5. Zchary1106/agent-interview-hub · 有「证据表」的国内大厂面经聚合

- 489 stars / 2026-09-14 / MIT / **贡献者仅 1 人** / 74 commits / 近 3 月 51 次。
- 实测题量: 2-4 级标题 1,780 条中 **717 条为问句式**; 「参考答案」标记 **240 处**; `八股文完整答案集.md` **69 题**(与宣称一致); 12 道手撕题中 **7 道是 Agent 工程题**(RRF 融合排序、Tool 参数校验、带循环守卫的 ReAct、幂等工具执行器、流式 SSE 解析、Agent Trace 聚合、多 Agent 文件冲突检测)。
- **独有优势**: `data/question_signals.json` 12 条「题目-证据」信号(companies/rounds/source_count/last_seen_at/answer_outline), 明确写 "Frequency means distinct source records in this repository, not total interviews across the market" —— 面试前可直接按 `source_count` 排序刷题。`data/interviews.json` 62 条结构化来源(含 `source_url`/`confidence`/`verified_at`)。
- 源码引用**部分可核验**: `Agent Harness与编码代理测评.md` 引的 Codex 路径逐条在 `openai/codex` HEAD 树里存在(`codex-rs/core/src/exec_policy.rs`、`otel_init.rs`、`linux-sandbox/`、`compact.rs`、`mcp.rs` 等全部命中), `AskForApproval` 的 `Never/OnRequest/UnlessTrusted/Granular` 四值也逐字对上。

**硬伤**: **4 个公司目录是 TODO 空壳**(`微软/岗位要求.md`、`谷歌/`、`商汤科技/`、`初创公司/` 全文仅一行 `> TODO: 补充岗位 JD`), 但 README 给这 4 家打了 ✅; **Claude Code「源码入口」在公开仓库不存在**(anthropics/claude-code 顶层无 `src/` 目录, 1,590 条 path 实测), 照文档去读会 404; **MCP 注解字段写错**(写 `annotations: {"readOnly": true}`, 规范实为 `readOnlyHint/destructiveHint/idempotentHint/openWorldHint`); **Langfuse 示例是 v4 已删除的旧 API**(`@langfuse.trace()` 装饰器 + `langfuse.span()`, v4 只有 `start_as_current_observation`)。

**读法(6-8 h)**: 精读 `通用知识/2026-Agent工程化新考点.md`、`Agent-Harness评测与发布门禁.md`(讲 Task/Environment/Outcome Spec 与三层 Grading、Shadow/Canary)、`MCP与工具生态.md`、`Agent Harness与编码代理测评.md`(**Codex 部分精读, Claude Code 部分跳过**); 刷 `高频拷打题-牛客热帖.md` 33 题 + `八股文完整答案集.md` 69 题自测; 速读 `data/question_signals.json` 12 条信号决定优先级。**不读**: 学习路线图(16 周计划对你只剩 2 周无效)、两个 `index.html`(生成物)、`模型微调完全指南.md` 与 `大模型推理优化与部署.md`(与简历重叠 + 非目标岗)。

**评分 3.7/5** —— 适合当「当日抽检 + 话术校准」工具, 不适合当主线教材。

---

### 6. rasbt/LLMs-from-scratch(书 + 仓库) · 防守型弹药

- 书: Manning 2024-09, ISBN 9781633437166, **368 页 7 章** + 附录 A-E, **无第二版、非 MEAP**; 配套仓库 105,005 stars / 69 贡献者 / 近 3 月 11 次提交 / `LICENSE.txt` 实为 **Apache-2.0**(GitHub 误报 NOASSERTION)。
- **中译本**: 《从零构建大模型》, 人民邮电/图灵, 2025-04-03, 325 页, **ISBN 978-7-115-66600-0**, 译者覃立波/冯骁骋/刘乾; **中译本额外含附录 F「理解推理大语言模型」**(英文原版无)。来源: 图灵官方 API `api.ituring.com.cn/api/book/3382`。
- **可跑性经实测**(不是静态检查): 用 `torch 2.14.0+cpu` 无 GPU 逐 cell 执行 ch02/ch03 —— **ch03 37/37 输出与仓库保存值逐字一致**(含 `SelfAttention_v1/v2`、`CausalAttention`、`MultiHeadAttentionWrapper` 全部张量值), ch02 49/49 一致(另精确复现了刻意保留的 `KeyError('Hello')`), 无 TODO。三平台 CI 在跑 `pytest --nbval`。
- 21 道章末练习**全部有 `exercise-solutions.ipynb`**; 另有免费 170 页 "Test Yourself" PDF。

**硬伤(对你而言)**: 正文**停留在 GPT-2 世代** —— ch04 正文中 `kv cache`/`GQA`/`MoE`/`RoPE`/`RMSNorm` 命中 **0**, ch05 正文中 `vllm`/`tensorrt`/`quantiz`/`flash` 命中 **0**; KV cache、现代架构、DPO 全在仓库 README 的 "Bonus Material"(且 `ch05/` 下已有 17 个 bonus 子目录含 Qwen3.5/Gemma 4/DeepSeek Sparse Attention —— 把它当"书的内容"读会无限膨胀); 分布式训练全书只有 `appendix-A/DDP-script.py` 一个脚本; 作者明确**不接受扩展主章节代码的 PR**。
- **缺口覆盖为零**: A2A、Multi-Agent 编排、混合检索、沙箱/K8s、Tracing/Eval pipeline、vLLM —— 6 项全不覆盖。

**读法(12-14 h)**: Ch3 注意力 3.5h(精读+跑码) → Ch4 GPT 实现 3h → 附录 D 训练循环增强 1h → 附录 E LoRA 1.5h → bonus `ch04/03_kv-cache` 1.5h → Ch1/Ch2/Ch5/Ch7 速读 5h。**跳**: Ch6 分类微调(与 BERT 项目重叠)、附录 A(PyTorch 入门)、其余 bonus。
**定位**: 被问到 transformer/KV cache/微调原理时**不露怯**的防守弹药, 不是进攻弹药。作者也未把它定位成 Agent 岗用书。

**评分 2.6/5**(若只评「LLM 原理口径」则是 4.5/5)。续作《Build a Reasoning Model》2026-06 出版(440 页, 仓库 5,231 stars)**1.5/5, 不建议投入**。

---

### 7. Stanford CS336「Language Modeling from Scratch」 · 质量极高但方向错了

- Spring 2026(第 3 次开课), **Percy Liang + Tatsunori Hashimoto**, 5 units, 19 讲, 5 个作业; 讲义仓库 3,760 stars / pushed 2026-09-12。
- 「从零」是字面意义的: A1 starter 目录只有 150 B 的 `__init__.py`, 明令禁用 `torch.nn.functional`/`torch.optim` 现成实现; A4 的 `cs336_data/` 官方注释直接写 "This folder is basically empty!"。
- 公开度好: 5 份 handout(A1 **47 页**/A2 48/A3 8/A4 16/A5 39)、公开 `tests/adapters.py` 机制(本地 pytest 可自测)、公开 leaderboard、YouTube playlist。

**致命三条**(对你的窗口而言):
1. **成本**: A1 handout 标注合计约 17 B200-h(leaderboard 单次提交限 45 分钟/0.75 B200-h), A5 约 26 B200-h —— **A1+A5 ≈ 43 B200-h ≈ $270**(按官方 Modal $6.25/h); 还有 A2 的 ≤6 卡、A4 的 **8×B200**。两周窗口内不可行。
2. **方向**: 19 讲里 **没有任何一讲**涉及 agent harness / tool calling / MCP / multi-agent / sandbox / tracing。L10 Inference 是模型侧(KV cache/量化/并行解码), 不是服务化工程。缺口清单 8 项它覆盖约 1-2 项。
3. **政策冲突**: A1 handout 原文「AI tools are not permitted for implementing any part of any assignment. This includes both coding agents (e.g., Cursor Agents, Codex, Claude Code)」并要求关闭 IDE 自动补全 —— 与你 134-commits 的 agentic 工作流**直接冲突**。

**许可风险**: `lectures` 仓库与 `assignment5-alignment` 仓库**无 LICENSE**(API 返回 `license: null`), 只有 A1-A4 是 MIT —— 把讲义搬进公开仓库/博客有风险。

**读法**: **≤4 小时**, 只读 L2(resource accounting/FLOPs/arithmetic intensity)、L10(Inference)、L12(Evaluation), 选读 L6(Triton/kernel)、L16(RLVR/GRPO 术语, 衔接你的 DPO 经验)。**不做任何作业**。

**评分 2.8/5** —— 机会成本论证: 同样 20 小时给 CMU 11-768 至少命中 8 项缺口。若只想解决「vLLM/TensorRT-LLM 私有化部署」这一个 JD 加分项, **直接读 vLLM 官方文档的 PagedAttention/continuous batching 两页 + TensorRT-LLM README 的 feature 列表, 2 小时搞定**, 不要绕道 CS336。

---

### 8. datawhalechina/happy-llm · 强项与你的简历重叠, 弱项正是你的缺口

- 33,823 stars / 2026-08-08 / 贡献者 33 / **近 3 个月仅 3 次提交** / open issues 69 且 2026-08-24 的实质技术 issue(`第二章 transformer.py 维度不一致`、`章节5: SFT 无法学习到 im_end`)**至 9-15 未处理**。
- 「从零手写」成色真实: 第二章手写 Transformer(14 KB)、第五章手写 LLaMA2(36 KB `k_model.py`)并训出 215M 参数模型(ModelScope 可下载验证); 第八章 GRPO/On-Policy Distillation/Search-R1/ReTool **四条线各有同步版+异步版完整实现**。
- **硬伤**: `docs/chapter6/6.4[WIP] 偏好对齐.md` **文件名自带 `[WIP]`**、仅 2,754 CJK 字、**全文 grep `DPO|KTO|PPO|直接偏好` 命中 0 次** —— 一个标题为「通过强化学习进行偏好对齐」的章节里完全没有 DPO; 文内小节编号还从 6.4 跳到 `## 7.2.2`。Agent 侧极薄: 第七章整章仅 6,854 字, Agent 只是 7.3 一节; 全库 `MCP` **0 次**、`A2A` **0 次**。
- 许可: CC BY-NC-SA 4.0。

**读法**: **不建议投入 10 小时**。若一定要花, 上限 3 小时: 第八章 8.4 ReTool(尤其 8.4.5「代码执行环境与安全边界」+ `retool/sandbox.py`, 注意其自述「**不能提供可信安全隔离**」)1.5h、8.1 GRPO 1.0h、6.3 高效微调 0.5h。

**评分 1.8/5** —— 强项(Transformer/SFT/PEFT/RLHF 训练)与你 2023-2024 的 NLP 履历高度重叠, 弱项(Agent 协议/编排/评测/可观测)正好是你的缺口。**投入产出比在 2 周窗口里最差。**

---

### 9. Lau-Jonathan/LLM-Agent-Interview-Guide · 停更半年的算法侧八股速查

- 858 stars 但 **提交历史只有 1 条**(2026-02-28 创建即提交, `🎉 Initial commit: LLM & Agent Interview Guide - 300+ 面试题`), **6.5 个月零更新**, 0 issue / 0 PR; README 却写「🔄 持续更新中...」, 徽章 `Last Updated 2026.02`。
- 许可: GitHub 标 NOASSERTION 但 `LICENSE` 实为 Apache-2.0 全文(API 识别失败仅因版权行被改写)—— 无使用限制。
- 题量虚高: README 声称 300+, 实测 `^### Q:` **97 条** + 10 道手撕 + 11 场面经 97 条编号题(与前模块同题反复) → 去重后约 110-130 道; 全库仅 **52,728 字符 ≈ 5.3 小时**。
- 答案抽查 3 题全对(自注意力 `1/√d_k` 方差推导、KV Cache 显存公式手算 2 GiB 与文中一致、MHA/MQA/GQA/MLA 对比表); 无 AI 水文特征。
- **硬伤**: 全库 `A2A`、`Harness`、`Kubernetes/K8s`、`OpenTelemetry`、`Tracing`、`可观测` 命中**均为 0**; `LangGraph/CrewAI/AutoGen` 各仅 1 次(且都在同一张框架推荐表里)。2025 下半年以后的 Agent 工程化主线**整块缺失**。11 场面经带公司/团队/月份但 **0 条链接、0 个 ID**, 无法回溯。

**读法**: **1-1.5 小时**。只读 `05-Agent/01-Agent-Complete-Guide.md`(校对 MCP/Function Calling 表述)、`04-RAG/01-RAG-Complete-Guide.md`(RRF 公式快速回忆)、`08-Coding/01-Coding-Problems.md` 的 2 题(自注意力 + LoRA 当手撕热身)。`10-RealQuestions` **直接跳过**(无源)。

**评分 2.2/5** —— 最大风险是"看着全": 10 个模块标题覆盖到 Agent/系统设计, 但 Agent 全章仅 6.7 KB, 读完仍答不出「沙箱怎么隔离」「eval harness 怎么建」。

---

## 缺口覆盖矩阵(核心决策依据)

缺口来自 `A级岗位精读.md` 的「面试前必补」+ `04-技术复习清单.md` P0。

| 缺口项 | CMU 11-768 | ai-agent-book | zero2Agent | hello-agents | agent-interview-hub | rasbt | CS336 |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| MCP 协议细节 | **L2 slides 44 处 MCP** | 一章一节(77 次) | 856 次 | **专章+代码** | 专章 20 题 | 无 | 无 |
| **A2A 协议** | 公开材料未见 | 一节(12 次) | 100 次(**有错**) | **专章+可跑代码** | 概念级 | 无 | 无 |
| Multi-Agent 编排 | L5 + L14/L15 + L18 | 专章 | 专章(LangGraph 271) | 专章+4 框架 demo | LangGraph 203 | 无 | 无 |
| 混合检索 + Rerank | L20(幻灯片未公开) | **第三章实现级+公式** | 检索专章 | 提及 | Rerank 122 | 无 | 无 |
| Agent 沙箱 / K8s | L13(未公开) | 三级隔离专节(无 K8s) | 226 次 + K8s 87 | 提及 | 88 次 | 无 | 无 |
| Tracing / 可观测性 | L16(未公开) | **第七章 span 树 + OTel** | 111 次 | 提及(无数据模型) | 专章 | 无 | 无 |
| Eval pipeline / Harness | **L6 harness 定义 + SWE-bench** | **第七章整章 + 42 实验台账** | 487 次 + 专章 | 第十二章 BFCL+GAIA | 三层 Grading | 无 | 部分思想 |
| vLLM / 推理框架 | 无 | 弱(5 处) | 39 次 | 1 处 | 专章 68 次 | bonus KV-cache | A5/L10 |
| 上下文工程 / 记忆 | L3 + L4 | **第二章整章** | 专章 | 第九章 | 专章 | 无 | 无 |
| **合计命中** | **8/9(含未公开标题)** | **7/9** | **8/9** | 6/9 | 7/9 | 1/9 | 1.5/9 |

> **读法要点**: 三个「主选/次选」是**互补**而非替代关系 —— CMU 提供学院派术语与 harness 定义(但 L13/L16/L20 无内容), ai-agent-book 提供可观测性与检索的实现级血肉, zero2Agent 提供面经话术与沙箱/Infra 场景题。**单吃任一个都有明显空洞。**

---

## 学习路线(7 天 × 2h, 共 14 小时)

对齐 30 天计划的第 3-4 周(面试强化 + 真实面试), 按「竞岗考点速查」的岗位优先级排(鼎桥/腾讯 > 蚂蚁 > MiniMax)。

| Day | 内容 | 时长 | 产出 |
|:--:|---|:--:|---|
| D1 | **CMU 11-768 L2 Tool Use slides**(MCP host/client/server、`FastMCP.from_openapi` OpenAPI 投影、**credential brokering/凭据分离**、OpenAPI vs MCP 四维对比) | 2h | MCP 口径升级: 从「我做过 9 个工具」→「我知道协议在 auth/discovery 维度的边界在哪」 |
| D2 | **CMU L3(长上下文) + L6(Coding Agents)**: 抄下 `Harness = prompts + tools + agent loop + context management` 与 SWE-bench `FAIL_TO_PASS/PASS_TO_PASS` 评测链 | 2h | 腾讯混元岗的岗位定义原句 + 评测链术语 |
| D3 | **ai-agent-book 第二章 上下文工程**(KV Cache 友好设计 / 动态提示词与 Agent Skills / Agent 状态栏 / 上下文压缩) | 2h | 「Harness 内功」——你简历里唯一没系统化过的部分 |
| D4 | **ai-agent-book 第七章 Agent 的评估**(重点: Agent 的可观测性 / 生产级评估基础设施 / 统计显著性 / τ²-bench 解剖) | 2h | 把「验收 harness 73/73」改写成 eval pipeline + trace/span 模型 |
| D5 | **跑 CMU A1 Part1** + 读 ai-agent-book 第三章 BM25/混合检索段 | 2h | 白板能画 ReAct 循环 + 能推 BM25/RRF 公式 |
| D6 | **CMU A1 Part2**(`compact_context` 6000 token 阈值 A/B + `token-usage-analysis.md`) | 2h | 可讲的「上下文压缩代价」分析(直接对应 MiniMax 上下文管理) |
| D7 | **zero2Agent `16-agent-infra`**(Sandbox/K8s 控制链路/Checkpoint 恢复/fencing token/CRD/Ray) + `06-multi-agent-collab`(**先按 A2A 现行规范纠错**) | 2h | 沙箱与 Agent Infra 的场景题话术 |

**每日穿插 15 分钟**(不占上表时长): 用 `agent-interview-hub` 的 `data/question_signals.json` 12 条信号按 `source_count` 排序抽检 + `八股文完整答案集.md` 69 题自测。

**若目标含鼎桥(必考 A2A)**: D3 或 D5 替换为 **hello-agents 第十章 A2A 段 + 跑通 `code/chapter10/09_A2A_WithAgent.py`**(2h)。

### 应急 3 天版(面试已排, 只剩 6-8 小时)

1. **CMU L2 + L6 slides**(3h): MCP 协议边界 + harness 定义 + SWE-bench 评测链。
2. **ai-agent-book 第七章「Agent 的可观测性」一节 + 第三章 BM25 段**(2h): span/trace 数据模型 + 混合检索公式。
3. **hello-agents 第十章 A2A 段**(1.5h): Agent Card + 任务状态机(**用现行规范口径, 不用 zero2Agent 的**)。
4. **agent-interview-hub 的 69 题八股自测**(1h): 找盲区。

### 与现有材料的衔接(避免重复投入)

仓库里已有 `05-技术补课-P0.md`(12 主题面试口径)与 `06-本周补课讲义-W1.md`(4 课 + 10 题自测)——它们已解决「**术语化 + 60 秒口径**」。本报告推荐的外部材料解决的是**另一件事**: 「能讲出实现深度与失败案例」。分工:

| 主题 | 已有材料(P0/W1) | 外部材料补什么 |
|---|---|---|
| MCP | 三角色/三原语/Streamable HTTP 口径 | CMU L2: 凭据分离、OpenAPI 投影、auth/scope 边界 |
| A2A | MCP vs A2A 一句话区分 | hello-agents 第十章: Agent Card 字段、任务状态机、可跑代码 |
| 混合检索 | RRF 公式 + rerank 位置 | ai-agent-book 第三章: BM25 完整公式 + 1,440 轨迹实证 |
| 沙箱 | cgroup/namespace 概念 | CMU L13 标题 + zero2Agent: K8s 控制链路、fencing token |
| Tracing | OTel trace/span/attribute 概念 | ai-agent-book 第七章: span 树建模 + OpenInference 约定 |
| Eval Harness | 「73/73 就是 Harness」话术 | ai-agent-book 第七章 + hello-agents 第十二章: 从外部到内部评估的基础设施 |

---

## 已知材料错误清单(引用前必须纠偏)

这几条是实测确认的**事实错误**, 照抄会在面试里被打脸。

| # | 出处 | 错误内容 | 现行事实 | 证据 |
|:--:|---|---|---|---|
| 1 | zero2Agent `06-multi-agent-collab` | 「A2A 的 `TaskState.BLOCKED` 标记任务卡死, 协议层自动设置并触发超时回收」 | **A2A 规范无 `BLOCKED` 状态** | A2A 规范全文 `BLOCKED` 命中 **0 次**(我实测) |
| 2 | zero2Agent `06-multi-agent-collab` | Agent Card 路径 `/.well-known/agent.json` | 现行为 **`/.well-known/agent-card.json`** | 规范中 `agent-card.json` 出现 **5 次**, `well-known/agent.json` **0 次**(我实测) |
| 3 | agent-interview-hub `MCP与工具生态.md` | 工具注解 `annotations: {"readOnly": true}` | MCP 规范为 `readOnlyHint` / `destructiveHint` / `idempotentHint` / `openWorldHint` | `modelcontextprotocol/schema` 2025-11-25 |
| 4 | agent-interview-hub `Agent安全与评估体系.md` | Langfuse 用 `@langfuse.trace()` 装饰器 + `langfuse.span()` | Langfuse **v4(2026-03 重写)已移除**, 现为 `start_as_current_observation` | `langfuse/langfuse-python` main 分支 |
| 5 | agent-interview-hub `Agent Harness与编码代理测评.md` | 给出 6 处 Claude Code「源码入口」如 `src/Task.ts` | anthropics/claude-code 公开仓库**无 `src/` 目录** | `git/trees/HEAD?recursive=1` 1,590 条实测 |
| 6 | agent-interview-hub 公司目录 | README 对微软/谷歌/商汤/初创公司打 ✅ | 4 个文件全文仅一行 `> TODO: 补充岗位 JD` | 逐文件原文 |
| 7 | Lau-Jonathan README | 「300+ 面试题」「持续更新中」 | 实测 **97 条** `Q:`; **仅 1 次提交, 停更 6.5 个月** | `commits?per_page=100` 长度 1 |
| 8 | happy-llm `6.4[WIP] 偏好对齐.md` | 标题为「通过强化学习进行偏好对齐」 | 全文 `DPO|KTO|PPO|直接偏好` 命中 **0 次**; 小节编号跳到 7.2.2 | 文件原文 |
| 9 | CS336 `assignment1-basics/README.md` | 标题写 "CS336 Spring 2025" | 同仓库 handout PDF 已标 **Spring 2026 / Version 26.0.3** | README vs handout 首页 |

---

## 不做清单(明确止损)

- 不通读任何一套教程。全库体量: zero2Agent 243 h / agent-interview-hub 62 h / hello-agents 30 h / ai-agent-book 30 h —— 窗口只有 14-20 h。
- 不 `git clone` ai-agent-book 全仓(780 MB, 96% 是实验轨迹/媒体/译本)。
- 不做 CS336 任何作业(A1+A5 ≈43 B200-h ≈$270, 且违反其 AI 工具禁令)。
- 不做 happy-llm 与 rasbt 书的训练侧章节(与 2023-2024 履历重叠)。
- 不读 hello-agents 第五章(Coze/Dify 截图教程)、第十三至十六章(案例)、agent-interview-hub 的两个 `index.html`、zero2Agent 的 `learn-agent-training`。
- 不引用下列无源内容: Lau-Jonathan 的 11 场面经(0 链接)、agent-interview-hub `华为/真实面经-网络实录.md`(4 条 0 链接)。被追问出处会露怯。
- 不把 CC BY-NC-SA 材料(hello-agents / happy-llm / zero2Agent PDF)放进公司内训或商用场景。

---

## 来源

**一手仓库与页面**(取值时间 2026-09-15):
- CMU 11-768: https://www.cmu-agents.com/ (schedule/syllabus/assignments 页, 含 SPA JS 包 `assets/index-C-o2vrHw.js` 原文比对) | slides `https://www.cmu-agents.com/slides/lecture-{01-agents,02-tool-use,03-context,04-skills-memory,05-planning,06-coding-agents}.pdf` | A1 仓库 `https://github.com/cmu-agents/assignment-1`(`ASSIGNMENT.md` 384 行) | 课程号核实: https://www.lti.cs.cmu.edu/misc-pages/intranet-course-info.html
- Stanford CS336: https://cs336.stanford.edu/ | 讲义仓库 `stanford-cs336/lectures` | handout: `stanford-cs336/assignment{1-basics,2-systems,3-scaling,4-data,5-alignment}`
- https://github.com/bojieli/ai-agent-book (`book/*.md`, `docs/EXPERIMENT_STATUS.md`, `LICENSE`, `pyproject.toml`, `chapter*/README.md`)
- https://github.com/ranxi2001/zero2Agent (`learn-agent-interview/**`, `examples/agent-api-lab/**`, `publish-pdf/templates/metadata.yaml`, `THIRD_PARTY_NOTICES.md`, `AGENTS.md`)
- https://github.com/datawhalechina/hello-agents (`docs/**`, `code/chapter10/**`, `Extra-Chapter/**`, `LICENSE.txt`)
- https://github.com/Zchary1106/agent-interview-hub (`通用知识/**`, `data/question_signals.json`, `data/interviews.json`, `面试算法题/**`, 公司目录)
- https://github.com/datawhalechina/happy-llm (`docs/**`, 尤其 `docs/chapter6/6.4[WIP] 偏好对齐.md`)
- https://github.com/rasbt/LLMs-from-scratch (`ch0*/**`, `LICENSE.txt`, `requirements.txt`, `pyproject.toml`) | 书页 https://www.manning.com/books/build-a-large-language-model-from-scratch | 中译本 https://api.ituring.com.cn/api/book/3382
- https://github.com/Lau-Jonathan/LLM-Agent-Interview-Guide
- A2A 规范: https://github.com/a2aproject/A2A (`docs/specification.md`) | MCP 规范: `modelcontextprotocol/schema` 2025-11-25
- 交叉核验用仓库树: `openai/codex`、`anthropics/claude-code`、`langfuse/langfuse-python`(HEAD 树, 2026-09-15)

**独立复核记录**(本报告作者亲自执行, 非转述):
- 下载并剥取 CMU L2/L6 slides 文本(`lecture-02-tool-use.pdf` 60 页 16,617 字符; `lecture-06-coding-agents.pdf` 82 页 21,138 字符), 逐字取得 `Harness = prompts + tools + agent loop + context management`; 实测 `MCP` 44 次 / `FastMCP` 13 次(L2)。
- HTTP 探测 L7/L13/L16/L20 slides 全部 **404**(故「沙箱/可观测性/Rerank」三块无内容可读)。
- 抓取 CMU 站点 SPA JS 包验证 L13/L15/L16/L18/L20 讲次标题原文。
- 核对 A2A 规范: `BLOCKED` 0 次、`agent-card.json` 5 次、`well-known/agent.json` 0 次 → 确认 zero2Agent 两处错误。
- 抓取 `cmu-agents/assignment-1/ASSIGNMENT.md`: 384 行 / 30,723 字节。
- GitHub API 取全部 9 个对象的 stars/pushed_at/license/contributors/commits/tree 元数据。

**本报告所依据的仓库内文档**: `job-search/00-锚定/profile.md`、`job-search/00-锚定/30天计划.md`、`job-search/01-岗位调研/A级岗位精读.md`、`job-search/03-面试准备/00-总览.md`、`03-目标岗位面试方案.md`、`04-技术复习清单.md`、`05-技术补课-P0.md`、`06-本周补课讲义-W1.md`、`01-简历bullet逐条深挖.md`、`个人履历终版.md`。
