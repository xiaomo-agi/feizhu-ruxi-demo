# 入戏 · 手帐输出模板（HTML 渲染规范）

> 用途：智能体产出最终路书时，按本模板输出完整 HTML 文档。设计概念：追剧手帐·青春纪念册——奶油纸底、拍立得封面、日历页卡片、情书便签台词、票根按钮。

## 渲染规则

1. 完整输出一个 HTML 文档（DOCTYPE 到 </html>），CSS 全部内联在 <style>，不引用外部资源（字体 CDN 除外）
2. 组件映射：
   - 封面 = .cover 拍立得（剧名+副题+出发信息）
   - 航班/酒店 = .ticket 机票票卡（表格+.t-tip 建议+.stub 票根按钮）
   - 每一天 = .day-card 日历页（左 .d-num 大日期数字 + 右标题）
   - 点位 = .blk 块（b-title 带 emoji）
   - **图片 = .polaroid 拍立得相框**（每天至少 1 张：flyai 返回的 mainPic/picUrl 直接用 <img>，配 .cap 手写小字注明「D几 · 地点 · 一个感受」；无图的点位不硬凑，跳过）
   - **台词彩蛋 = .quote 粉色情书便签**（正文引号原文 + small 出处/集数状态）
   - 预约/注意 = .warn 黄色警示条
   - 剧迷注解 = .note 粉色侧条
   - 预订链接 = .stub 票根按钮（<a>，✂ 虚线感由 CSS 实现）
   - 美食清单 = .food-grid 贴纸墙（.food 小卡，可微旋转）
   - 预算 = .budget 账本表格（末行 .total）
   - 贴士 = .tips 便利贴网格（.tip，双色交替）
   - 尾声 = .finale（一句收束台词 + .seal 红章「Based on fly.ai · 实时数据」）
3. 数据规矩：所有价格照 flyai 实查值填入；台词**逐字**从知识库引用；导航链接用 `https://uri.amap.com/search?keyword=点位名&city=城市` 挂在 .stub 或 .blk 内；图片只用 flyai 返回的真实图 URL
4. 移动优先：单列 max-width 700px 居中

## 完整 CSS（直接复制进 <style>）

