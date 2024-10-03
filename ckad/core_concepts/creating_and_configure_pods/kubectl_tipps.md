---
tags:
  - CKAD
  - Kubernetes
  - Tipps
---

# Foramting mit Kubectl

Kubectl beherrscht verschiedene möglichkeiten des Ouputs.

```bash
kubectl [command] [TYPE] [NAME] -o <output_format>
```

1. -o json --> Als json Ausgabe
2. -o name --> Gibt nur den Namen der Ressource zurück
3. -o wide --> Gibt zusätzliche Informationen raus
4. -o yaml --> Als yaml Ausgabe

Hier sind einige Beispiele:

## Json

```bash
kubectl create namespace test-123 --dry-run -o json
{
    "kind": "Namespace",
    "apiVersion": "v1",
    "metadata": {
        "name": "test-123",
        "creationTimestamp": null
    },
    "spec": {},
    "status": {}
}
```

## Wide

```bash
master $ kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE     IP          NODE     NOMINATED NODE   READINESS GATES
busybox   1/1     Running   0          3m39s   10.36.0.2   node01   <none>           <none>
ningx     1/1     Running   0          7m32s   10.44.0.1   node03   <none>           <none>
redis     1/1     Running   0          3m59s   10.36.0.1   node01   <none>           <none>
master $
```

### Weitere Quellen

<https://kubernetes.io/docs/reference/kubectl/overview/>
<https://kubernetes.io/docs/reference/kubectl/cheatsheet/>
