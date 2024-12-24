
# Kubernetes Guide

This document provides an overview of Kubernetes concepts, configurations, and troubleshooting tips.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Container Failure Handling](#container-failure-handling)
3. [Affinity and Probes](#affinity-and-probes)
4. [Network Policies](#network-policies)
5. [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
6. [Common Troubleshooting Issues](#common-troubleshooting-issues)
7. [Ingress Overview](#ingress-overview)
8. [Basic Kubernetes Questions](#basic-kubernetes-questions)
9. [Intermediate Kubernetes Questions](#intermediate-kubernetes-questions)
10. [Advanced Kubernetes Scenarios](#advanced-kubernetes-scenarios)
11. [Custom Resources and Operators](#custom-resources-and-operators)
12. [Core Concepts and Architecture](#core-concepts-and-architecture)
13. [Scenario-Based Questions](#scenario-based-questions)

---

## Introduction

Kubernetes (K8s) is a powerful open-source system for automating deployment, scaling, and management of containerized applications.

More details can be found in this [Kubernetes Architecture Overview](https://medium.com/@himanshusangshetty/kubernetes-architecture-and-components-explained-e489e98db15d).

---

## Container Failure Handling

When a container fails, Kubernetes uses various self-healing mechanisms such as:
- **Kubelet** restarts containers based on pod policies.
- **Liveness Probes** detect unhealthy containers and restart them.
- **ReplicaSets** maintain the desired pod count.
- **Pod Eviction** ensures pods are rescheduled on healthy nodes.

Example liveness probe:
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 3
  periodSeconds: 5
```

---

## Affinity and Probes

- **Node Affinity** and **Pod Affinity** manage pod placement.
- **Liveness Probes** ensure containers remain healthy.
- **Readiness Probes** verify application readiness to serve traffic.

---

## Network Policies

Control pod communication using Network Policies:
- **Pod Selector** specifies targeted pods.
- **Ingress and Egress Rules** control inbound and outbound traffic.

---

## Role-Based Access Control (RBAC)

Kubernetes RBAC defines resource permissions:
- **Roles** for namespace-specific resources.
- **ClusterRoles** for cluster-wide resources.
- Use `RoleBinding` and `ClusterRoleBinding` for assignments.

Example:
```bash
kubectl create rolebinding example --role=my-role --user=user1 --namespace=my-namespace
```

---

## Common Troubleshooting Issues

1. **Image Issues** (`ErrImagePull` or `ImagePullBackOff`):
   - Verify image accessibility.
   - Add image pull secrets if needed.

2. **Resource Constraints** (`Insufficient CPU/Memory`):
   - Adjust resource limits in pod configurations.

3. **CrashLoopBackOff**:
   - Inspect container logs:
     ```bash
     kubectl logs <pod-name>
     ```

4. **Volume Mount Issues**:
   - Check PersistentVolumeClaims (PVCs) and mount paths.

---

## Ingress Overview

Ingress provides external access to services:
- **Path-based Routing**
- **Host-based Routing**
- **TLS Termination**

Example:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-app-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /app1
            pathType: Prefix
            backend:
              service:
                name: app1-service
                port:
                  number: 80
```

---

## Basic Kubernetes Questions

- **How to deploy a simple web app?**
  Create a Deployment and apply it:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: web-app
  spec:
    replicas: 3
    template:
      spec:
        containers:
          - name: web-app
            image: nginx:latest
  ```

---

## Intermediate Kubernetes Questions

- **How to ensure high availability?**
  Use multiple replicas and health checks.

- **How to scale deployments?**
  ```bash
  kubectl scale deployment my-app --replicas=5
  ```

---

## Advanced Kubernetes Scenarios

1. **Horizontal Pod Autoscaler**:
   Scale pods based on CPU utilization:
   ```bash
   kubectl autoscale deployment my-app --min=2 --max=10 --cpu-percent=50
   ```

2. **Pod Affinity Example**:
   Ensure pods run on specific nodes:
   ```yaml
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
         nodeSelectorTerms:
           - matchExpressions:
               - key: zone
                 operator: In
                 values:
                   - east
   ```

---

## Custom Resources and Operators

Operators automate management tasks using CRDs:
- Example CRD:
  ```yaml
  apiVersion: apiextensions.k8s.io/v1
  kind: CustomResourceDefinition
  metadata:
    name: mysqlclusters.example.com
  spec:
    names:
      kind: MySQLCluster
      plural: mysqlclusters
  ```

---

## Core Concepts and Architecture

- **Networking**: CNI plugins, Service types, Network Policies.
- **Storage**: PersistentVolumes (PVs), PersistentVolumeClaims (PVCs).
- **Security**: RBAC, Pod Security Policies, image vulnerability scans.

---

## Scenario-Based Questions

1. **Disaster Recovery**:
   - Backup cluster state using tools like Velero.
   - Restore using snapshots.

2. **Scaling Strategies**:
   - Horizontal scaling via HPA.
   - Vertical scaling by resizing pods or nodes.

---

For more YAML examples and detailed explanations, refer to the [full guide](#).
