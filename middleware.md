---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Partner Middleware API
description: API zum Anlegen eines neuen Partners bei Europace.

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: Previous page
        url: '#'
    next:
        content: Next page
        url: '#'
---
## Beschreibung

Der Partner-Middleware Service dient zum Anlegen eines neuen Partners im Partnermanagement von [Europace](https://neu.europace.de/), wenn dieser noch nicht existiert. Mithilfe der PartnerId vom neuen oder bereits existierenden Partner wird ein Vorgang in [Kreditsmart](https://www.finmas.de/kreditsmart/) angelegt.

## Schnittstelle

Die Schnittstelle des Partner-Middleware Service basiert auf der Schnittstelle von Europace, um einen Vorgang `https://developer.europace.de/api/kex-vorgang-import-api/` anzulegen und wurde um fehlenden Werte ergänzt.
Zusätzliche bietet die Schnittstelle den Import von Dokumenten an `https://developer.europace.de/api/kex-vorgang-update-api/`.

**Tippgeber Werte**
```
"Tippgeber": {
  "vorname": "Max",
  "nachname": "Mustermann",
  "anrede": "Herr",
  "telefon": "0612-298-xxxx",
  "email": "max.mustermann@email.de"
},
```
Zudem wurde die Schnittstelle um die Kommentar Funktion und die Dokumente Funktion von Europace `https://developer.europace.de/api/kex-vorgang-update-api/` ergänzt.

**Kommentar Wert**
```
"kommentare": ["hallo ECON"]
```

## Anlegen eines neuen Vorgangs

Neue Vorgänge werden per HTTP POST angelegt. Die URL für das Anlegen von einen Vorgang im Echtgeschäft ist:<br>
`https://partnermiddleware.drkleinservice.de/vorgang?environment=PRODUCTION`

Die URL für das Anlegen von Vorgängen in der Testumgebung ist:<br>
`https://partnermiddleware.drkleinservice.de/vorgang`

Die Daten werden als JSON Dokument im Body des POST Requests übermittelt. Ein erfolgreicher Aufruf resultiert in einer Response mit dem HTTP Statuscode 201 CREATED.

### Authentifizierung

Die Authentifizierung erfolgt über ein API JWT Token.

```
Request-Header Name: X-Authentication
Request Header Value: (jwt wird von Dr. Klein Support ausgestellt)
```
`401`, wenn die Authentifizierung fehlschlägt.

### Beispiel

```
Request-Header Name: X-Authentication
Request Header Value: xxx

Request-Header Name: content-type
Request Header Value: application/json;charset=utf-8

POST
https://partnermiddleware.drkleinservice.de/vorgang

{
  "kundenbetreuer": {
    "partnerId": "TEST"
  },
  "antragsteller1": {
    "personendaten": {
      "vorname": "Max",
      "nachname": "Mustermann"
     }
  },
  "tippgeber": {
    "vorname": "Max",
    "nachname": "Mustermann",
    "anrede": "HERR",
    "telefon": "0612-298-xxxx",
    "email": "max.mustermann@spkrnn.de"
  },
  "kommentare": ["hallo ECON"]
}
```
Der Bereich `kundenbetreuer` und `antragsteller1` ist die gleiche API wie die von Europace. Neu ist der `tippgeber` Bereich, die `kommentare`.

Der `kundenbetreuer` und der `antragsteller1` werden über die Kreditsmart KEX-Import API importiert, die `tippgeber` Daten werden für die Partnererstellung im Partnermanagment verwendet und die `kommentare`, um in der Ereignislasche die Checkboxen AGB und Datenschutzbestimmungen, sowie weitere Kommentare zu importieren.

## Hinzufügen von Dokumenten

Diese Schnittstelle ermöglicht das Importieren von Dokumenten zu einem bestehenden Vorgang.
Dokumente werden per HTTP POST importiert. Die URL für das importieren von Dokumenten ist:<br>
`https://partnermiddleware.drkleinservice.de/vorgang/{vorgangsnummer}/dokument`

Die Daten werden als JSON Dokument im Body des POST Requests übermittelt. Ein erfolgreicher Aufruf resultiert in einer Response mit dem HTTP Statuscode 201 CREATED.

### Authentifizierung

Die Authentifizierung erfolgt über ein API JWT Token.

```
Request-Header Name: X-Authentication
Request Header Value: (jwt wird von Dr. Klein Support ausgestellt)
```
401, wenn die Authentifizierung fehlschlägt.

### Beispiel

**Dokumente Wert**
```
Request-Header Name: X-Authentication
Request Header Value: xxx

Request-Header Name: content-type
Request Header Value: application/json;charset=utf-8

POST
https://partnermiddleware.drkleinservice.de/vorgang/{vorgangsnummer}/dokument

{
  "filename": "Example.pdf", 
  "base64Content": "JVBERXXX"
}
```
