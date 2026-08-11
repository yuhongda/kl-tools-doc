---
title: 从 38 个真实 profile 学到的烘焙逻辑
date: 2026-08-11
categories: [roasting]
---

`kpro-tools` 从 38 个真实 profile 中总结出以下规律，并复现在生成器中。

## 海拔（同处理法、同冲煮方式下）

| 项目 | 0-1200m | 1000-1700m | 1500-2200m | 2000-2700m |
|---|---|---|---|---|
| 一爆温度 expect_fc（Filter-Natural） | 212 | 209 | 206 | 205 |
| 一爆温度（Filter-Washed） | 213 | 208 | 206 | 204 |
| 一爆温度（Espresso-Natural） | 208 | 206 | 204 | 202 |
| 一爆温度（Espresso-Washed） | 210 | 205 | 203 | 201 |
| roast_levels（Filter-Natural 第0档） | 215 | 211.8 | 209 | 207.4 |
| 预热功率（Filter-Natural） | 850 | 850 | 850 | 800 |
| 风扇降速起始点 | 365s | 370s | 375s | 380s |

规律：**海拔越高 → 探针一爆温度越低（每档约 -2~-3°C）、roast_levels 整体
下移、曲线前期更慢、风扇更早保持高速、预热功率略低**。高海拔豆密度大、
内部结构紧，需要更稳的前段能量管理和更短的「表观」一爆温度。

## 处理法

- 同海拔下 Washed 比 Natural 的 `expect_fc` 高约 1°C（Filter 213 vs 212；
  Espresso 210 vs 208）；
- Washed 的预热功率明显更低（Espresso 670 vs 770），roast_levels 整体高
  约 1°C；
- Washed 曲线中段（~160°C）更晚到达：Espresso Washed 189s vs Natural 172s
  （0-1200m）。

对应专业烘焙经验：水洗豆更「干净」、吸热/放热行为更温和，需要略低的起始
能量；日晒豆糖分多、美拉德反应更活跃，起始能量更高、一爆探针温度更低。

## 冲煮方式

- Espresso 曲线点更密（9 个锚点），Development 控制在 ~24% DTR；
- Filter 曲线 4–5 个锚点，DTR 33–42%，末端温度更高（225–230 vs 212–222）；
- Espresso 的 roast_levels = 一爆温度 + [0,2,4,6,8,10,12]；
  Filter 的 roast_levels = 一爆温度 + [3,5,7,9,11,13,15]。

## 烘焙度（深烘）

深烘时 `recommended_level` 5.x、预热功率提高到 1050（低海拔）、曲线末端
225–239°C、总时长可达 12–13.5 分钟（Very dark 757s）、深烘滤泡常带
zone1 +3°C/min 的干燥期 boost（60–180s）。roast_levels 阈值本身不变——
「深浅」由 level 档位实现。
