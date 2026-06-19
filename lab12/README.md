# Lab 12: kubectl Essentials -  Kubernetes Lab 12

## Overview

This lab focused on mastering the Kubernetes command-line tool (**kubectl**) by deploying and managing an NGINX pod within a Kubernetes cluster.

The goal was to gain hands-on experience with resource creation, inspection, troubleshooting, log analysis, namespace management, port forwarding, and Kubernetes API exploration.

---

## Technologies Used

* Kubernetes
* kubectl
* NGINX
* YAML
* K3s Cluster

---

## Learning Objectives

* Deploy workloads using Kubernetes manifests
* Inspect and troubleshoot Kubernetes resources
* View and analyze container logs
* Execute commands inside running containers
* Access applications through port forwarding
* Work with namespaces and contexts
* Generate Kubernetes YAML manifests
* Explore Kubernetes API schemas

---

# Part 1: Pod Deployment

## Create the Pod

Applied a Kubernetes manifest to deploy an NGINX pod.

```bash
kubectl apply -f sample-nginx.yaml
```

## Verify Pod Status

```bash
kubectl get pods
kubectl get pods -o wide
```

**Skills Learned**

* Deploying workloads
* Verifying pod health
* Viewing scheduling information

---

# Part 2: Resource Inspection

## Inspect Pod Details

```bash
kubectl describe pod sample-nginx
```

## View Labels

```bash
kubectl get pods --show-labels
```

## Filter Resources by Label

```bash
kubectl get pods -l app=sample-nginx
```

**Skills Learned**

* Understanding pod metadata
* Reading events and conditions
* Identifying node placement

---

# Part 3: Output Formatting

## View Resources as YAML

```bash
kubectl get pod sample-nginx -o yaml
```

## View Resources as JSON

```bash
kubectl get pod sample-nginx -o json
```

## Extract Specific Fields Using JSONPath

### Pod Status

```bash
kubectl get pod sample-nginx -o jsonpath='{.status.phase}'
```

### Node Name

```bash
kubectl get pod sample-nginx -o jsonpath='{.spec.nodeName}'
```

### Container Image

```bash
kubectl get pod sample-nginx -o jsonpath='{.spec.containers[0].image}'
```

### Container Name

```bash
kubectl get pod sample-nginx -o jsonpath='{.spec.containers[*].name}'
```

### Pod IP Address

```bash
kubectl get pod sample-nginx -o jsonpath='{.status.podIP}{"\n"}'
```

**Skills Learned**

* Kubernetes object structure
* JSONPath querying
* Data extraction from API responses

---

# Part 4: Logs

## View Container Logs

```bash
kubectl logs sample-nginx
```

## View Recent Logs

```bash
kubectl logs sample-nginx --tail=20
```

## View Logs with Timestamps

```bash
kubectl logs sample-nginx --timestamps=true
```

**Skills Learned**

* Reading application logs
* Troubleshooting containers
* Monitoring application startup

---

# Part 5: Port Forwarding

## Create Local Tunnel

```bash
kubectl port-forward pod/sample-nginx 8080:80
```

## Test Connectivity

```bash
curl http://localhost:8080
```

Successfully accessed the NGINX welcome page through the Kubernetes API server.

**Skills Learned**

* Port forwarding
* Application testing
* Network connectivity validation

---

# Part 6: Running Commands Inside Containers

## Check NGINX Version

```bash
kubectl exec sample-nginx -- nginx -v
```

## View NGINX Configuration

```bash
kubectl exec sample-nginx -- cat /etc/nginx/nginx.conf
```

**Skills Learned**

* Container inspection
* Command execution inside pods
* Application configuration review

---

# Part 7: Namespace Management

## Create Namespace

```bash
kubectl create namespace lab12-practice
```

## Deploy into Namespace

```bash
kubectl apply -f sample-nginx.yaml -n lab12-practice
```

## Verify Resources

```bash
kubectl get pods -n lab12-practice
```

## Configure Default Namespace

```bash
kubectl config set-context --current --namespace=lab12-practice
```

**Skills Learned**

* Namespace isolation
* Multi-environment management
* Context configuration

---

# Part 8: Kubernetes API Discovery

## Explore Resource Documentation

```bash
kubectl explain pod
kubectl explain pod.spec
kubectl explain pod.status
```

**Skills Learned**

* Self-service Kubernetes documentation
* Resource schema exploration
* Understanding Kubernetes objects

---

# Part 9: Labels and Annotations

## View Existing Labels

```bash
kubectl get pod sample-nginx --show-labels
```

## Inspect Annotations

```bash
kubectl describe pod sample-nginx
```

**Skills Learned**

* Resource organization
* Metadata management
* Label-based filtering

---

# Key Concepts Learned

* Kubernetes Pods
* kubectl Administration
* YAML Manifests
* Labels and Annotations
* Namespaces
* Port Forwarding
* Logs and Troubleshooting
* JSONPath Queries
* Container Inspection
* Kubernetes API Exploration
* Context Management

---

# Outcome

Successfully deployed, inspected, managed, and troubleshot Kubernetes workloads using kubectl while gaining practical hands-on experience with the most commonly used Kubernetes operational commands.

This lab strengthened foundational Kubernetes administration skills required for DevOps, Cloud, Platform Engineering, and Site Reliability Engineering (SRE) roles.


Learning Objectives:

- Pods
- Labels
- Annotations
- Resource Requests
- Resource Limits
- Environment Variables
- kubectl commands

Author: Babajide Ajisafe
