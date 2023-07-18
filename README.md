# oereb-web-service-docker

Use the following env varaibles to setup the service

- DBURL
- DBUSR
- DBPWD
- DBSCHEMA 
- TMPDIR
- MININTERSECTION
- TZ

```
docker run -p8080:8080 -e TZ=Europe/Zurich -e MININTERSECTION=0.1 -e TMPDIR=/tmp -e DBURL=jdbc:postgresql://host.docker.internal:54321/grundstuecksinformation -e DBUSR=gretl -e DBPWD=gretl -e DBSCHEMA=live sogis/oereb-web-service
```

## Github Action

Es gibt sowas wie einen Dev-Build-Job, der es ermöglicht, die _pdf4oereb_-Version selber zu wählen und nicht die mit dem Fatjar gelieferte Version zu verwenden. Dazu muss im _build-dev.gradle_ die gewünschte Version explizit gesetzt werden (momentan `2.0.+`). Anschliessend muss der Workflow manuell ausgeführt werden und es muss für den Wert `build.gradle environment` "dev" gewählt werden. Alles andere führt den normalen Workflow aus.

(Ist sicher verbesserungswürdig...)


