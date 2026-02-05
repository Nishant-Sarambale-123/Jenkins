Classic **Kubernetes / EKS interview scenario** 👌
Let’s answer it the way an interviewer expects: **clear reasons + how to verify**.

---

## Scenario

👉 Deployment replicas = **5**
👉 Only **3 pods are running**

This means **2 pods failed to schedule or failed to start**.

---

## Most common reasons (in order of probability)

### ✅ **1. Insufficient cluster resources (MOST COMMON)**

Nodes don’t have enough:

* CPU
* Memory

Check:

```bash
kubectl describe deployment <deploy-name>
kubectl describe pod <pending-pod>
```

You’ll see:

```
0/3 nodes are available: Insufficient cpu
```

✔ Fix:

* Add nodes (Cluster Autoscaler)
* Reduce resource requests

---

### ✅ **2. Pod stuck in Pending state**

Pods are created but **scheduler can’t place them**.

Check:

```bash
kubectl get pods
```

If status = `Pending` → scheduling issue.

---

### ✅ **3. NodeSelector / Affinity / Taints**

Pods restricted to certain nodes.

Examples:

* `nodeSelector`
* `nodeAffinity`
* Node taints without tolerations

Result:

* Only some nodes eligible

Check:

```bash
kubectl describe pod <pod-name>
```

---

### ✅ **4. Pod IP exhaustion (EKS VERY COMMON 🔥)**

Subnet doesn’t have enough free IPs.

Symptoms:

* Pod stuck in `ContainerCreating`
* Event:

```
failed to assign an IP address to container
```

Check:

```bash
kubectl logs -n kube-system aws-node
```

✔ Fix:

* Larger subnet
* Prefix delegation
* Add secondary CIDR

---

### ✅ **5. Image pull issues**

If pods crash during startup.

Status:

* `ImagePullBackOff`
* `ErrImagePull`

Check:

```bash
kubectl describe pod
```

---

### ✅ **6. CrashLoopBackOff**

Pods start then crash.

Only running pods are counted as **Running**.

Check logs:

```bash
kubectl logs <pod>
```

---

### ✅ **7. PodDisruptionBudget (PDB)**

PDB restricts number of pods running.

Example:

```yaml
minAvailable: 3
```

Kubernetes won’t bring all replicas up.

---

### ✅ **8. Quota limits**

Namespace-level quotas limit:

* Pods
* CPU
* Memory

Check:

```bash
kubectl get resourcequota
```

---

## Quick debugging flow (interview gold ⭐)

```bash
kubectl get pods
kubectl describe pod <non-running-pod>
kubectl get nodes
kubectl describe deployment <deploy>
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Interview-ready answer 🎯

> If a Deployment requests 5 replicas but only 3 are running, it usually means the remaining pods could not be scheduled or started. Common causes include insufficient node resources, node affinity or taints, pod IP exhaustion in EKS, image pull errors, CrashLoopBackOff, PodDisruptionBudgets, or namespace resource quotas.

---

## One-liner (senior-level 🔥)

> If replicas are not matching in a Deployment, always check pod events first—Kubernetes tells you exactly why.

If you want:

* 🧠 **EKS-specific follow-up questions**
* 🔥 **Real prod outage explanation**
* 🧪 **Mock interview drill**

Just say 👍
