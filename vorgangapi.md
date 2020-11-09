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
## Vorgang API

### Beschreibung
Die Vorgang API dient zum Überleiten von Partner-, Tippgeber und Vorgangsdaten.

### Vorgang importieren

Die Schnittstelle zum Vorgang Import basiert auf den Import von Daten der [Schnittstelle von Europace](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api){:target="_blank"}.
Das **Format** und die **Struktur** wurde für die **Vorgangsdaten** übernommen und um **Partner-**, **Tippgeber-** und **Customdaten**, sowie individuellen **Kommentaren** ergänzt.

Ein neuer Vorgang wird per `HTTP POST` angelegt. 

| Stage | Url |
| :---- | --- |
| production | `https://api.drkleinservice.de/vorgang?environment=PRODUCTION` |
| development | `https://api.drkleinservice.de/vorgang` |


Die Daten werden als JSON Dokument im Body des `POST` Requests übermittelt. 
Ein erfolgreicher Aufruf resultiert in einer Response mit dem HTTP Statuscode `201 CREATED`.

### Authentifizierung

Die Authentifizierung erfolgt über [OAuth2](https://oauth.net/2/){:target="_blank"}.

Um einen Client zu registrieren brauchen wir die folgenden Daten:

| Wert | Beschreibung |
| :--- | ------------ |
| email | E-Mail des Partners. |
| name | Vor- und Zuname des Partners. |
| telefonnummer | Telefonnummer des Partners. |
| organisation | Organisationsname des Partners. |

Wende dich mit den Daten bitte an den [Dr. Klein Ratenkredit Support](mailto:info@drklein-rk.de?subject=API%20Credentials%20Anfrage) und nach einer Verifizierung unsererseits stellen wir dir eine `client-id` und ein `client-secret` zur Verfügung.

Mit den `client credentials` und der Access token url: `https://api.drkleinservice.de/oauth2/token` bekommst du ein `access-token`. 
Mit dem `access-token` als Bearer-Token und dem `scope: drkleinrk-api/vorgang:import` in der Anfrage kannst du somit einen Vorgang anlegen.

```
Request-Header Name: Authorization
Request Header Value: Bearer {jwt}
```
`401`, wenn die Authentifizierung fehlschlägt.

### Vorgang

**Vorgang Daten**

| Wert | Referenz zur Europace Dokumentation |
| :--- | :---------------------------------- |
| leadquelle | [Leadquelle](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/#vorgang){:target="_blank"} |
| antragsteller1 | [Antragsteller](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/#antragsteller){:target="_blank"} |
| antragsteller2 | [Antragsteller](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/#antragsteller){:target="_blank"} |
| haushalt | [Haushalt](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/#haushalt){:target="_blank"} |
| finanzbedarf | [Finanzbedarf](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api/#finanzbedarf){:target="_blank"} |

**Tippgeber Daten (tippgeber)**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| externeMitarbeiternummer | String | Die Mitarbeiternummer des Tippgebers. |
| vorname | String | Der Vorname des Tippgebers. |
| nachname | String | Der Nachname des Tippgebers. |
| anrede | String | Die Anrede des Tippgebers. Werte sind HERR oder FRAU. |
| telefon | String | Die Telefonnummer des Tippgebers. |
| email | String | Die Email des Tippgebers. |

**Kommentare (kommentare)**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| kommentare | [String] | Kommentare zum Vorgang. |

**Partner Daten (partner)**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| vertriebsschlüssel | String | Der Vertriebsschlüssel des Partners. |
| kundenummer | String | Die Kundennummer des Kreditinteressenten. |
| custom | Array | Benutzerdefinierte Werte die nicht in dem Standard dieser Schnittstelle enthalten sind. |

**Custom Daten (custom)**

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| key | String | Der name vom `key`. |
| value | various types| Der `key` kann den Datentyp `boolean`, `integer`, `date`, `string` haben. |

### Beispiel

**Request**

```text
Request-Header Name: Authorization
Request Header Value: Bearer {jwt}

Request-Header Name: content-type
Request Header Value: application/x-www-form-urlencoded

POST
https://api.drkleinservice.de/vorgang
```

```json
{
  "vorgang": {
    "leadquelle": "Bank im Ventil",
    "antragsteller1": {
      "personendaten": {
        "vorname": "Max",
        "nachname": "Mustermann"
      },
      "wohnsituation": {
        "anschrift": {
          "strasse": "Musterstraße.",
          "hausnummer": "41a",
          "plz": "10713",
          "ort": "Berlin",
          "wohnhaftSeit": "2016-01-01"
        },
        "anzahlPersonenImHaushalt": 1,
        "wohnart": "ZUR_MIETE"
      }
    },
    "haushalt": {
      "kontoverbindung": {
        "iban": "DE98600100700160451700",
        "gehoertZuAntragsteller": "ANTRAGSTELLER_1"
      },
      "ausgaben": {
        "privateKrankenversicherungen": [
          {
            "betragMonatlich": 200.01,
            "gehoertZuAntragsteller": "ANTRAGSTELLER_1"
          }
        ],
        "mietausgaben": [
          {
            "betragMonatlich": 200.01,
            "gehoertZuAntragsteller": "ANTRAGSTELLER_1"
          }
        ]
      }
    },
    "finanzbedarf": {
      "finanzierungszweck": "FREIE_VERWENDUNG",
      "finanzierungswunsch": {
        "kreditbetrag": 10000,
        "laufzeitInMonaten": 24,
        "ratenzahlungstermin": "MONATSENDE"
      }
    }
  },
  "tippgeber": {
    "externeMitarbeiternummer": "078181726-121",
    "vorname": "Max",
    "nachname": "Mustermann",
    "anrede": "HERR",
    "telefon": "0612-298-xxxx",
    "email": "max.mustermann@partner.de"
  },
  "kommentare": [
    "Max Mustermann hat 3 Kreditkarten."
  ],
  "partner": {
    "vertriebsschluessel": "KOOPERATION_MIT_DRKLEIN-RK",
    "kundennummer": "143251ab",
    "custom": [
      {
        "key": "telefonTermin",
        "value": "2021-01-10 13:00:00"
      },
      {
        "key": "filialId",
        "value": "123123"
      }
    ]
  }
}
```

**Response**
```json
{
  "drkleinrkVorgangsnummer": "6e8627d4-1512-11eb-adc1-0242ac120002",
  "europaceVorgangsnummer": "AB1234",
  "crmKennung": "DKR-AB1267",
  "messages": []
}
```

| Wert | Datentyp | Beschreibung |
| :--- | :------- | :----------- |
| drkleinrkVorgangsnummer | [String] | Vorgangsnummer von der Dr. Klein RK GmbH zum neuen Vorgang. |
| crmKennung | [String] | Kennung für den Finanzierungsberater im Prozess. |
| europaceVorgangsnummer | [String] | Vorgangsnummer von Kreditsmart zum neuen Vorgang. |

Die Daten zum Antragsteller, zum Haushalt und zum Finanzierungsbedarf werden 
über die von Europace bereitgestellte API [KEX-Import API](https://docs.api.europace.de/privatkredit/vorgaenge/kex-vorgang-import-api){:target="_blank"} an Kreditsmart übergeben. 
Die Daten zum Tippgeber werden für die Partnererstellung im Partnermanagment verwendet und die Daten zu den Kommentaren, um in der Ereignislasche von Kreditsmart den Finanzierungsberater weitere Informationen anzuzeigen.

### Begfriffsklärung

| Begriff | Beschreibung |
| :------ | :----------- |
| Kreditsmart | Frontend für die Kreditberatung. |
| Kreditsmart-Ereignislasche | Frontend Bereich in dem Kommentare angezeigt werden. |
| Partnermanagement | Benutzerverwaltung von Europace. |

