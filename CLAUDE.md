# CLAUDE.md

> Maintained by **AGI_Ananas**

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

This is a reverse-engineered version of Anthropic's Claude Code CLI tool, with an **OpenAI-compatible API adapter layer** that allows connecting to any OpenAI-compatible API (DeepSeek, Ollama, etc.).

## Commands

```bash
bun install
bun run dev          # Dev mode
bun run build        # Build to dist/cli.js
echo "say hello" | bun run src/entrypoints/cli.tsx -p  # Pipe mode
```

## Architecture

### Runtime & Build

- **Runtime**: Bun (not Node.js)
- **Build**: `bun build src/entrypoints/cli.tsx --outdir dist --target bun`
- **Module system**: ESM with TSX

### Key Files

| File | Purpose |
|------|---------|
| `src/entrypoints/cli.tsx` | Entry point with runtime polyfills |
| `src/main.tsx` | Commander.js CLI definition |
| `src/services/api/claude.ts` | Core API client (streaming, tools, betas) |
| `src/services/api/client.ts` | Provider selection (Anthropic/Bedrock/Vertex/Foundry/**OpenAI-compat**) |
| `src/services/api/openai-compat/` | **OpenAI-compatible API adapter layer** |
| `src/components/OpenAICompatSetup.tsx` | **Interactive setup UI for third-party APIs** |
| `src/utils/model/providers.ts` | API provider type definitions |
| `src/query.ts` | Main query function with tool call loop |
| `src/QueryEngine.ts` | Conversation state orchestrator |
| `src/screens/REPL.tsx` | Interactive REPL screen (React/Ink) |

### OpenAI-compat Adapter

The adapter in `src/services/api/openai-compat/` translates between Anthropic and OpenAI formats:
- `request-adapter.ts` — Anthropic request → OpenAI request
- `stream-adapter.ts` — OpenAI SSE stream → Anthropic event stream
- `thinking-adapter.ts` — DeepSeek R1 `reasoning_content` / QwQ `<think>` tags
- `index.ts` — Duck-typed Anthropic client that upstream code calls transparently

### Feature Flags

All `feature()` calls are polyfilled to return `false`. React Compiler output has `_c()` memoization boilerplate — this is normal.