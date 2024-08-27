---
tags:
  - CKAD
  - Kubernetes
  - Pods
---

# Pods

Kurze Übsericht über das Pod Konzept

- ein Pod ist die kleinste Einheit in Kubernetes
- ein Pod kann mehrere Container enthalten
- Es gibt verschiedene Modi wie die Container zusammenarbeiten können
  - Sie können sich nur den Storage teilen, oder aber auch Netzwerk und weitere
    Ressourcen (RAM, ENV, Secrets, Namespace)
  - Der Sinn von Pods ist es, eine logische Kapsel um den Container und seine
    Abhängigkeiten zu schaffen. Keine händisches bemühen um Netzwerk zwischen
    den Container. Container und entsprechender helper werden wie eine Einheit
    gehandhabt.

Einfachste Methode einen Pod zu starten:

```bash
kubectl run nginx --image nginx
```

## Pods mit Yaml erstellen

Pods haben eine minimale Anforderungen gegen eine Yaml. Sie benötigen
`kind, apiVersion, metadata und spec`. Die werte welche unter all diesen
Elementen gesetzt werden dürfen sind definiert. Die definition kann mit
`kubectl explain ressource.ebene` eingesehen werden. Die Ausnahme hier bildet
nur `labels`. Unter diesem Element darf definiert werden was der Nutzer sich
wünscht.

![Minimum Yaml](/ckad/images/pods_minimum_yaml.png)

## Kommandos

```bash
kubectl get pods # zeigt die Pods im Namespace an (Restarts, Readyness, etc)
kubectl describe pod my-pod # zeigt detaillierte Probleme zum Pod, den Status usw.
```

## Pods bearbeiten

Mit dem folgenden Befehl kann man Pods im laufenden betrieb bearbeiten ohne das
Manifest zu verändern.

```bash
kubectl edit <pods|deploy|statefulset> podname # Pod kann im laufenden Betrieb bearbeitet werden
```

Allerdings lassen sich so nicht alle Felder ändern. Es sind nur folgende:

- spec.containers.image
- spec.initcontainers.image
- spec.activeDeadlineSeconds
- spec.tolerations
- spec.terminationGracePeriodSeconds

Es ist außerdem möglich anhand von bestehenden Ressourcen ein Manifest zu
erstellen

```bash
kubectl get pods <pod> -o yaml > pod.yaml # laufender pod
kubectl run nginx --image nginx --dry-run=client -o yaml > pod.yaml # erstellt einen Pod, aber als Dry run
# damit werden nicht so viele unnötige Felder in die Datei geschrieben
```
