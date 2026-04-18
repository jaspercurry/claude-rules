# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Check Prior Art First

**Most problems aren't novel. Look before building.**

Before designing or implementing non-trivial work:
- For contained tasks: search for existing solutions (libraries, in-repo patterns, prior discussions) before inventing.
- For large or architectural tasks: pause and ask whether the user wants to bring in external research or prior art before you start. Don't assume "just dive in."
- If you're about to design a new abstraction, check whether a standard one already exists.

**The test:** Can you name at least one prior example (library, pattern, or past implementation) that informs your approach? If no, you may be reinventing.

## 3. Diagnose Before Solving

**Find the cause before building the fix.**

When something is broken or behaving unexpectedly:
- Form a hypothesis about the cause. State it explicitly.
- Gather evidence before writing a fix — add logging, reproduce in isolation, read the actual error, bisect the change.
- If the evidence contradicts the hypothesis, revise it. Don't patch around a theory you haven't confirmed.
- A plausible-sounding fix is not a located cause.

**The test:** Before writing the fix, can you point to the specific line or condition that produces the observed behavior?

## 4. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

**When cutting is a judgment call, name it — don't hide it.** If you can see a reasonable argument for more structure (defensive handling, extensibility, abstraction that aids readability), surface it before deciding. Silent simplification is as bad as silent over-engineering.

## 5. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

**The test:** Every changed line should trace directly to the user's request.

## 6. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:

```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 7. Close the Loop

**Run the code, read the output. Runtime beats reasoning.**

After making a change, before declaring it done:
- Execute the code, not just the types. Read the actual logs, errors, and output.
- For UI/frontend work: spin up the dev server and exercise the feature in a browser (Chrome MCP, headless browser, whatever is available). Check the console and network tab.
- For scripts and CLIs: run with realistic inputs. Read stdout/stderr, not just the exit code.
- For backend/API changes: hit the endpoint, inspect the response, check server logs.
- If the environment genuinely can't run it, say so explicitly — don't claim success from reading the diff.

**The test:** Can you point to a runtime signal (a passing test, a log line, a rendered screen, an API response) that confirms the change does what you claim?

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
