# Claude Code (OpenAI Compatible Fork)

基于 Claude Code CLI 逆向还原项目的二次开发版本，**新增 OpenAI 兼容 API 适配层**，支持 DeepSeek、Ollama、优云智算等任意 OpenAI 兼容 API。

> Maintained by **AGI_Ananas**
>
> 原始逆向工程基础来自社区，本仓库在此基础上实现了第三方模型接入能力。

## 快速开始

### 环境要求

- [Bun](https://bun.sh/) >= 1.3.11（必须最新版，`bun upgrade`）
- Node.js >= 18

### 安装 & 运行

```bash
bun install
bun run dev
```

启动后选择第四个选项 `OpenAI-compatible API`，按引导输入 API 地址、Key、模型名即可：

```
Select login method:
  1. Claude account with subscription · Pro, Max, Team, or Enterprise
  2. Anthropic Console account · API usage billing
  3. 3rd-party platform · Amazon Bedrock, Microsoft Foundry, or Vertex AI
❯ 4. OpenAI-compatible API · DeepSeek, Ollama, QwQ, etc.
```

配置自动保存到 `~/.claude.json`，下次启动无需重复输入。

### 环境变量方式（可选）

```bash
export CLAUDE_CODE_USE_OPENAI_COMPAT=1
export OPENAI_COMPAT_BASE_URL=https://api.deepseek.com
export OPENAI_COMPAT_API_KEY=sk-xxx
export OPENAI_COMPAT_MODEL=deepseek-chat
bun run dev
```

## 新增：OpenAI 兼容 API 适配层

### 支持的 API 提供商

| 提供商 | Base URL | 示例模型 |
|--------|----------|----------|
| 优云智算 | `https://api.modelverse.cn/v1` | MiniMax-M2.5, gpt-5.4 |
| DeepSeek | `https://api.deepseek.com` | deepseek-chat, deepseek-reasoner |
| Ollama | `http://localhost:11434` | qwen2.5:7b, llama3 |
| 任意 OpenAI 兼容 API | 自定义 URL | 自定义模型名 |

### 架构

适配层位于 `src/services/api/openai-compat/`，通过 duck-typing 伪装 Anthropic SDK 客户端，上游代码零改动：

```
claude.ts → getAnthropicClient() → createOpenAICompatClient()
  ├─ request-adapter.ts: Anthropic params → OpenAI params
  ├─ fetch() → 第三方 API
  └─ stream-adapter.ts: OpenAI SSE → Anthropic 事件流
```

### 改动文件清单

**新增 5 个文件：**

| 文件 | 用途 |
|------|------|
| `src/services/api/openai-compat/index.ts` | 伪 Anthropic 客户端入口 |
| `src/services/api/openai-compat/request-adapter.ts` | 请求格式转换 |
| `src/services/api/openai-compat/stream-adapter.ts` | 流式响应转换 |
| `src/services/api/openai-compat/thinking-adapter.ts` | 思考模型适配（DeepSeek R1 / QwQ） |
| `src/components/OpenAICompatSetup.tsx` | 交互式配置界面 |

**修改 5 个现有文件：**

| 文件 | 改动 |
|------|------|
| `src/utils/model/providers.ts` | 加 `openai_compat` provider 类型 |
| `src/utils/model/configs.ts` | 每个 config 加 `openai_compat` 字段 |
| `src/services/api/client.ts` | `getAnthropicClient()` 加 openai_compat 分支 |
| `src/components/ConsoleOAuthFlow.tsx` | 登录界面加第四个选项 |
| `src/entrypoints/cli.tsx` | 启动时自动加载已保存配置 |

## 其他命令

```bash
# 管道模式
echo "say hello" | bun run src/entrypoints/cli.tsx -p

# 构建
bun run build
```

## 许可证

本项目仅供学习研究用途。Claude Code 的所有权利归 [Anthropic](https://www.anthropic.com/) 所有。