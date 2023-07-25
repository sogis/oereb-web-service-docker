# oereb-web-service-docker

## Konfigurieren und Starten

| Name | Beschreibung | Standard |
|-----|-----|-----|
| `DBURL` | JDBC-Connection-String für PostgreSQL-Datenbank. |  |
| `DBUSR` | Benutzerlogin für Datenbank. |  |
| `DBPWD` | Passwort für Datenbank. | |
| `TMPDIR` | Verzeichnis zum Speichern von temporären Dateien. |  |
| `MININTERSECTION` | Länge- und Flächengrösse, die beim Verschneiden von Linien und Flächen ignoriert wird. |  |
| `TZ` | Timezone: Damit die korrekte Uhrzeit im Auszug verwendet wird. |  |
| `DPI` | QGIS-DPI-Parameter, der für WMS-Requests verwendet wird. | `200` |
| `CADASTRE_AUTHORITY_URL` | URL, der Katasterstelle. Die URL wird verwendet, um aus der DB die restlichen Infos zum Amt zu requesten. | `https://agi.so.ch` |
| `PLAN_FOR_LANDREGISTER` | WMS-URL für Grundbuchplan-WMS (ohne ausgefüllte Hüsli...). | `https://geo.so.ch/api/wms?SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&FORMAT=image%2Fpng&TRANSPARENT=true&LAYERS=ch.so.agi.grundbuchplan_oereb&STYLES=&SRS=EPSG%3A2056&CRS=EPSG%3A2056&TILED=false&DPI=96&OPACITIES=255&t=675&WIDTH=1920&HEIGHT=710&BBOX=2607051.2375,1228517.0374999999,2608067.2375,1228892.7458333333` |
| `PLAN_FOR_LANDREGISTER_MAIN_PAGE` | WMS-URL für Grundbuchplan-WMS | `https://geo.so.ch/api/wms?SERVICE=WMS&VERSION=1.3.0&REQUEST=GetMap&FORMAT=image%2Fpng&TRANSPARENT=true&LAYERS=ch.so.agi.hintergrundkarte_sw&STYLES=&SRS=EPSG%3A2056&CRS=EPSG%3A2056&TILED=false&DPI=96&OPACITIES=255&t=675&WIDTH=1920&HEIGHT=710&BBOX=2607051.2375,1228517.0374999999,2608067.2375,1228892.7458333333`  |
| `TOMCAT_THREADS_MAX` | Maximal gleichzeitig laufende Threads in der Anwendung (kompliziert...). | `200` |
| `TOMCAT_ACCEPT_COUNT` | ... | `100` |
| `TOMCAT_MAX_CONNECTIONS` | ... | `10000` |
| `HIKARI_MAX_POOL_SIZE` | Anzahl DB-Connections im DB-Connectionpool. Muss mit den zur Verfügung gestellten Ressourcen zusammenpassen (RAM, CPU) und maximal erlaubter Threads. Die Anwendung braucht pro Auszug circa 3 DB-Connections. D.h. Bei 10 Connection im Pool sollte `TOMCAT_THREADS_MAX` circa 3-4 sein. | `10` |
| `LOG_LEVEL_DB_CONNECTION_POOL` | Loglevel des DB-Connectionpool-Frameworks. | `info` |
| `LOG_LEVEL_DB_CONNECTION` | Loglevel von Spring JDBC-Template. | `info` |
| `LOG_LEVEL_FRAMEWORK` | Loglevel des von Spring Boot. | `info` |
| `LOG_LEVEL_OEREB` | Loglevel des eigenen Anwendungscodes. | `info` |


```
docker run -p8080:8080 -e TZ=Europe/Zurich -e MININTERSECTION=0.1 -e TMPDIR=/tmp -e DBURL=jdbc:postgresql://host.docker.internal:54321/grundstuecksinformation -e DBUSR=gretl -e DBPWD=gretl -e DBSCHEMA=live sogis/oereb-web-service
```

## Entwicklung

### Build

Es gibt drei GH Action Workflows:

- `ows-docker-prod`: Verwendet den deployten OEREB-Web-Service (Fatjar) und packt ihn in ein Dockeriamge.
- `ows-docker-pdf`: Verwendet das deployte Thinjar und überschreibt die pdf4oereb-Dependency-Version mit der im `build-pdf.gradle` angegebenen Version. Aktuell wir einfach `+` verwendet.
- `ows-docker-branch`: Buildet die Anwendung selber in einem Docker-Builder-Image. Es wird vom "edigonzales"-Repo ausgecheckt. Der Branch muss zwingend angeben werden.

Der "Prod"-Workflow tagged eine Patch-Version. Die beiden anderen nur maximal bis zur Minor-Version.