```css
:root{--paper:#f7f1e6;--ink:#3a3a3a;--navy:#2c3a63;--navy-deep:#1f2a49;--pink:#e89bb0;--pink-soft:#fdf0f3;--tape:#f0d9a8;--tape2:#d8e6c3;--line:#d9cfbc;--gold:#c9a86a}
*{margin:0;padding:0;box-sizing:border-box}
html{scroll-behavior:smooth}
body{font-family:'LXGW WenKai','PingFang SC',serif;background:var(--paper);color:var(--ink);line-height:1.8;font-size:16px}
.wrap{max-width:700px;margin:0 auto;padding:28px 18px 80px}
.cover{background:#fff;padding:16px 16px 20px;box-shadow:0 12px 40px rgba(44,58,99,.18);transform:rotate(-1.6deg);margin:20px 6px 10px;position:relative}
.cover .photo{background:linear-gradient(135deg,#2c3a63 0%,#4a5f96 55%,#e89bb0 130%);height:190px;border-radius:2px;display:flex;flex-direction:column;align-items:center;justify-content:center;color:#fff;text-align:center;padding:20px}
.cover .photo h1{font-size:34px;margin:10px 0 6px;letter-spacing:2px}
.tape{position:absolute;width:96px;height:30px;background:var(--tape);opacity:.82;top:-13px;left:50%;transform:translateX(-50%) rotate(-3deg);box-shadow:0 2px 6px rgba(0,0,0,.12)}
.meta-strip{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin:22px 0 8px}
.meta-strip span{background:#fff;border:1.5px dashed var(--navy);border-radius:999px;padding:5px 16px;font-size:14px;color:var(--navy)}
.toc{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin:26px 0 34px}
.toc a{text-decoration:none;font-size:14px;padding:6px 14px;border-radius:6px;background:var(--pink-soft);color:#a5566e;border:1.5px solid #eec6d1}
.sec-title{display:flex;align-items:center;gap:14px;margin:52px 0 20px}
.sec-title .stamp{background:var(--navy);color:#fff;padding:8px 18px;font-size:18px;border-radius:4px;transform:rotate(-2deg);box-shadow:3px 3px 0 var(--pink)}
.sec-title .line{flex:1;border-top:2px dashed var(--line)}
.ticket{background:#fff;border-radius:14px;box-shadow:0 8px 26px rgba(31,42,73,.12);margin-bottom:18px;overflow:hidden}
.ticket .t-head{background:var(--navy);color:#fff;padding:12px 20px;display:flex;justify-content:space-between;align-items:center}
.ticket .t-head .tag{background:var(--pink);color:#fff;font-size:12px;padding:2px 10px;border-radius:999px;margin-right:8px}
.ticket .t-body{padding:16px 20px}
.ticket table{width:100%;border-collapse:collapse;font-size:14px}
.ticket th{color:#9a8f7d;font-weight:400;font-size:12.5px;text-align:left;border-bottom:1.5px dashed var(--line);padding:4px 6px}
.ticket td{padding:8px 6px;border-bottom:1px solid #efe9dc}
.ticket .rec{background:var(--pink-soft)}
.ticket .t-tip{padding:10px 20px;font-size:13.5px;color:#7a7060;background:#fbf8f1}
.stub{display:inline-block;margin:10px 20px 16px;text-decoration:none;background:var(--navy);color:#fff;padding:9px 22px;font-size:14.5px;border-radius:4px;transition:all .25s;box-shadow:0 4px 12px rgba(44,58,99,.3)}
.stub:hover{transform:translateY(-2px);background:var(--navy-deep)}
.day-card{background:#fff;border-radius:4px;box-shadow:0 10px 30px rgba(31,42,73,.1);margin:30px 0;overflow:hidden}
.day-card .d-head{display:flex;align-items:stretch}
.day-card .d-num{background:var(--navy);color:#fff;min-width:96px;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:14px 8px}
.day-card .d-num b{font-size:34px;line-height:1;font-family:Georgia,serif}
.day-card .d-num i{font-style:normal;font-size:12px;letter-spacing:3px;opacity:.75;margin-top:4px}
.day-card .d-title{flex:1;padding:14px 18px;display:flex;flex-direction:column;justify-content:center;background:linear-gradient(90deg,#fff,#fdf8ef)}
.day-card .d-title h3{font-size:18.5px;color:var(--navy-deep)}
.day-card .d-title small{color:#9a8f7d;font-size:13px}
.d-body{padding:6px 20px 18px}
.blk{padding:14px 4px;border-bottom:1px dashed #e8e0cf}
.blk .b-title{font-size:16.5px;color:var(--navy-deep);margin-bottom:6px}
.blk p,.blk li{font-size:14.5px;color:#55503f}
.blk ul{padding-left:20px;margin:4px 0}
.note{background:var(--pink-soft);border-left:4px solid var(--pink);border-radius:0 8px 8px 0;padding:10px 14px;margin:10px 0;font-size:13.5px;color:#8a5568}
.warn{background:#fdf6e3;border-left:4px solid var(--gold);border-radius:0 8px 8px 0;padding:10px 14px;margin:10px 0;font-size:13.5px;color:#8c7137}
.quote{background:#fff9fb;border:1.5px solid #f2ccd8;border-radius:10px;padding:14px 16px 12px;margin:14px 0;position:relative;transform:rotate(-.6deg);box-shadow:2px 4px 10px rgba(232,155,176,.25);font-size:15.5px;color:#a5566e;text-align:center}
.quote::before{content:'📌';position:absolute;top:-12px;left:50%;transform:translateX(-50%)}
.quote small{display:block;color:#c58f9f;font-size:12px;margin-top:5px}
.food-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:14px}
.food{background:#fff;border-radius:10px;padding:14px;box-shadow:0 5px 16px rgba(0,0,0,.08);border:1px solid #f0e9da}
.food b{display:block;color:var(--navy-deep);font-size:15.5px;margin-bottom:4px}
.food span{font-size:12.5px;color:#978c78}
.budget{background:linear-gradient(#fffdf6,#faf5e9);border:1.5px solid var(--line);border-radius:12px;padding:20px}
.budget table{width:100%;border-collapse:collapse;font-size:14.5px}
.budget td{padding:7px 4px;border-bottom:1px dashed #e5dcc8}
.budget td:last-child{text-align:right;color:#7a6c4d}
.budget .total{font-size:16px;color:var(--navy);font-weight:700;border-bottom:none;padding-top:12px}
.tips{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:14px}
.tip{background:var(--tape2);border-radius:2px;padding:14px 16px;font-size:13.5px;box-shadow:3px 4px 10px rgba(0,0,0,.1);transform:rotate(-.8deg);color:#4e5c42}
.tip:nth-child(even){transform:rotate(.9deg);background:#fdeecb}
.tip b{color:#3e4a35;display:block;margin-bottom:3px}
.finale{margin-top:56px;text-align:center;padding:36px 20px;background:#fff;border-radius:14px;box-shadow:0 12px 36px rgba(44,58,99,.12);transform:rotate(.5deg)}
.finale .fq{font-size:18px;color:var(--navy);margin-bottom:10px}
.finale p{color:#8a8172;font-size:14.5px}
.seal{display:inline-block;margin-top:20px;border:2.5px solid #c96b6b;color:#c96b6b;border-radius:8px;padding:6px 14px;font-size:13px;transform:rotate(-4deg);letter-spacing:1px;opacity:.85}
.polaroid{background:#fff;padding:10px 10px 8px;margin:16px auto;width:88%;box-shadow:0 8px 22px rgba(31,42,73,.18);transform:rotate(-1.4deg)}
.polaroid.rt{transform:rotate(1.3deg)}
.polaroid img{width:100%;display:block;border-radius:2px;aspect-ratio:4/2.6;object-fit:cover;background:#eee}
.polaroid .cap{text-align:center;font-size:13px;color:#8b8071;padding:7px 0 2px}
footer{margin-top:36px;text-align:center;font-size:12.5px;color:#a89c86}
@media(max-width:520px){.cover .photo h1{font-size:26px}.day-card .d-num{min-width:74px}.day-card .d-num b{font-size:26px}}
```

