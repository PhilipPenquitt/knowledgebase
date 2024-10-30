# Abhängigkeiten

Die Abhängigkeiten zu den Modulen wird in der sogenannten go.mod festgehalten.
In dieser Datei wird das Modul als vollqualifizierte URL mit der Version
hinterlegt. Ggf. wenn nur ein Paket importiert wurde sogar noch das Package

```yaml
github.com/you/project/v2/cmd
```

Wenn das Modul bezogen wird, wird das im GOPATH unter pkg abgelegt.

## Nützliche Befehle

In der go.mod werden die Informationen zu den Modulen abgelegt. Dazu existiert
auch eine go.sum Datei. Diese enthält die hash werte zu unseren Abhängigkeiten.
Das dient der Prüfung der Integrität und um Versionen zu vergleichen.

```bash
go mod init <package> # erstellt mein Modul also go.mod
go mod tidy # löscht nicht verwendete Abhängigkeiten und die go.sumdb
go mod vendor # man kann alle abhängigkeiten in das aktuelle Projekt geladen. In ein "vendor" Verzeichnis
go mod verify # verifiziert die Packete nach sumdb ignoriert aber vendor
```

Ein vendor Verzeichnis hat immer Vorrang vor den anderen Pfaden. Es muss die
`go.mod` nicht angepasst werden. Allerdings nur beim Import und Ausführungen,
`go mod verify` ignoriert es.
