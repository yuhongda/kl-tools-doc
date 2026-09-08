---
title: kpro 文件名生成规则与 Kaffelogic Studio 兼容性
date: 2026-09-08
categories: [guide, file-format]
---

## 为什么需要限制文件名

Kaffelogic Studio 写入 `.klog` 时，会把本次烘焙使用的 Profile 文件名保存到
`profile_file_name` 字段。对现有 Studio 生成的日志进行对比后可以确认，这个字段的
兼容长度是 **47 个 UTF-8 字节**，不是 47 个字符。

因此中文文件名会比英文文件名更早达到上限。例如一个较长的中文 Profile 名称可能在
klog 中只剩下前半段，导致复盘页面无法仅凭截断后的名称准确匹配原始 Profile。

Kaffelogic 的帮助文档也说明，Studio 可以从日志中关联并提取烘焙时使用的 Profile，
所以文件名需要在 Studio 的字段限制内保持稳定且尽量可辨识。

## 生成规则

短文件名（不超过 47 个 UTF-8 字节）保持原样。超长文件名会按以下顺序处理：

1. 提取并压缩文件名中的主要信息：产地、处理法、海拔、冲煮方式、烘焙度、风味目标和节奏；
2. 使用稳定的短 hash 标识完整原始名称；
3. 将缩写名称与 hash 组合，并确保最终名称不超过 47 个 UTF-8 字节；
4. 完整的 Profile 信息仍保存在 `.kpro` 内容和元数据中，文件名只承担识别和关联作用。

当前缩写约定如下：

| 信息 | 缩写示例 |
| --- | --- |
| 中文产地 | `埃塞俄比亚...` → `埃塞` |
| 处理法 | Natural → `N`，Washed → `W`，Honey → `H` |
| 冲煮方式 | Espresso → `E`，Filter → `F` |
| 烘焙度 | Medium → `M`，Light → `L`，Dark → `D` |
| 风味目标 | Fruity + Floral → `Fru+Flo` |
| 节奏 | Fast Pace → `F`，Balanced Pace → `B`，Slow Pace → `S`，Auto Pace → `A` |

例如：

```text
原始名称：
埃塞俄比亚（耶加雪菲-西达摩-古吉） - Natural - 2200m - Espresso - Medium - Fruity+Floral - Balanced Pace.kpro

兼容名称：
埃塞-N-2200-E-M-Fru+Flo-B~a1b2c3d4.kpro
```

其中 `a1b2c3d4` 是根据完整原始名称生成的稳定 hash。即使两个 Profile 的可读缩写
前缀相同，只要原始名称不同，hash 也会不同，从而避免截断后重名。

## 对复盘匹配的影响

新生成的 kpro 文件名不会超过 Studio 的 47 字节字段限制，因此之后生成的 klog 可以
完整保存该文件名。复盘页面导入 klog 时，可以使用这个字段自动匹配资料库中的 Profile，
同时保留完整的风味目标与 Profile 参数。

对于历史 klog，如果 `profile_file_name` 已经被 Kaffelogic Studio 截断，应用无法从
截断文本中恢复原始长文件名。这类日志仍可正常复盘；如果资料库中没有同名 Profile，
应用会提示用户，但不会清空当前复盘日志。

## 设计原则

- 兼容 Studio：最终文件名不超过 47 个 UTF-8 字节；
- 信息可读：优先保留用户最关心的 Profile 组成元素；
- 稳定防重名：hash 来自完整原始名称，而不是截断后的前缀；
- 元数据完整：文件名是索引，不替代 `.kpro` 内部的完整 Profile 描述。
