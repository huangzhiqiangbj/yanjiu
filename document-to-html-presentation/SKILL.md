---
name: document-to-html-presentation
description: 将 Word/Markdown 研究报告或长文档，转化为图文并茂、排版精美（PPT卡片风格+数据可视化+精炼表格）的交互式 HTML 网页简报，支持手机端适配与一键导出长图。若用于 `docs/研究/` 或 `docs/周报/` 工作流，默认与同批次的 Markdown、Word 保持同文件夹归档。
---

# Document to HTML Presentation Skill

这个 Skill 用于将枯燥的长篇研究报告、Word 文档或 Markdown 文件，转化为一个现代化的、适合在线阅读和分享的“可视化长图文网页（Web Presentation）”。

## 触发条件
当用户提出以下类似需求时使用：
- “把这个 word 文档输出为 html 网页”
- “生成可视化长图文报告”
- “把这份报告做成图文结合的网页”
- “用 PPT 排版的方式重构这份文档的 HTML”

## 核心工作流 (Workflow)

### 第一步：内容解析与结构重组 (Analyze & Restructure)
1. 使用 `Read` 工具读取目标文档的内容。
2. 识别文档的逻辑结构：
   - **默认优先采用简版三段式**：`本期判断`、`资讯快览`、`逐条拆解`。
   - **核心摘要/结论**：适合放入 `本期判断`，用 1-2 段普通 HTML 正文表达。
   - **资讯列表/榜单内容**：适合放入 `资讯快览`，默认优先做成 `带标签的时间线快览` 或 `卡片+时间线合并快览`，而不是把快览卡片和一周脉络拆成两个重复区块。
   - **时间线快览默认写法**：优先只保留 `时间 + 标签 + 名称`，除非那条解释句真的提供了额外阅读价值，否则不要在每个时间轴节点下再补一段说明。
   - **事件 + 启发/分析**：适合放入 `逐条拆解`，按条目纵向展开。
   - **核心数据/对比**：只有在数据确实能显著帮助理解时，才提取生成图表或精炼表格；不是每份文档都必须有图表。
   - **数据密集型段落**：可选使用 `AntV T8 (Narrative Text)` 做局部语义高亮，但不要让主标题、导语和大段正文完全依赖 T8。
3. 若源文档末尾包含 `附：参考来源`、`参考来源` 或同类来源列表，默认把它视为 `Markdown/Word 保留信息`，不要自动渲染进最终 HTML 页面；只有用户明确要求在页面中展示来源时才保留。
4. 对 `研究简报 / 研究报告 / 政策简报` 类 HTML，默认不要加入 `本页依据……整理`、`本版按三段式重组`、`页面中的图形均根据……重新编排` 这类解释性尾注或防御性元说明。除非用户明确要求展示编辑说明，否则页面只保留面向读者有信息价值的内容。
5. 若源文档属于 `周报`、`榜单`、`Top N`、`政策简报`、`多条资讯汇总` 这类 `3条及以上条目型文档`，必须在保持三段式主结构的前提下增加页面内的视觉模块，不允许退化成“导语 + 简单卡片 + 纯正文拆解”。

