# 锚定档案（get-job.skill Stage 0）

更新: 2026-08-28 | 状态: 在职跳槽, 尚未开始投递

## 基本信息

- 姓名: 崔亦哲 | 出生: 1996.11 | 现居: 深圳 | 电话/微信: 17794230123
- 学历: 兰州大学 · 计算机科学与技术 本科（2019 年毕业）
- 工作年限: 7 年研发（2019.07 起） | 期望薪资: 30K/月（税前）| 目标城市: 成都为主, 杭州/上海/深圳为辅

## 目标锚点

- 主线方向: **AI Agent 应用/平台开发 + 代码智能/研发效能**（MCP 服务、代码图谱引擎、RAG 故障诊断、Agent 驱动交付）
- 支线方向: RAG/知识库平台
- 兜底方向: LLM 算法工程、Java/Go 后端
- 公司类型: 大厂 AI 团队 / AI 明星公司 / 中厂·创业公司（排除国企银行）
- 在职状态: 华为 OD 在职（2021.07–至今），边工作边投，面试需协调工作时间
- 敏感点:
  - 华为 OD = 外包身份，简历/背调表述需策略（职级与年限如实，公司写华为+OD 标识要谨慎）
  - 7 年研发但 Java 后端仅 2.5 年（2019-2022），面试长年限问题需准备
  - 异地求职（成都/杭州/上海），简历可标注"可立即到岗/接受异地"

## 技术事实库（简历全部数字必须可回溯）

- 代码图谱: Clang AST(C/C++) + AST+Jedi(Python) + LSP(Java) → Go 重写(tree-sitter + LSP, 四语言), Python 基线逐字段对齐, 验收 harness 73/73
- LangSpec 框架（语言无关遍历器 + 语言特化规格）、Graph Buffer（跨文件节点合并、确定性去重）、CBM 置信链/MRO 方法解析
- MCP 服务: 9 工具（search_code / get_file_dependencies / search_error_log / list_external_calls 等）
- Agent 驱动交付: 134 commits / 132 issues / 1 个月; 5 仓 CI/CD（双平台构建矩阵、pre-push 门禁、PyInstaller）; 101+ 单元测试 / 624+ 用例 / coverage 82% 地板
- 数据管道: CMC 同步平台, SQLite→PostgreSQL + Alembic, ETL 增量, 同步耗时 -54.5%, 回拉接口 -45.9%
- FaultPro: RAG（清洗→向量检索→精排）、eBPF 慢 IO 定界（VFS→IO 调度器→NFS 分段, 开销 <2%）、多格式日志适配器（ASan 栈、GCC 后缀函数名、pc:/lr: 栈帧）、opencode 批量诊断 Agent 工作流（超时续跑/部分输出复用）
- NLP/LLM: 配置模板匹配(长文本+BERT, top5 95%)、参数匹配(Snorkel+CAS+BERT, top10 75%, 缓存+numba 耗时 -95%)、LLM 重排(top3 93%)、通义千问 ICL+DPO（测试集 99%）、ChatGLM/盘古评估
- 后端: Java 微服务（重构模块接口简化 50%）、数字地图 Docker 部署、医疗健康体检系统
- 技术栈: Python/Go(主力), Java, C/C++; MCP/RAG/DPO/ICL/BERT/Snorkel; Claude Code/Codex/opencode; Clang AST/tree-sitter/LSP/Jedi/CBM/MRO; eBPF/PostgreSQL/Alembic/ETL/CI-CD/PyInstaller

## 约束

- 在职: 面试节奏慢, 周一到周五晚上/周末为主; 投递注意对现雇主屏蔽公开渠道信息
- 时间窗口: 2026-08-28 → 2026-09-28（约 30 天）
- 产出确认点: 简历定稿前必须用户确认（get-job Stage 2 检查点）
