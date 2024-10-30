# UmgebungsVariablen

go env zu anzeigen
go env -w zum schreiben
go  env -u zum löschen

go nutzt zum Pakete runterladen einen Proxy

`proxy.golang.org, direct` --> es wird direkt der Proxy verwendet. Über die
Variable GOPRIVATE kann eine Liste angegeben werden welche Pakete nicht über
diesen Proxy zu laden.
