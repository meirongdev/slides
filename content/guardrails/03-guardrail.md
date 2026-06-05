+++
title = "What is a guardrail?"
weight = 30
outputs = ["Reveal"]
+++

## 🧠 What is a "guardrail"?

> A guardrail makes the **right thing easy** and the **wrong thing fail loudly** — automatically, every time.

Two complementary layers:

| Layer | Goal | Mechanism |
|---|---|---|
| **Context** | Tell the agent the rules *up front* | `AGENTS.md`, conventions, examples |
| **Enforcement** | Catch what slips through | Static analysis, hooks, CI, runtime |

Context reduces mistakes. Enforcement guarantees they never merge.
