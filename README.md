# Dudu 中转站

国内用 GPT、Codex、Claude Code，不想自己搭中转时用这个。

- 控制台：https://api.dududu.cloud
- API 地址：`https://api.dududu.cloud`
- 注册即可使用，注册后在控制台充值、建令牌

接入教程：

- [手动配置](./docs/manual.md)
- [用 CC Switch 配置](./docs/cc-switch.md)

## 费率

按官方美元价乘分组倍率扣费。`0.13x` 就是官方价的 13%。余额在控制台看。

分组名和控制台里看到的一致。价格以后台当时显示为准。

### GPT / Codex

| 分组 | 倍率 | 适合 |
| --- | --- | --- |
| gpt plus+ team混池 | 0.075x | 最便宜。能用，但稳定性一般 |
| gpt 稳定版 | 0.13x | 日常默认走这条 |
| 紧急特供 / 企业专线 / gpt5专供 | 0.22x | 要更稳、走专线时用。用之前先小请求测一下 |

### Claude Code

| 分组 | 倍率 | 适合 |
| --- | --- | --- |
| kiro 分组 | 0.075x | 便宜档 |
| kiro max | 0.25x | 手感更接近 Cursor |
| CC cursor 反代 | 0.4x | Cursor 反代档 |
| ccmax分组 | 0.8x | 更高倍率档 |

新建令牌时把分组选对。选错分组，扣费和稳定性都会对不上。

## 先跑通

Claude Code：

```bash
export ANTHROPIC_BASE_URL=https://api.dududu.cloud
export ANTHROPIC_AUTH_TOKEN=你的令牌
```

GPT / Codex：

```bash
export OPENAI_BASE_URL=https://api.dududu.cloud/v1
export OPENAI_API_KEY=你的令牌
```

令牌在控制台创建。完整步骤看上面两份教程。

## 使用边界

- 目前主要做 GPT 系列和 Claude Code
- 便宜档会抖，生产不要只靠最便宜那条
- 不保证一直可用，重要项目留官方账号做备用
- 不是 OpenAI / Anthropic 官方

## 联系方式

如果不会可以联系站长
vx：notionrealistic
qq：2729218653



