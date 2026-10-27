# 02 配置模型和 Key

左下角点「设置」→「模型接口（社区版）」。

## 预设
社区版默认用 **Kimi**（backend/.env 默认就是 Kimi）；Kimi 不可用时换 **硅基流动**。**DeepSeek 暂不支持**（未能实测）。

| 按钮 | 接口地址 | 模型名 | 说明 |
|---|---|---|---|
| Kimi（推荐默认） | https://api.moonshot.cn/v1 | kimi-k2.6 | 2026-10-27 实测 3 张图（1 简单 + 2 复杂：后台仪表盘、价格页）都写出了页面，每张约 40–85 秒，版式、表格、卡片还原好（另有 1 次网络连接失败）。按量计费，新账号赠送额度以 Kimi 官方为准 |
| 硅基流动（备选） | https://api.siliconflow.cn/v1 | Qwen/Qwen3-VL-32B-Instruct | 2026-10-27 实测：2 次都写出了页面，但慢（约 2–5 分钟），版式比 Kimi 粗糙；按量计费 |
| OpenRouter | https://openrouter.ai/api/v1 | nvidia/nemotron-3.5-super-vl:free | 免费模型。2026-10-27 实测：一次成功生成可打开的 HTML，另有一次服务商返回 502。免费模型随时可能限流、变慢或下线 |
| 阿里云百炼（北京） | https://dashscope.aliyuncs.com/compatible-mode/v1 | qwen3.8-flash | **未实测**，按量计费；能否看图、是否兼容以百炼官方文档为准 |
| 自定义（清空） | — | — | 清空两项，改用 backend/.env 或自己填 |

也可以手动填其他支持看图和函数调用的 OpenAI 兼容服务。社区版两种接口都支持：Kimi、硅基流动、DeepSeek 的地址会自动走 **Chat Completions**（/chat/completions），其他地址走 **Responses**（/responses）。如果自动判断不对，可在 `backend/.env` 写 `OPENAI_API_STYLE=chat` 或 `OPENAI_API_STYLE=responses`。

**DeepSeek**：官方文档称 `deepseek-flash` 支持看图，但我们 2026-10-27 测试的账号余额为 0，接口返回「Insufficient Balance」（余额不足），**未能实测生成**，所以没有做预设。

## Key 怎么放（两种，任选）
- **页面里填**（推荐）：填在「API Key」框里。它只存在当前页面内存里，**刷新或关闭页面就没了**，不会写进浏览器存储，也不会写进硬盘。我们实测过：填写、生成、刷新后，浏览器 localStorage 里都没有 Key。
- **环境变量**：启动前在终端设置 `OPENAI_API_KEY=你的key`（Windows：`set OPENAI_API_KEY=你的key`）再运行脚本。

OpenRouter 的 Key 在 openrouter.ai 注册后自己创建；Kimi 的 Key 在 platform.moonshot.cn（Kimi 开放平台）创建；硅基流动的 Key 在 cloud.siliconflow.cn 创建；百炼的 Key 在阿里云百炼控制台创建。**不要把 Key 发给任何人，包括卖家。**

## 免费额度提醒
OpenRouter 免费模型有每天、每分钟的请求上限（以 OpenRouter 官方说明为准）。一次“生成”至少要 2 次请求（模型先写文件，再回复一句话），模型多想几步就更多；2026-10-27 实测一次成功生成用了 3 次。修改同样要花请求。
