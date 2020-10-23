---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Vorgang Import API
description:

# Micro navigation
micro_nav: true

---
## Beschreibung
Die Lead API dient zum Überleiten von Partner-, Tippgeber und Vorgangsdaten.

## Lead API

### Lead importieren

Die Schnittstelle zum Vorgang import basiert auf den Import von Daten der [Schnittstelle von Europace](`https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/`).
Das Format und die Struktur wurde für die Vorgangsdaten übernommen und um Partner-, Tippgeber- und Customdaten, sowie individuellen Kommentaren ergänzt.

Neue Vorgang werden per `HTTP POST` angelegt. Die URL für das Anlegen von einen Lead im Echtgeschäft ist:

`https://api.drkleinservice.de/vorgang?environment=PRODUCTION`

Die URL für das Anlegen von einen Vorgang in der Testumgebung ist:
`https://api.drkleinservice.de/vorgang`

Die Daten werden als JSON Dokument im Body des `POST` Requests übermittelt. 
Ein erfolgreicher Aufruf resultiert in einer Response mit dem HTTP Statuscode `201 CREATED`.

### Authentifizierung

Die Authentifizierung erfolgt über ein API JWT Token.

```
Request-Header Name: X-Authentication
Request Header Value: (jwt wird von Dr. Klein Support bereitgestellt)
```
`401`, wenn die Authentifizierung fehlschlägt.

### Lead

**Partner Daten**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| vertriebsschlüssel | String | Der Vertriebsschlüssel des Partners. |

**Custom Daten**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| key | String | Der name vom `key`. |
| value | various types| Der `key` kann den Datentyp `boolean`, `integer`, `date`, `string` haben. |

**Tippgeber Daten**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| externeMitarbeiternummer | String | Die Mitarbeiternummer des Tippgebers. |
| vorname | String | Der Vorname des Tippgebers. |
| nachname | String | Der Nachname des Tippgebers. |
| anrede | String | Die Anrede des Tippgebers. Werte sind HERR oder FRAU. |
| telefon | String | Die Telefonnummer des Tippgebers. |
| email | String | Die Email des Tippgebers. |

**Kommentare**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| kommentare | [String] | Kommentare zum Lead. |

### Beispiel

**Request**

```text
Request-Header Name: X-Authentication
Request Header Value: xxx

Request-Header Name: content-type
Request Header Value: application/json;charset=utf-8

POST
https://api.drkleinservice.de/lead
```

```json
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
    "email": "max.mustermann@partner.de"
  },
  "data": [
    {
      "key": "einwilligungUnterschrieben",
      "value": true
    },{
      "key": "organisationsNummer",
      "value": "123456-121"
    }
  ],
  "kommentare": ["Max Mustermann hat 3 Kreditkarten."]
}
```

**Response**
```json
{
  "vorgangsId": "6e8627d4-1512-11eb-adc1-0242ac120002",
  "europaceVorgangsnummer":  "N94L7T",
  "messages": []
}
```

Der Bereich `kundenbetreuer` und `antragsteller1` ist genauso wie die [API von Europace](`https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/`)  aufgebaut. Neu ist der `tippgeber`, `data` und `kommentare` Bereich.

Die Daten vom `kundenbetreuer` und vom `antragsteller1` werden 
über die von Europace bereitgestellte API [KEX Vorgang Import API](`https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/`) an Kreditsmart übergeben. 
Die `tippgeber` Daten werden für die Partnererstellung im Partnermanagment verwendet und die `kommentare`, um in der Ereignislasche von Kreditsmart den Finanzierungsberater weitere Informationen anzuzeigen.

| Begriff | Beschreibung |
| :------ | :----------- |
| Kreditsmart | Frontend für die Kreditberatung. |
| Kreditsmart-Ereignislasche | Frontend Bereich in dem Kommentare angezeigt werden. |
| Partnermanagement | Benutzerverwaltung von Europace. |

