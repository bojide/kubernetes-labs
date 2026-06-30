# Pod Lifecycle, Health Checks & Probes #

## Overview

This section demonstrates how Kubernetes assigns Quality of Service (QoS) classes based on CPU and memory requests and limits.

Three pods were deployed:

- BestEffort
- Burstable
- Guaranteed

---

## 1. BestEffort QoS

A BestEffort pod has **no CPU or memory requests or limits** configured.

### Command

```bash
kubectl describe pod qos-besteffort
```

### Result

![BestEffort QoS](images/qos-BestEffort.jpg)

### Observation

- QoS Class: **BestEffort**
- No resource requests
- No resource limits
- First pod to be evicted during memory pressure

---

## 2. Burstable QoS

Burstable pods define resource requests that are lower than their limits.

### Command

```bash
kubectl describe pod qos-burstable
```

### Result

![Burstable QoS](images/qos-burstable.png)

### Observation

- QoS Class: **Burstable**
- Resource requests configured
- Resource limits configured
- Higher priority than BestEffort

---

## 3. Guaranteed QoS

Guaranteed pods have identical CPU and memory requests and limits.

### Command

```bash
kubectl describe pod qos-guaranteed
```

### Result

![Guaranteed QoS](images/qos-guaranteed.png)

### Observation

- QoS Class: **Guaranteed**
- Requests equal limits
- Highest scheduling priority
- Last pod to be evicted
 ## What I Learned

- Learned how Kubernetes assigns QoS classes.
- Used `kubectl describe` to inspect pod resource allocation.
- Compared BestEffort, Burstable and Guaranteed pods.
- Understood Kubernetes eviction priority during memory pressure.

 Author: 
**Babajide Ajisafe** 
GitHub: https://github.com/bojide 
LinkedIn: https://linkedin.com/in/babajide-ajisafe

