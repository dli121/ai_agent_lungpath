# Kaggle Healthcare AI Agent 项目要求 Review

Review 日期：2026-06-15  
内部完成目标：2026-06-23  
官方提交截止：2026-06-25

## 1. 比赛核心要求

这个 Kaggle 项目要求做一个 Healthcare AI Agent。重点不是普通问答，而是能围绕医疗场景进行多步推理、调用工具或数据、提出追问，并输出结构化、安全、可解释的建议。

Agent 应该展示：

- 理解患者症状或健康问题。
- 提出相关 follow-up questions。
- 使用外部知识、数据集、API、retrieval 或本地工具。
- 输出结构化、有用的回答。
- 可以提示 possible conditions 或 next steps，但不能把结果说成医学诊断。
- 展示 ReAct 或类似的 reasoning-action loop。
- 体现真实可用性、安全意识和可解释性。

## 2. 必交材料

最终有效提交必须包含以下内容：

| 组件 | 要求 |
| --- | --- |
| Kaggle Writeup | 项目报告，最多 1500 词。需要说明项目标题、简介、方法、agent workflow、工具、功能和结果。 |
| Media Gallery | Writeup 附件中的视觉材料。必须有 cover image。也可以把 demo video 放在这里。 |
| Public Notebook | 公开 Kaggle Notebook，包含完整可运行代码。必须有清晰说明，不能依赖登录、API key 或付费资源。 |
| Demo Video | YouTube 视频，最长 5 分钟。需要演示 agent 如何工作、关键功能和真实使用场景。 |
| Public GitHub Repository | 必须提供公开 GitHub 仓库。需要完整源码、README、依赖说明和运行方法。 |
| Optional Live Demo | 推荐但非必须。 |

## 3. 评分标准

| 指标 | 分数 | 实际含义 |
| --- | ---: | --- |
| Usefulness | 25 | 是否解决了一个真实、清晰的 healthcare workflow。 |
| Reasoning Ability | 25 | 是否有明确多步 agent logic，而不是一次性直接回答。 |
| Technical Implementation | 20 | 是否模块化、可运行、文档清楚、技术结构合理。 |
| Innovation & Creativity | 15 | 是否比普通 symptom checker 更有辨识度。 |
| Real-world Applicability | 15 | 是否安全、可用，并且清楚说明限制。 |

## 4. 主要风险

需要避免以下问题：

- 缺少 GitHub repository link。
- 缺少 public Kaggle Notebook。
- Notebook 依赖 API key、私有数据、登录或付费资源。
- 缺少 YouTube demo video。
- Kaggle Writeup 超过 1500 词。
- 过度声称医学诊断、confirmed missed diagnosis 或 clinical decision support。
- 没有安全 caveat 就输出 speculative condition suggestion。
- 只做 direct-answer chatbot，没有明显 reasoning、tool use 或 data use。

## 5. 与 IRP 的对齐方式

这个 Kaggle 项目应该作为 IRP 的辅助输出，而不是另做一个泛化医疗聊天机器人。

IRP 主线：

**Explainable Pre-Diagnostic Opportunity Graph for Lung Cancer**

IRP 的核心不是预测肺癌，也不是临床决策支持，而是做一个 non-clinical engineering prototype：从 first observed lung cancer diagnosis code 往前看，把 All of Us / OMOP-style routine data 中的 smoking、COPD/respiratory disease、visits、medications、imaging/procedures、measurements、survey/social-risk variables 等事件组织成：

- timeline；
- graph nodes；
- candidate temporal motifs；
- feasibility summary；
- small visual prototype。

Kaggle 版本可以做成一个更轻量、更公开可演示的版本：

**一个面向肺部健康相关症状或 pre-diagnostic event history 的 Healthcare AI Agent。它会询问缺失信息，检索候选 motifs，构建 timeline/graph，并给出安全的 follow-up next steps，但不做诊断。**

这样可以同时满足 Kaggle 的 agent 要求，并产出可复用于 IRP 的 motif catalogue、schema、visual prototype 和 safety language。

## 6. 推荐项目定位

推荐标题：

**LungPath Agent: Explainable Pre-Diagnostic Opportunity Graph Assistant**

一句话简介：

**A non-diagnostic healthcare AI agent that uses ReAct-style reasoning, local retrieval, and OMOP-style event timelines to identify explainable lung-health opportunity motifs and suggest safe follow-up steps.**

建议明确声明：

- 不是 lung cancer predictor。
- 不是 clinical decision support system。
- 不是 missed-diagnosis detector。
- 是一个 research / engineering prototype，用于展示 pre-diagnostic pattern exploration 的可解释工作流。
- 使用 synthetic 或本地公开示例数据，保证 Kaggle Notebook 不需要 API key 也能完整运行。

## 7. 2026-06-23 前的最小可行系统

目标不是做大，而是做一个可信、清楚、范围安全的 demo。

### Core Agent Loop

建议实现一个简单 ReAct-style loop：

1. **Input parsing**  
   接收 symptoms、duration、age band、smoking history、respiratory history，以及可选 prior events。

2. **Reasoning step**  
   判断 safety level、缺失信息、需要调用哪些本地工具。

3. **Action/tool step**  
   调用本地工具，例如：
   - symptom matcher；
   - red-flag checker；
   - follow-up question generator；
   - motif retriever；
   - OMOP-style timeline builder；
   - graph builder。

4. **Observation step**  
   总结检索到的数据、匹配到的规则、timeline evidence。

5. **Response step**  
   返回：
   - safety caveat；
   - follow-up question；
   - candidate non-diagnostic motifs；
   - suggested next steps；
   - timeline/graph explanation。

### Local Tools

最低可行工具集：

