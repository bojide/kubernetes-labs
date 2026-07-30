# Lab 15 — ConfigMaps, Secrets & the Downward API

## Overview

In this lab, I explored how Kubernetes separates application configuration from container images using **ConfigMaps**, **Secrets**, and the **Downward API**.

I learned how applications can consume configuration as environment variables or mounted files, how Kubernetes stores Secret data internally, and how the Downward API exposes Pod metadata to running containers.

---

## Objectives

- Create a dedicated Kubernetes namespace
- Create and inspect ConfigMaps
- Create and inspect Secrets
- Understand `stringData` vs `data`
- Inject ConfigMap values as environment variables
- Inject Secret values as environment variables
- Mount ConfigMaps as files
- Mount Secrets as files
- Expose Pod metadata using the Downward API
- Observe how ConfigMap updates affect mounted volumes versus environment variables

---

# Lab Architecture

```
Namespace
│
├── ConfigMap
│     ├── Environment Variables
│     └── Mounted Files
│
├── Secret
│     ├── Environment Variables
│     └── Mounted Files
│
└── Pod Metadata
      └── Downward API
```

---

# Technologies Used

- Kubernetes
- kubectl
- YAML
- ConfigMap
- Secret
- Downward API
- Linux Shell

---

# Project Files

```
lab15-configmaps-and-secrets/
│
├── 01-configmap.yaml
├── 02-secret.yaml
├── 03-pod-env-injection.yaml
├── 04-pod-volume-mount.yaml
├── 05-downwardapi.yaml
├── README.md
└── images/
```

---

# What I Learned

## ConfigMaps

ConfigMaps allow application configuration to be stored separately from container images.

In this lab I learned how to:

- Store configuration as key-value pairs
- Store complete configuration files
- Inject configuration into Pods
- Mount configuration as files

---

## Secrets

Secrets store sensitive information such as:

- Database usernames
- Passwords
- Connection strings
- API keys

I also learned the difference between:

### stringData

- Human-readable
- Used only when creating a Secret
- Kubernetes automatically converts it

### data

- Stored by Kubernetes
- Base64 encoded
- Returned whenever the Secret is retrieved

Although Secret values are Base64 encoded, this is **encoding—not encryption**.

---

## Environment Variable Injection

The first Pod demonstrated two methods of injecting configuration.

### envFrom

Imports every key from a ConfigMap or Secret.

Example:

```
LOG_LEVEL
APP_ENV
DB_HOST
DB_PORT
DB_USER
DB_PASSWORD
```

### valueFrom

Imports only selected keys.

Example:

```
LOGGING_LEVEL
DATABASE_PASSWORD
```

This approach gives much finer control over which values are exposed to the application.

---

## Mounted ConfigMap Files

Instead of environment variables, ConfigMaps can also be mounted as files.

Example:

```
/etc/config/

APP_ENV
DB_HOST
DB_PORT
LOG_LEVEL
app.properties
```

Applications can simply read these files during runtime.

---

## Mounted Secret Files

Secrets were also mounted as files.

Example:

```
/etc/secret/

DB_USER
DB_PASSWORD
DB_URL
```

Inside the container, Kubernetes automatically decoded the Base64 values before presenting them to the application.

---

## Downward API

The Downward API allows Kubernetes to expose Pod metadata to running containers without modifying the application.

Example files:

```
/etc/podinfo/

pod-name
namespace
labels
annotations
mem-limit
```

This allows applications to discover information about themselves while running.

---

# Live Update Behavior

One of the most important concepts demonstrated in this lab is how Kubernetes handles configuration updates.

## Mounted ConfigMap

When the ConfigMap changes:

- Mounted files update automatically
- Changes become visible after the kubelet synchronization interval

## Environment Variables

Environment variables do **not** update.

They are loaded only when the Pod starts.

Updating the ConfigMap requires recreating or restarting the Pod before new values become available.

---

# Key Takeaways

- ConfigMaps separate configuration from application code.
- Secrets manage sensitive configuration.
- `stringData` is write-only.
- Kubernetes stores Secret values in the `data` field.
- Secret values are Base64 encoded.
- Mounted ConfigMaps update automatically.
- Environment variables do not update after Pod creation.
- The Downward API exposes Pod metadata without hardcoding values.

---

# Screenshots

## Namespace

- lab15-01-create-namespace.jpg
- lab15-02-verify-namespace.jpg

## ConfigMap

- lab15-03-configmap-created.jpg
- lab15-04-configmap-description.jpg

## Secret

- lab15-05-secret-created.jpg

## Environment Variables

- lab15-06-enter-container.jpg
- lab15-07-environment-variables.jpg

## Volume Mounts

- lab15-08-configmap-files.jpg
- lab15-09-configmap-properties.jpg
- lab15-10-secret-files.jpg

## Downward API

- lab15-11-downward-api.jpg

## Exit

- lab15-12-exit-container.jpg

---

# Cleanup

```bash
kubectl delete namespace cm-secrets-lab
```

Verify:

```bash
kubectl get namespaces
```

The namespace **cm-secrets-lab** should no longer exist.

---

# Skills Demonstrated

- Kubernetes
- ConfigMaps
- Secrets
- Downward API
- Environment Variables
- Volume Mounts
- YAML
- Linux CLI
- kubectl
- Troubleshooting
