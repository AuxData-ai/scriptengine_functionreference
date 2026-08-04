# HTTP Modul

## Initialisierung in der ScriptEngine
`getModule("http");`

## Funktionen

### `string http_get(url, header)`
Führt einen Http-Get Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>

### `string http_post(url, contentType, header, body)`
Führt einen Http-Post Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| contentType | string | der ContentType des Bodies. z.B. "application/json" |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |
| body | string | der Body der mitgeschickt wird. In der Regeln ein JSON oder XML Objekt das in einen string umgewandelt wurde. |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>

### `string http_put(url, contentType, header, body)`

Führt einen Http-Put Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| contentType | string | der ContentType des Bodies. z.B. "application/json" |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |
| body | string | der Body der mitgeschickt wird. In der Regeln ein JSON oder XML Objekt das in einen string umgewandelt wurde. |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>

### `string http_delete(url, header)`
Führt einen Http-Delete Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>


### `string http_queryEscape(value)`

Optimiert den übergegebenen string für einen Http Aufruf. Es werden Sonderzeichen durch Http-konforme Steuerzeichen ersetzt. Wie zum Beispiel Leerzeichen. Das Ergebnis wird zurückgegeben
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| value | string | der string der für einen http Aufruf optimiert werden soll. |

**Rückgabewert**

Der optimierte string, der dann zum Beispiel als Parameter an einen http-Request angehängt werden kann.</details>

### `string http_search(search, exactMatch)`
Führt eine Google Custom Search aus und liefert das Ergebnis zurück. 
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| search | string | der Suchbegriff wonach gesucht werden soll. **Wichtig**: Hier wird keine Optimierung und Anpassung des Suchbegriffs druchgeführt. Besteht die Gefahr, dass Leer - bzw. Sonderzeichen im Suchbegriff enthalten sind, muss http_queryEscape vorher auf den Suchbegriff ausgeführt werden. Ansonsten wird ein fehlerhaftes Ergebnis zurückgeliefert. |
| exactMatch | bool | Soll genau nach diesem Suchbegriff gesucht werden, oder sind Ergebnisse zu teilen des Suchstrings möglich |

**Rückgabewert**

das Google Suchergebnis als string. Hierbei handelt es sich um ein JSON DOkument das in ein JSON Objekt umgewandelt und anschließend durchsucht werden kann.
</details>

### `string http_patch(url, contentType, header, body)`

Führt einen HTTP-PATCH-Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück. PATCH eignet sich für partielle Ressourcen-Updates, bei denen nur einzelne Felder geändert werden sollen (im Unterschied zu PUT, das die Ressource vollständig ersetzt).
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| contentType | string | der ContentType des Bodies. z.B. `"application/json"` |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |
| body | string | der Body der mitgeschickt wird. In der Regel ein JSON-Objekt das in einen string umgewandelt wurde. |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktion kümmern.

**Beispiel**

```javascript
var module = getModule("http");

var body = JSON.stringify({ tags: [1, 5] });
var result = http_patch(
    "https://api.example.com/documents/42",
    "application/json",
    null,
    body
);
var parsed = JSON.parse(result);
log_info("Aktualisiertes Dokument: " + parsed.id);
```
</details>

### `string http_getBinary(url, header)`

Führt einen HTTP-GET-Aufruf durch und liefert den Antwort-Body als **Base64-codierten String** zurück. Damit können binäre Dateien (PDF, Bilder, DOCX usw.) heruntergeladen und anschließend z.B. in der Wissensdatenbank gespeichert werden.

> **Hinweis:** Die rohen `http_*`-Scriptfunktionen unterliegen einer URL-Whitelist; Localhost-Adressen sind geblockt. Um interne/selbst gehostete Dienste (z.B. Paperless-ngx) anzusprechen, einen **HTTP-Service** mit Antwort-Kodierung `Base64` konfigurieren und über `http_service` aufrufen.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url |
| header | object | key, value Pair der Header Informationen (kann `null` sein) |

**Rückgabewert**

Der Antwort-Body als Base64-codierter string. Dieser kann direkt an `knowledgedb_saveBinary` oder `document_*`-Funktionen übergeben werden.

**Beispiel — PDF-Download und Speicherung in der Wissensdatenbank**

