---
title: 交叉验证：与 Studio 7.4.3 逐点对拍，零误差
date: 2026-08-11
categories: [verification]
---

生成器不是「看起来差不多」，而是经过了多重验证：

## 1. 回读校验

`validate_generated()` 检查键齐全、锚点时间单调、温度范围合法、
roast_levels 7 档递增。

## 2. 控制点算法逐点对拍

用本工具重算全部 38 个真实 profile 的 76 条曲线（roast + fan）的贝塞尔
控制点，与文件存储值对比——**零误差（max delta = 0）**。这证明格式解析
与平滑算法和 Kaffelogic Studio 7.4.3 完全一致。

## 3. Studio 实测

用 Kaffelogic Studio（本机 7.4.3）直接打开生成的 `output/*.kpro`，
进程正常加载、无任何解析错误（调试日志无异常）。

## 4. 烘焙指标校验

生成曲线的一爆温度/时间、DTR（滤泡 ~35–42%、espresso ~24%）、
100°C/160°C 到达时间、末端温度均落在真实 profile 区间内；全程
ROR > 0.2°C/min（防停顿下限），曲线可被烘焙机正常跟随。

## Swift / Python 交叉验证

kl-tools（Swift）与 kpro-tools（Python）两套实现跑 96 组
（2 冲煮 × 2 处理法 × 6 风格 × 4 海拔）生成的 `.kpro` 逐字节一致
（除时间戳），唯一差异是 1 个浮点 10 位小数尾数（1 ULP，不影响烘焙机
与 Studio）。
