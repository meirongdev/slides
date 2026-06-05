+++
title = "Layer 2 — Enforcement"
weight = 50
outputs = ["Reveal"]
+++

{{% section %}}

## 🔍 Layer 2 — Why static analysis?

> `AGENTS.md` *defines* the standards. Static tools *enforce* them.

The gap if you stop at documentation:

- 📋 Standards live only in prose — easy to ignore
- 🤖 Agents (and humans) miss subtle requirements
- 🐌 Manual review is slow and subjective
- 🚨 Violations quietly slip into commits

The fix: **automated, measurable, blocking checks** — so review can focus on *functional logic*, not brace placement.

---

## The Java guardrail trio

| Tool | Catches | Config |
|---|---|---|
| **Checkstyle** | style, naming, formatting, line length | `checkstyle.xml` |
| **SpotBugs** | NPEs, resource leaks, bad casts, overflow, concurrency | `spotbugs-exclude.xml` |
| **ArchUnit** | layering, cycles, forbidden APIs, conventions | `*ArchitectureTest.java` |

All three run inside one command:

```bash
mvn verify     # or: make coverage
```

If any gate fails, the build stops. No green build → no merge.

---

## 🎨 Checkstyle — style is not a debate

**Without it:** inconsistent naming, random indentation, unreadable diffs.

**With it:**
- ✅ Naming conventions validated mechanically
- ✅ Indentation, whitespace, line length enforced
- ✅ Pairs with a formatter (Spotless / google-java-format)
- ✅ `make format` auto-fixes most violations

> Style stops being a code-review opinion and becomes a build result.

---

## 🐛 SpotBugs — bug patterns before runtime

Bytecode-level analysis that catches what compiles but breaks:

- ✅ Potential **null pointer dereferences**
- ✅ **Resource leaks** — unclosed streams / connections
- ✅ Type-cast & `equals`/`hashCode` mistakes
- ✅ Integer **overflow / underflow**
- ✅ Common **concurrency** hazards

> "It compiles" ≠ "it's correct." SpotBugs closes part of that gap for free.

---

## 🏛️ ArchUnit — architecture as a test

**Without it:** circular deps, layering violations, business logic in the wrong layer, the same anti-pattern copy-pasted everywhere.

**With it** — architecture rules become **JUnit tests**:

- ✅ Layer separation (controller → service → repository)
- ✅ No circular dependencies between packages
- ✅ Controlled access between modules
- ✅ Ban specific anti-patterns and APIs
- ✅ The architecture is **documented in executable code**

{{% /section %}}
