---
layout: page
title: 关于
permalink: /about/
---

## 项目简介

### [kl-tools](https://github.com/yuhongda/kl-tools)

macOS 原生 App + CLI 的 Kaffelogic profile 生成器。左侧填写生豆参数
（产地、处理法、海拔、冲煮方式、烘焙风格），右侧实时预览曲线
（蓝 = 烘焙温度、橙 = 风扇转速、红 = 一爆、绿 = 结束、紫 = ROR）。

支持中英文产地识别与别名匹配（如「耶加雪菲」「曼特宁」），自动按产区特征
调整曲线；含水率参数会微调干燥段与预热功率。生成文件可直接保存到
Kaffelogic 的 `roast-profiles` 目录，或复制 `.kpro` 全文。

Natural / Washed 基线来自 Kaffelogic 最新 16 条 Filter / Espresso 海拔矩阵，
风味目标与烘焙节奏以受限的阶段变换叠加；官方曲线逐点 Golden Test 与 99 条
Community Profile 兼容测试共同防止算法回归。

### [kpro-tools](https://github.com/yuhongda/kpro-tools)

Python 实现的 `.kpro` 分析与生成器，包含：

- `.kpro` 文件格式解析（纯文本 `key:value`，贝塞尔曲线编码）；
- 与 Kaffelogic Studio 7.4.3 同源的贝塞尔控制点算法；
- 从 38 个真实 profile 学习得到的海拔、处理法、冲煮方式、烘焙度调整逻辑；
- 生成结果的多重验证：回读校验、76 条曲线控制点逐点对拍（零误差）、
  Studio 实测加载、烘焙指标区间检查。

## 验证方式

1. 回读检查生成文件的键齐全、锚点时间单调、温度范围合法；
2. 用工具重算全部 38 个真实 profile 的 76 条曲线控制点，与文件存储值
   零误差对拍，证明格式解析与平滑算法和 Studio 完全一致；
3. 用 Kaffelogic Studio 7.4.3 直接打开生成的 `.kpro`，正常加载无解析错误；
4. 生成曲线的一爆温度/时间、DTR、各阶段到达时间均落在真实 profile 区间内。

## 联系方式

GitHub：[yuhongda](https://github.com/yuhongda)

邮箱：<silverage.y@gmail.com>
