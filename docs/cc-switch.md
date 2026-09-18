# 用 CC Switch 接入 Dudu 中转站

适合已经在用 [CC Switch](https://ccswitch.io) 切 Claude Code / Codex 供应商的人。点几下就能换线路，不用每次手改环境变量。

需要先有：

- CC Switch 已安装。官网：https://ccswitch.io
- 已在 https://api.dududu.cloud 注册，并建好令牌

还没有令牌的话，先看 [手动接入](./manual.md) 的第 1、2 步。令牌不通，切工具也没用。

## 1. 打开 CC Switch

1. 打开 CC Switch
2. 顶部先选你要接的应用：`Claude Code` 或 `Codex`
3. 点右上角 **+**

预设列表里没有 Dudu，选 **自定义**。

## 2. 接 Claude Code

在 Claude Code 标签下添加自定义供应商：

- 名称：`Dudu`
- API Key：控制台复制的令牌
- 端点地址：`https://api.dududu.cloud`
- 不要以 `/` 结尾
- 不要勾「完整 URL 模式」
- 认证按默认走。多数情况是 `ANTHROPIC_AUTH_TOKEN`

保存后，在供应商列表里点 **启用**。

然后新开 Claude Code，发一句很短的话。能回、控制台用量有增加，就是通了。

如果 401：

1. 把认证改成 `ANTHROPIC_API_KEY` 再试一次
2. 检查端点是不是多写了 `/v1`

对应 JSON 大概是：

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "你的令牌",
    "ANTHROPIC_BASE_URL": "https://api.dududu.cloud"
  }
}
```

## 3. 接 Codex

切到顶部 `Codex`，再添加一条自定义供应商，不要和 Claude 共用同一套地址格式。

- 名称：`Dudu`
- API Key：同一条令牌即可
- 端点地址：`https://api.dududu.cloud/v1`
- 不要以 `/` 结尾
- 不要勾「完整 URL 模式」，除非官方文档明确要求填完整路径

保存后启用，再重启 Codex。

Codex 这边常见是 OpenAI 兼容协议。添加时如果有「API 格式」，选供应商文档要求的那种；Dudu 按 OpenAI 兼容填。选完如果提示需要本地路由，按 CC Switch 的提示打开即可。

## 4. 确认走的是 Dudu

1. CC Switch 当前启用的供应商名称是 `Dudu`
2. 重启对应 CLI，不要用旧窗口
3. 发一个很短的请求
4. 打开 https://api.dududu.cloud 看令牌用量是否增加

用量不动，说明还在走以前的官方或别的中转。回到 CC Switch 看有没有启用错应用标签。

## 5. 换分组

费率跟令牌绑定的分组走，不跟 CC Switch 的供应商名称走。

要换便宜档或稳定档：

1. 到控制台编辑令牌，改分组
2. 或新建一条令牌，再在 CC Switch 里改 API Key
3. 改完重新启用一次

分组和费率见 [README](../README.md)。

## 常见问题

**Claude 和 Codex 填成同一个地址**  
Claude 用 `https://api.dududu.cloud`，Codex 用 `https://api.dududu.cloud/v1`。混用会 404 或认证失败。

**启用了但不生效**  
旧的终端还握着上一份环境变量。把 Claude Code / Codex 全关，再开。

**完整 URL 模式要不要开**  
默认关。只有供应商要求填完整路径时才开。Dudu 用根地址或 `/v1` 前缀即可。

**想同时留官方备用**  
在 CC Switch 里把官方也加成一个供应商。平时启用 Dudu，官方挂了再切回去。不要把官方 Key 填进 Dudu 的配置里。
