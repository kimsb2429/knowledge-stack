---
description: Route a question to the right knowledge-stack specialist (code, docs, or work) based on its content. If the question spans domains, delegate to multiple specialists and synthesize.
---

# Ask the knowledge stack

Question: $ARGUMENTS

Decide which specialist(s) to invoke:

- **code-specialist** — questions about code structure, definitions, references, symbol search across repos.
- **docs-specialist** — questions answerable from indexed document corpora (policy, manuals, written guidance).
- **work-specialist** — questions about Confluence pages, Jira tickets, or workplace project state.

If the question clearly belongs to one domain, delegate to that specialist via the Task tool. If it spans domains (e.g. "is this auth pattern documented in Confluence and how is it implemented in our auth-service repo?"), delegate to multiple in parallel and synthesize their answers into a single response.

If the question doesn't fit any specialist, answer it directly and note that it fell outside the stack.
