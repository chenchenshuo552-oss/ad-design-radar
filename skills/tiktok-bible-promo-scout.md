---
name: "tiktok-bible-promo-scout"
description: "TikTok 圣经类 App 推广素材 Scout：白名单搜索→筛选→完整周报→GitHub Pages→飞书归档（每期限 4 积分）"
---

# TikTok 圣经类 App 推广素材 Scout 工作流

## 触发条件 (Trigger)
当用户说"跑一期周报"、"更新圣经素材周报"、"跑 scout"、"生成这周 TikTok 圣经素材报告"等时触发。

## 目标
从 TikTok 真实搜索抓取圣经类 App 推广素材（官方投放 / 达人种草 / UGC 带货），自动筛选推广与泛内容，生成完整版周报，部署可点链接的在线素材浏览器，并归档到独立飞书集合文档。

## 前置条件
- 工具链目录：`tiktok-mcp/`（包含 scout.mjs、save_all.mjs、filter_promo.mjs、weekly_pipeline.mjs 等脚本）
- API Key：从 `probe-local.mjs` 的 `TIKNEURON_MCP_API_KEY: 'xxx'`（32 位）抽取，运行时不落盘、不进任何文档
- TikNeuron 账号（https://tikneuron.com/signin），免费账号一次性 20 积分

## 执行步骤 (Steps)

### 1. 安全声明
搜索开始前必须声明："禁止输入任何敏感信息。本次仅执行 Bible 相关搜索，关键词白名单已启用。"

### 2. 运行一键流水线（推荐）
```powershell
cd tiktok-mcp
node weekly_pipeline.mjs
```
自动完成：4 词搜索（scout.mjs，白名单强制）→ save_all.mjs 落盘 scout_all.json → filter_promo.mjs 分类（classified.json）→ gen_report.mjs → build_browser.mjs / gen_promo_links.mjs（HTML 素材浏览器）→ deploy_promo.mjs（GitHub Pages）→ 校验（真实 videoID + 无密钥泄漏）。

### 3. 生成完整版周报
```powershell
cd ..
node gen_full_report.mjs
```
输出：`promo_report_full_YYYYMMDD.md`（自动读取最新数据、自动 MMDD 期标题、数字/素材名/结论全部动态推导，无硬编码旧数据）。

### 4. 归档到飞书集合文档
将完整版 md 用飞书文档更新工具（feishu_update_doc）**分 4~5 段**追加/插入到独立集合文档 PIPhdirdpovri0xpf1al1MkHgQd（新一期放最前）：
- 第 1 段：标题（`# TikTok 圣经类 App 推广素材周报 · MMDD 期`，H1 可折叠）+ 自动生成说明 + 分析对象（链接三行间必须空行）+ 本期速览 + 强推素材&强推文案
- 第 2 段：推广素材精拆（全量）
- 第 3 段：泛内容灵感池 + 文案套路汇总 + 创意复用点 + 本期总结
- 第 4~5 段：附录全量清单表格（过大拆两段）+ 结尾语

注意：大内容单次 append 会报 field validation failed，必须分段；表格追加时若紧跟表格块会渲染成纯文本，需先删旧再以新表头追加。

### 5. 汇报结果
回复给出：1) 本地报告绝对路径；2) 飞书文档链接；3) 在线素材浏览器链接；4) 关键数据摘要（推广条数/泛内容条数/Top 素材/品牌前三）；5) 报错或拦截原因。

## 默认关键词（白名单，每期固定 4 个，禁止增减）
1. Bible app daily devotional
2. Bible verses for anxiety
3. Christian morning prayer
4. Bible study routine

## 积分铁律（极其重要）
- 每次 TikNeuron MCP 调用 = 1 积分；每期只跑上述 4 词 = **恰好 4 积分**。
- 禁止任何扩展/批量搜索（曾一次跑几十词把积分烧光）。
- 免费账号 = **一次性 20 积分**（非每月发放），用完换新账号 key（每账号一次 20 积分）；Pro $7.49/月 = 500 积分（每月重置）。
- 若报 Insufficient credits 或返回空结果（每词 1 raw 但解析 0 条）＝当前账号积分用尽：**立即停止并报告换 key，不反复重试**。
- 官网：https://tikneuron.com/pricing / https://tikneuron.com/signin / https://tikneuron.com/tools/tiktok-mcp

## 安全铁律（最高优先级）
1. 仅允许 Bible 相关关键词搜索；scout.mjs 内置 SAFE_PATTERNS + SAFE_QUERIES 白名单，非 Bible 词直接 BLOCKED（退出码 2），任何情况下不得绕过。
2. 报告只写入独立集合文档 PIPhdirdpovri0xpf1al1MkHgQd；**绝对禁止修改/编辑团队现有文档**（如 Bible Note 双周会 等 wiki）。
3. 搜索开始前声明禁止输入任何敏感信息。
4. API key 不落盘、不进文档、不写入任何输出。

## 周报结构（每期固定，与 0831/0907 一致）
`# TikTok 圣经类 App 推广素材周报 · MMDD 期`（H1）→ 自动生成说明 → 分析对象·素材浏览器（GitHub Pages 链接，行间空行）→ 一、本期速览（数据总览+品牌热度表+Top素材表）→ ⭐强推素材&✍️强推文案 → 二、推广素材精拆 → 三、泛内容灵感池 → 四、文案套路汇总 → 五、创意复用点 → 六、本期总结 → 附录全量清单。

## 在线链接格式（飞书内必须用，禁止本地路径）
- 推广链接页：https://chenchenshuo552-oss.github.io/ad-design-radar/promo-materials/promo_links_YYYYMMDD.html
- 全量浏览器：https://chenchenshuo552-oss.github.io/ad-design-radar/promo-materials/tiktok_material_browser_YYYYMMDD.html

## 工具链说明
- `scout.mjs`：TikNeuron MCP tiktok_search 真实搜索 + 白名单强制
- `save_all.mjs`：抽取 key、落盘 scout_all.json
- `filter_promo.mjs`：推广（官方/CTA/品牌种草）vs 泛内容分类
- `gen_report.mjs` / `build_browser.mjs` / `gen_promo_links.mjs`：报告 + HTML 浏览器
- `deploy_promo.mjs`：GitHub Pages 部署
- `gen_full_report.mjs`：完整版周报生成器（动态）
- `weekly_pipeline.mjs`：一键流水线
