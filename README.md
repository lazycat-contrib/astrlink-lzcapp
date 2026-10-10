# astrlink-lzcapp

[AstrLink](https://github.com/Calcium-Ion/AstrLink) 的懒猫微服打包。

本机（微服）运行的 AI 网关：把 Codex、Claude 等订阅与各家 API 提供商接入一处，
为 Codex / Claude Code / OpenCode 等 Agent 提供统一的 OpenAI / Anthropic / Gemini
兼容接口；调用记录、用量统计与隐私检测（提醒 / 拦截 / 脱敏）都在本机完成。

## 交付方式（镜像模式，双商店发布）

引用上游 Docker Hub 镜像 `calciumion/astrlink`，经懒猫镜像源交付：

- [`lazycat-action.yml`](.github/lazycat-action.yml) — 按 SemVer 追踪上游镜像版本
  （`channel: stable`；Action v1.3.0+）；由 Action 用 `LZC_API_TOKEN` 把镜像转存到
  `registry.lazycat.cloud` 并改写 manifest（官方商店要求 `delivery.mode: lazycat`）
- 同时发布**官方商店**与**喵喵（私有）商店**；每日定时检查上游新版本，发现即自动发版
- 数据持久化：`/lzcapp/var/data` → 容器 `/data`（配置、登录态、用量、模型）
- 容器 entrypoint 以 root 启动、把 `/data` 移交给内置 `astrlink` 用户后降权运行，
  因此绑定的数据目录无需手工处理属主

## 使用

1. 首次打开应用进入控制台，按提示**设置管理密码**（启动后 10 分钟内有效；重启可重新打开窗口）
2. 在「API 提供商」中接入订阅 / API 密钥并拉取模型；在「访问令牌」中为 Agent 创建令牌
3. Agent 的 Base URL 填本应用域名（OpenAI 兼容以 `/v1` 结尾），API Key 填访问令牌
4. 忘记控制台密码：在懒猫「应用设置」中打开「重置控制台密码」并重新配置实例，
   重启后密码被清除、可重新设置（随后请关闭该开关）

## 版权

内容来自 [Calcium-Ion/AstrLink](https://github.com/Calcium-Ion/AstrLink)（仓库许可证
Apache-2.0；因链接 RelayKit，组合程序按 AGPL-3.0 分发，详见上游 LICENSING.md）。
本仓库仅为打包，不修改上游内容。
