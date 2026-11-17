# dsh-exa-parallel-mcp

给 DeepSeek Harness（DSH）用的 MCP 搜索接入 bundle：把 Exa 和 Parallel 两个免密钥的远程 MCP 服务器注册进 DSH，装好后会话里可以直接调用 `mcp__exa__*` 和 `mcp__parallel__*` 系列工具。

## 包含什么

| 服务器 | 端点 | 说明 |
| --- | --- | --- |
| Exa | `https://mcp.exa.ai/mcp` | 常规搜索与网页正文字段提取 |
| Parallel | `https://mcp.parallel.ai/mcp` | 补充搜索、批量抓取、垂直数据源 |

两个连接都用 `streamable-http` 传输，匿名可用、不需要 API key，并且 `failOnStartupError: false`，连不上时只报错，不影响会话启动。

## 前提

DSH 运行时版本 `>=0.2.0-rc.1 <0.3.0-0`，并在 Web 界面里使用。

## 安装

克隆仓库：

```bash
git clone https://github.com/RunhuaChen/dsh-exa-parallel-mcp.git
```

在 DSH 的会话里让 Agent 把这个本地目录装成 bundle：

> 用 plugin_manager 把 dsh-exa-parallel-mcp 仓库目录装成 bundle，目录是 /你的路径/dsh-exa-parallel-mcp

或者直接调用插件管理工具：

```json
{ "action": "install_bundle", "target": "/你的路径/dsh-exa-parallel-mcp" }
```

安装完成后刷新页面；如果刷新后工具没有出现，把 DSH 应用完整重启一次。

## 卸载

```json
{ "action": "remove_bundle", "target": "@RunhuaChen/dsh-exa-parallel-mcp" }
```

## 工作原理

bundle 只有一个 Cordis 组合补丁 `cordis.patch.yml`：往组合里插入两行 `@deepseek-ai/dsh-mcp-client`，分别指向 Exa 和 Parallel 的 streamable-http 端点。`index.js` 是空的宿主入口，没有客户端代码。

## 注意

- 两个服务器匿名可用，不需要密钥；不要把密钥写进对话或配置文件。
- 同一时间只应启用一份指向相同端点的 MCP 配置，避免出现重复的工具名。