| Tool | 作用 |
| --- | --- |
| `red_flag_checker` | 对 chest pain、severe breathlessness、coughing blood、sudden confusion、severe symptoms 等进行 urgent-care 升级提示。 |
| `symptom_case_retriever` | 在 starter dataset 或小型本地 CSV 中检索相似 symptom cases。 |
| `motif_retriever` | 匹配 smoking + chronic cough、COPD visits + imaging、recurrent respiratory visits、weight loss + fatigue 等候选 motif。 |
| `timeline_builder` | 把事件转换为 index date 前的 lookback timeline。 |
| `graph_builder` | 构建 patient factors、events、observations、candidate motifs 之间的节点和边。 |
| `follow_up_question_generator` | 根据缺失变量提出最有价值的下一步问题。 |

### Data Strategy

只使用本地、Notebook 可运行数据：

- 保留 `healthcare_ai_agent_dataset.csv`，用于基础 symptom matching。
- 在 notebook 中加入一个小型 synthetic OMOP-style lung-health event table，或新增本地 CSV。
- 加一个 compact motif catalogue table，字段可以包括：
  - `motif_id`
  - `motif_name`
  - `required_events`
  - `lookback_window`
  - `explanation`
  - `safety_caveat`
  - `next_step_language`

这样可以避免 API key 和私有数据依赖，同时保留 IRP 的核心方法形态。

## 8. Candidate Motif Catalogue v1

第一版 motifs 应该简单、可解释、安全：

| Motif | Example Pattern | Safe Interpretation |
| --- | --- | --- |
| Smoking + persistent respiratory symptoms | Smoking history plus cough or breathlessness over weeks | 可能值得做 lung-health review；不是诊断。 |
| COPD/respiratory history + repeated visits | COPD/asthma/bronchitis history plus repeated respiratory encounters | 表示 recurrent respiratory burden，适合做 timeline review。 |
| Constitutional symptoms + respiratory symptoms | Fatigue or weight loss plus cough or breathlessness | 如果持续存在，应建议 clinical follow-up。 |
| Imaging/procedure opportunity | Respiratory symptoms plus chest X-ray/CT/procedure event | 用于解释 timeline 中何时出现过 imaging/procedure。 |
| Medication escalation pattern | Repeated inhaler/antibiotic/steroid events | 可能表示反复或加重的 respiratory episodes。 |
| Social-risk barrier motif | Missed visits, transport barrier, smoking exposure, or survey risk | 更适合 care-navigation follow-up，而不是 condition suggestion。 |

## 9. Visual Prototype

Kaggle demo 至少建议包含两个图：

- **Patient timeline**：把事件按 first observed index event 前的月份排列。
- **Opportunity graph**：节点包括 symptoms、history、visits、medications、imaging、measurements、social-risk factors 和 matched motifs。

推荐库：

- `pandas` 处理数据。
- `matplotlib` 画 timeline。
- `networkx` + `matplotlib` 画 graph。

除非时间非常充足，否则不建议先做复杂前端。

## 10. Safety Language

建议统一使用以下 caveat：

- "This is not a medical diagnosis."
- "The agent identifies candidate patterns for follow-up or discussion."
- "Emergency symptoms such as severe chest pain, severe shortness of breath, coughing blood, fainting, or sudden confusion require urgent medical attention."
- "The prototype is for educational and engineering demonstration only."
- "It does not confirm lung cancer, missed diagnosis, or clinical negligence."

## 11. 2026-06-23 前时间表

| 日期 | 目标输出 |
| --- | --- |
| 2026-06-15 | 完成 scope、requirements review 和 project framing。 |
| 2026-06-16 | 定义 motif catalogue v1、synthetic OMOP-style schema 和 safety language。 |
| 2026-06-17 | 实现 local tools：symptom retrieval、red-flag checker、motif retrieval、follow-up question generator。 |
| 2026-06-18 | 在 Kaggle notebook 中实现 ReAct-style agent loop 和 structured response format。 |
| 2026-06-19 | 加入 timeline 和 graph visualizations，生成 cover image 或截图。 |
| 2026-06-20 | 清理 notebook，补充 markdown explanation、sample runs，并完整执行验证。 |
| 2026-06-21 | 整理 GitHub README、dependency list 和 project structure。 |
| 2026-06-22 | 写 Kaggle Writeup，控制在 1500 词以内；准备 demo video 脚本和演示流程。 |
| 2026-06-23 | Final QA：public notebook、GitHub link、YouTube upload、media gallery、submission checklist。 |

## 12. 推荐 Demo Flow

5 分钟视频可以按这个结构：

1. 说明问题和 safety scope。
2. 输入一个 respiratory symptom case。
3. 展示 agent 提出 follow-up question。
4. 展示 tool use：dataset retrieval、motif matching、timeline building、graph building。
5. 展示 structured response，包括 caveats 和 next steps。
6. 展示 timeline 和 graph visual outputs。
7. 结尾说明它如何支持 IRP feasibility prototype。

## 13. 最推荐策略

最稳妥的策略是：

- 不做泛化 symptom checker，而是做 IRP 对齐的 lung-health opportunity graph agent。
- 做小而完整的 deterministic agent，保证 Kaggle Notebook 不依赖外部 LLM API。
- 把 agent reasoning 用结构化字段展示，例如 `reasoning_summary`、`selected_tools`、`observations`、`response`。
- 如果有时间，再加入 optional LLM adapter，但不要让它成为 notebook 运行的必要条件。
- 强调 explainability、safety 和 technical correctness，而不是 prediction accuracy。
- 把 Kaggle 产出直接复用到 IRP：motif catalogue、feasibility language、timeline visualization、graph visualization 和 README。

