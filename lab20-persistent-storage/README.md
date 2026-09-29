# Lab 20 — Persistent Storage on k3s

## Overview

This lab tests data persistence across Pod deletion and StatefulSet scale-down on a five-node k3s cluster. The cluster uses the `local-path` StorageClass, rather than the `standard` class in the GKE exercise.

## Environment

- Namespace: `storage-lab`
- StorageClass: `local-path`
- Provisioner: `rancher.io/local-path`
- Binding mode: `WaitForFirstConsumer`
- Reclaim policy: `Delete`
- Access mode: `ReadWriteOnce`
- Request: 1 GiB per PVC

`local-path` stores each volume on a specific node. This lab proves persistence across Pod replacement; it does not test recovery if that node is lost.

## Writer and reader

`app-data-pvc` was initially `Pending` until `writer-pod` requested it. The PVC then became `Bound`. The writer saved `/data/message.txt`. After the writer was deleted, `reader-pod` mounted the same PVC and read the original message dated `2026-09-25T17:51:49Z`.

## StatefulSet

`volumeClaimTemplates` created separate PVCs named `data-db-store-0`, `data-db-store-1`, and `data-db-store-2`. Scaling the StatefulSet to zero removed its Pods while the three PVCs stayed `Bound`.

![PVCs retained after scale-down](screenshots-lab20/01-statefulset-pvcs-retained-after-scale-down.jpg)

After scaling back to three replicas, each Pod read its original identity file. The stored timestamps remained `17:59:54Z`, `18:00:01Z`, and `18:00:10Z`.

![Original data after scale-up](screenshots-lab20/02-statefulset-data-survived-scale-up.jpg)

## Lifecycle lesson

Deleting a Pod leaves its PVC intact. The observed reclaim policy is `Delete`, so deleting a PVC triggers removal of its PV and local data. Clean up the PVCs only after saving the lab evidence.

## Writer and reader evidence

![Writer data survived pod deletion](screenshots-lab20/03-writer-data-survived-pod-deletion.jpg)
