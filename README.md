# ClassPulse 课堂脉搏

课堂随机抽取和背诵记录工具，Windows 桌面程序，用 PyQt5 写的。数据全放在本地文件里，不联网也能跑。

当前版本 1.0.1.1。

## 用在哪

上课点名、抽背，记下谁背过谁没过，一轮一轮往下走。抽取按动态权重算：抽得少、背得差的学生权重会涨，避免总抽到同几个人，也让薄弱的学生多被照顾到。

## 功能

- 抽取：开始/停止、自动与手动两种模式、不重复开关、重置本轮、撤销和跳过
- 标记：已背过、未背熟、未背过三种，本轮统计实时更新
- 统计：总览、积分、课文三页图表，下面接抽取记录和学生明细两张表，可以切历史轮次
- 课文：指定课文或随机课文，每篇有自己的权重，主界面双击就能调，抽中概率跟着变
- 皮肤：云白、夜曜、霓虹、极光、校园、纸上、极简、石墨共八套。切换时卡片、文字、按钮、SVG 图标一起换，不是只换个背景色
- 插件：目录式插件，每个插件一个文件夹加一份 `manifest.json`；插件中心分「已安装」和「插件市场」两页
- 手机抽取：电脑上开个局域网页面，手机连同一个 Wi-Fi 扫码打开，能看进度、新建轮次、翻结果。教室没网的话电脑开热点也行
- 名单和课文：纯文本文件，换电脑直接复制 `students.txt` 和 `texts.txt`

## 运行

需要 64 位 Python 3.13：

```bash
pip install PyQt5 qrcode pillow
python main_qt.py
```

第一次启动会自己生成 `config.json`。想跑 32 位环境也能用，32 位 Python 装同样的包即可。

## 插件

插件放在 `plugins/<插件名>/`，目录里至少两个文件：`manifest.json` 和入口 `main.py`。

manifest 字段：

| 字段 | 说明 |
|---|---|
| `id` | 唯一标识，建议 `com.你.插件名`，装了之后靠它认版本 |
| `name` | 显示名 |
| `version` | 版本号，市场靠它判断有没有更新 |
| `author` | 作者 |
| `description` | 简介 |
| `icon` | 图标文件名，可留空 |
| `entry` | 入口文件名 |
| `permissions` | 权限声明，见下 |
| `tags` | 分类标签 |

`permissions` 只认这几个键：`filesystem` `config` `ui` `tts` 属于常规权限，`draw` `network` `clipboard` `process` 属于敏感权限，装了之后会单独提示。

## 上架插件

插件市场的索引就是本仓库根目录的 `plugin_market.json`，客户端直接拉 raw 地址。

流程：

1. fork 本仓库
2. 在 `plugin_market.json` 的 `plugins` 数组末尾加一条，字段照上面填，`download_url` 指向你那个 `.arcx` 的 raw 地址
3. 把自己打好的 `.arcx` 也传到 fork 的仓库根目录
4. 提 PR，说明里写清楚插件做什么、要哪些权限

合并之后等几分钟 CDN 缓存刷新，客户端在插件市场点刷新就能看到。

## 目录

```
main_qt.py            主窗口
settings_window.py    设置窗口，31 个页面
skin_engine.py        皮肤引擎，QSS 生成和颜色映射
ui_kit.py             通用控件：按钮、弹窗、分段选择、Toast
classpulse_*.py       课堂脉搏相关模块：数据层、放映批注、悬浮层、面板
plugin_manager.py     插件加载与执行
plugin_market.py      插件市场、索引解析、版本比较
lan_service.py        局域网服务（手机抽取）
lan_pages.py          手机端页面
icons/                SVG 图标，按名字取
plugins/              插件目录
```

## 版本说明

1.0.1.1 是从 A13Rollcall V6.8 系列改名过来的。改的只有品牌部分：程序名、标题栏、关于页、安装包名、图标、版本号。班级名 A13 还在 `config.json` 的 `class_name` 里，没有动；`a13rollcall://` 这个 URL 协议也保留着，旧快捷方式不会失效。

## 许可

个人使用和教学使用免费。不要拿去卖。
