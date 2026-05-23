# Google Genkit Middleware — Composable Hooks for Agentic Apps — 7/10

## What it is
On May 14, Google announced a middleware system for their Genkit agent framework. It lets you intercept AI agent calls at three levels: the whole conversation (generate-level), the model call (model-level — add retries, fallbacks), and individual tool calls (tool-level — require human approval, sandbox filesystem access). You stack middleware layers like a chain, and each one wraps the next. It's not Claude-specific — it works with any AI provider.

## Why you'd want it (specific to your stack)
Your Jarvis multi-agent setup currently has no fault tolerance. If a sub-agent in your Council or fixer loop fails, the whole run usually dies. The pattern here — "retry the model call on transient errors, fall back to a different model if needed, require approval before sensitive tool calls" — is exactly what your autonomous writing runs need when Claude has a hiccup at 2am. The Claude Code hooks system supports the same pattern; this gives you the vocabulary for implementing it cleanly.

## Why I think it's worth your attention
This is the Google signal that layered middleware hooks are becoming the industry standard for agentic reliability. Anthropic will ship something similar natively. Learning the pattern now means you can implement it in your skills before it's officially first-party.

## What to do
This is informational — it's a Google framework you wouldn't install. But the three-layer pattern (generate / model / tool hooks) is worth implementing natively in your Jarvis orchestration skills. React 👍 if you want me to sketch a Claude Code hooks design inspired by this pattern.

🔗 https://developers.googleblog.com/announcing-genkit-middleware-intercept-extend-and-harden-your-agentic-apps/