### 第二步：生成图表与精炼表格 (Generate Charts & Tables)
1. **普通长文/研究文档**：图表与表格按需使用，前提是它们真的能提升理解，而不是为了“看起来高级”强行加复杂度。
2. **周报/榜单/多条资讯汇总**：默认必须至少生成 `1个精炼表格 + 1组洞察卡片`，并在 `时间线 / 信息图 / 真正有信息增量的数据可视化` 中至少选择一种。不要再把“图表”机械理解成必须塞一个柱状图或饼图。
3. **允许派生的数据类型**：当原文缺少现成统计图数据时，可以根据已出现且可核验的文档结构派生可视化数据，但这只应作为最后手段，例如 `推进层级分布`、`主体类型分布`。严禁编造新数字，也不要把任何能一眼看出的目录结构都硬做成图。
4. **数据可视化要有信息增量**：优先选择最能支持判断的可视化形式。若 `类别分布`、`国内/国际数量`、`入选条目数`、`国家数量`、`累计发布日期节奏` 只是重复读者一眼就能看出的结构信息，默认不要做成图。只有当图表能帮助读者更快理解关键差异、趋势或数量级时才作图；否则宁可保留时间线、对比表或卡片。
4.1 **出现关键数据时默认要作图**：只要原文出现 `学校数`、`教师数`、`学生数`、`预算`、`课时`、`比例`、`通过率`、`样本量`、`平台用户规模`、`部署数量` 等关键数据，且它们可以支撑判断，就不要只停留在表格或正文，默认应生成有信息增量的图表或信息图。
5. **生成表格**：对于不适合做成图表的多维对比信息，使用 HTML `<table>` 标签构建精炼表格。表格内容必须高度概括，避免大段文字堆砌；周报/榜单类页面中，表格应优先承担“快速对比”功能，并做得更像成品卡片，而不是普通表格截图。若复杂样式会影响长图导出可读性，优先退回到更简单的 `3列表格`，例如 `资讯 / 核心动作 / 最值得跟踪`。
6. **补充视觉模块**：对于周报/榜单类页面，除表格外，默认优先补 `dimension-grid / timeline / insight matrix / spotlight card` 这类有判断价值的模块。`metric-strip` 仅在数字本身确实有判断价值时才使用，不要为了“显得丰富”硬塞。
7. **真实图表优先走 visualization skill**：只要判断“这张图真的有信息增量”，必须先读取 sibling skill [visualization](../visualization/SKILL.md)，再按其中路由调用对应图表能力生成真实图表文件或图像资源。不要手写一个“看起来像图表”的静态占位块来替代真正图表，也不要为了凑版面硬生成低价值统计图。
7.1 **看不清等于不合格**：图表中的中文标签、数字、百分比、图题和关键数值必须在网页和长图导出中清晰可读。若真实图表字号过小、低值标签看不清、数据太挤或不同量纲混放造成误读，必须继续调整，必要时改成 HTML 原生大字号图表、参数面板、进度条或其他更易读的信息图。
8. **图表生成前必须走 visualization 路由**：如果要做折线图、柱状图、面积图、散点图、词云、地图或结构化表格，先读 `visualization/SKILL.md`，再读所需的 `chart-visualization` 参考；不能凭感觉猜参数。
9. **图题与图注只写读者需要知道的结果**：不要在最终页面中写 `这里不再用……`、`改成信息图后……`、`用横向条形对比……`、`由于单位不同……` 这类编辑过程说明。图题、图注和图旁说明应直接陈述结论、对比关系或读者应关注的结果，不解释制图方法。

### 第三步：构建 HTML 与 CSS 骨架 (Build HTML & CSS)
创建一个**完全响应式（Mobile-First）**的 HTML 页面，必须包含以下高级排版 CSS 样式：
1. **移动端适配**：必须包含 `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">`，并使用 `@media (max-width: 768px)` 调整卡片网格为单列（`grid-template-columns: 1fr`），缩小内边距（padding）。
2. **精美表格 (`.custom-table`)**：带有斑马纹、表头背景色、底部强调边框，且在手机端支持横向滚动（`overflow-x: auto`）。
3. **PPT 风格网格卡片 (`.ppt-grid`, `.ppt-card`)**：用于展示核心判断。带有顶部彩色边框（红、蓝、黄、绿）和悬停浮动阴影效果。
4. **维度分析卡片 (`.dimension-grid`, `.dim-card`)**：带有 Emoji 图标和浅色背景（如浅蓝、浅绿）的左右布局卡片。
5. **时间线或进展模块 (`.timeline`, `.timeline-item`)**：用于表现一周内事件密度、推进顺序和阶段演进，比简单数量统计更适合周报类页面。时间线条目默认应支持直接显示 `政策 / 地方 / 应用` 这类标签。
6. **图表容器 (`.chart-wrapper`)**：带有浅色背景、圆角、边框和图片说明（Caption）的居中容器。
7. **精致表格壳层 (`.table-shell`, `.custom-table`)**：支持表格外卡片容器、表头强调、行级分组和更强的视觉层级。
8. **重点高亮 (`.key`, `.label`)**：用于在逐条拆解中对已核验的核心信息做 `加粗 + 明亮色` 标注。默认优先使用**单一高可见度蓝色**，避免因为颜色过多打乱阅读节奏；只有用户明确要求更丰富的分色体系时，才额外引入黄、绿、紫、红等配色。若高亮文本可能跨行，优先用 `亮色文字 + 粗体`，不要默认用容易在导出长图中出现整块折叠感的底色高亮。
9. **悬浮导出按钮 (`.export-btn`)**：固定在右下角的“导出长图”按钮。

