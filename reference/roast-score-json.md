# Roast Score JSON 接入说明

处理链路是：`相机 → 光源校准后的 Lab → 匹配曲线 → 分段线性插值 → Roast Score → Roast Level`。

每条曲线记录 `id`、`version`、`scale`、`deviceModel`、`sampleType`、`reference`、`algorithmVersion`、`points` 和 `bands`。点位的 L\* 必须严格递增且不重复，分数也必须严格递增。`deviceModel: "*"` 表示通用 iPhone 曲线；咖啡粉和灰卡需要独立曲线。

匹配先检查样本类型、参考物和算法版本，再优先选择当前手机专用曲线，最后才使用通用 iPhone 曲线。没有匹配曲线、输入超出范围或 JSON 无效时，保留 L\* 并显示对应状态，不外推、不重新计算旧历史结果。

当前启用的曲线是 `iphone-shared-whole-bean-v1`，量表名为 `KL Roast Score`，适用于整豆、白纸和算法版本 2。它是相机经验估算，不是专业仪器标定。
