# Import

Aufbau eines Imports:

```go
import alias "github.com/super/wichtig"
```

Hier wird ein Paket importiert und ein anderen Namen gegebe

```go
import . "github.com/log"
```

Damit kann ich auf Funktionen des packages zugreifen ohne Prefix. Also stat

```go
import . "github.com/log"
log.Println("irgendwas")
Println("geht auch") // Die Funktion kommt aber auch aus dem log Package
```

Aber keine best practice.
