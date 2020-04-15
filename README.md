# partner-middleware-api

Schnittstellenbeschreibung Dr. Klein RK GmbH für den Partner-Middleware Service

## Beschreibung

Der Partner-Middleware Service dient zum Anlegen eines neuen Partners im Partnermanagement von Europace, wenn dieser noch nicht existiert. Mithilfe der PartnerId vom neuen oder bereits existierenden Partner wird ein Vorgang in Kreditsmart angelegt.

## Schnittstelle

Die Schnittstelle des Partner-Middleware Service basiert auf der Schnittstelle von Europace, um einen Vorgang anzulegen (https://developer.europace.de/api/kex-vorgang-import-api/) und wurde um fehlenden Werte ergänzt.

**Tippgeber Werte**
```
"Tippgeber": {
  "vorname": "Sparkassen Max",
  "nachname": "Sparkassen Mustermann",
  "anrede": "Herr",
  "telefon": "0612-298-xxxx",
  "email": "sparkassenmax.mustermann@spkrnn.de"
},
```
Zudem wurde die Schnittstelle um die Kommentar Funktion und die Dokumente Funktion von Europace (https://developer.europace.de/api/kex-vorgang-update-api/) ergänzt.

**Kommentar Wert**
```
"kommentare": ["hallo ECON"]
```
**Dokumente Wert**
```
"dokumente": [{"filename": "Example.pdf", "base64Content": "JVBERXXX"}]
```

## Anlegen eines neuen Vorgangs

Neue Vorgänge werden per HTTP POST angelegt. Die URL für das Anlegen von einen Vorgang im Echtgeschäft ist:
https://partnermiddleware.drkleinservice.de/vorgang?environment=PRODUCTION

Die URL für das Anlegen von Vorgängen in der Testumgebung ist:
https://partnermiddleware.drkleinservice.de/vorgang

Die Daten werden als JSON Dokument im Body des POST Requests übermittelt. Ein erfolgreicher Aufruf resultiert in einer Response mit dem HTTP Statuscode 201 CREATED.

### Authentifizierung

Die Authentifizierung erfolgt über ein API JWT Token.

```
Request-Header Name: X-Authentication
Request Header Value: (jwt wird von Dr. Klein Support ausgestellt)
```
401, wenn die Authentifizierung fehlschlägt.

### Beispiel

```
Request-Header Name: X-Authentication
Request Header Value: xxx

Request-Header Name: content-type
Request Header Value: application/json;charset=utf-8

POST
http://partnermiddleware.drkleinservice.de/vorgang

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
    "email": "sparkassenmax.mustermann@spkrnn.de"
  },
  "kommentare": ["hallo ECON"],
  "dokumente": [{"filename": "Example.pdf", "base64Content": "JVBERXXX"}]
}
```
Der Bereich *kundenbetreuer* und *antragsteller1* ist die gleiche API wie die von Europace. Neu ist der *tippgeber* Bereich, die *kommentare* und die *dokumente*.

Der *kundenbetreuer* und der *antragsteller1* werden über die Kreditsmart KEX-Import API importiert, die *tippgeber* Daten werden für die Partnererstellung im Partnermanagment verwendet und die *kommentare*, um in der Ereignislasche die Checkboxen AGB und Datenschutzbestimmungen, sowie weitere Kommentare zu importieren. Die Dokumente um in Europace Vorgang Dokumente zu importieren.
