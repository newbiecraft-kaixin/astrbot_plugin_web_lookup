# astrbot_plugin_web_lookup

**给 AI 装上联网查证和看视频的能力**：搜索 → 校验 → 抓正文 → 挑最相关的一页；也能解析抖音 / 快手 / 西瓜 / B站链接，取标题、评论、字幕。

AstrBot 4.x 插件。本仓库从 [dafeiyu-open](https://github.com/newbiecraft-kaixin/dafeiyu-open) 拆出，代码相同。

## 它做什么

- **`web_lookup` 工具（免 API Key）**：多引擎聚合出候选名单 → 404 / 超时 / 登录墙校验 → 流式抓正文 → 相似度挑最相关的一页，截断后返回给模型
- **直接抓链接**：给一个 URL 就抓正文；B站视频还能取 AI / CC 字幕
- **视频解析**：抖音 / 快手 / 西瓜 / B站链接 → 标题、作者、前 N 条评论；没字幕时可转写音频前若干秒
- **B站刷视频**：`bili_browse`、`bili_pool`、`bili_like` 三个工具 + `视频` 指令，按标签从片单池里挑，带防风控的停留时长
- **扫码登录**：`看视频` 走到需要登录的分支时，用真浏览器截二维码，扫码后自动把 SESSDATA 写进配置

## 指令

| 指令 | 作用 |
| --- | --- |
| `视频` / `刷视频` | 按配置的标签刷 B站，一次若干条 |
| `看视频 <链接>` | 解析指定视频链接，回标题、评论与字幕 |

## 模型工具

`web_lookup` · `bili_browse` · `bili_pool` · `bili_like` · `send_meme`（由表情包插件提供，非本插件）

## 配置

配置项见 [_conf_schema.json](_conf_schema.json)（AstrBot 插件配置页同名字段）。常调的几个：

| 配置 | 默认 | 作用 |
| --- | --- | --- |
| `max_results` / `keep_n` | 8 / 5 | 搜索候选数与实际抓取的页数 |
| `max_chars` | 1500 | 返回给模型的正文上限 |
| `concurrency` | 4 | 并发抓取数 |
| `bili_cookie` | 空 | B站 SESSDATA，填了才能拿 AI 字幕 |
| `bili_tags` | `["搞笑","pvz"]` | 刷 B站用的标签 |
| `douyin_cookie` / `kuaishou_cookie` / `ixigua_cookie` | 空 | 填了才能拿评论 |

## 依赖

- 基础功能只用标准库 + AstrBot 自带依赖
- 扫码登录需要本机有 Playwright（浏览器自动化），没有时提示手填 Cookie，不影响其他功能
- 无字幕视频的音频转写依赖本机转写环境，缺省时只回标题与评论

## 已知限制

- 各平台反爬会变，解析失败时请先看日志里被拦的还是页面结构变了
- 西瓜网页版已下线，只有 App 链接可解析，没有扫码入口
- 求快不求全：返回的是「最相关的一页」，不是全网汇总

## 许可与来源

AGPL-3.0，见 [LICENSE](LICENSE)。

本项目是 [dafeiyu-open](https://github.com/newbiecraft-kaixin/dafeiyu-open) 的一部分，单独拆仓以便插件市场分发。基于 [AstrBot](https://github.com/AstrBotDevs/AstrBot)（AGPL-3.0）的插件接口开发，未分发 AstrBot 本体代码。完整署名见 [NOTICE.md](NOTICE.md)。
