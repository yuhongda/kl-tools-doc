---
layout: home
title: 首页
---

# KL Tools —— Kaffelogic 烘焙工具集

一套为 [Kaffelogic Studio / Nano 7](https://kaffelogic.com/) 打造的曲线工具，
根据生豆参数（产地、处理法、海拔、冲煮方式、烘焙风格）一键生成可用的
`.kpro` 烘焙曲线文件。

- **kl-tools**：macOS 原生 App + CLI，界面实时预览曲线，支持保存到
  Kaffelogic 目录或导出 `.kpro`；
- **kpro-tools**：Python 实现的 `.kpro` 解析器与生成器，附带格式说明、
  控制点算法和从 38 个真实 profile 学到的烘焙逻辑。

两套实现共用同一套贝塞尔曲线算法，并经过与 Kaffelogic Studio 7.4.3
反编译源码的逐点对拍验证，生成的曲线可相互交叉验证（逐字节一致）。

了解更多，请查看[关于]({{ '/about/' | relative_url }})页与下面的最新文章。