字体引入（放 <head>）：
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lxgw-wenkai-webfont@1.7.0/style.css">
```

## HTML 骨架示例（结构对照）

```html
<div class="wrap">
  <div class="cover"><div class="tape"></div><div class="photo"><h1>剧名</h1><div class="sub">副题</div></div><div class="caption">一句话题词</div></div>
  <div class="meta-strip"><span>📅 日期</span><span>📍 路线</span></div>
  <nav class="toc"><a href="#day1">D1 ...</a></nav>
  <div class="sec-title"><span class="stamp">✈️ 往返机票</span><span class="line"></span></div>
  <div class="ticket"><div class="t-head">...</div><div class="t-body"><table>...</table></div><div class="t-tip">💡 推荐...</div><a class="stub" href="飞猪链接">预订 →</a></div>
  <div class="day-card" id="day1"><div class="d-head"><div class="d-num"><b>01</b><i>DAY</i></div><div class="d-title"><h3>...</h3><small>...</small></div></div>
    <div class="d-body">
      <div class="blk"><div class="b-title">📍 点位名</div><ul><li>...</li></ul>
        <div class="quote">「台词原文」<small>—— 出处/集数状态</small></div>
        <div class="warn">⚠️ 预约提醒</div></div>
    </div>
  </div>
  <div class="food-grid">...</div>
  <div class="budget">...</div>
  <div class="tips">...</div>
  <div class="finale"><div class="fq">「收束台词」</div><div class="seal">Based on fly.ai · 实时数据</div></div>
  <footer>价格以预订时实际为准</footer>
</div>
```
