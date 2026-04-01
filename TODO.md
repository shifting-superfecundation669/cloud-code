# TODO

> Maintained by **AGI_Ananas**

## OpenAI-compat Adapter

- [x] Request format conversion (system/messages/tools)
- [x] Streaming SSE conversion (OpenAI → Anthropic events)
- [x] Tool call support (bidirectional)
- [x] Thinking model support (DeepSeek R1 / QwQ)
- [x] Interactive setup UI (provider select + input)
- [x] Config persistence (~/.claude.json)
- [x] Auto-load config on startup
- [x] URL smart handling (/v1 suffix)
- [ ] Image/multimodal testing with third-party models
- [ ] More provider presets
- [ ] `/model` command to switch models without re-login

## Packages

- [x] `url-handler-napi` — URL handler
- [x] `modifiers-napi` — Modifier key detection
- [x] `audio-capture-napi` — Audio capture
- [x] `color-diff-napi` — Color diff (full TS implementation)
- [x] `image-processor-napi` — Image processor