### 第三步补充：逐条拆解的高亮规则
对 `周报 / 榜单 / 多条资讯汇总` 类页面，`逐条拆解` 中不能只把 `事件：`、`思考与启发：` 这些标签做样式区分；还必须在每条正文里额外标出 `1-3` 个真正决定阅读价值的核心短语。

执行要求：
1. `事件` 段至少高亮 `1-2` 个已核验的关键信息，如规则名称、关键机制、覆盖规模、部署动作、核心课时、关键角色。
2. `思考与启发` 段至少高亮 `1-2` 个结构判断抓手，如治理边界、制度链条、教师准备度、区域推进机制、课程工程、评价变革等。
3. 不要整句高亮，也不要把每个名词都做成高亮；默认控制在每段 `1-3` 个锚点。
4. 若当前 HTML 中出现了 `.key` 样式定义，但 `逐条拆解` 正文没有插入对应高亮 span，视为未完成高亮要求。
5. 若用户没有明确要求多色高亮，默认统一使用蓝色，不把颜色本身做成额外的信息负担；重点仍通过锚点选择和加粗强度来区分主次。

### 第四步：集成长图导出脚本，并谨慎使用 T8 (Integrate Scripts)
1. **主标题、导语、关键正文段落优先用普通 HTML**：例如 `h1.hero-title`、`p.hero-summary`、`.narrative-block p`。这样导出长图时能被 `.mobile-export-mode` 可靠放大。
2. **T8 仅用于局部增强**：例如短段落中的数字高亮、说明性补充块。不要把整个页面的主阅读内容都交给 T8，否则导出长图时字号控制可能失效。
3. 引入 html2canvas CDN：`<script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>`。
4. 编写 `exportToImage()` 函数，实现点击按钮自动截取 `#app-container` 并触发 PNG 长图下载。

### 第五步：组装与输出 (Assemble & Output)
1. 将 CSS、HTML 结构、图表/表格/补充视觉模块与导出脚本组装成一个完整的 `.html` 文件。
2. 使用 `Write` 工具将文件保存到用户指定目录（或原文件同级目录），命名为 `原文件名_可视化长图文报告.html`。若源文档已经位于 `docs/研究/` 或 `docs/周报/` 下的某个批次文件夹中，HTML 必须继续保存到该批次文件夹；若用户只给出 `docs/研究/` 或 `docs/周报/` 作为目标目录，则先创建与源文档基名一致的批次文件夹，若重名则按 `_2`、`_3` 递增，再把 HTML 写进去。
3. 提示用户在手机或电脑浏览器中打开预览，并告知可以使用右下角按钮“一键导出长图”。
4. 若使用了 `visualization` 生成真实图表，图表文件也必须与同批次的 `.md`、`.docx`、`.html` 放在同一批次目录，不能散落到临时目录后只在页面里写死外链。

## 周报与榜单类页面的最低视觉标准

当源文档属于 `周报`、`榜单`、`Top N`、`多条资讯简报`，且正文中存在 `3条及以上` 的独立条目时，最终 HTML 默认必须同时满足：

