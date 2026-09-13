# 色值仪小组件

WidgetKit 扩展提供主屏小号／中号组件和锁屏圆形／矩形入口。用户可在主屏编辑器中添加“KL 烘焙曲线 → 色值仪”，点击组件通过 `klprofile://roast-color` 打开完整测量流程。

组件读取 App Group `group.com.yu.kl-tools.roast-color` 中的最近测量快照。主 App 保存或删除记录后请求 WidgetKit 刷新；组件只读取最近样本，不读取相机图像或完整历史。没有测量时显示开始测量，只有 L\* 时不会虚构 Roast Score。

视觉采用雾面透明 Baby Blue：默认外观使用蓝色渐变染色、柔和高光和细描边；iOS 26 的透明／色调模式由系统控制玻璃与壁纸透出。真机发布时，主 App 和扩展的签名配置都必须包含该 App Group。

Widget URL、快照序列化、无分数时保留 L\* 和路由过滤均有独立验证；扩展通过 `RoastColorWidget` target 嵌入 iOS App。
