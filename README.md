# claude-rules

Personal behavioral guidelines for Claude Code, loaded globally via `~/.claude/CLAUDE.md`.

Built on top of [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills), which distilled four behavioral principles from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls. Three additional principles — **Check Prior Art First**, **Diagnose Before Solving**, and **Close the Loop** — were added to address failure modes observed in personal use.

## Why this exists

Claude Code is capable but has predictable failure modes: picking an interpretation silently when things are ambiguous, over-engineering simple requests, touching code it shouldn't, and jumping to fixes before the root cause is located. These guidelines are a short, durable set of rules that bias behavior away from those defaults — applied to every project, on every machine, without copy-pasting anything per-repo.

They bias toward caution over speed. For trivial tasks, judgment still applies.

## How it's used

[`CLAUDE.md`](./CLAUDE.md) is symlinked into `~/.claude/CLAUDE.md`, which Claude Code auto-loads on every session regardless of the working directory. Any project-specific `CLAUDE.md` in a given repo merges on top, so this file acts as an always-on baseline rather than a replacement for per-project context.

New machine setup:

```bash
mkdir -p ~/code && \
  git clone https://github.com/jaspercurry/claude-rules ~/code/claude-rules && \
  ln -sf ~/code/claude-rules/CLAUDE.md ~/.claude/CLAUDE.md
```

Updating the rules: edit `CLAUDE.md`, commit, push. Every session on every machine with the symlink in place picks up the latest on next launch.