1. **保留三段式主结构**：仍以 `本期判断`、`资讯快览`、`逐条拆解` 为一级结构，不额外堆砌“建议”“说明”“方法”等无关章节。
2. **英雄区不要为了凑密度硬塞小栏目**：`metric-strip` 只有在数字本身真的有判断价值时才使用；若没有，就用更强的导语、配色和洞察卡承接。
3. **快览区不能只有纯文本卡片**：卡片需要有明显层级，如编号、标签、强调色、短结论。
4. **数据可视化必须有信息增量**：不要默认做 `类别分布`、`国内/国际数量`、`入选条目数`、`累计发布日期节奏` 这类结构自明图；优先使用来自资讯正文的关键数字，如覆盖学校数、教师数、学生数、预算、课时、平台用户规模、调查比例等生成图表。若没有这类数字，优先时间线、推进路径、关系信息图或精炼表格，而不是硬做图。
5. **必须有 1 个精炼表格**：承担“快速对比”功能，例如 `资讯 / 推进层级 / 核心动作 / 落地关键`，且表格外观要像精致卡片，而不是普通表格。
6. **必须至少有 1 组洞察类视觉卡片**：如 `dimension-grid`、`insight matrix`、`timeline cards` 等，用于承接结构化判断。
7. **逐条拆解要能扫读**：允许对已核验重点信息使用 `加粗 + 颜色强调` 做轻量标注，但必须克制，不能通篇荧光笔。
逐条拆解中的 `事件` 与 `思考与启发` 两段，默认都要有关键词级高亮，不能只保留标签行加粗。
8. **长图导出样式要单独设计**：不能只是把网页原样粗暴放大；导出态应重新控制背景、边框、间距、字号和卡片层级。
9. **不允许退化成纯文字长页**：如果最终页面看起来只是“好看一点的 Markdown”，说明视觉化不达标。
10. **所有视觉元素都要基于原文**：允许重组、允许派生结构统计，但不允许补不存在的数据和结论。

