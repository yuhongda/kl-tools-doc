---
title: kl-tools：macOS 原生 App + CLI 的 Kaffelogic 曲线生成器
date: 2026-08-11
categories: [tools]
---

`kl-tools` 是一个根据生豆参数一键生成 Kaffelogic Studio / Nano 7 可用
`.kpro` 烘焙曲线文件的 macOS 原生工具，提供 GUI App 和 CLI 两种使用方式。

## GUI App

双击打开 `build/kl-tools.app`（本地构建已 ad-hoc 签名，可直接运行）。
左侧填写参数，右侧实时预览曲线：

- 蓝 = 烘焙温度，橙 = 风扇转速，红 = 一爆，绿 = 结束，下方紫色为 ROR；
- 底部显示一爆温度/时间、DTR、预热功率、roast_levels；
- 「另存为…」任选保存位置；
- 「保存到 Kaffelogic 目录…」自动保存到
  `~/Library/Application Support/com.kaffelogic/kaffelogic/*/roast-profiles/`
  或 `~/kaffelogic/roast-profiles/`；
- 「复制文本」把 `.kpro` 全文复制到剪贴板。

## CLI

```bash
# 从源码构建
swift build -c release

# 生成一条曲线
.build/release/kl-tools generate --origin "Ethiopia Yirgacheffe" \
    --process washed --altitude 2200 --brew filter --style mediumLight -o output/
```

常用参数：

| 参数 | 可选值 / 说明 |
|---|---|
| `--process` | `natural` 日晒 / `washed` 水洗 |
| `--altitude` | 海拔（米），必填 |
| `--brew` | `filter` 滤泡 / `espresso` 意式 |
| `--style` | `light` / `mediumLight` / `medium` / `mediumDark` / `dark` / `veryDark` |
| `--level` | 自定义档位 0.1–5.9（可选） |
| `--origin` / `--designer` / `--notes` | 产地、设计者、备注 |
| `-o` | 输出目录（默认 `output/`） |
| `--dry-run` | 只打印不写文件 |

## 产区识别

自动识别 `--origin` 或界面「产地」下拉（支持中英文、别名、单词匹配，如
`Ethiopia Yirgacheffe`、`耶加雪菲`、`Kenya AA`、`曼特宁`），并按产区特征
调整曲线；内置列表没有的产地可选「自定义…」输入。识别不到的产地按标准
曲线生成，界面会显示实际应用的调整项。

## 含水率

`--moisture`（或界面「含水率」输入框）填生豆包装标注的含水率（%），常见
9–13%，默认按 11% 基准：含水每高 1%，干燥段约 +2s、预热 +8W、中段 +1.5s、
发展期 +1s；低于基准则反向缩短/降低。超出 8–14% 按边界值处理。
