# 入戏 · 跟着剧情去旅行

2026 飞猪 AI 旅行创新大赛参赛作品 · 团队：小mo AI

说一部剧 + 出发城市 + 几天 → 一本可导航、可预订、可发帖的逐日打卡手帐。

## 在线 Demo（点开即玩）

- **首页**：https://xiaomo-agi.github.io/feizhu-ruxi-demo/
- 《恶作剧之吻》台湾 6 天 5 晚（广州出发）：[/kiss.html](https://xiaomo-agi.github.io/feizhu-ruxi-demo/kiss.html)
- 《去有风的地方》大理 3 天（杭州出发）：[/dali.html](https://xiaomo-agi.github.io/feizhu-ruxi-demo/dali.html)

## 复现方式（约 15 分钟）

1. 登录[阿里云百炼控制台](https://bailian.console.aliyun.com) → **Managed Agents** → 创建智能体
2. **知识库**：新建知识库，上传本仓库 4 个文档——
   - `knowledge/剧知识库-去有风的地方.md`
   - `knowledge/剧知识库-人世间.md`
   - `knowledge/剧知识库-恶作剧之吻.md`
   - `docs-output-template.md`（手帐输出模板/渲染规范）
3. **系统提示词**：粘贴 `docs-system-prompt.md` 的内容（从「你是入戏」开始）
4. **工具**：接入 flyai skill（飞猪官方 skill，实时商品库）
5. 对话输入示例：
   > 我想跟着《去有风的地方》去大理，成都出发 3 天慢节奏，我自己一个人
6. 智能体将：检索剧知识库 → flyai 实查机票/酒店/门票 → 编排逐日动线 → 按输出模板渲染手帐 HTML

## 架构说明

- **内容层**（百炼 Managed Agent，模型 GLM-5.1）：剧知识库检索 + flyai 实时库存 + 动线编排 + 手帐内容生成
- **渲染层**（输出模板 + 渲染 skill）：手帐视觉、高德导航链接（坐标→`uri.amap.com` 规则固化在模板）、飞猪预订票根——保证每次输出一致可用；裸 agent 未挂 skill 时输出朴素 HTML 且默认给 Google Maps 链接（大陆不可达）

## 数据说明

- 机票/酒店/门票价格与预订链接：飞猪实时查询（demo 页价格为生成日实价，以预订时为准）
- 剧知识库信源：大理文旅官网、新华网、央广网、人民网等官方与权威媒体，逐条交叉核验；单源存疑信息保留「待核验」标注
- 机位对照图源：小红书 @94101752935（借用于机位示意）
