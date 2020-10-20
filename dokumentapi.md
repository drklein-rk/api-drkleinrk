---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Dokument Import API

## Beschreibung
Die Dokumente Import API ermöglicht das Importieren von maximal 10 MB großen Dokumenten zu einen bestehenden Lead.

## Dokumente Import API

### Dokumente importieren

Die Schnittstelle ermöglicht das Importieren von Dokumenten zu einem bestehenden Lead.

Neue Dokumente werden per `HTTP POST` angelegt. Die URL für das Anlegen von Dokumenten ist:

`https://api.drkleinservice.de/lead/{leadId}/dokument`

Die Daten werden als JSON Dokument im Body des `POST` Requests übermittelt. 
Ein erfolgreicher Aufruf resultiert in einer Response mit dem HTTP Statuscode `201 CREATED`.

### Authentifizierung

Die Authentifizierung erfolgt über ein API JWT Token.

```
Request-Header Name: X-Authentication
Request Header Value: (jwt wird von Dr. Klein Support bereitgestellt)
```
`401`, wenn die Authentifizierung fehlschlägt.

### Dokument

Dateityp: `.pdf`

maximale Dateigröße: `10MB`

**Dokument Daten**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| filename | String | Der Dateiname.
| base64Content | String | base64 kodierte Datei.


### Beispiel

```text
Request-Header Name: X-Authentication
Request Header Value: xxx

Request-Header Name: content-type
Request Header Value: application/json;charset=utf-8

POST
https://api.drkleinservice.de/lead/{leadId}/dokument
```

```json
{
  "filename": "Example.pdf", 
  "base64Content": "JVBERXXX"
}
```
