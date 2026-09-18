# 手动接入 Dudu 中转站

适合不装切换工具、自己改环境变量或配置文件的人。

需要先有：

- 能打开 https://api.dududu.cloud
- 一个邮箱，用来注册
- 已经能在本机跑 Claude Code，或 Codex / 兼容 OpenAI 的客户端

## 1. 注册并拿令牌

1. 打开 [Dudu 中转站](https://api.dududu.cloud)
2. 注册并登录
3. 按控制台提示充值
4. 进入「令牌」页面，新建一条令牌
5. 分组选你要的线路，名称自己起
6. 创建后立刻复制令牌，页面只展示一次

分组对应费率：

- GPT 日常：`gpt 稳定版`（0.13x）
- GPT 先省钱：`gpt plus+ team混池`（0.075x）
- Claude 先省钱：`kiro 分组`（0.075x）
- Claude 更接近 Cursor：`kiro max`（0.25x）

完整费率表见 [README](../README.md)。

## 2. 测一下令牌是通的

把 `你的令牌` 换成刚复制的值。

```bash
curl https://api.dududu.cloud/v1/models \
  -H "Authorization: Bearer 你的令牌"
```

能返回模型列表就说明账号和令牌没问题。这里报 401，先回头检查令牌和分组，不要继续改客户端。

## 3. 接 Claude Code

PowerShell 当前窗口临时生效：

```powershell
$env:ANTHROPIC_BASE_URL = "https://api.dududu.cloud"
$env:ANTHROPIC_AUTH_TOKEN = "你的令牌"
```

如果客户端不认 `ANTHROPIC_AUTH_TOKEN`，改成：

```powershell
$env:ANTHROPIC_BASE_URL = "https://api.dududu.cloud"
$env:ANTHROPIC_API_KEY = "你的令牌"
```

然后新开一个 Claude Code 窗口，发一句很短的话。能回就通了。

长期使用不要把令牌写进会提交到 Git 的文件。可以写进用户目录下的 Claude 配置，或系统环境变量。

常见配置位置：

- Windows：用户环境变量，或 Claude Code 自己的 settings
- 字段只用 Base URL 和令牌，不要把 `/v1` 接到 Claude 的地址后面

## 4. 接 Codex / GPT 客户端

Codex 和多数 OpenAI 兼容客户端要带 `/v1`。

PowerShell：

```powershell
$env:OPENAI_BASE_URL = "https://api.dududu.cloud/v1"
$env:OPENAI_API_KEY = "你的令牌"
```

如果走 Codex 的 `config.toml`，大致是：

```toml
model_provider = "dudu"
model = "gpt-5.6"

[model_providers.dudu]
name = "dudu"
base_url = "https://api.dududu.cloud/v1"
wire_api = "responses"
requires_openai_auth = true
```

`auth.json` 里放：

```json
{
  "OPENAI_API_KEY": "你的令牌"
}
```

模型名以控制台「可用模型」为准，不要抄过期名字。

## 5. 对一下有没有走 Dudu

通了之后回控制台看令牌用量。有新消费，说明请求打到了中转。用量一点没动，多半还在走官方，或开错了终端窗口。

## 常见问题

**401 / 403**  
令牌复制少了字符，或 Claude 用了带 `/v1` 的地址。Claude 用根地址 `https://api.dududu.cloud`，GPT / Codex 才用 `/v1`。

**连得上但特别慢、动不动 429**  
先换「稳定版」或更高倍率分组。最便宜的混池会抖。

**扣费和心理预期差很多**  
看令牌绑的是哪条分组。`0.075x` 和 `0.8x` 差十倍以上。

**Windows 里环境变量设了但不生效**  
当前窗口设的变量，关窗口就没了。已经打开的 Claude Code / Codex 也不会自动读新变量，要先关再开。
