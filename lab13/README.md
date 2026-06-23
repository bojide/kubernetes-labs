# Lab 13 – Labels, Selectors, Annotations, and Custom Resource Definitions (CRDs)

## Objective

The goal of this lab was to understand how Kubernetes uses labels and selectors to organize, identify, and manage resources, and to learn how the Kubernetes API can be extended using Custom Resource Definitions (CRDs).

---

# Part 1 – Labels

Labels are key-value pairs attached to Kubernetes objects.

Example:

```yaml
labels:
  app: web-server
  env: production
  tier: frontend
```

Labels are stored under:

```yaml
metadata:
  labels:
```

Labels provide a way to categorize and identify resources.

Examples:

* app=web-server
* env=production
* tier=frontend
* lab=13-api-objects

---

# Part 2 – Equality-Based Selectors

Selectors allow Kubernetes to find resources based on labels.

Examples:

```bash
kubectl get pods -l env=production
kubectl get pods -l tier=frontend
kubectl get pods -l app=web-server
```

Multiple selectors can be combined using commas.

Example:

```bash
kubectl get pods -l env=production,tier=frontend
```

A comma represents AND logic.

Meaning:

```text
env=production AND tier=frontend
```

---

# Part 3 – Set-Based Selectors

Set-based selectors provide more advanced filtering.

Examples:

```bash
kubectl get pods -l 'env in (production,staging)'
kubectl get pods -l 'tier notin (frontend)'
kubectl get pods -l 'tier'
```

Useful operators:

* in
* notin
* exists

These selectors are heavily used by Deployments, ReplicaSets, Services, and NetworkPolicies.

---

# Part 4 – Viewing Labels

Labels can be displayed using:

```bash
kubectl get pods --show-labels
```

This command allows verification of labels attached to resources.

---

# Part 5 – Managing Labels Imperatively

Labels can be added, modified, or removed from running resources.

Add:

```bash
kubectl label pod pod-frontend-prod version=v2
```

Modify:

```bash
kubectl label pod pod-frontend-prod env=prod-v2 --overwrite
```

Remove:

```bash
kubectl label pod pod-frontend-prod env-
```

Key Learning:

Changing labels immediately affects selector results.

---

# Part 6 – Annotations

Annotations are metadata used for documentation and tooling.

Example:

```bash
kubectl annotate pod pod-frontend-prod deployment-tool=kubectl
```

Difference:

Labels:

* Used for selection and grouping.

Annotations:

* Used for informational metadata.
* Not used by selectors.

---

# Part 7 – Custom Resource Definitions (CRDs)

A CRD extends the Kubernetes API with a new resource type.

Built-in resources:

* Pod
* Deployment
* Service
* ConfigMap

Custom resource created in this lab:

```yaml
kind: Greeting
```

After installing the CRD, Kubernetes recognized a new API resource:

```text
Greeting
```

This demonstrated how Kubernetes can be extended beyond its built-in object types.

---

# Part 8 – Creating Custom Resources

Created:

```text
hello-world
hola-mundo
```

Resource Example:

```yaml
apiVersion: training.example.com/v1
kind: Greeting
```

These objects behaved similarly to native Kubernetes resources.

Examples:

```bash
kubectl get greetings
kubectl get gt
kubectl describe greeting hello-world
```

---

# Part 9 – Schema Validation

The Greeting CRD included validation rules.

Valid languages:

* english
* spanish
* french
* yoruba
* swahili

Attempting to create:

```yaml
language: klingon
```

resulted in:

```text
Unsupported value: "klingon"
```

Key Learning:

CRDs support API-level validation using OpenAPI schemas.

---

# Part 10 – Labels on Custom Resources

Labels work on custom resources exactly as they do on Pods.

Example:

```bash
kubectl label greeting hola-mundo language=spanish
```

Selectors can then be used:

```bash
kubectl get greetings -l language=spanish
```

This demonstrates that labels are a universal Kubernetes concept.

---

# Key Takeaways

By completing this lab I learned:

* How labels identify Kubernetes objects.
* How selectors locate resources using labels.
* Difference between equality-based and set-based selectors.
* How annotations differ from labels.
* How to add, modify, and remove labels.
* How Kubernetes APIs can be extended using CRDs.
* How to create and manage custom resources.
* How schema validation protects API integrity.
* How labels and selectors work on both native and custom resources.

---

# Skills Practiced

* kubectl
* Labels
* Selectors
* Annotations
* CRDs
* Custom Resources
* OpenAPI Validation
* Resource Discovery
* Kubernetes API Extension

---

# Outcome

Successfully deployed and managed labeled Pods, created a Custom Resource Definition (CRD), created multiple Greeting custom resources, validated schema enforcement, and used selectors across both native and custom Kubernetes objects.


Created By:

**Babajide Ajisafe**
Cloud | DevOps | Kubernetes Engineer

GitHub: https://github.com/bojide
Email: jideajisafe@gmail.com

Passionate about designing, automating, and managing scalable cloud-native infrastructure using modern DevOps and Kubernetes technologies.
