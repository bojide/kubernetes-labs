# Lab 15 – ConfigMaps, Secrets, and the Downward API

## Overview

This lab demonstrates how Kubernetes manages application configuration and sensitive information using **ConfigMaps** and **Secrets**, and how the **Downward API** exposes Pod metadata to running containers.

During this lab I:

- Created a dedicated namespace
- Created and inspected a ConfigMap
- Created and verified a Secret
- Injected configuration as environment variables
- Mounted ConfigMaps and Secrets as files
- Used the Downward API to expose Pod metadata
- Verified mounted files and environment variables inside running Pods

---

# Prerequisites

- Kubernetes cluster
- kubectl configured
- Namespace isolation

---

# Step 1 — Create Namespace

Created a dedicated namespace for the lab.

```bash
kubectl create namespace cm-secrets-lab
kubectl get namespace cm-secrets-lab
```

![Namespace Created](screenshots-lab15/lab15-01-namespace-created.jpg)

---

# Step 2 — Create ConfigMap

Applied the ConfigMap manifest containing application configuration.

```bash
kubectl apply -f 01-configmap.yaml
```

Verified the ConfigMap.

![ConfigMap Created](screenshots-lab15/lab15-02-configmap-created.jpg)

---

# Step 3 — Create Secret

Applied the Secret manifest.

```bash
kubectl apply -f 02-secret.yaml
```

![Secret Created](screenshots-lab15/lab15-03-secret-created.jpg)

---

## Verify Kubernetes Base64 Encoding

Retrieved the Secret to verify that Kubernetes stores Secret data in the **data** field using Base64 encoding.

![Secret stringData to data](screenshots-lab15/lab15-04-secret-stringdata-to-data.jpg)

---

# Step 4 — Environment Variable Injection

Created a Pod that imports ConfigMap and Secret values as environment variables.

```bash
kubectl apply -f 03-pod-env-injection.yaml
```

![Environment Injection Pod Created](screenshots-lab15/lab15-05-env-injection-pod-created.jpg)

Verified the Pod reached the Running state.

![Environment Injection Pod Running](screenshots-lab15/lab15-05-env-injection-pod-running.jpg)

Entered the running container.

```bash
kubectl exec -it env-injection-pod -n cm-secrets-lab -- sh
```

![Enter Container](screenshots-lab15/lab15-06-enter-container.jpg)

Verified that ConfigMap values were injected successfully.

![ConfigMap Files](screenshots-lab15/lab15-07-configmap-files.jpg)

Verified the LOG_LEVEL value.

![LOG_LEVEL](screenshots-lab15/lab15-08-log-level.jpg)

---

# Step 5 — Volume Mounts

Created a Pod that mounts ConfigMaps and Secrets as files.

```bash
kubectl apply -f 04-pod-volume-mount.yaml
```

Verified the Pod reached the Running state.

![Volume Mount Pod Running](screenshots-lab15/lab15-08-volume-mount-pod-running.jpg)

Verified the mounted **app.properties** file.

![Application Properties](screenshots-lab15/lab15-09-app-properties.jpg)

Verified mounted Secret files.

![Secret Files](screenshots-lab15/lab15-10-secret-files.jpg)

Verified Secret volume symbolic links and permissions.

![Secret Volume](screenshots-lab15/lab15-11-secret-volume.jpg)

---

# Step 6 — Downward API

Verified Pod metadata exposed through the Downward API.

Checked:

- Pod name
- Namespace
- Labels
- Memory limit

![Downward API](screenshots-lab15/lab15-11-downward-api.jpg)

---

# Step 7 — Exit the Pod

Exited the running container after completing verification.

![Exit Pod](screenshots-lab15/lab15-12-exit-pod.jpg)

---

# Key Concepts Learned

## ConfigMaps

- Store non-sensitive configuration
- Can be consumed as environment variables
- Can also be mounted as files

## Secrets

- Store sensitive information
- Kubernetes stores them Base64 encoded
- Mounted files are automatically decoded inside the Pod

## Downward API

Allows applications running inside a Pod to discover:

- Pod name
- Namespace
- Labels
- Resource limits

without hardcoding those values.

---

# Files Included

- 01-configmap.yaml
- 02-secret.yaml
- 03-pod-env-injection.yaml
- 04-pod-volume-mount.yaml

---

# Cleanup

```bash
kubectl delete namespace cm-secrets-lab
```

---

# Skills Demonstrated

- Kubernetes ConfigMaps
- Kubernetes Secrets
- Environment Variable Injection
- Volume Mounts
- Secret Management
- Downward API
- Pod Metadata
- kubectl
- YAML

# Outcome

By completing this lab, I successfully:

- Created and managed Kubernetes ConfigMaps for application configuration.
- Created Secrets and verified how Kubernetes stores them using Base64 encoding.
- Injected ConfigMap and Secret values into Pods as environment variables.
- Mounted ConfigMaps and Secrets as files inside running Pods.
- Used the Downward API to expose Pod metadata to applications.
- Verified the differences between environment variable injection and volume-mounted configuration.
- Gained practical experience managing application configuration using Kubernetes best practices.

# Outcome

By completing this lab, I successfully:

- Created and managed Kubernetes ConfigMaps for application configuration.
- Created Secrets and verified how Kubernetes stores them using Base64 encoding.
- Injected ConfigMap and Secret values into Pods as environment variables.
- Mounted ConfigMaps and Secrets as files inside running Pods.
- Used the Downward API to expose Pod metadata to applications.
- Verified the differences between environment variable injection and volume-mounted configuration.
- Gained practical experience managing application configuration using Kubernetes best practices.

# Created By

## Babajide Ajisafe

Cloud | DevOps | Kubernetes

GitHub:
https://github.com/bojide

LinkedIn:
https://linkedin.com/in/babajide-ajisafe

---

Passionate about designing, automating, and managing scalable cloud-native infrastructure using Kubernetes, Docker, Terraform, AWS, and modern DevOps practices.
