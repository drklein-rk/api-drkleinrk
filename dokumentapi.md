---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Dokument Import API
description:

# Micro navigation
micro_nav: true

---
## Beschreibung
Die Dokument Import API ermöglicht das Importieren von einen maximal 10 MB großen Dokument zu einen bestehenden Vorgang.

## Dokument Import API

### Dokument importieren

Die Schnittstelle ermöglicht das Importieren von einen Dokument zu einem bestehenden Vorgang.

Ein neues Dokumente wird per `HTTP POST` angelegt. Die URL für das Anlegen von einen Dokument ist:

`https://api.drkleinservice.de/lead/{vorgangId}/dokument`

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
| filename | String | Der Name des Dokuments.
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
