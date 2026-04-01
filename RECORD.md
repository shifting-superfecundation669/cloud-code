# Claude Code 项目运行记录

> Maintained by **AGI_Ananas**
>
> 项目: claude-code (OpenAI Compatible Fork)

---

## 一、项目目标

将 claude-code 项目运行起来并新增 OpenAI 兼容 API 支持，允许接入 DeepSeek、Ollama、优云智算等第三方模型。

---

## 二、当前状态：已可运行 + OpenAI 兼容 API 已接入

```bash
bun run dev
```

| 测试 | 结果 |
|------|------|
| Anthropic Direct API | ✅ |
| Bedrock / Vertex / Foundry | ✅ |
| **OpenAI 兼容 API（DeepSeek/Ollama/优云智算）** | **✅** |
| 交互式配置界面 | ✅ |
| 配置持久化 & 自动加载 | ✅ |
| 流式对话 | ✅ |
| 工具调用 | ✅ |

---

## 三、OpenAI 兼容适配层改动记录

### 3.1 新增文件

| 文件 | 用途 |
|------|------|
| `src/services/api/openai-compat/index.ts` | 伪 Anthropic 客户端，处理 `.withResponse()` Promise 链 |
| `src/services/api/openai-compat/request-adapter.ts` | Anthropic → OpenAI 请求格式转换 |
| `src/services/api/openai-compat/stream-adapter.ts` | OpenAI SSE → Anthropic 事件流转换 |
| `src/services/api/openai-compat/thinking-adapter.ts` | DeepSeek R1 / QwQ 思考模型适配 |
| `src/components/OpenAICompatSetup.tsx` | 交互式配置界面 |

### 3.2 修改的现有文件

| 文件 | 改动 |
|------|------|
| `src/utils/model/providers.ts` | 类型加 `openai_compat`，`getAPIProvider()` 加检测 |
| `src/utils/model/configs.ts` | 每个 config 加 `openai_compat` 字段 |
| `src/services/api/client.ts` | `getAnthropicClient()` 加 openai_compat 分支 |
| `src/components/ConsoleOAuthFlow.tsx` | 登录界面加第四个选项 + onChange + 状态拦截 |
| `src/entrypoints/cli.tsx` | 启动时从 ~/.claude.json 加载已保存配置 |

### 3.3 已验证

| 提供商 | 模型 | 状态 |
|--------|------|------|
| 优云智算 (api.modelverse.cn) | MiniMax-M2.5, gpt-5.4 | ✅ |