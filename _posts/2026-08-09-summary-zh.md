---
layout: default
title: "Horizon Summary: 2026-08-09 (ZH)"
date: 2026-08-09
lang: zh
---

> 从 28 条内容中筛选出 9 条重要资讯。

---

**科技新闻**
1. [Shopify 用 MySQL 替换 Redis 实现库存预留扩展](#item-tech-news-1) ⭐️ 8.0/10
2. [全球最大单体 AI 算力设施在内蒙古乌兰察布投产](#item-tech-news-2) ⭐️ 8.0/10
3. [Triton：面向 QEMU 的 DirectX 11 驱动](#item-tech-news-3) ⭐️ 7.0/10
4. [Claude Code Auto 模式成为 Pro、Max、Team 计划默认](#item-tech-news-4) ⭐️ 7.0/10
5. [macOS 屏幕共享高危漏洞允许无密码登录任意账户，苹果已发布修复](#item-tech-news-5) ⭐️ 7.0/10
6. [Cloudflare：五年后 AI 机器人流量或达人类千倍](#item-tech-news-6) ⭐️ 7.0/10

**财经新闻**
1. [伯克希尔 Q2 运营利润增长 16%，新任 CEO 开始动用巨额现金储备](#item-finance-news-1) ⭐️ 8.0/10
2. [月之暗面引入国资股东并调整架构，推进赴港上市](#item-finance-news-2) ⭐️ 8.0/10
3. [内华达州电力公司起诉数据中心开发商，要求其负担 10 亿美元电网升级](#item-finance-news-3) ⭐️ 7.0/10

---

## 科技新闻

<a id="item-tech-news-1"></a>
### [Shopify 用 MySQL 替换 Redis 实现库存预留扩展](https://shopify.engineering/scaling-inventory-reservations) ⭐️ 8.0/10

Shopify 工程团队在一篇案例研究中介绍了将库存预留系统从 Redis 迁移到 MySQL 的实践，以应对扩展需求。他们采用有界连接池设计，将每个可售单元映射为一行，并在每商品/地点组合维护最多 1000 行的可用池，预留操作从池中消费行，再通过补充流程回收。团队发现真正的瓶颈并非数据库设计，而是测量与观察之外的因素。该方案在大规模库存预留场景下实现了良好扩展，为同类系统优化提供了经验。

hackernews · adletbalzhanov · 8月8日 22:32 · [社区讨论](https://news.ycombinator.com/item?id=49226536)

**「背景」** Shopify 的结账流程需要在买家开始支付时为商品预留库存，以防止超卖，因此其库存预留系统必须支撑大规模、高并发的请求。此前该系统基于 Redis 内存数据存储实现，后来 Shopify 工程团队将其替换为 MySQL，并利用 SKIP LOCKED、复合主键和连接可见性等机制来满足扩展目标。Shopify 处理着美国超过 14% 的电商交易，这使得这项数据库架构迁移具有重要的规模参考意义。

**「影响」** 对运行大规模库存系统的工程团队而言，该有界连接池模式提供了一种不依赖 Redis 即可实现高并发预留的可行路径，同时要求配合补充回收流程来处理超时或中止的订单流程。

**「社区讨论」** 评论中有人提出更简单的方案：在预留时扣减库存并同时记录进行中订单，由后台进程超时回收；也有人质疑为何不构建专用数据库，而多数读者肯定该案例的真实性和可读性，并注意到作者强调的真正瓶颈是测量方式而非数据库设计。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://shopify.engineering/scaling-inventory-reservations">We replaced Redis with MySQL for inventory reservations—and ...</a></li>
<li><a href="https://www.hellointerview.com/learn/system-design/in-the-wild/shopify-inventory-reservations">How Shopify Moved Inventory Reservations from Redis to MySQL</a></li>

</ul>
</details>

**标签**: `#mysql`, `#redis`, `#scaling`, `#database-design`, `#shopify`

---

<a id="item-tech-news-2"></a>
### [全球最大单体 AI 算力设施在内蒙古乌兰察布投产](https://www.globaltimes.cn/page/202608/1367666.shtml) ⭐️ 8.0/10

远景科技集团于 8 月 6 日宣布其“远景乌兰察布星河基地”正式投产。该基地位于内蒙古乌兰察布，建筑面积达 12 万平方米，支持百万 GPU 并行计算，规划总容量为 2GW，号称全球最大的单体 AI 算力设施及全球 Token 产出能力最强的单体 AI 数据中心，绿电占比超过 80%。乌兰察布是国家“东数西算”八大节点之一，距北京约 240 公里，数据传输延迟约 4.2 毫秒，数据中心电价较京津冀地区低约 50%。该基地是远景“戈壁使命”计划的首个旗舰项目，旨在为国产算力集群提供可复制的解决方案。此前，华为、阿里巴巴、苹果、快手等企业已在此布局算力设施。

telegram · zaihuapd · 8月9日 05:06

**「背景」** 乌兰察布是国家“东数西算”八大算力枢纽节点之一，也是承接京津冀算力需求的重要区域。远景科技集团于 2026 年 6 月在巴黎 VivaTech 上发布了“戈壁使命”全球计划，该基地是其首个旗舰项目，旨在满足国内对百万 GPU 互联的需求，并为大规模国产算力集群提供可复制的解决方案。

**「影响」** 这一设施的投产将显著增强国内 AI 训练和推理的算力供给，并借助当地低电价和绿电优势降低运营成本，为国产算力集群的大规模部署提供一个可参考的样板。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.globaltimes.cn/page/202608/1367666.shtml">World&#x27;s largest single AI computing facility enters operation in China&#x27;s Ulanqab - Global Times</a></li>
<li><a href="https://www.fuelsandlubes.com/newswire/envision-commissions-galaxy-campus-in-ulanqab-establishing-a-new-model-for-gigawatt-scale-ai-infrastructure/">Envision Commissions Galaxy Campus in Ulanqab, Establishing a New Model for Gigawatt-Scale AI Infrastructure - F&amp;L Asia</a></li>

</ul>
</details>

**标签**: `#AI infrastructure`, `#data center`, `#GPU computing`, `#green energy`, `#China tech`

---

<a id="item-tech-news-3"></a>
### [Triton：面向 QEMU 的 DirectX 11 驱动](https://blog.getutm.app/2026/introducing-triton-directx-11-driver-for-qemu/) ⭐️ 7.0/10

Triton 是一个新的开源 DirectX 11 驱动，专门面向 QEMU，旨在为 Windows 虚拟机提供 GPU 加速，填补了此前在仅有单块独立显卡的 Linux 主机上难以在 Windows 虚拟机中实现图形加速的空白。这项进展对虚拟化、图形和系统软件开发者及高级用户具有实际价值，也让 Windows 访客系统在 QEMU 生态中获得更多关注。据分析，这一开发不是范式转变，但属于高价值的技术进展，尤其对虚拟化和游戏场景有显著改善。目前关于支持范围、兼容性以及是否适用于 VirtualBox 等具体细节尚不明确，仍需参考原始文章。

hackernews · electricant · 8月8日 13:33 · [社区讨论](https://news.ycombinator.com/item?id=49221711)

**「背景」** QEMU 是一款广泛使用的开源虚拟机监控器，但长期以来 Windows 虚拟机缺乏良好的 GPU 加速支持，3D 图形性能受限。UTM 项目开发者 Osy 新发布了名为 Triton 的 Windows DirectX 11 驱动，配合 Neptune 组件，为 QEMU 虚拟机带来完整的 DirectX 11 图形支持；该驱动的开发过程中还借助了 Claude Opus 5 和 Claude Fable 5 等 AI 工具。这一进展填补了 Windows 客户机图形加速的长期空白。

**「影响」** 对于在 QEMU 上运行 Windows 虚拟机的开发者和高级用户，Triton 提供了原生 DirectX 11 加速的开放实现，可能减少对 GPU 直通等复杂方案的依赖，从而简化图形加速环境的搭建。

**「社区讨论」** 评论者普遍表示欢迎，认为这是多年来期待已久的解决方案，并指出 Windows 访客系统终于得到更多关注；也有用户询问是否支持 DirectX 10 及更早版本、能否用于 VirtualBox，还有人调侃“Triton”已成为至少第三个与 GPU 相关的项目名称。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://blog.getutm.app/2026/introducing-triton-directx-11-driver-for-qemu/">Introducing Triton: DirectX 11 driver for QEMU | UTM Blog</a></li>
<li><a href="https://www.phoronix.com/news/Triton-DirectX-11-QEMU-Driver">AI Helped Create A DirectX 11 Driver For QEMU VMs - Phoronix</a></li>
<li><a href="https://www.generationamiga.com/2026/08/01/utm-triton-brings-directx-11-graphics-to-qemu-on-apple/">UTM Triton brings DirectX 11 graphics to QEMU on Apple – GenerationAmiga.com</a></li>

</ul>
</details>

**标签**: `#virtualization`, `#graphics`, `#QEMU`, `#DirectX`, `#open-source`

---

<a id="item-tech-news-4"></a>
### [Claude Code Auto 模式成为 Pro、Max、Team 计划默认](https://simonwillison.net/2026/Aug/8/auto-mode/#atom-everything) ⭐️ 7.0/10

Anthropic 宣布，从 2026 年 8 月 14 日起，Claude Code 的 Pro、Max、Team 计划中 auto mode 将成为新会话的默认设置。这一变更基于 Anthropic 对 1,053 名付费测试者的对照研究：当会话中的权限提示被替换为明显危险命令时，只有 13.6%的人类测试者拒绝该操作，而 auto mode 可拦截 89%的危险动作。Anthropic 还援引第三方 Trajectory Labs 截至 2026 年 7 月 17 日的评估称，在针对最新版 Claude Code 和 Codex 的 720 个间接提示注入攻击尝试中，运行 auto mode 的 Claude Fable 5、Opus 5 和 Sonnet 5 没有一次攻击成功。不过，Simon Willison 指出仍有 11%的危险动作未被阻止，并认为针对恶意第三方软件包等攻击形态需要更多独立验证。

rss · Simon Willison · 8月8日 22:36

**「背景」** Claude Code 是 Anthropic 的命令行 AI 编程工具，auto mode 是一种让模型在会话中自动执行操作、只在必要时请求用户批准的模式。Anthropic 此前在内部表示，几乎所有员工都使用 auto mode，并称已基本缓解提示注入和数据外泄等主要风险类别，此次默认变更正是基于这种信心以及新公布的评测结果。

**「影响」** 对 Claude Code Pro、Max 和 Team 计划用户而言，8 月 14 日后新会话将默认使用 auto mode，从而明显减少人工确认提示，但仍需留意 auto mode 无法覆盖的约 11%危险动作以及提示注入攻击的残余风险。

**标签**: `#Claude Code`, `#Anthropic`, `#AI coding tools`, `#developer tools`, `#product update`

---

<a id="item-tech-news-5"></a>
### [macOS 屏幕共享高危漏洞允许无密码登录任意账户，苹果已发布修复](https://x.com/calif_io/status/2086022794840793454) ⭐️ 7.0/10

安全研究人员公开了苹果 macOS 屏幕共享功能中的一个关键漏洞 PoC（CVE-2026-65400）。一旦屏幕共享处于开启状态，任何网络攻击者都可在不知道密码的情况下，以任意账户身份登录受影响的 Mac。苹果已在 macOS 26.6.1 中修复此漏洞，用户应尽快升级。研究人员称已逆向工程该补丁以厘清漏洞根因与利用路径，完整技术分析将于明日发布。

telegram · zaihuapd · 8月8日 14:20

**「背景」** macOS 的屏幕共享（Screen Sharing）是系统内置的远程桌面功能，允许用户通过网络查看或控制其他 Mac。CVE-2026-65400 是影响该功能的身份验证绕过漏洞，攻击者可在不知道密码的情况下远程登录任意账户；它与此前披露的 CVE-2026-43760 是两个独立漏洞，但都涉及同一服务。苹果已在 2026 年 7 月 27 日和 8 月 6 日发布相关补丁，其中包含 macOS 26.6.1 更新。

**「影响」** 对于所有启用了屏幕共享且尚未升级到 macOS 26.6.1 的 Mac 用户，该漏洞意味着攻击者可以在无需任何凭据的情况下远程登录任意账户，可能导致数据泄露或系统被完全控制。用户应立即应用系统更新以消除风险。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.huntress.com/blog/macos-screen-sharing-rce-patched">From Screen Share to Root Access: Breaking Down CVE - 2026 -43760...</a></li>
<li><a href="https://securityvulnerability.io/vulnerability/CVE-2026-65400">CVE - 2026 - 65400 : Authentication Vulnerability in macOS Products by...</a></li>
<li><a href="https://thecybersecguru.com/news/cve-2026-65400-macos-screen-sharing-authentication-bypass/">CVE - 2026 - 65400 : macOS Screen Sharing Flaw... | The CyberSec Guru</a></li>

</ul>
</details>

**标签**: `#macOS`, `#security`, `#vulnerability`, `#CVE`, `#screen sharing`

---

<a id="item-tech-news-6"></a>
### [Cloudflare：五年后 AI 机器人流量或达人类千倍](https://www.techspot.com/news/113410-cloudflare-humans-could-become-rounding-error-bots-generate.html) ⭐️ 7.0/10

Cloudflare 在第二季度财报电话会上预测，若当前趋势持续，五年后非人类流量将达到人类流量的 1000 倍。CFO Thomas Seifert 表示，人类在互联网上将变成一个“舍入误差”，原因并非人类流量下降，而是机器人流量增长过快。这一趋势主要由 AI 智能体驱动，CEO Matthew Prince 曾预测机器人流量将在 2027 年底超过人类，但该节点已在今年提前到来。智能体系统行为接近正常浏览，却能以机器速度大规模重复，一个简单提示就可能触发数千次请求。

telegram · zaihuapd · 8月9日 02:08

**「背景」** Cloudflare 是一家提供 CDN、DNS 和网络安全服务的公司，能观察全球互联网流量的构成。此前其 CEO Matthew Prince 曾预测机器人流量将在 2027 年底超过人类流量，但实际上这一转折点已在今年到来。智能体 AI 驱动的自动化流量是主要推手：它们的行为接近正常浏览，却能以机器速度大规模重复请求，例如一个简单提示就可能触发数千次请求，从而大幅放大非人类流量占比。

**「影响」** 若该预测成立，网站运营者、内容发布商和安全团队将不得不优先应对 AI 代理流量，否则其数据分析、广告计量与安全防护可能被机器请求淹没。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.techspot.com/news/113410-cloudflare-humans-could-become-rounding-error-bots-generate.html">Cloudflare says humans could become a &quot; rounding error &quot; as bots ...</a></li>

</ul>
</details>

**标签**: `#AI`, `#web traffic`, `#bots`, `#Cloudflare`, `#industry forecast`

---

## 财经新闻

<a id="item-finance-news-1"></a>
### [伯克希尔 Q2 运营利润增长 16%，新任 CEO 开始动用巨额现金储备](https://www.cnbc.com/2026/08/08/berkshire-hathaway-earnings-q2-2026.html) ⭐️ 8.0/10

伯克希尔哈撒韦第二季度运营利润同比增长 16%至 129.8 亿美元，CEO 格雷格·阿贝尔开始部署巨额现金，通过约 45 亿美元回购和近 200 亿美元净买入股票，使现金储备从创纪录的 3974 亿美元降至 3655 亿美元。

rss · CNBC Finance · 8月8日 13:28

**「背景」** 阿贝尔今年年初接替巴菲特出任 CEO；此前伯克希尔已连续 14 个季度净卖出股票，股东一直呼吁将部分现金投资于国债之外。

**标签**: `#Berkshire Hathaway`, `#earnings`, `#buybacks`, `#capital allocation`, `#Greg Abel`

---

<a id="item-finance-news-2"></a>
### [月之暗面引入国资股东并调整架构，推进赴港上市](https://www.theblockbeats.info//flash/360480) ⭐️ 8.0/10

据英国《金融时报》报道，月之暗面（Moonshot AI）正在重组股权结构并引入多家国资背景投资者，以争取监管部门批准其赴港上市；公司近期完成两轮融资，估值最高预计达 500 亿美元。此前市场传闻公司计划本月提交香港 IPO 申请、募资约 30 亿美元，但月之暗面回应称消息不实。

telegram · zaihuapd · 8月8日 09:02

**「背景」** 月之暗面（Moonshot AI）成立于 2023 年，总部位于北京，开发 Kimi 系列大语言模型；据第三方资料，截至 2026 年 5 月其累计融资约 20 亿美元、估值约 200 亿美元，此前投资方包括阿里、红杉等。

**「影响」** 据报道，Moonshot AI 正与中金和高盛等投行讨论赴港 IPO，若推进，将使其现有股东——包括阿里巴巴、腾讯及国资背景投资方——通过港股市场获得退出渠道，也可能带动更多中国 AI 公司选择香港上市。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.studioglobal.ai/zh-cn/discover/reports/research-moonshot-ai-company-structure-products-growth-6a11183daebd9ec36509ac08">Moonshot AI（Kimi背后的公司）：结构、产品、投资人与增长全解析 | ...</a></li>
<li><a href="https://finance.biggo.com/news/79be67b2-81f1-4b76-8b57-9dc541707c0f">Moonshot AI Eyes Hong Kong IPO Within Six Months at $31.5 Billion Valuation; Kimi K3 Benchmark Scores Rattle Silicon Valley — BigGo Finance</a></li>
<li><a href="https://www.startuphub.ai/ai-news/artificial-intelligence/2026/moonshot-ai-eyes-hong-kong-ipo-as-china-ai-race-heats-up">Moonshot AI Eyes Hong Kong IPO as China AI Race Hea… | StartupHub.ai</a></li>
<li><a href="https://www.bloomberg.com/news/articles/2026-07-19/china-s-moonshot-plans-ipo-in-six-months-after-ai-breakthrough">Moonshot Plans IPO in Six Months After China AI Breakthrough - Bloomberg</a></li>

</ul>
</details>

**标签**: `#Moonshot AI`, `#Hong Kong IPO`, `#state capital`, `#AI industry`, `#corporate restructuring`

---

<a id="item-finance-news-3"></a>
### [内华达州电力公司起诉数据中心开发商，要求其负担 10 亿美元电网升级](https://www.sina.cn/news/detail/5329879165568444.html) ⭐️ 7.0/10

内华达州最大电力供应商内华达能源公司已起诉一家在建数据中心开发商，要求其承担约 10 亿美元的电网升级费用，否则可能上调电价。内华达能源公司为全州 90%用户供电，并称这两座数据中心建成后耗电将约占其总发电量三分之一；开发商则指责对方未兑现供电承诺却要求其投入升级。

telegram · zaihuapd · 8月9日 01:35

**「事件背景」** 内华达能源公司（NV Energy）是内华达州最大电力供应商，为约 90%的用户供电。该公司已向联邦法院起诉数据中心开发商 Tract Capital Management，指控其试图规避监管程序，并拒绝承担扩建电网的费用，纠纷的核心是谁应支付约 10 亿美元的电网升级成本。

**「影响」** 若内华达能源公司获准将基建成本转嫁，内华达州约 90%的普通家庭和企业用户可能面临更高电费。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.cbsnews.com/news/nevada-data-center-lawsuit-ai-energy-costs/">Nevada energy company sues data center in first-of-its-kind ...</a></li>
<li><a href="https://thenevadaglobe.com/702times/nv-energy-sues-data-center-developer-in-first-of-its-kind-fight-over-ai-power-buildout/">NV Energy Sues Data Center Developer in First-of-Its-Kind ...</a></li>
<li><a href="https://www.kolotv.com/2026/07/29/nv-energy-sues-tract-over-data-center-regulations/">NV Energy sues Tract over data center regulations - KOLO</a></li>

</ul>
</details>

**标签**: `#energy`, `#data centers`, `#lawsuit`, `#electricity rates`, `#infrastructure`

---