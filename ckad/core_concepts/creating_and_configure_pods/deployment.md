---
tags:
  - CKAD
  - Kubernetes
  - Deployment
---

# Deployment

Deployment sind einer weitere Abstraktion aufbauend auf den bisherigen. Sie
nutzen Pods und Replicasets.

## Aufbau der Yaml Definition

In Kubernetes erweitert ein Manifest das andere. So ist es auch hier. eine
Deployment Definition ist eine Hülle in die das Template von einem Pod gelegt
wird.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  template:
    metadata:
      labels:
        run: nginx
    name: nginx
    containers:
      - image: nginx
        name: nginx
    dnsPolicy: ClusterFirst
    restartPolicy: Always
  replicas: 3
  selector:
    matchLabels:
      run: nginx
```

Als Erweiterung der Pod Definitin benötigt das Deployment in seiner Spec eine
Zahl für die replicas. Hier sind das 3, sowie eine selector Definition mit
matchlabels damit das ReplicaSet die Pods erkennt welche es zu steuern hat.

## Kommandos

```bash
kubectl create -f deployment_definition.yml
kubectl get deployments
kubectl delete deployment <name> # löscht sofort die Pods
kubectl replace -f replica_definition # um die Definition anzupassen
kubectl scale --replicas=2 replicaset <name> # erhöht die Pod anzahl, ändert aber nicht das Manifest im Kubernetes!
kubectl scale -replicas=6 -f replica_definition.yml # erhöht die Pod anzahl, ändert aber nicht das Manifest im Kubernetes!
```
