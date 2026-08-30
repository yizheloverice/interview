# 求职工具选型调研报告

调研日期: 2026-08-28 | 对象: 4 年 Java/Python 开发社招, 一个月求职窗口 | 方式: 浅克隆 6 个仓库逐文件取证

## 结论(TL;DR)

| 优先级 | 工具 | 用途 |
|---|---|---|
| **主选** | get-job.skill | 岗位调研 + 改简历 + 分轮次面试准备一条龙 |
| **面试加练** | MockInterview.skill | 压力面试模拟, 隐藏评分标准 + 信心盲区 |
| **规则参考** | industry-resume-toolkit | 直接读其 `patterns/岗位/开发.md` + 企业性质 patterns, 不装也能用 |

不选理由: ASu-skills 偏校招(/offer 是校招进度, /contributor 是给无项目经历的人补经历); JobOK 面向学生/早期职场人且亮点偏投递管理; resume.skill 单用只覆盖简历环节, 与 MockInterview 同作者可组成两件套(备选)。

## 项目总览

| 项目 | Stars | 最近提交 | 许可 | 覆盖环节 | 安装方式 |
|---|---|---|---|---|---|
| [get-job.skill](https://github.com/agentenatalie/get-job.skill) | 455 | 2026-07-22 | CC BY-NC-ND 4.0 | 调研+简历+面试 | `npx skills add` 或 clone 到 skills 目录 |
| [industry-resume-toolkit](https://github.com/shangsitongshizaitiantang/industry-resume-toolkit) | 323 | 2026-06-06 | CC BY-NC-ND 4.0 | 简历(方法论/诊断/JD匹配) | `install.sh` 或手动复制 |
| [JobOK](https://github.com/GresonKwan/JobOK) | 367 | 2026-06-18 | MIT | 优势挖掘+JD匹配+简历+面试+投递跟踪 | clone 到 skills 目录 |
| [MockInterview](https://github.com/Sean-9/MockInterview) | 17 | 2026-07-28 | 声明 MIT(仓库无 LICENSE 文件) | 模拟面试 | clone 到 skills 目录 |
| [resume.skill](https://github.com/Sean-9/resume.skill) | 13 | 2026-08-10 | 声明 MIT(仓库无 LICENSE 文件) | 简历(证据闸门+压测) | clone 到 skills 目录 |
| [ASu-skills](https://github.com/Hisn00w/ASu-skills) | 2744 | 2026-08-28 | MIT | 6 入口(贡献/酥化/简历/同款/面试/进度) | Claude Code plugin 或 Codex 对话装 |

## 逐项取证要点

### get-job.skill(主选)

- 全流程三段链路, 每段产物是下一段输入: Stage 1 WebSearch 调研岗位(核心能力关键词/业务线/隐性门槛/轮次流程) → Stage 2 目标定位+能力迁移翻译改简历(python-docx 出 docx) → Stage 3 分轮次+逐 bullet 深挖+证据化审计+复盘题库。来源: `SKILL.md`。
- 交货前强制质量闸门 P0/P1/P2 + 四张覆盖矩阵(JD能力→bullet→面试准备→证据→轮次来源)。来源: `SKILL.md` 质量闸门章节。
- 诚实底线: 不编经历、背调硬信息(学历/在职时间/职位名)不可动、"AI 辅助完成"是加分项。来源: `SKILL.md` 诚实底线章节。
- 明确覆盖社招/跳槽/转行(description 含 "社招、跳槽")。
- 安装: `npx skills add agentenatalie/get-job.skill` 或 `git clone ... ~/.claude/skills/get-job`; 不装也可把 `references/*.playbook.md` 当 prompt 直接用于任意 AI。来源: README 安装章节。
- 依赖 WebSearch 能力(任支持联网的 agent runtime 均可, 不限于 Claude)。来源: `SKILL.md` Stage 1。
- 注意: 定位名为"实习.skill", 但能力描述明确覆盖社招; 许可 CC BY-NC-ND(个人使用完全允许)。

### industry-resume-toolkit(规则参考, 不装)

- 三层框架: 信息卫生(排雷: 二维码/备考证书/Office 拆分等) → 业务价值翻译(动作→业务意义) → 经历挖掘(面试官式追问补细节)。来源: `skill/SKILL.md`。
- 岗位 rules 覆盖开发岗, `skill/patterns/岗位/开发.md` 对 Java/Python 后端高度对口:
  - 技术栈分层+熟练度标注(精通/熟练/掌握, "了解"不许写精通)
  - 每条经历"业务背景+技术方案+量化结果"三段(示例: QPS 5w+ 场景、P99 320ms→45ms、DB QPS 降 80%)
  - 量化必须工程级(QPS/P99/数据量/覆盖率)而非"大幅提升"
  - 项目区分"个人贡献 vs 团队产出"(大厂面试核心追问)
  - 深度+广度兼顾, GitHub 链接加分
- 5 个 slash 命令: /诊断 /改简历 /创建简历 /JD匹配 /加规则; 输入 docx/pdf/md 输出 docx 保原排版。
- 无面试准备功能(v0.3 规划中)——这是不选它做主力而用作规则库的原因。
- 许可 CC BY-NC-ND: 可用、不可商用、不可二次分发, 个人自用无碍。README 明确说明可把 SKILL.md+patterns 当 prompt 喂给任何 AI。

### MockInterview.skill(面试加练)

- 四个 agent 角色: RECRUITER(命题带隐藏评分标准) / INTERVIEWER(考官, 凶狠度可调) / ASSESSOR(打档+信心落差) / REPORTER(维度差距报告)。来源: `SKILL.md`。
- 四阶段: 情报命题 → 模拟面试(锁定简历动作沿"工具→量级→判断→成果"逐层挖) → 差距报告(维度雷达/信心盲区/简历经历体检) → 弱项复训。来源: `SKILL.md`。
- JD+简历双输入主线; 支持 docx/pdf/图片/md/txt。结果存档 `./interview_saves/{岗位}.md` 供复训闭环。
- 只做面试, 不做简历; 与同作者 resume.skill 联动(共用经历库)。
- Stars 低(17)但功能密度高, 个人项目, 无 LICENSE 文件(SKILL.md 声明 MIT)。

### resume.skill(备选套件前半)

- 三道工序固定顺序: 证据闸门(数字只能来自用户原话, 追溯不到素材 ID 不生成) → 八秒排序(HR 前 8 秒三行) → 4 层追问链压测。来源: `SKILL.md`。
- 四角色(MINER/GATEKEEPER/WRITER/STRESSER), 8 Phase 逐阶段人机确认; 经历库 `jobsearch/profile.md` 复用, 换 JD 不用重采。
- 输出 docx/pdf/md/txt(随包 Noto Sans CJK 字体离线渲染); 每版附修改说明表+差距清单+追问清单。
- 与 MockInterview 联动闭环"简历→面试"。作为 get-job.skill 的备选(更重人机交互, 每步要确认, 耗时)。

### JobOK(备选)

- 证据驱动全流程: Intake → 经历资产库 → 优势挖掘(证据→行为→能力→岗位信号) → 目标岗位假设(3-5 个) → JD 标准化(jobs.jsonl) → 确定性打分(score_job_matches.py, 60 分以下观察池) → 简历优化(needs_proof 标记) → 面试故事库 → 投递跟踪 CSV。来源: `SKILL.md`。
- 有独立 Python 脚本可脱离 agent 用: extract_resume_text.py / normalize_jobs.py / score_job_matches.py。
- description 面向学生/实习/早期职场人("students, interns, or early-career"), 社招可用但非其主场景。
- MIT, 文档最全(install-cn/usage-cn/faq-cn/safety-cn)。

### ASu-skills(不选)

- 6 入口: /contributor(开源贡献补经历) /asu(经历酥化) /make-resume(可编辑 HTML 简历) /asu-resume(高密度技术简历) /interview(简历问穿) /offer(校招进度)。
- 核心受众是应届/实习生: /offer 明确"校招进度管理", /contributor 主打"缺少可验证项目或协作经历"。4 年社招用不到主链路。
- 但 stars 最高(2744)、今天还在提交、跨 Claude Code/Codex/Trae/opencode/deepseek 多平台插件; 若用其 /asu + /interview + /make-resume 三小块亦可, 只不是最优选择。

## 使用路径(30 天)

前置: 确认本机有 Claude Code 或 Codex(支持 Agent Skills 的 runtime)。

1. 安装主工具: `npx skills add agentenatalie/get-job.skill`(或 `git clone https://github.com/agentenatalie/get-job.skill ~/.claude/skills/get-job`)。
2. 第 1 周 锚定与简历: 用 `SKILL.md` Stage 0 锚定 2-3 个目标方向(Java 后端/数据工程/平台工程等) → Stage 1 让 AI 调研目标公司岗位(拿真实 JD 和轮次) → Stage 2 改简历; 同时把 industry-resume-toolkit 的 `patterns/岗位/开发.md` 和对应企业性质 patterns 内容作为规则喂给 AI(可直接问 AI 读本地文件或用 paste 方式)。
3. 第 2 周 定向投递: 每投一个岗位跑一遍 JD 定向版本; 简历定稿后跑 get-job Stage 3 的"简历 bullet 逐条深挖"(防止面试被问穿)。
4. 第 3 周 模拟面试: 装 MockInterview.skill(clone 到 skills 目录), 先"练习模式+深挖技术", 临考前每日一次"考试模式+Bar-raiser"; 用其"信心盲区"定位自以为会实则弱的题。
5. 第 4 周 真实面试与复盘: 面完跑 get-job `99-面后复盘题库.md` 复盘, 答不上的简历点回改; 保持投递节奏。

无 Claude Code/Codex 的降级路径: 不安装任何工具, 直接把 get-job.skill 的 `references/*.playbook.md` + industry-resume-toolkit 的 `patterns/岗位/开发.md` 作为方法论文档, 在任意能联网的 agent(如当前环境)中按此方法执行同样的三步流程; MockInterview 同理, 按 SKILL.md 的四角色/四阶段结构手动模拟。

## 来源

所有结论基于下列本地克隆的一手文件(2026-08-28 拉取), 非二手转述:

- /tmp/job-tools/get-job.skill/{SKILL.md, README.md, scripts/README.md, references/*.md}
- /tmp/job-tools/industry-resume-toolkit/{README.md, skill/SKILL.md, skill/patterns/岗位/开发.md}
- /tmp/job-tools/MockInterview/{README.md, SKILL.md}
- /tmp/job-tools/resume.skill/{README.md, .claude/skills/resume-skill/SKILL.md}
- /tmp/job-tools/JobOK/{README.md, SKILL.md, docs/install-cn.md}
- /tmp/job-tools/ASu-skills/{README.md, skills/interview/SKILL.md}

Stars/提交时间来自 GitHub REST API(2026-08-28)。
