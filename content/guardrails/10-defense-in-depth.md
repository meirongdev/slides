+++
title = "Defense in depth"
weight = 100
outputs = ["Reveal"]
+++

## 🔁 Defense in depth

```text
IDE / Agent      AGENTS.md              curate context (Layer 1)
    |
git commit       pre-commit + commit-msg    format, compile, unit test, message
    |
git push         pre-push: spotbugs + verify (IT) + spec-check
    |
CI               Checkstyle . SpotBugs . ArchUnit . test . JaCoCo  (blocking)
    |
Kubernetes       probes . limits . admission policies       runtime
    |
Incident         kubectl events / logs / debug              observe
```

Each layer is cheap, fails fast, and catches what the previous one missed.
