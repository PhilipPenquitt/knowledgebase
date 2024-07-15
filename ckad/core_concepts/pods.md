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
  - Sie können sich nur den Storage teilen, oder aber auch Netzwerk und
    weitere Ressourcen (RAM, ENV, Secrets, Namespace)
  - Der Sinn von Pods ist es, eine logische Kapsel um den Container und seine
    Abhängigkeiten zu schaffen. Keine händisches bemühen um Netzwerk zwischen
    den Container. Container und entsprechender helper werden wie eine Einheit
    gehandhabt.

einfachste Methode einen Pod zu starten

```bash
kubectl run nginx --image nginx
```
