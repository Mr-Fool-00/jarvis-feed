# Briefing: VS Code 1.128 — Multi-Chat Claude Agents GA + Copilot Vision GA

**Score:** 7/10 · **build_worthy:** ❌ NO (use it)
**Source:** Visual Studio Magazine, VS Code release notes · July 8, 2026
**URLs:**
- https://visualstudiomagazine.com/articles/2026/07/08/claude-ai-gets-yet-another-boost-in-vs-code-1-128.aspx
- https://code.visualstudio.com/updates/v1_128

---

## Multi-Chat Claude Agents

VS Code 1.128 ships **Multi-Chat Claude Agents** as GA in the Agents window. What this means:

- You can have multiple related Claude agent chats open in one VS Code session simultaneously
- Any chat can be branched from any earlier turn — navigate back to turn 3 in chat 1, branch from there to explore a different approach without losing the original
- Chats share the same file tree context and open editor state
- You can compare outputs from two different approaches side-by-side in the chat panel

**The practical workflow this enables**: instead of "commit to approach A, if it fails start over with approach B," you can run approach A and approach B simultaneously, see which makes more progress after 5 turns, and kill the other one.

This is VS Code's version of what CC's parallel sub-agents already do — but native in the IDE, with full editor context (open files, active terminal, git status) available to both chats.

## Copilot Vision

Images and PDFs in chat, now GA:
- Screenshot a UI error → paste directly in chat → agent sees and responds
- Design mockup as PNG → agent can reference visual layout
- PDF documentation → agent reads inline (no file-upload workaround)

Previously required base64 encoding or external URL workarounds. Now it's paste-and-go.

---

## What to try

If Leo is on VS Code:
1. Update to 1.128
2. Open the Agents panel
3. Start a refactoring task — when you hit a decision point ("should I restructure the module or just fix the function?"), click "Branch from here" and explore both paths

The branch-from-turn feature is the most useful one. It's version control for your agent conversation, inside the editor.
