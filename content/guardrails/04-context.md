+++
title = "Layer 1 — Context"
weight = 40
outputs = ["Reveal"]
+++

{{% section %}}

## 📄 Layer 1 — Context Engineering

> "Curate what the AI sees so it has to guess less."

- Brief the agent like a **new teammate** on day one
- Give it **rules + constraints + concrete examples**
- Better context → fewer surprises, less rework

**Key insight:** `AGENTS.md` is the agent-agnostic place to write this down once — readable by humans *and* by any AI tool.

🔗 https://agents.md/

---

## What the AI actually sees

The context window, top to bottom:

```text
System instructions    → vendor/tool base behavior
Custom instructions    → your AGENTS.md, team conventions
Conversation history   → prompts, replies, corrections
Implicit context       → open files, selection, git diff
Explicit references    → #file, pasted snippets
Tool outputs           → build / test / lint feedback
```

You **control** the middle layers and the tool outputs.
You **don't control** model reasoning or perfectly repeatable output — so make the controllable parts strong.

---

## `AGENTS.md` — structure

A plain Markdown contract that lives in the repo:

```markdown
# Project Overview
## Build and Test Commands       # make build / make test / mvn verify
## Code Quality and Style        # JavaDoc on public methods, no NPEs
## Architecture                  # constructor injection, immutable DTOs
## Security                      # no PII in logs, validate all input
## Plugins                       # Checkstyle / SpotBugs / ArchUnit configs
```

Keep it at the **workspace root** — the single source of truth that
`CLAUDE.md` and other assistant docs inherit from.

---

## `AGENTS.md` — discovery: nearest wins

```text
project-root/
|-- AGENTS.md          ← 1. read first  (global rules)
+-- src/main/
    |-- AGENTS.md      ← 2. read next   (module rules)
    +-- java/com/app/service/
        |-- AGENTS.md  ← 3. read last   (most specific — overrides parent)
        +-- UserService.java   ← file being generated
```

> Rules **merge top-down**; the nearest `AGENTS.md` wins on conflict.
> Put broad rules at the root, exceptions close to the code.

{{% /section %}}