```javascript
var module = getModule("http");

// PDF von einer externen URL herunterladen
var base64Pdf = http_getBinary(
    "https://api.example.com/documents/42/download",
    null
);

// Base64-Inhalt in die Wissensdatenbank übernehmen.
// agentId und containerId kommen aus den Script-Parametern; das Dateiformat
// wird automatisch aus der Dateiendung des Namens ermittelt (kein mimeType-Argument).
// Leerer documentId-String ("") legt ein neues Dokument an.
var documentId = knowledgedb_saveBinary(agentId, containerId, "Rechnung_42.pdf", base64Pdf, "");
```
</details>

### `string http_postMultipart(url, header, partsJson)`

Führt einen HTTP-POST als `multipart/form-data`-Upload durch. Die einzelnen Teile (Parts) werden als JSON-Array übergeben. Damit können gleichzeitig Textfelder und binäre Dateien (deren Inhalt als Base64-String vorliegt) an eine API übermittelt werden — typischerweise für Dokumenten-Upload-Endpunkte.

> **Hinweis:** Auch hier gilt die URL-Whitelist (kein Localhost). Für interne Dienste einen **HTTP-Service** mit Multipart-Konfiguration verwenden.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url |
| header | object | zusätzliche Header (kann `null` sein; `Content-Type` wird automatisch auf `multipart/form-data` gesetzt) |
| partsJson | string | JSON-Array der Parts (Aufbau siehe unten) |

**Part-Schema**

Jedes Element im Array ist ein Objekt mit folgenden Feldern:

| Feld | Typ | Pflicht | Beschreibung |
| ------ | ------ | ------ | ------ |
| name | string | ja | Name des Formularfelds |
| kind | string | ja | `"field"` für Textfelder, `"file"` für Datei-Parts |
| value | string | nur bei `kind:"field"` | Textwert des Feldes |
| filename | string | nur bei `kind:"file"` | Dateiname (wird im Content-Disposition-Header gesendet) |
| contentBase64 | string | nur bei `kind:"file"` | Dateiinhalt als Base64-codierter string |

**Rückgabewert**

Das Ergebnis des Aufrufs als string (in der Regel die JSON-Antwort der API).

**Beispiel — Dokument-Upload zu Paperless-ngx über direkten Aufruf**

```javascript
var module = getModule("http");

// Zuvor via http_getBinary oder http_service (Base64) heruntergeladenes PDF
var base64Content = "..."; // Base64-String des Dokuments

var parts = JSON.stringify([
    { name: "title",            kind: "field", value: "Rechnung Q2" },
    { name: "correspondent",    kind: "field", value: "Lieferant GmbH" },
    { name: "document",         kind: "file",  filename: "rechnung.pdf",
      contentBase64: base64Content }
]);

var result = http_postMultipart(
    "https://paperless.example.com/api/documents/post_document/",
    null,
    parts
);
log_info("Upload-Ergebnis: " + result);
```

> **Tipp für selbst gehostetes Paperless (Localhost):** Da `http_postMultipart` Localhost-URLs blockt, einen HTTP-Service mit aktiviertem Multipart-Upload konfigurieren und ihn per `http_service` aufrufen (siehe unten).
</details>

### `string | object http_service(serviceId, params [, returnFull])`

Ruft einen konfigurierten HTTP-Service der Organisation auf. Normalerweise wird der Antwort-Body als string zurückgegeben. Mit dem optionalen dritten Parameter `returnFull = true` liefert die Funktion stattdessen ein Objekt mit Status-Code, Body und Antwort-Headern — nützlich wenn der Aufrufer den HTTP-Status auswerten oder bestimmte Header (z.B. `Location`, `X-Document-Id`) lesen muss.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| serviceId | number | die numerische ID des HTTP-Services (sichtbar in der URL des Editors) |
| params | object | key, value Pair der Parameter, die in URL/Body/Header des Services eingesetzt werden |
| returnFull | bool | optional, Standard `false`. Bei `true` wird ein Objekt `{status, body, headers}` zurückgegeben statt nur der Body-String |

**Rückgabewert**

* Wenn `returnFull` nicht angegeben oder `false`: der Antwort-Body als string (unverändertes Verhalten).
* Wenn `returnFull = true`: ein Objekt mit den Feldern:

| Feld | Typ | Beschreibung |
| ------ | ------ | ------ |
| status | number | HTTP-Statuscode (z.B. 200, 201, 404) |
| body | string | Antwort-Body als string (bei `responseencoding: "base64"` als Base64-String) |
| headers | object | Antwort-Header als Objekt (Schlüssel → Array von Werten) |