## HTML 模板参考 (Template Reference)

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <!-- 强制移动端适配 -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>{文档标题}</title>
  <style>
    /* 基础样式 */
    body { background-color: #f1f5f9; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; color: #334155; margin: 0; padding: 0; }
    #app-container { max-width: 960px; margin: 40px auto; background: #fff; padding: 50px 80px; border-radius: 16px; box-shadow: 0 10px 30px rgba(0,0,0,0.05); line-height: 1.8; text-align: justify; text-justify: inter-ideograph; }
    .section-title { color: #0f172a; border-bottom: 2px solid #e2e8f0; padding-bottom: 12px; margin: 50px 0 24px; display: flex; align-items: center; gap: 12px; font-size: 24px; font-weight: 900; }
    .section-title::before { content: ''; width: 6px; height: 24px; background: #3b82f6; border-radius: 4px; }
    .hero-title { margin: 0 0 24px; font-size: 42px; line-height: 1.3; color: #0f172a; font-weight: 900; }
    .hero-summary, .narrative-block p { margin: 0 0 18px; font-size: 20px; line-height: 1.9; color: #334155; text-align: justify; text-justify: inter-ideograph; }
    
    /* PPT卡片样式 */
    .ppt-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 24px; }
    .ppt-card { padding: 24px; border-radius: 12px; border: 1px solid #e2e8f0; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); position: relative; overflow: hidden; }
    .ppt-card:nth-child(1) { border-top: 4px solid #ef4444; }
    .ppt-card:nth-child(2) { border-top: 4px solid #3b82f6; }

    /* 维度分析卡片 */
    .dimension-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; margin: 26px 0 32px; }
    .dim-card { display: flex; gap: 16px; align-items: flex-start; background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 14px; padding: 20px; }
    .dim-icon { width: 44px; height: 44px; border-radius: 12px; background: #dbeafe; display: flex; align-items: center; justify-content: center; font-size: 22px; flex: 0 0 44px; }
    .dim-content h4 { margin: 0 0 8px; color: #0f172a; font-size: 20px; font-weight: 900; }
    .dim-content p { margin: 0; color: #475569; font-size: 18px; line-height: 1.8; text-align: justify; text-justify: inter-ideograph; }

    /* 时间线样式 */
    .timeline { margin: 28px 0 34px; display: flex; flex-direction: column; gap: 14px; }
    .timeline-item { display: grid; grid-template-columns: 82px 28px 1fr; gap: 14px; align-items: start; }
    .timeline-date { color: #2563eb; font-size: 16px; font-weight: 900; padding-top: 2px; }
    .timeline-dot { width: 14px; height: 14px; border-radius: 999px; background: #3b82f6; box-shadow: 0 0 0 4px #dbeafe; margin-top: 4px; }
    .timeline-content { background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 12px; padding: 14px 16px; }
    .timeline-tag { display: inline-block; margin-bottom: 10px; padding: 6px 12px; border-radius: 999px; font-size: 13px; font-weight: 900; line-height: 1.2; }
    .timeline-tag.policy { background: #dbeafe; color: #1d4ed8; }
    .timeline-tag.local { background: #fef3c7; color: #92400e; }
    .timeline-tag.application { background: #dcfce7; color: #15803d; }
    .timeline-content h4 { margin: 0 0 6px; color: #0f172a; font-size: 24px; font-weight: 900; }
    .timeline-content p { margin: 0; color: #475569; font-size: 18px; line-height: 1.8; text-align: justify; text-justify: inter-ideograph; }

    /* 图表样式 */
    .chart-wrapper { margin: 40px 0; text-align: center; background: #f8fafc; padding: 30px 15px; border-radius: 12px; border: 1px solid #e2e8f0; }
    .chart-wrapper img { max-width: 100%; border-radius: 8px; }
    
    /* 精炼表格样式 */
    .table-shell { margin: 30px 0; padding: 18px; border-radius: 14px; background: #f8fafc; border: 1px solid #e2e8f0; }
    .custom-table { width: 100%; border-collapse: collapse; font-size: 18px; text-align: left; border-radius: 8px; overflow: hidden; box-shadow: 0 0 0 1px #e2e8f0; background: #fff; }
    .custom-table thead tr { background-color: #f8fafc; color: #1d4ed8; font-weight: 900; }
    .custom-table th, .custom-table td { padding: 16px 20px; border-bottom: 1px solid #e2e8f0; }
    .custom-table th { font-size: 20px; }
    .custom-table tbody tr:nth-of-type(even) { background-color: #fcfcfd; }
    .custom-table tbody tr:last-of-type { border-bottom: 2px solid #3b82f6; }

    /* 重点高亮 */
    .label { display: inline-block; margin-right: 8px; padding: 4px 10px; border-radius: 999px; background: #dbeafe; color: #1d4ed8; font-weight: 900; }
    .label-event { background: #dbeafe; color: #1d4ed8; }
    .label-insight { background: #dbeafe; color: #1d4ed8; }
    .key { color: #1d4ed8; font-weight: 900; }
    
    /* 悬浮导出按钮 */
    .export-btn { position: fixed; bottom: 30px; right: 30px; background: #3b82f6; color: white; border: none; padding: 12px 24px; border-radius: 50px; box-shadow: 0 4px 12px rgba(59,130,246,0.3); cursor: pointer; font-size: 16px; font-weight: bold; z-index: 999; transition: transform 0.2s; }
    .export-btn:hover { transform: translateY(-2px); background: #2563eb; }

    /* 移动端响应式适配 */
    @media (max-width: 768px) {
      #app-container { margin: 15px; padding: 25px 20px; border-radius: 12px; }
      .ppt-grid, .dimension-grid { grid-template-columns: 1fr !important; }
      .custom-table { display: block; overflow-x: auto; white-space: nowrap; }
      .export-btn { bottom: 20px; right: 20px; padding: 10px 20px; font-size: 14px; }
      .section-title { font-size: 24px; }
    }

    /* 专为长图导出设计的移动端大字号排版 (1080px 宽) */
    .mobile-export-mode { width: 1080px !important; max-width: 1080px !important; margin: 0 auto !important; padding: 80px 60px !important; border-radius: 0 !important; box-shadow: none !important; background: #ffffff !important; }
    .mobile-export-mode h1, .mobile-export-mode .hero-title { font-size: 76px !important; margin-bottom: 40px !important; line-height: 1.22 !important; font-weight: 900 !important; }
    .mobile-export-mode p, .mobile-export-mode li, .mobile-export-mode td, .mobile-export-mode th, .mobile-export-mode .dim-content p, .mobile-export-mode .timeline-content p, .mobile-export-mode span { font-size: 38px !important; line-height: 1.82 !important; text-align: justify !important; text-justify: inter-ideograph !important; }
    .mobile-export-mode .hero-summary, .mobile-export-mode .narrative-block p { font-size: 44px !important; line-height: 1.88 !important; text-align: justify !important; text-justify: inter-ideograph !important; }
    .mobile-export-mode h2.section-title { font-size: 54px !important; margin: 80px 0 40px !important; padding-bottom: 20px !important; }
    .mobile-export-mode .section-title::before { height: 56px !important; width: 12px !important; }
    .mobile-export-mode h3, .mobile-export-mode h4, .mobile-export-mode .dim-content h4, .mobile-export-mode .timeline-content h4 { font-size: 48px !important; margin-bottom: 20px !important; }
    .mobile-export-mode .ppt-grid, .mobile-export-mode .dimension-grid { grid-template-columns: 1fr !important; gap: 40px !important; }
    .mobile-export-mode .ppt-list-item { flex-direction: column !important; padding: 40px !important; }
    .mobile-export-mode .ppt-number { font-size: 64px !important; margin-bottom: 24px !important; }
    .mobile-export-mode .ppt-card, .mobile-export-mode .dim-card { padding: 40px !important; }
    .mobile-export-mode .custom-table { font-size: 38px !important; display: table !important; word-break: break-word !important; white-space: normal !important; }
    .mobile-export-mode .custom-table th, .mobile-export-mode .custom-table td { padding: 32px 24px !important; }
    .mobile-export-mode .chart-caption { font-size: 32px !important; margin-top: 30px !important; }
    .mobile-export-mode .dim-icon { width: 120px !important; height: 120px !important; font-size: 60px !important; border-radius: 24px !important; }
  </style>
</head>
<body>
  <div id="app-container">
    <div class="hero">
      <h1 class="hero-title">{标题}</h1>
      <p class="hero-summary">{摘要导语}</p>
    </div>

    <h2 class="section-title">一、核心摘要</h2>
    <div class="dimension-grid">
      <div class="dim-card">
        <div class="dim-icon">🚦</div>
        <div class="dim-content">
          <h4>治理边界前置</h4>
          <p>用规则先划清学校中 AI 的可用区、慎用区与禁区。</p>
        </div>
      </div>
      <div class="dim-card">
        <div class="dim-icon">📚</div>
        <div class="dim-content">
          <h4>课程实施变刚性</h4>
          <p>把课程、课时和实施路径做成可执行安排，而不只停留在口号层面。</p>
        </div>
      </div>
    </div>
    <h2 class="section-title">二、资讯快览</h2>
    <div class="timeline">
      <div class="timeline-item">
        <div class="timeline-date">3.24</div>
        <div class="timeline-dot"></div>
        <div class="timeline-content">
          <span class="timeline-tag policy">政策</span>
          <h4>{资讯标题}</h4>
        </div>
      </div>
    </div>

    <h2 class="section-title">三、关键信号对比</h2>
    <div class="table-shell">
      <table class="custom-table">
        <thead>
          <tr>
            <th>资讯</th>
            <th>核心动作</th>
            <th>最值得跟踪</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{资讯名称}</td>
            <td>{动作}</td>
            <td>{关键条件}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="narrative-block">
      <p>{关键说明正文}</p>
    </div>
  </div>

  <!-- 导出长图按钮 -->
  <button class="export-btn" onclick="exportToImage()">📸 一键导出长图</button>

  <!-- 引入脚本 -->
  <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
  
  <script>
    function resetExportState(btn, container, originalText) {
      container.classList.remove('mobile-export-mode');
      btn.innerHTML = originalText;
      btn.style.opacity = '1';
      btn.style.pointerEvents = 'auto';
    }

    function canvasToBlob(canvas) {
      return new Promise((resolve, reject) => {
        canvas.toBlob((blob) => {
          if (!blob || blob.size === 0) {
            reject(new Error('浏览器返回了空图片数据'));
            return;
          }
          resolve(blob);
        }, 'image/png');
      });
    }

    function getExportScaleAttempts(container) {
      const rect = container.getBoundingClientRect();
      const safeMaxSide = 16384;
      const safeMaxArea = 120000000;
      const width = Math.max(Math.ceil(rect.width), 1);
      const height = Math.max(Math.ceil(rect.height), 1);
      const sideLimitedScale = Math.min(safeMaxSide / width, safeMaxSide / height);
      const areaLimitedScale = Math.sqrt(safeMaxArea / (width * height));
      const preferredScale = Math.min(2, sideLimitedScale, areaLimitedScale);
      const attempts = [preferredScale, Math.min(preferredScale, 1.5), 1, 0.85, 0.7, 0.55];

      return attempts
        .filter((value) => Number.isFinite(value) && value > 0.3)
        .map((value) => Number(value.toFixed(2)))
        .filter((value, index, list) => list.indexOf(value) === index);
    }

    async function renderExportBlob(container) {
      const rect = container.getBoundingClientRect();
      const width = Math.max(Math.ceil(rect.width), 1);
      const height = Math.max(Math.ceil(rect.height), 1);
      const attempts = getExportScaleAttempts(container);
      let lastError = null;

      for (const scale of attempts) {
        try {
          const canvas = await window.html2canvas(container, {
            scale,
            useCORS: true,
            backgroundColor: '#ffffff',
            windowWidth: 1080,
            windowHeight: height,
            width,
            height,
            scrollX: 0,
            scrollY: -window.scrollY,
            imageTimeout: 15000,
            logging: false,
          });
          const blob = await canvasToBlob(canvas);
          return blob;
        } catch (error) {
          lastError = error;
          console.warn('导出长图重试失败，尝试更低分辨率', { scale, error });
        }
      }

      throw lastError || new Error('长图生成失败');
    }

    async function exportToImage() {
      const btn = document.querySelector('.export-btn');
      const container = document.getElementById('app-container');
      if (!btn || !container || !window.html2canvas) {
        alert('导出组件尚未准备完成，请刷新页面后重试。');
        return;
      }

      const originalText = btn.innerHTML;
      btn.innerHTML = '生成中...';
      btn.style.opacity = '0.7';
      btn.style.pointerEvents = 'none';
      container.classList.add('mobile-export-mode');

      await new Promise((resolve) => setTimeout(resolve, 320));

      try {
        if (document.fonts && document.fonts.ready) {
          await document.fonts.ready;
        }
        const blob = await renderExportBlob(container);
        const objectUrl = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.download = '可视化长图文报告.png';
        link.href = objectUrl;
        document.body.appendChild(link);
        link.click();
        link.remove();
        setTimeout(() => URL.revokeObjectURL(objectUrl), 1000);
        resetExportState(btn, container, originalText);
      } catch (error) {
        console.error('导出失败:', error);
        resetExportState(btn, container, originalText);
        alert('导出失败：页面过长时浏览器会限制图片尺寸。我已自动尝试降采样；如果仍失败，请告诉我，我再改成分段导出。');
      }
    }
  </script>
</body>
</html>
```

## 关键原则 (Rules)
1. **绝不丢失原文信息**：排版可以改变，但原文档的核心内容、段落必须完整保留。
2. **拒绝干瘪的列表**：如果原文有 1,2,3,4 的长列表，必须将其转换为 `.ppt-grid` 或 `.dimension-grid` 卡片排版。
3. **图表与表格并重**：单一数据趋势用图表（Chart），多维度的对比或分类信息必须提炼为精美的 `.custom-table` 表格，表格文字必须高度精炼。对于周报、榜单、简报类页面，默认至少同时出现两者。
4. **移动端优先**：必须严格保留 `@media (max-width: 768px)` 相关的 CSS，确保手机端阅读体验（包括表格的横向滚动）。
5. **长图导出功能**：必须在 HTML 中包含 `html2canvas` 脚本和 `.export-btn` 按钮，实现一键保存为高清 PNG。
6. **长图排版优先可控 HTML**：主标题、导语、关键正文必须使用普通 HTML 元素，不要完全依赖 T8 渲染，否则导出长图时字号控制可能失效。
7. **不要机械复用源文档的排名词作为标题或主结构**：如果原文叫 `Top5周报`，输出标题默认写成更中性的 `教育科技与AI前沿周报`；排序信息只保留在内容顺序中，不必反复写 `Top5`、`Top 1`、`Top 2`。
8. **图表与表格按文档类型决定刚性程度**：普通长文可按需使用；但 `周报 / 榜单 / 多条资讯汇总` 默认必须有 `1个精炼表格`，并至少有 `1种真正有信息增量的可视化模块`，除非用户明确要求极简版。
8.1 **当适合图表时，要主动调用 visualization**：只要图表能明显提升理解，就不要停留在“我可以做图”或“用表格也行”；必须主动加载 `visualization` skill，产出真实图表。
9. **不要在最终交付页加入“制作说明 / 重组说明 / 解释性尾注”**：例如“本版按三段式重组原文”这类元说明，除非用户明确要求显示编辑说明。
10. **不要加入防御性事实边界说明**：例如“本页依据某报告整理”“图形根据已披露数据重新编排”“不新增原报告未披露事实”等尾注，默认都不进页面。来源管理应放在 Markdown/Word 或由用户明确指定展示。
11. **时间线和对比表标题下默认不要再补解释段**：若标题本身已足够清楚，就不要再机械加一小段说明文字；只有它确实提供新的阅读帮助时才保留。
12. **正文默认两端对齐**：主阅读区的正文段落、卡片文字和拆解说明默认使用 `两端对齐`，不要输出左参差正文。
13. **来源默认不进 HTML**：若源文档含 `附：参考来源` 或同类来源列表，默认不要渲染进最终 HTML 页面；如需展示，必须由用户明确要求。
14. **周报/榜单页要有视觉密度，但不要堆无效模块**：优先保留 `dimension-grid`、`timeline`、`custom-table`、重点高亮等真正帮助阅读的元素；不要默认塞 `metric-strip` 和低信息增量的结构统计图。
15. **字号不能像说明页一样偏小**：对周报/榜单类页面，正文、时间线标题、卡片标题、表头和表格正文都应比普通网页大一档，至少达到“可以快速扫读”的阅读级别；不要让真正关键的信息比章节标题小太多。
16. **长图导出态必须继续放大**：导出后的正文、时间线标题、表头和重点标签必须在手机长图里清晰可读，不能出现“网页里还能看、导出长图就看不清”的情况。
17. **批次文件夹要保持一致**：若该 HTML 属于 `docs/研究/` 或 `docs/周报/` 下某次正式生成，HTML、PNG 预览图和其他网页派生文件必须与同批次的 `.md`、`.docx` 放在同一批次文件夹内，不要散落到父目录。