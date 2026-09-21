# learning-docs

> 让 AI 帮你写**由浅及深、有据可查**的技术学习文档，并直接产出可浏览的 docsify 站点。
>
> An agent skill that turns a topic or an existing codebase into a learning-oriented documentation site (docsify).
> Two modes: *research* (survey a topic from the web) and *codebase walkthrough* (explain an existing project).
> See [English summary](#english-summary) below.

```bash
npx skills add XuHenghenglumia/learning-docs
```

---

## 它解决什么问题

让 AI "写文档" 很容易，但产出的东西常常是：开场先甩抽象定义、大段并排的段落、例子全靠编。

这个 skill 用一套**强制纪律**约束生成过程，让文档真的能被读懂：

- **由浅及深**：每个概念必须按「直觉比喻 → 真实实例 → 源码/形式化定义」三层递进展开，顺序不可颠倒；
- **实例先行**：先摆真实产品/真实代码怎么做的，再抽象出原理，而不是反过来；
- **事实核实**：关键论断必须落到官方文档或源码层面，博客与官方冲突时以官方为准并注明差异；没查证的写「待确认」，**禁止编造**；
- **代码引用必须带 `路径:行号`**，引用片段控制在 15 行以内；
- **对比用表格、选型给决策树、复杂结构画 ASCII 图**，不依赖外部图片。

## 两种模式

| 模式 | 触发场景 | 需要你提供 | 产出位置 |
| --- | --- | --- | --- |
| **调研型** | 「调研 XX 并整理成文档」「做一份学习笔记」 | 一个技术主题 | `<当前目录>/<主题英文短名>-docs/` |
| **项目讲解型** | 「帮我理解/讲讲这个项目」「读懂这个代码库」 | 一个本地仓库或知名开源项目 | `<项目根目录>/learn-docs/` |

判定不了时它会反问你一次，不会瞎猜。

### 章节骨架

调研型（从问题切入，不是从定义切入）：

```
1. 为什么需要 X        ← 从具体问题/危险场景切入
2. 现有实例盘点        ← 真实产品怎么做，表格总览 + 逐个展开
3. 底层原理拆解        ← 把实例背后的技术逐层剥开，弱→强排列
4. 优劣分析            ← 每种实现能做到什么 vs 牺牲了什么
5. 理论/本质           ← 数学或第一性原理层面（主题适合时）
6. 总结                ← 异同对照表、选型决策树、原则清单
```

项目讲解型：

```
1. 这个项目是什么      ← 解决什么问题、给谁用、一句话定位
2. 整体架构与目录导览  ← 模块划分图 + 关键文件职责
3. 开发思路与设计决策  ← 从 git history / issue / README 找动机，不事后推测
4. 核心流程走读        ← 挑 1~3 条主线流程，带 文件:行号 走读
5. 关键技术原理深挖    ← 项目依赖的核心技术往下挖一层
6. 如何运行与实验      ← 让你自己动手验证理解的步骤
7. 总结与延伸阅读
```

## 工作流程

```
第 0 步  判定模式（调研 / 项目讲解）
第 1 步  先出章节大纲给你确认 —— 确认前不动笔
第 2 步  收集素材（多角度检索 / 通读代码 + 跑构建测试）
第 3 步  按写作细则展开写作
第 4 步  组装 docsify 站点（index.html + README + _sidebar.md + 各章节）
第 5 步  交付：报告结构 + 手动启动命令（不自动拉起服务）
```

## 产出长什么样

```
learn-docs/
├── index.html      ← docsify 站点（带全文搜索、MathJax 公式、Prism 代码高亮）
├── README.md       ← 首页：阅读路线 + 一分钟版本的总括直觉
├── _sidebar.md     ← 侧边栏，与章节一一对应
└── 01-*.md ...     ← 各章节
```

浏览方式（skill 只给命令，不会擅自后台起服务，也不去探测端口或猜 IP）：

```bash
cd learn-docs && python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

端口被占用就自己换一个（8001 或任意空闲端口）。

## 安装

```bash
# 交互式安装（会询问装到哪些 agent、全局还是项目）
npx skills add XuHenghenglumia/learning-docs

# 非交互：装到所有已检测到的 agent，全局作用域
npx skills add XuHenghenglumia/learning-docs -g --all

# 只看不装
npx skills add XuHenghenglumia/learning-docs --list
```

也可以用 [skills CLI](https://github.com/vercel-labs/skills) 支持的任意来源格式（GitHub URL、git URL、本地路径）。

**手动安装**：把 `skills/learning-docs/` 整个目录拷到你的 agent 技能目录，例如：

| Agent | 全局路径 |
| --- | --- |
| Claude Code | `~/.claude/skills/` |
| Codex / Cursor / OpenCode 等 | `~/.agents/skills/` |
| Pi | `~/.pi/agent/skills/` |

## 环境要求

| 能力 | 用途 | 必需 |
| --- | --- | --- |
| 读文件 / 写文件 | 读代码、读参考资料、产出文档 | 是 |
| 执行命令 | 跑构建/测试验证对行为的描述 | 建议 |
| 联网检索（`web_search` 等） | 调研型的素材收集与事实核实 | 调研型必需 |
| 用户提问工具 | 大纲确认、模式判定 | 可选，没有就转为对话里询问 |

纯中文写作场景：文档正文为中文，术语保留英文原词（首次出现附中文注释）。

## 仓库结构

```
.
├── README.md
├── LICENSE
└── skills/
    └── learning-docs/
        ├── SKILL.md                  ← 主流程（模式判定 → 大纲 → 素材 → 写作 → 站点 → 交付）
        ├── references/writing-style.md  ← 写作细则（递进原则、结构手法、语言规范、引用纪律）
        └── assets/index.html         ← docsify 站点模板（搜索 + 公式 + 代码高亮）
```

## English summary

**learning-docs** is an agent skill for producing learning-oriented technical documentation as a
browsable [docsify](https://docsify.js.org/) site. It has two modes:

- **Research mode** — survey a technical topic across the web (multiple query angles, official docs
  and source code prioritized over blog posts) and write a progressive explainer.
- **Codebase walkthrough mode** — read an existing repository and produce a guide that takes a
  reader from "what is this project" down to core flows and underlying mechanisms, citing
  `path:line` for every claim.

Hard rules it enforces: layering every concept as *intuition → real example → formal definition* in
that order; examples before abstractions; tables for comparisons; decision trees for trade-offs;
ASCII diagrams for structure; and **no fabricated facts** — anything unverified is marked as such.
Document text is written in Chinese, keeping English terms for technical vocabulary.

```bash
npx skills add XuHenghenglumia/learning-docs
```

Requires an agent with file read/write and (for research mode) web search.

## License

[MIT](LICENSE)
