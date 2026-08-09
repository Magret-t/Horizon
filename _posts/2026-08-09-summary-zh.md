---
layout: default
title: "Horizon Summary: 2026-08-09 (ZH)"
date: 2026-08-09
lang: zh
---

> 从 49 条内容中筛选出 11 条重要资讯。

---

**科技新闻**
1. [Shopify 用 MySQL 替代 Redis 扩展库存预留](#item-tech-news-1) ⭐️ 8.0/10
2. [Triton：为 QEMU 打造的开源 DirectX 11 驱动](#item-tech-news-2) ⭐️ 8.0/10
3. [基因组语言模型首次生成可存活的噬菌体基因组](#item-tech-news-3) ⭐️ 8.0/10
4. [macOS 屏幕共享高危漏洞：无需密码即可登录任意账户](#item-tech-news-4) ⭐️ 8.0/10
5. [Claude Code 将自动模式设为 Pro、Max 和 Team 版默认](#item-tech-news-5) ⭐️ 7.0/10
6. [腾讯将 WorkBuddy 列为战略级 AI 产品](#item-tech-news-6) ⭐️ 7.0/10
7. [全球最大单体 AI 算力设施在乌兰察布投产](#item-tech-news-7) ⭐️ 7.0/10
8. [SpaceX 拟建月球自动工厂生产 AI 卫星](#item-tech-news-8) ⭐️ 7.0/10

**视频剪辑与音乐制作**
1. [iPhone 拍摄恐怖片 Fire Alive 使用 Blackmagic Camera 应用获电影节入围](#item-video-1) ⭐️ 6.0/10

**财经新闻**
1. [伯克希尔 Q2 运营利润增长 16%，新 CEO 阿贝尔开始动用巨额现金](#item-finance-news-1) ⭐️ 8.0/10
2. [月之暗面引国资股东、调整架构推进赴港上市](#item-finance-news-2) ⭐️ 7.0/10

---

## 科技新闻

<a id="item-tech-news-1"></a>
### [Shopify 用 MySQL 替代 Redis 扩展库存预留](https://shopify.engineering/scaling-inventory-reservations) ⭐️ 8.0/10

Shopify 工程团队详细介绍了用 MySQL 替代 Redis 实现库存预留的架构：库存不再用单行数量字段，而是为每个可售单元维持一行；为避免超大规模商品产生过多行，系统为每商品/地点组合维护一个有界可用行池，上限为 1,000 行，预留操作通过消耗池中行完成，并由补充流程回收或重建行。该设计将预留逻辑放入 MySQL 事务，利用行级锁和一致锁顺序避免死锁，从而支持比 Redis 方案更高的扩展性。团队还总结称真正的瓶颈并非数据库设计，而是观测与测量之外的问题。文章引发了关于更简单超时回收方案和文章内部一致性的讨论。

hackernews · adletbalzhanov · 8月8日 22:32 · [社区讨论](https://news.ycombinator.com/item?id=49226536)

**「背景」** Shopify 原本使用 Redis 处理库存预留的并发控制，但在多地点库存、ACID 一致性以及大规模扩展方面遇到瓶颈。他们在工程博客中介绍，新方案改用 MySQL，将每个可售单元对应一行，并为每个商品/地点组合维护一个上限 1,000 行的有界可用池，通过数据库事务同时保证预留操作和库存台账的一致性。据称该方案撑住了 2025 年黑色星期五峰值约每分钟 510 万美元的销售额，吞吐量不降反升。

**「影响」** 该方案为在高并发库存预留场景遇到 Redis 瓶颈的工程师提供了可落地的替代路径：将可售单元映射为行并使用每商品/地点 1,000 行的有界池，以事务保证一致性。

**「社区讨论」** 评论中有人提出更简单的超时回收方案并质疑复杂设计的必要性；也有人称赞该实践有价值，同时指出文章在锁顺序等部分存在表名不一致等写作问题。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://shopify.engineering/scaling-inventory-reservations">We replaced Redis with MySQL for inventory ... - Shopify</a></li>
<li><a href="https://byteiota.com/shopify-killed-redis-for-mysql-and-scaled-bigger/">Shopify Killed Redis for MySQL — and Scaled Bigger | byteiota</a></li>

</ul>
</details>

**标签**: `#MySQL`, `#Redis`, `#scalability`, `#inventory management`, `#Shopify`

---

<a id="item-tech-news-2"></a>
### [Triton：为 QEMU 打造的开源 DirectX 11 驱动](https://blog.getutm.app/2026/introducing-triton-directx-11-driver-for-qemu/) ⭐️ 8.0/10

Triton 是一个面向 QEMU 的新开源 DirectX 11 驱动，目标是为 Windows 虚拟机提供 GPU 加速图形能力。该驱动由 getutm.app 博客发布介绍，并已在 Hacker News 引发讨论。对仅有单块独立 GPU 的 Linux 主机而言，此前在 Windows 虚拟机中获得图形加速较为困难，Triton 被看作弥补这一长期缺口的方案。目前文章未提供具体版本、性能数据或兼容性细节，因此实际效果和 VirtualBox 等其它虚拟机平台的适配情况仍需进一步验证。

hackernews · electricant · 8月8日 13:33 · [社区讨论](https://news.ycombinator.com/item?id=49221711)

**「背景」** QEMU 是一个开源的机器模拟器与虚拟化工具，常用于在 Linux 主机上运行 Windows 虚拟机；过去这类虚拟机通常缺乏可用的 GPU 加速图形支持，尤其是在只有一块独立显卡的 Linux 机器上。Triton 是开发者 Osy 宣布的开源 DirectX 11 驱动，借助 Mesa 与 virglrenderer 等组件为 QEMU 中的 Windows 客户机提供 Direct3D 11 加速，且据称其中相当一部分代码由 Claude 等 AI 模型辅助生成。目前该驱动仍处于测试阶段，并非完善的产品。

**「影响」** 对使用 QEMU 运行 Windows 虚拟机的 Linux 用户，尤其是单 GPU 用户，这一驱动有望显著降低图形加速的配置门槛；但官方兼容范围与性能表现尚无明确数据。

**「社区讨论」** 评论者普遍期待已久，认为这是 Windows 虚拟机开源 3D 方案的进步；同时有人询问是否支持 VirtualBox、是否覆盖 DirectX 10 及更早版本，也有人指出“Triton”已是至少第三个同名 GPU 项目。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.phoronix.com/news/Triton-DirectX-11-QEMU-Driver">AI Helped Create A DirectX 11 Driver For QEMU VMs - Phoronix</a></li>
<li><a href="https://byteiota.com/utm-triton-ai-built-directx-11-driver-for-qemu-vms/">UTM Triton : AI-Built DirectX 11 Driver for QEMU VMs | byteiota</a></li>
<li><a href="https://worksetuplab.com/monitor-display-know-how/triton-directx-11-driver-for-qemu/">Triton : DirectX 11 Driver For QEMU - WorkSetupLab</a></li>

</ul>
</details>

**标签**: `#virtualization`, `#QEMU`, `#DirectX`, `#graphics`, `#open-source`

---

<a id="item-tech-news-3"></a>
### [基因组语言模型首次生成可存活的噬菌体基因组](https://www.reddit.com/r/MachineLearning/comments/1vjj4pr/r_generative_design_of_novel_bacteriophages_with/) ⭐️ 8.0/10

研究人员利用前沿基因组语言模型 Evo 1 和 Evo 2，以裂解性噬菌体 ΦX174 为设计模板，首次实现了可存活的噬菌体基因组生成式设计。模型生成的全基因组序列具有真实的遗传结构，并具备期望的宿主趋向性。实验验证显示，这些 AI 生成的基因组产生了 16 株有功能且具有显著进化新颖性的噬菌体。该结果标志着基因组语言模型在全基因组尺度上生成功能生物序列的能力首次得到实验证实。

reddit · r/MachineLearning · /u/moschles · 8月9日 07:11

**「背景」** 基因组语言模型是经过大规模基因组数据训练的人工智能模型，Evo 1 和 Evo 2 是其中的前沿模型。Evo 2 在数以百万计的噬菌体基因组上训练，作者还使用约 15,000 个 Microviridae 家族（包含 ΦX174 的家族）基因组对其进行微调，然后利用该模型生成了 302 个候选噬菌体基因组。这些设计以裂解性噬菌体 ΦX174 为模板，通过实验室合成与验证，最终获得了 16 个具有功能活性的新型噬菌体。

**「影响」** 对从事 AI 生物设计的研究者和合成生物学家而言，该成果表明全基因组尺度的生成式设计已从理论探索进入可实验验证的新阶段。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.implicator.ai/stanford-and-arc-institute-build-16-ai-designed-viruses-that-kill-e-coli/">Stanford AI Designs 16 Working Viruses That Kill E. Coli</a></li>
<li><a href="https://bradleywoolf.com/links-1/fine-tuning-evo2-to-generate-novel-phage-designs">Fine-tuning Evo 2 to generate novel phage designs</a></li>

</ul>
</details>

**标签**: `#AI`, `#genome language models`, `#synthetic biology`, `#machine learning research`, `#computational biology`

---

<a id="item-tech-news-4"></a>
### [macOS 屏幕共享高危漏洞：无需密码即可登录任意账户](https://x.com/calif_io/status/2086022794840793454) ⭐️ 8.0/10

苹果 macOS 屏幕共享功能被披露存在一个高危漏洞（CVE-2026-65400），攻击者可在不知道密码的情况下以任意账户身份远程登录受影响的 Mac，前提是屏幕共享已开启。安全研究人员已公开该漏洞的 PoC，并逆向工程了苹果的修复补丁以厘清根因与利用路径，完整技术分析将于明日发布。苹果已在 macOS 26.6.1 中修复此漏洞，建议用户尽快升级。

telegram · zaihuapd · 8月8日 14:20

**「背景」** macOS 的“屏幕共享”（Screen Sharing）是一项允许用户通过网络远程控制其他 Mac 的集成功能。CVE-2026-65400 是针对该功能的认证绕过漏洞，其根因是认证过程中的状态管理不当，使攻击者可在未提供密码的情况下以任意账户身份登录。该漏洞与近期另一个屏幕共享漏洞 CVE-2026-43760 不同但容易被混淆；苹果已分别于 2026 年 7 月 27 日和 8 月 6 日发布补丁进行修复。

**「影响」** 已开启屏幕共享的 macOS 用户应立即升级到 macOS 26.6.1，否则可能面临无需密码的远程登录风险，攻击者可借此完全控制受影响设备。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://securityvulnerability.io/vulnerability/CVE-2026-65400">CVE - 2026 - 65400 : Authentication Vulnerability in macOS Products by...</a></li>
<li><a href="https://www.huntress.com/blog/macos-screen-sharing-rce-patched">From Screen Share to Root Access: Breaking Down CVE - 2026 -43760...</a></li>
<li><a href="https://thecybersecguru.com/news/cve-2026-65400-macos-screen-sharing-authentication-bypass/">CVE - 2026 - 65400 : macOS Screen Sharing Flaw... | The CyberSec Guru</a></li>

</ul>
</details>

**标签**: `#security`, `#macOS`, `#CVE`, `#vulnerability`, `#patch`

---

<a id="item-tech-news-5"></a>
### [Claude Code 将自动模式设为 Pro、Max 和 Team 版默认](https://simonwillison.net/2026/Aug/8/auto-mode/#atom-everything) ⭐️ 7.0/10

Anthropic 宣布自 8 月 14 日起，在 Claude Code 的 Pro、Max 和 Team 套餐中，新会话默认启用 auto mode。该模式让代理在无需逐步人工批准的情况下执行操作；Anthropic 称其内部几乎所有员工都在使用该模式，并发布评估称其安全风险低于人工审核。在一项 1,053 名付费测试者参与的研究中，面对被替换为危险命令的权限提示，仅 13.6% 的人类拒绝，而 auto mode 可拦截其中 89% 的恶意操作。Anthropic 还引用第三方 Trajectory Labs 的测试，称截至 2026 年 7 月 17 日，Claude Fable 5、Opus 5 和 Sonnet 5 在 auto mode 下均未被 720 次间接提示注入攻击成功突破。Simon Willison 对此持谨慎态度，认为仍需要独立验证，并质疑恶意第三方包通过命令诱导数据外泄等场景能否被防护。

rss · Simon Willison · 8月8日 22:36

**「背景」** auto mode 是 Claude Code 的一种运行方式，允许编码代理在执行操作时不必每次都等待用户点击确认，以减轻确认疲劳。Claude Code 默认此前通常需要人工批准敏感操作；Anthropic 希望用模型内置的安全判断替代频繁的人工审核。但这类代理会读取第三方代码、文档等外部内容，因此面临提示注入攻击的担忧，即恶意指令隐藏在正常内容中诱使代理执行危险操作。

**「影响」** 从 8 月 14 日起，Pro、Max 和 Team 套餐的 Claude Code 新会话将默认采用自动模式，开发者会明显减少手动批准提示；Anthropic 的评估声称自动模式在部分间接提示注入测试中优于人类审核，但此结论尚未得到充分独立验证。

**标签**: `#Claude Code`, `#Anthropic`, `#AI coding tools`, `#developer tools`, `#AI safety`

---

<a id="item-tech-news-6"></a>
### [腾讯将 WorkBuddy 列为战略级 AI 产品](https://mp.weixin.qq.com/s/TRUjakoaprGFSYYQB301xw) ⭐️ 7.0/10

腾讯已将 WorkBuddy 列为内部战略优先级最高的 AI 产品之一，内部有说法称它是继 QQ、微信之后的第三个战略级产品。易观报告显示，2026 年第二季度 WorkBuddy 以 2097 万次 PC 端月访问量位居国内办公智能体平台第一，月活达 2000 万级别、日活百万级别。今年 7 月，腾讯将 QClaw 相关业务调整至 WorkBuddy 所在部门，多线探索收口。该产品已接入腾讯文档、企业微信、腾讯会议等生态，并支持混元、DeepSeek、GLM 等多种模型。目前仍处投入阶段、未设商业化 KPI，年内重点放在扩大企业客户覆盖上。

telegram · zaihuapd · 8月8日 13:50

**「背景」** WorkBuddy 是腾讯面向办公场景推出的 AI 智能体平台，可基于企业内外部工具与知识库完成文档、会议、协作等任务。腾讯此次内部提级，反映其在企业级 AI 赛道上的战略收拢，也意味着办公智能体被视为继社交产品之后的下一个战略方向。

**「影响」** 对腾讯企业服务生态和国内办公智能体市场而言，WorkBuddy 获得战略级资源配置后有望进一步巩固其国内第一的位置，并因年内不设商业化 KPI 而优先扩大企业客户覆盖。

**标签**: `#Tencent`, `#AI agents`, `#office automation`, `#China tech`, `#product strategy`

---

<a id="item-tech-news-7"></a>
### [全球最大单体 AI 算力设施在乌兰察布投产](https://www.globaltimes.cn/page/202608/1367666.shtml) ⭐️ 7.0/10

远景科技集团 8 月 6 日宣布，位于内蒙古乌兰察布的“远景乌兰察布星河基地”正式投产，并称其为全球最大单体 AI 算力设施。该基地建筑面积 12 万平方米，规划总容量 2GW，支持百万 GPU 并行计算，绿电占比超 80%。乌兰察布是国家“东数西算”八大节点之一，距北京约 240 公里，数据传输时延 4.2 毫秒，数据中心电价较京津冀低约 50%。该项目是远景“戈壁使命”计划的首个旗舰项目，旨在为国产算力集群提供可复制方案；不过相关规模与“全球之最”表述目前主要来自企业宣传，尚缺乏独立验证。

telegram · zaihuapd · 8月9日 05:06

**「背景」** 远景科技集团是中国的绿色科技企业，其乌兰察布星河基地于 2026 年 8 月 6 日投产，建筑面积 12 万平方米，规划容量 2 吉瓦，可支持百万 GPU 并行计算，并称拥有全球单体 AI 数据中心最高的 Token 产出能力。乌兰察布是“东数西算”国家算力枢纽节点之一，距北京约 240 公里，享有较低的电价和绿色电力优势，此前华为、阿里巴巴等企业也已在该地区布局算力设施。该基地是远景“戈壁使命”计划的首个旗舰项目，旨在为国产算力集群提供可复制的建设方案。

**「影响」** 该基地投产后，内蒙古乌兰察布作为“东数西算”节点将拥有大规模、绿电占比超 80%的算力供给，国内 AI 企业和开发者可在低电价、约 4.2 毫秒到北京的低时延条件下获取算力。据报道，DeepSeek 也正计划在该地建设约 1GW 的数据中心，显示该枢纽正吸引头部 AI 企业跟进布局，但相关规划尚未完全证实。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.globaltimes.cn/page/202608/1367666.shtml?trk=article-ssr-frontend-pulse_little-text-block">World &#x27; s largest single AI computing facility enters... - Global Times</a></li>
<li><a href="http://innermongolia.chinadaily.com.cn/2026-08/07/c_1203172.htm">World &#x27; s largest AI computing facility completed in Inner Mongolia</a></li>
<li><a href="https://www.aa.com.tr/en/asia-pacific/china-opens-worlds-largest-ai-computing-industrial-park-in-inner-mongolia/4021268">Anadolu Ajansı: China opens world &#x27; s largest AI computing industrial...</a></li>
<li><a href="https://www.globaltimes.cn/page/202608/1367769.shtml">Envision, DeepSeek’s Ulanqab pivot taps China ’ s energy strengths...</a></li>
<li><a href="https://windowsforum.com/windows-news.4/deepseek-ulanqab-data-center-1-gw-plan-is-still-unverified.441320/">DeepSeek Ulanqab Data Center : 1 GW Plan Is Still... | Windows Forum</a></li>

</ul>
</details>

**标签**: `#AI infrastructure`, `#data center`, `#AI computing`, `#green energy`, `#China tech`

---

<a id="item-tech-news-8"></a>
### [SpaceX 拟建月球自动工厂生产 AI 卫星](https://finance.yahoo.com/technology/articles/pure-insanity-elon-musk-details-173635969.html) ⭐️ 7.0/10

马斯克在 SpaceX 首次财报电话会议上公布了月球自动工厂计划：用 Starship 火箭运送设备，机器人从月球土壤中提取铝、钛、硅等矿物，批量生产 AI 计算卫星，并由电磁质量驱动器从月面直接发射入轨。月球环境极为严苛，存在磨蚀性月尘、悬殊昼夜温差和每 14 天交替的光照周期。前 SpaceX 副总裁 Jim Cantrell 称该计划“纯属疯狂”但认为马斯克能做到，业界认可技术可行性但认为其时间表偏乐观。SpaceX 当季营收 78 亿美元，太空部门因 Starship 投入亏损 2.05 亿美元。

telegram · zaihuapd · 8月9日 05:37

**「背景」** Starship 是 SpaceX 正在开发的超重型运载火箭，设计用于月球和火星任务，也被计划用于发射二代 Starlink 卫星。SpaceX 目前用猎鹰火箭每年约入轨 2500 吨，约占全球总入轨量的 80%–90%；马斯克称 Starship 的目标是每年入轨百万吨乃至千万吨级。月球虽有丰富矿物，但环境严苛；所谓“质量驱动器”是一种电磁弹射器，可利用月面真空把成品直接抛入轨道，从而省去从月球表面用火箭发射的环节。

**「影响」** 该计划若实现，将使卫星生产从地球转移到月球，从而绕开地球发射的运力和成本限制，改变现有航天供应链；但目前仍属概念阶段，缺乏具体时间表和验证细节。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://en.wikipedia.org/wiki/SpaceX_Starship">SpaceX Starship - Wikipedia</a></li>
<li><a href="https://budgyapp.com/spacex-moon-factories-mass-driver-ai-satellites-musk/">SpaceX Moon Factories Target Petawatt AI Compute Scale | Budgy App</a></li>
<li><a href="https://www.scientificamerican.com/article/elon-musk-reveals-spacex-plans-to-build-satellite-factories-on-the-moon/">Elon Musk reveals SpaceX plans to build satellite factories on the...</a></li>

</ul>
</details>

**标签**: `#SpaceX`, `#lunar manufacturing`, `#AI satellites`, `#robotics`, `#space technology`

---

## 视频剪辑与音乐制作

<a id="item-video-1"></a>
### [iPhone 拍摄恐怖片 Fire Alive 使用 Blackmagic Camera 应用获电影节入围](https://www.provideocoalition.com/fire-alive-horror-film-shot-with-blackmagic-camera-app/) ⭐️ 6.0/10

一部名为《Fire Alive》的恐怖长片完全用 iPhone 拍摄，使用 Blackmagic Camera 应用和实际灯光，在五天内完成，并在 2026 年 SF3 电影节获得入围奖。导演 James Demitri 曾从事二十年的名人及时尚摄影，之后转行叙事电影，他特意为自己设定了这一创作挑战。这个案例证明了低成本专业工作流的可行性，但报道本身并未提供具体的技术步骤或设置细节。

rss · ProVideo Coalition · 8月8日 12:35

**「背景」** 《Fire Alive》是一部完全用 iPhone 和 Blackmagic Camera 应用拍摄的恐怖长片，导演 James Demitri 来自悉尼，曾在时尚与名人摄影领域工作二十年。影片仅用五天拍摄完成，全程使用自然/实际光源，并在 2026 年 SF3 电影节获得入围奖。这则消息说明低成本手机拍摄配合专业应用和布光可以产出获得认可的作品。

**「影响」** 为独立创作者提供了一种用 iPhone 搭配 Blackmagic Camera 应用和实际灯光实现低成本、高完成度恐怖片的可参考工作流，能够快速启动并完成项目。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://www.provideocoalition.com/fire-alive-horror-film-shot-with-blackmagic-camera-app/">Fire Alive horror film shot with Blackmagic Camera app by Jose...</a></li>

</ul>
</details>

**标签**: `#Blackmagic Camera`, `#iPhone filmmaking`, `#indie film`, `#practical lighting`, `#horror`

---

## 财经新闻

<a id="item-finance-news-1"></a>
### [伯克希尔 Q2 运营利润增长 16%，新 CEO 阿贝尔开始动用巨额现金](https://www.cnbc.com/2026/08/08/berkshire-hathaway-earnings-q2-2026.html) ⭐️ 8.0/10

伯克希尔·哈撒韦第二季度运营利润同比增长 16%至 129.8 亿美元，上年同期为 111.6 亿美元；新 CEO 格雷格·阿贝尔开始部署现金，当季回购约 45 亿美元股票、净买入近 200 亿美元股票，现金储备从创纪录的 3974 亿美元降至 3655 亿美元。

rss · CNBC Finance · 8月8日 13:28

**「背景」** 巴菲特今年年初将 CEO 职位交给阿贝尔，自己继续担任董事长；此前伯克希尔已连续 14 个季度净卖出股票，现金储备也达到历史高点。

**标签**: `#Berkshire Hathaway`, `#Earnings`, `#Buybacks`, `#Capital Allocation`, `#Greg Abel`

---

<a id="item-finance-news-2"></a>
### [月之暗面引国资股东、调整架构推进赴港上市](https://www.theblockbeats.info//flash/360480) ⭐️ 7.0/10

据英国《金融时报》报道，月之暗面（Moonshot AI）正在重组股权结构并引入多家国资背景投资者，以争取监管部门批准其赴港上市。公司近期完成两轮融资，估值最高预计达 500 亿美元；但公司回应称“本月提交香港 IPO 申请、募资约 30 亿美元”的市场传闻不实。

telegram · zaihuapd · 8月8日 09:02

**「背景」** 月之暗面（Moonshot AI）是总部位于北京的人工智能公司，以开发大语言模型和 Kimi 产品闻名，被视为中国“AI 四小龙”之一。此次重组是在其筹备赴港上市背景下，将国内主体改制为股份有限公司并引入国资股东，为满足监管要求做铺垫。

<details><summary>参考链接</summary>
<ul>
<li><a href="https://en.wikipedia.org/wiki/Moonshot_AI">Moonshot AI - Wikipedia</a></li>
<li><a href="https://www.mexc.co/learn/article/what-is-kimiusdt-pre-ipo-futures-kimi-and-moonshot-ai-trading-explained/1">What Is KIMIUSDT Pre- IPO Futures? Kimi and Moonshot AI Trading...</a></li>

</ul>
</details>

**标签**: `#Moonshot AI`, `#IPO`, `#Hong Kong listing`, `#AI industry`, `#state-owned capital`

---