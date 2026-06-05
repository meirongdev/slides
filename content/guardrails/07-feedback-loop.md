+++
title = "The agent feedback loop"
weight = 70
outputs = ["Reveal"]
+++

## 🔁 The agent feedback loop

```text
+-----------------------------+
|   Coding Agent (any tool)   |
|   reads AGENTS.md, writes   |
+--------------+--------------+
               | generates code
               v
+-----------------------------+
|  mvn verify                 |
|  Checkstyle . SpotBugs .    |
|  ArchUnit . tests           |
+------+---------------+------+
       |               |
violations              all pass
       |               |
       v               v
feedback to agent      ready for human
(report + AGENTS.md)   functional review
       |
       +--> agent fixes -> re-runs verify -> loops until green
```

The tool output *is* the prompt for the next iteration.
