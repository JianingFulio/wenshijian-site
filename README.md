# 问先机官网 · 部署包

纯静态单页站点，无构建步骤、无外部字体/JS 依赖，解压到站点根目录即可访问。

## 目录结构

```
.
├── index.html          首页（唯一页面）
├── assets/
│   └── og-cover.png    分享卡片图 1200×630
├── robots.txt          爬虫规则（含 GPTBot / PerplexityBot 等 AI 引擎放行）
├── sitemap.xml         站点地图
└── llms.txt            给大模型的站点摘要（GEO 用）
```

## 部署方式

任选其一，**保持目录结构不变**：

1. **对象存储 + CDN**（阿里云 OSS / 腾讯云 COS / Cloudflare Pages）：整个目录上传，开启默认首页 `index.html`
2. **Nginx**：解压到站点根目录（如 `/usr/share/nginx/html`），`index index.html;`
3. **Vercel / Netlify**：直接拖拽本目录

要求：站点根路径为域名根目录（`https://wenshijian.com/`），因为 `robots.txt`、`sitemap.xml`、`llms.txt` 约定放在根目录。

## 上线后检查清单

- [ ] `https://wenshijian.com/` 正常打开，首屏动效正常（输入框轮播打字、数据条数字增长）
- [ ] `https://wenshijian.com/robots.txt`、`/sitemap.xml`、`/llms.txt` 均可访问
- [ ] `https://wenshijian.com/assets/og-cover.png` 可访问（微信/社交分享卡片依赖它）
- [ ] 页脚备案号 `沪ICP备2026053600号-2` 可点，跳转工信部
- [ ] 用 [Rich Results Test](https://search.google.com/test/rich-results) 验证结构化数据（FAQPage 应能通过）
- [ ] 把 `sitemap.xml` 提交到 Google Search Console / 百度搜索资源平台

## 需要确认的事项

1. **域名**：全站统一使用 `wenshijian.com`（共 22 处，含 canonical / OG / JSON-LD）。如需换成 `wenxianji.cn`，全局替换即可。
2. **结构化数据里的价格**：`SoftwareApplication.offers` 目前写的是 `price: 0`（免费体验）。上线后若有付费档，需要改成真实 Offers，否则搜索结果里的富摘要会不准。
3. **备案信息**：公司主体与备案号已挂在页脚，若有变更需同步 `index.html` 页脚。

## 技术说明

- 单文件 HTML，样式与脚本内联，首屏无额外请求
- 产品界面示意（线索集 / 发现区抽屉 / 客户抽屉）是 HTML + CSS 复刻，非图片，任何分辨率下都是矢量清晰
- 「覆盖企业主体」数字存在浏览器 `localStorage`，刷新后接着上次的数继续增长；换浏览器/无痕模式会从 1,070,000 重新开始
- 页面在 `?all=1&flat=1` 参数下会进入静态截图模式（关闭入场动效），正常访问无需带参数
