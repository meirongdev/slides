+++
title = "Shift left — local guardrails"
weight = 80
outputs = ["Reveal"]
+++

{{% section %}}

## 🪝 Shift left — local guardrails

CI is the last line of defense, not the first. Catch violations **before**
they leave the laptop, using Git hooks wired through a `Makefile`.

Self-installing — the first `make` sets it up:

```make
_HOOKS_PATH := $(shell git config --get core.hooksPath 2>/dev/null)
ifneq ($(_HOOKS_PATH),.githooks)
_ := $(shell test -d .git && git config core.hooksPath .githooks \
        && chmod +x .githooks/* 2>/dev/null)
endif
```

> Hooks live in `.githooks/` (version-controlled), not the un-tracked
> `.git/hooks/`. Everyone gets the same gates with zero setup.

---

## Three hooks, three checkpoints

```sh
# pre-commit  — fast feedback
mvn -q test                       # compile + unit tests
mvn -q spotless:check             # is it formatted?  (make format to fix)
# + if a DB changelog is staged → check it applies cleanly on a fresh DB

# commit-msg  — enforce Conventional Commits
^(feat|fix|docs|refactor|perf|test|build|ci|chore|revert)(\(scope\))?!?: ...

# pre-push    — the heavier gate before sharing
mvn -q spotbugs:check
mvn -q verify                     # integration tests + coverage + ArchUnit
```

Escape hatch for emergencies: `git push --no-verify`.

---

## The Makefile as the developer interface

One vocabulary for humans *and* agents — `AGENTS.md` points here:

```bash
make build            # mvn clean package -DskipTests
make test             # unit tests
make coverage         # mvn verify + jacoco report
make format           # spotless:apply  (auto-fix style)
make spotbugs         # spotbugs:check
make liquibase/check  # fresh container → update → validate → teardown
```

> Discoverable, repeatable commands beat tribal knowledge — and an agent
> can read the `Makefile` to learn how to build and test the repo itself.

{{% /section %}}
