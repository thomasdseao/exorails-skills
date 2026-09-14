---
name: "Kubernetes through Exorails"
description: "Using the Exorails Kubernetes connector well: the read path from namespaces to pods to logs, how to diagnose a failing deployment, and the rules before scale, restart or delete_pod."
---

# Kubernetes through Exorails

Tools: `namespaces`, `pods`, `deployments`, `services`, `describe`, `logs`, `events`, and `scale`, `restart`, `delete_pod` when writes are allowed. Lists are capped (100 items by default).

## Diagnosing

1. `deployments` in the namespace: ready versus desired replicas.
2. `pods` with the deployment's labels: phase, restarts, age. A pod in `CrashLoopBackOff` or with many restarts is the one to look at.
3. `describe` the pod: conditions, last state, reason (`OOMKilled`, `ImagePullBackOff`, failed probes).
4. `logs` of the pod, and of the previous container when it restarted. Bounded: ask for the tail.
5. `events` in the namespace, sorted by time: scheduling failures, image pulls, probe failures.

Report what you found before proposing an action.

## Acting

`scale`, `restart` and `delete_pod` exist only when writes are allowed.

- `restart` rolls the deployment: pods are replaced one by one. Safe for stateless workloads; confirm for anything with a persistent volume.
- `scale` changes replicas. Scaling to zero stops the service: say so and wait for a yes.
- `delete_pod` removes one pod; a deployment recreates it. Never delete a pod of a StatefulSet without the user's confirmation.

Always name the namespace and the object you are about to act on.

## Do not

There is no `apply`, no `exec`, no secret reading through the connector, on purpose. Do not try to reach them through another tool.
