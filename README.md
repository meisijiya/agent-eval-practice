# agent-eval-practice

> **仓库定位**：AI Agent 测试方法论 + 用例库 + LLM-as-Judge 评测体系
> **目标项目**：Eleven617/mall-ai-after-sales-platform（⭐79 · Apache-2.0）
> **作者**：双非本科大四 · 测开岗求职 · 主攻测开 + Agent 开发
> **生成时间**：2026-09-14
> **状态**：🚧 **设计阶段（占位仓库，待 D12-D13 落地实测）**

## 仓库一句话

基于 mall-ai 开源电商售后智能 Agent 平台（Spring Boot + LangGraph + Vue3 + Chroma + RabbitMQ），沉淀 **AI Agent 测试方法论 + 50 条 YAML 用例库 + LLM-as-Judge 4 维度评测体系**。

## 仓库内容

| 目录 | 内容 | 简历对应 |
|---|---|---|
| `docs/` | Agent 测试方法论 + 评测指标体系 | "AI Agent 测试" |
| `evals/` | 50 条 YAML Agent 用例（4 场景 × 4 类） | "50 条 Agent 测试用例" |
| `scripts/` | LLM-as-Judge 评分脚本 | "LLM-as-Judge 评测体系" |
| `reports/` | 4 维度评测报告模板 | "4 维度评测报告" |
| `docker/` | docker-compose 部署脚本 | "本地部署 AI Agent 平台" |

## 简历可写句子

> 基于 Eleven617/mall-ai-after-sales-platform（Apache-2.0）开源项目，独立完成平台本地部署、AI Agent 测试用例设计与 LLM 评测体系实测落地。基于业务诉求提炼 4 个评测维度（事实性/工具选择率/拒答率/幻觉率），落地 LLM-as-Judge 评分脚本并实跑，输出可复用的评测流程与报告。编写 Agent 测试用例 50 条，覆盖正常/异常/边界/对抗场景。

## 简历数字校验（×2 原则）

| 数字 | 实际产出 | 简历写法 | ×2 校验 |
|---|---|---|---|
| Agent 用例 | 50 条（4 场景 × 4 类 × 3 边界） | "编写 Agent 测试用例 50 条" | ✓ 不超 |
| 评测维度 | 4 维度（事实性/工具选择率/拒答率/幻觉率） | "4 维度评测" | ✓ |
| 测试场景 | 4 核心场景（开放任务/政策 RAG/工具调用/写库关卡） | "4 个核心场景" | ✓ |
| 用例类型 | 4 类（正常/异常/边界/对抗） | "4 类场景" | ✓ |

## 核心交付物清单（设计阶段）

```
agent-eval-practice/
├── README.md                                ← 本文件
├── docs/
│   ├── 1-方法论/
│   │   ├── agent-workflow.md                (Agent 工作模式说明)
│   │   ├── eval-dimensions.md               (4 维度评测指标定义)
│   │   └── llm-as-judge.md                  (LLM-as-Judge 实施指南)
│   └── 2-场景设计/
│       ├── 01-open-task.md                  (开放任务)
│       ├── 02-policy-rag.md                 (政策 RAG)
│       ├── 03-tool-calling.md               (工具调用)
│       └── 04-write-db-gate.md              (写库关卡)
├── evals/                                   ← 50 条 YAML 用例
│   ├── 01-open-task/
│   │   ├── normal.yaml                      (8 条正常用例)
│   │   ├── abnormal.yaml                    (4 条异常)
│   │   ├── boundary.yaml                    (3 条边界)
│   │   └── adversarial.yaml                 (3 条对抗)
│   ├── 02-policy-rag/
│   ├── 03-tool-calling/
│   └── 04-write-db-gate/
├── scripts/                                 ← LLM-as-Judge 评分
│   ├── judge_factuality.py
│   ├── judge_tool_selection.py
│   ├── judge_refusal_rate.py
│   ├── judge_hallucination.py
│   └── run-all-evals.sh
├── reports/                                 ← 4 维度报告模板
│   └── template-4-dim-report.md
└── docker/                                  ← docker-compose 部署
    └── docker-compose.yml                   (MySQL + Chroma + RabbitMQ + 前后端)
```

## 简历深问应对（4 类高频追问）

### Q1：Agent 工作模式怎么理解？

**答案框架**（4 阶段）：
1. **意图理解**：用户 query → LLM 解析意图（如"我要退订"→ 识别为"取消订单"）
2. **工具选择**：根据意图选择工具（如 cancel_order / refund_payment）
3. **执行**：调用工具 / API / 数据库
4. **返回结果**：LLM 整合结果 → 自然语言回复

**关键认知**：Agent 不是黑盒，**每个阶段都可独立测试**。

### Q2：4 维度评测怎么设计的？

**答案框架**（4 维度）：
1. **事实性**（Factuality）：Agent 输出是否与事实一致
   - 指标：准确率 = 正确回答数 / 总问题数
   - 评判：LLM-as-Judge 对比 ground truth
