# astrbot_plugin_web_lookup

AstrBot 4.x 插件，给模型提供联网查资料和解析视频链接的工具。

仓库从 [dafeiyu-open](https://github.com/newbiecraft-kaixin/dafeiyu-open) 拆出，代码相同。

## 功能

- `web_lookup` 工具（不需要 API Key）：多引擎取候选 → 排除 404、超时、登录墙 → 抓正文 → 用相似度挑最相关的一页，截断后返回
- 直接给 URL 抓正文；B 站视频可以取 AI 字幕或 CC 字幕
- 视频链接解析：抖音、快手、西瓜、B 站，返回标题、作者、前几条评论；没有字幕时转写音频前一段
- B 站刷视频：`bili_browse`、`bili_pool`、`bili_like` 三个工具，按标签从片单池挑，带停留时长
- 扫码登录：`看视频` 遇到需要登录的情况时，用浏览器截二维码，扫码后把 SESSDATA 写进配置

## 指令

| 指令 | 作用 |
| --- | --- |
| `视频` / `刷视频` | 按配置的标签刷 B 站，一次若干条 |
| `看视频 <链接>` | 解析指定链接，返回标题、评论和字幕 |

## 模型工具

`web_lookup`、`bili_browse`、`bili_pool`、`bili_like`

## 配置

字段定义见 [_conf_schema.json](_conf_schema.json)，和 AstrBot 插件配置页一一对应。常用的几项：

| 配置 | 默认 | 作用 |
| --- | --- | --- |
| `max_results` / `keep_n` | 8 / 5 | 搜索候选数、实际抓取的页面数 |
| `max_chars` | 1500 | 返回给模型的正文上限 |
| `concurrency` | 4 | 并发抓取数 |
| `bili_cookie` | 空 | B 站 SESSDATA，填了才能取 AI 字幕 |
| `bili_tags` | `["搞笑","pvz"]` | 刷 B 站用的标签 |
| `douyin_cookie` / `kuaishou_cookie` / `ixigua_cookie` | 空 | 填了才能取评论 |

## 依赖

- 基础功能只用标准库和 AstrBot 自带依赖
- 扫码登录需要本机有 Playwright，没有时提示手动填 Cookie，其他功能不受影响
- 无字幕视频的音频转写依赖本机转写环境，没有时只返回标题和评论

## 已知问题

- 各平台反爬会变，解析失败时先看日志，判断是被拦截还是页面结构变了
- 西瓜网页版已下线，只有 App 链接能解析，也没有扫码入口
- 返回的是最相关的一页，不是全网汇总

## 许可与来源

AGPL-3.0，见 [LICENSE](LICENSE)。

本项目是 [dafeiyu-open](https://github.com/newbiecraft-kaixin/dafeiyu-open) 的一部分，单独建仓库为了发布到插件市场。开发时使用 [AstrBot](https://github.com/AstrBotDevs/AstrBot)（AGPL-3.0）的插件接口，没有分发 AstrBot 自身的代码。署名见 [NOTICE.md](NOTICE.md)。
