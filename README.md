# new-api-cn (formerly AllRouter middleware)

Patched fork of [QuantumNous/new-api](https://github.com/QuantumNous/new-api) used by AllRouter.

## Patches

### 1. OpenGraph / Twitter Card meta (SEO)

Stock `new-api` serves an empty `<title>` and the default meta description. This fork injects:

```html
<title>AllRouter · 25 models · Kimi K3 at official pricing</title>
<meta property="og:title" content="AllRouter">
<meta property="og:image" content="https://allrouter.ai/og-kimi-k3.png">
<meta name="twitter:card" content="summary_large_image">
<meta name="description" content="One key, 25 frontier LLMs. Kimi K3, Claude, GPT-5, Gemini, DeepSeek, GLM, Grok at official prices. Alipay/WeChat supported.">
```

See `web/public/index.html.patch` for the diff.

### 2. Kimi K3 Codex streaming fix

Stock new-api mediations Moonshot's K3 wire protocol and does not emit `response.completed`. This causes Codex desktop to infinite-retry on longer generations.

Our patch (`relay/channel/moonshot/adaptor.go.patch`):
- Detect `usage.finish_reason` in the streaming delta
- Emit `event: response.completed` after the last `content_block_delta`
- Compatible with both Moonshot's native streaming and Anthropic protocol

### 3. CN-friendly defaults

- Default CDN edge to `cn-north-1` (when detected in CN IP range)
- Default pricing in CNY when `X-Locale: zh-CN` header
- Native support for Alipay / WeChat Pay via `payment/` package

## Using with our hosted version

You don't need to self-host. AllRouter runs this fork at https://allrouter.ai with:

- 25 models including Kimi K3 at official pricing
- $5 free credits on signup
- Alipay/WeChat Pay

## Why we upstream

We intend to merge patches back upstream to [QuantumNous/new-api](https://github.com/QuantumNous/new-api). PRs pending review.

## License

MIT (same as upstream new-api)