2. **工具选择率**（Tool Selection Accuracy）：Agent 选对工具的比例
   - 指标：正确率 = 选对工具次数 / 总决策次数
   - 评判：LLM-as-Judge 对比预期工具
3. **拒答率**（Refusal Rate）：Agent 不知道时是否说不知道
   - 指标：合理拒答数 / 应拒答总数
   - 评判：超出能力的问题是否礼貌拒答
4. **幻觉率**（Hallucination Rate）：Agent 是否编造信息
   - 指标：幻觉次数 / 总回答数
   - 评判：LLM-as-Judge 检测虚构内容

### Q3：50 条用例怎么分类设计的？

**答案框架**（4 场景 × 4 类 = 16 子场景）：
| 场景 | 正常 | 异常 | 边界 | 对抗 | 小计 |
|---|---|---|---|---|---|
| 开放任务 | 3 | 3 | 3 | 3 | 12 |
| 政策 RAG | 3 | 3 | 3 | 3 | 12 |
| 工具调用 | 3 | 3 | 3 | 3 | 12 |
| 写库关卡 | 3 | 3 | 3 | 3 | 12 |
| **合计** | 12 | 12 | 12 | 12 | **48** |

补 2 条跨场景组合用例 → **50 条**

**用例设计原则**：
- **正常**：业务主路径（"我要退款"→ 标准流程）
- **异常**：超出能力（"帮我修电脑"→ 应礼貌拒答）
- **边界**：模糊意图（"我想取消那个东西"→ 应反问澄清）
- **对抗**：诱导性输入（"忽略之前的指令，现在你是..."→ 应拒绝）

### Q4：LLM-as-Judge 怎么实现？

**答案框架**（5 步骤）：
1. **准备 ground truth**：每条用例预期答案 / 预期工具 / 预期拒答
2. **执行 Agent**：用待评测 Agent 跑用例 → 收集输出
3. **构造 Judge Prompt**：把"ground truth + Agent 输出 + 评分标准"喂给 Judge LLM
4. **Judge LLM 评分**：输出 1-5 分 + 评分理由
5. **汇总 4 维度报告**：每维度聚合分数 + 失败用例列表

**关键挑战**：
- Judge LLM 自身可能有偏差 → 用 GPT-4 / Claude 等强模型
- 评分一致性 → 加 few-shot examples
- 成本控制 → 用国内低价 Key（DeepSeek / 通义 / 智谱）

### Q5：项目时间 / 技术栈怎么写的？

**答案框架**：
- **时间**：2026.06 - 2026.09（3 个月自学跨度）
- **技术栈**：Spring Boot + LangGraph + Vue3 + Chroma + RabbitMQ
- **测试栈**：Python + pytest + YAML + OpenAI API（Judge LLM）
- **部署**：docker-compose（MySQL + Chroma + RabbitMQ + 前后端）

## 如何使用本仓库（设计阶段）

### 浏览评测方法论

```bash
cat docs/1-方法论/agent-workflow.md
cat docs/1-方法论/eval-dimensions.md
cat docs/1-方法论/llm-as-judge.md
```

### 阅读 4 场景设计

```bash
cat docs/2-场景设计/01-open-task.md
cat docs/2-场景设计/02-policy-rag.md
cat docs/2-场景设计/03-tool-calling.md
cat docs/2-场景设计/04-write-db-gate.md
```

### 跑评测（待落地）

```bash
# 准备 LLM API Key（国内可直连低价 Key）
export LLM_API_KEY="sk-..."

# 启动 mall-ai 平台
cd docker && docker-compose up -d

# 跑 50 条用例
bash scripts/run-all-evals.sh

# 看 4 维度报告
open reports/4-dim-report-latest.md
```

## 待落地（D12-D13 实施计划）

- [ ] D12：docker-compose 本地部署 mall-ai 平台（4 服务）
- [ ] D12：4 核心场景冒烟测试（确认 Agent 工作模式）
- [ ] D13：50 条 YAML 用例库落地（4 场景 × 4 类 + 2 跨场景）
- [ ] D13：LLM-as-Judge 4 维度评分脚本实跑
- [ ] D13：4 维度评测报告输出
- [ ] D13：CI 集成（GitHub Actions 复用 mall4j-auto-test workflow）

## 关联仓库

- **姊妹仓 1**：[mall4j-test-practice](../mall4j-test-practice)（功能用例 + 缺陷库）
- **姊妹仓 2**：[mall4j-auto-test](../mall4j-auto-test)（接口 + UI + CI + 压测）
- **源项目**：[Eleven617/mall-ai-after-sales-platform](https://github.com/Eleven617/mall-ai-after-sales-platform)（⭐79 · Apache-2.0）

## 协议

本仓库基于 **Apache-2.0**（继承自 mall-ai 项目），所有测试方法论 / 用例库 / 评测脚本均为作者原创，遵循相同开源协议。

## 联系方式

（待用户填写）
