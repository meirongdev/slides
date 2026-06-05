+++
title = "Guardrails at runtime"
weight = 90
outputs = ["Reveal"]
+++

{{% section %}}

## ☸️ Guardrails at runtime — Kubernetes

Static analysis stops bad *code*. The cluster stops bad *behavior*:

- **Liveness / readiness / startup probes** — don't route traffic to a pod that isn't ready; restart one that's stuck
- **Resource requests & limits** — cap blast radius; avoid noisy-neighbor & OOM cascades
- **Manifest validation** — `kubeconform` / schema checks in CI
- **Admission policies** — OPA/Gatekeeper or Kyverno reject non-compliant workloads at the door

> Same philosophy as ArchUnit: encode the rule once, fail loudly when it's broken.

---

## ☸️ Debugging on Kubernetes — the toolbox

When a guardrail trips, investigate fast:

```bash
kubectl get pods -o wide                 # status, restarts, node
kubectl describe pod <pod>               # events: OOMKilled, ImagePullBackOff…
kubectl logs <pod> -c <container> -f     # stream logs
kubectl logs <pod> --previous            # logs from the crashed container
kubectl exec -it <pod> -- sh             # shell inside a running pod
kubectl port-forward <pod> 8080:8080     # hit the service locally
kubectl debug <pod> --image=busybox \    # ephemeral container for
        --target=<container>             #   distroless images
```

> Read the **events** first — `CrashLoopBackOff`, `OOMKilled`, and
> `ImagePullBackOff` each point at a different fix.

---

## ☸️ Tightening the inner loop

Don't rebuild-push-redeploy by hand on every change:

- **Telepresence / mirrord** — run the service **locally** while it's
  wired into the live cluster: real dependencies, instant reload, your
  debugger attached — no image build per change.

> Faster feedback at *every* layer — IDE, build, hook, CI, cluster — is the
> whole point of guardrails.

{{% /section %}}
