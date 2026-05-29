# claude-mem — 8/10

**78,300 stars. That's the headline.**

## What it is

Every time Claude does something in a session (uses a tool, completes a task, makes a decision), a hook captures it. At the end of the session, those observations get compressed by AI and stored in a SQLite database. When you start a new session, the relevant past observations get injected back in automatically. You can also search your full session history with natural language: "what did I decide about the magic system?" and get a real answer.

Slightly more complex than claude-memory-compiler: needs Node.js and a vector database (Chroma), runs a little web server on your machine so you can browse your session history in a browser.

## Why you'd want it

Same core problem as claude-memory-compiler: Claude forgets between sessions. This fixes it. The difference: claude-mem uses vector search so it's smarter about what it retrieves. If you've had 200 book-writing sessions, it can find the specific memory about "how Fate-Anchor's magic costs work" without reading everything.

For a one-book project, claude-memory-compiler is enough. For a multi-book series where you're tracking hundreds of world decisions across 50+ sessions, claude-mem's semantic search becomes the right tool.

## Why I think it's worth your attention

78,000 stars is not a fluke. That's the community saying: this is the default solution. Most people who want persistent Claude Code memory end up here. The "it just works" installation (`npx claude-mem install`) is real — one command, works with Claude Code.

The tradeoff vs claude-memory-compiler: more complex system (Node.js + Bun + Chroma vs just Python), but smarter retrieval.

## What to do

This is third-party code (Node.js). Per our safety rule: I've reviewed the README and architecture. The developer (thedotmack) is less established than Cole Medin, but 78K stars and 6.7K forks suggest serious community inspection. The code captures all tool use observations — it sees everything Claude does in your sessions. Privacy feature exists (`<private>` tag) but requires you to manually tag sensitive content.

The star count is real validation. The complexity is real too.

**React 👍 and I'll build a Jarvis-native version** tuned for fiction writing — same hook pattern, markdown storage (no external Chroma), fiction-specific extraction (character states, plot beats, world rules, voice patterns). Lighter weight and under our control.

React 🚀 if you want to install claude-mem directly yourself — inspect the code first, it's open source.

🔗 https://github.com/thedotmack/claude-mem

**Safety gate status:** Large star count = community-vetted but also large attack surface. Node.js runtime. Captures ALL session activity. Recommend Jarvis-built version for Leo's use case (lighter, fiction-tuned, no external dependencies).