**Vorteile gegenüber rohen `http_*`-Funktionen**

* HTTP-Services umgehen die URL-Whitelist — Localhost-Adressen und interne Netze sind erreichbar.
* Credentials (API-Keys, Tokens) bleiben in den Umgebungsvariablen des Services und sind nicht im Script-Code sichtbar.
* Multipart-Upload und Base64-Antwort-Kodierung (für Binär-Downloads) werden im Editor konfiguriert, nicht im Script.

**Beispiel 1 — Einfacher Aufruf (Body als string)**

```javascript
var module = getModule("http");

var result = http_service(12, { documentId: "42" });
var parsed = JSON.parse(result);
log_info("Dokument-Titel: " + parsed.title);
```

**Beispiel 2 — returnFull: Status und Location-Header auslesen**

```javascript
var module = getModule("http");

var res = http_service(15, { title: "Rechnung Q2" }, true);
if (res.status === 201) {
    var location = res.headers["Location"];
    log_info("Neu angelegtes Dokument unter: " + location);
} else {
    log_error("Upload fehlgeschlagen, Status: " + res.status + ", Body: " + res.body);
}
```

**Beispiel 3 — Paperless-Workflow: Binär-Download + Multipart-Upload über HTTP-Services**

Voraussetzung: Zwei HTTP-Services sind konfiguriert —
- Service 20 „Paperless Download" mit Methode GET und **Antwort-Kodierung: Base64**
- Service 21 „Paperless Upload" mit Methode POST und **Multipart-Upload** aktiviert (Part `document` als Datei, Content-Parameter `file`)

```javascript
var module = getModule("http");

// 1. Dokument binärsicher herunterladen (liefert Base64-String)
var downloadRes = http_service(20, { documentId: "123" }, true);
if (downloadRes.status !== 200) {
    log_error("Download fehlgeschlagen: " + downloadRes.status);
    return;
}

// 2. Dokument in Ziel-Paperless hochladen (Multipart)
var uploadResult = http_service(21, {
    title:    "Archiv-Kopie",
    filename: "dokument_123.pdf",
    file:     downloadRes.body   // Base64-Inhalt aus dem Download
});
log_info("Upload-Ergebnis: " + uploadResult);
```
</details>

### `string http_service_by_name(serviceName, params)`

Wie `http_service`, jedoch wird der HTTP-Service nicht über seine numerische ID, sondern über seinen **Namen** innerhalb der eigenen Organisation aufgelöst. Das macht den Code lesbarer und robuster gegenüber ID-Wechseln. Die Namens-Auflösung ist nicht case-sensitiv und ignoriert führende/abschließende Leerzeichen.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| serviceName | string | der Name des HTTP-Services, wie im Editor angezeigt |
| params | object | key, value Pair der Parameter |

**Rückgabewert:** der Antwort-Body als string. Wird der Name in der Organisation nicht gefunden, liefert die Funktion einen Fehler-String zurück.

```javascript
var module = getModule("http");

var result = http_service_by_name("Paperless Download", { documentId: "42" });
log_info(result);
```
</details>

### Wrapper-Modul `httpservices` — `httpsvc_<Name>(params)`

Statt jeden HTTP-Service per ID oder Namen aufzurufen, kann mit `getModule("httpservices")` für **jeden HTTP-Service der Organisation automatisch eine benannte Wrapper-Funktion** erzeugt werden. Der Funktionsname besteht aus dem Präfix `httpsvc_` und dem bereinigten Service-Namen (Sonderzeichen werden zu `_`, deutsche Umlaute transliteriert, z.B. „Paperless Übersicht" → `httpsvc_Paperless_Uebersicht`).

<details><summary>Details</summary>

Das Modul wird bei `getModule("httpservices")` einmalig geladen und injiziert die Wrapper. Jeder Wrapper nimmt genau ein Parameter-Objekt entgegen und gibt den Antwort-Body als string zurück.

```javascript
getModule("httpservices");

// entspricht http_service_by_name("Paperless Download", {...})
var result = httpsvc_Paperless_Download({ documentId: "42" });
log_info(result);
```

> **Hinweis:** Das Präfix `httpsvc_` ist bewusst vom Modul-Präfix `http_` verschieden, damit es nicht mit den eingebauten `http_*`-Funktionen kollidiert.
</details>
