---
tags:
  - mms
  - minikube
---

# Minikube installation

[Download][Download] Hier kann man ein Binary herunterladen oder es via Homebrew installieren.
Ich habe mich für die Variante mit Homebrew entschieden.

```bash
brew install minikube
```

If which minikube fails after installation via brew, you may have to remove the old minikube links and link the newly installed binary:

```bash
brew unlink minikube
brew link minikube
```

[Download]: https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fhomebrew
