---
title: .kpro 文件格式解析：不是 JSON，也不是二进制
date: 2026-08-11
categories: [format]
---

`.kpro` 是 **UTF-8 纯文本**：每行一个 `key:value`，用 `:` 分隔，LF 换行
（`profile_description` 里用字面量 `\v` 表示换行）。

两个曲线字段是自定义编码：

```text
roast_profile: <6 个数/点>  =  时间, 温度, 左控制点x, 左控制点y, 右控制点x, 右控制点y
fan_profile:   <6 个数/点>  =  同上，y 为转速 RPM（内部按 ×0.1 存取）
```

每个曲线点是一条三次贝塞尔曲线段的锚点：主点 + 左右两个手柄。加载时
Studio 会把首点左手柄和末点右手柄强制置 0，并把为 0 的手柄按
`CONTROL_POINT_RATIO=0.3` 自动平滑重算——所以即使手柄全为 0 也能平滑显示。

> 以上结论来自直接反编译本机安装的 Kaffelogic Studio 7.4.3
> （Python 2.7 + wxPython 编写）后，从 `bezier.py` 的
> `profilePointsToString` 和 `core_studio.py` 的 `stringToDataObjects`
> 确认，而不是猜测。

## 固定参数（所有真实 profile 一致）

PID Kp=0.717 / Ki=0 / Kd=3.55；比热调整 80–180°C、Kp=2.1 / Kd=4.0；
`roast_min_desired_rate_of_rise=0.2`；`roast_end_by_time_ratio=0.75`；
预热 240°C / 模式5 / 20–60s；冷却 16500→15500 RPM、100°C 降速；等等。

## 使用

```bash
python3 generate.py --origin "Ethiopia Yirgacheffe" --process washed \
    --altitude 2200 --brew filter --style medium-light -o output/
```

生成的文件可直接放入 Kaffelogic Studio 打开（自动平滑曲线），或拷贝到
U 盘 `/kaffelogic/roast-profiles/` 给机器加载。
