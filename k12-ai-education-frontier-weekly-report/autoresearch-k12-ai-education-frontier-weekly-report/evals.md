# Evals For `k12-ai-education-frontier-weekly-report`

## Evaluation setup
- Runs per experiment: `5`
- Max score per experiment: `20`
- Frozen input pack: verified weekly report pack for `2026-03-12` to `2026-03-20`
- Goal: tighten the harness after the initial suite saturated at 100%, with more attention to exact title/date compliance, audience-specific emphasis, brief-style execution, and evidence boundaries

EVAL 1: 标题日期与固定骨架精确合规
Question: 输出是否始终保留固定报告标题 `# K-12教育科技与AI前沿周报`，日期行使用 `3月12日-3月20日` 这种不含年份的月日格式，并完整保留五个规定主章节标题与至少两张 Markdown 表格？
Pass: 无论用户强调教育官员、校长还是综合读者，报告标题都不被改写成“治理周报”“AI周报”等变体；日期行不含年份；五个主章节标题准确，且至少两张表格存在。
Fail: 报告标题被改写，日期行写成 `2026年3月12日-3月20日` 或类似带年份形式，缺失或改写任一主章节标题，或表格少于两张。

EVAL 2: 读者偏置足够明显
Question: 输出是否把读者定制主要体现在摘要和正文启发里，而不是只换标题；并且不同读者版本在关注重点上有清晰可见的偏置？
Pass: 教育官员版明显优先写政策治理、区域推进、评价与规模化条件；校长版明显优先写课程实施、教师培训、课堂使用、校园治理或家校沟通；综合版兼顾三类读者而不坍缩为单一视角。
Fail: 不同版本除标题外差异很弱，或指定读者的核心关注点没有在摘要和至少两条启发中得到明显体现。

EVAL 3: 简讯体与表格压缩执行到位
Question: 输出是否保持 skill 要求的简讯体：表格单元格以短语或短句为主、重点条目用 `1、2、3、` 加标签行展开，并且没有明显膨胀成大段长篇说明？
Pass: 表格整体保持扫读友好，重点条目均使用结论句加 `事件`/`思考与启发`/`对教育的影响`/`成效` 等标签行展开，没有明显冗长的说明性大段。
Fail: 多个表格单元格写成整句解释，或多个重点条目明显超出简讯体风格、展开成偏长的说明文。

EVAL 4: 时间窗与证据边界克制
Question: 输出是否严格把本期新增动态限制在研究包给定的 `2026-03-12` 至 `2026-03-20` 时间窗内，并且不补充研究包之外的事实、数字、效果或因果结论；若证据不足，是否明确写出 `暂无公开数据`、`公开信息不足` 或同等边界表述？
Pass: 没有把时间窗外旧闻写成本期新增，也没有编造研究包之外的数据或效果；证据不足处有明确边界表述。
Fail: 出现超出时间窗的新增条目、研究包外事实补写，或把证据不足的案例写成确定性结论。
