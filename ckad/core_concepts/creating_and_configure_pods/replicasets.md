---
tags:
  - CKAD
  - Kubernetes
  - Replicaset
  - Replica Controller
---

# Replica Set

Replica Sets werden verwendet um mehrere Instanzen von einem Pod zu erstellen.
Das kann genutzt werden um für eine Anwendung eine Hochverfügbarkeit zu
ermöglichen. Aber Replica Sets werden auch von Deployments und anderen
Kubernetes Objekten verwendet.

![Replica Set](/ckad/images/Replication%20Controller.png)

## Aufbau der Yaml Definition

In Kubernetes erweitert ein Manifest das andere. So ist es auch hier. eine
Replicaset Definition ist eine Hülle in die das Template von einem Pod gelegt
wird.

Hier eine Pod Definition

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
    - image: nginx
      name: nginx
      resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

Nun im Vergleich das Replica Set dazu. Es fällt auf, das nur der Kopf bis zur
spec unterschiedlich ist. Danach wird eine `template` Sektion eingeführt in die
das Pod Manifest liegt.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
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

Als Erweiterung der Pod Definitin benötigt das Replicaset in seiner Spec eine
Zahl für die replicas. Hier sind das 3

### Unterschiede Replication Controller und Replica Set

Im Gegensatz zum veralteten Replication Controller, benötigt das Replica Set
auch einen `selector` und darunter einen `matchLabels`. Hintergrund ist, das
Anhand dieser Labels die Anwendungen identifiziert werden die zu diesen
Replicaset gehören. Das bedeutet man kann mit der Änderung der Labels Pods im
Replicaset steuern, die nicht durch dieses erstellt wurden.

## Labels und Selector

Die Labels und Selectoren sind notwendig um die Pods zu erkennen welche vom
ReplicaSet kontrolliert werden sollen. Das ist deswegen notwendig, weil nur mit
diesem System das Skalieren ermöglicht werden kann. Mittels Labels weiß das
Replicaset genau, welche Pods es löschen soll, oder ob genügend vorhanden sind.

## Kommandos

```bash
kubectl create -f replica_definition.yml
kubectl get replicaset
kubectl delete replicaset <name> # löscht sofort die Pods
kubectl replace -f replica_definition # um die Definition anzupassen
kubectl scale --replicas=2 replicaset <name> # erhöht die Pod anzahl, ändert aber nicht das Manifest im Kubernetes!
kubectl scale -replicas=6 -f replica_definition.yml # erhöht die Pod anzahl, ändert aber nicht das Manifest im Kubernetes!
```
