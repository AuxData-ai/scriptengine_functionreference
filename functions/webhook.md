# Webhook-getriggerte Functions

Wenn eine Funktion über einen konfigurierten Inbound-Webhook aufgerufen wird, erhält sie automatisch vier vordefinierte Parameter. Diese Parameter enthalten alle relevanten Informationen der eingehenden HTTP-Anfrage und müssen **nicht** in der Funktionskonfiguration deklariert werden — die Plattform befüllt sie bei jedem Webhook-Aufruf automatisch.

## Automatische Parameter

| Parametername | Typ | Beschreibung |
|---|---|---|
| `body` | string | Der rohe Request-Body als string. Enthält bei JSON-Anfragen den JSON-Text, bei Formulardaten die URL-kodierten Daten, oder einen beliebigen anderen Text. |
| `headers` | string | Die HTTP-Request-Header als JSON-kodierter string (`map[string][]string`). Jeder Header-Name ist ein Schlüssel; der Wert ist ein Array von strings (da HTTP mehrere Werte pro Header erlaubt). |
| `query` | string | Die URL-Query-Parameter als JSON-kodierter string (`map[string][]string`). Entspricht dem gleichen Format wie `headers`. |
| `method` | string | Die HTTP-Methode der eingehenden Anfrage in Großbuchstaben, z.B. `"POST"`, `"GET"`, `"PUT"`. |

> **Hinweis:** `headers` und `query` sind JSON-Strings — sie müssen im Script mit `JSON.parse()` in ein Objekt umgewandelt werden, bevor auf einzelne Felder zugegriffen werden kann. Jeder Wert ist ein Array von strings.

## Zugriff auf Parameter im Script

```javascript
// headers und query als JSON-String → Objekt umwandeln
var headers = JSON.parse(params["headers"]);
var query   = JSON.parse(params["query"]);
var body    = params["body"];
var method  = params["method"];
```

## Beispiel: Paperless-ngx Webhook-Integration

Dieses Beispiel zeigt eine Funktion, die von einem Paperless-ngx Workflow-Action per Webhook aufgerufen wird. Paperless übergibt die Dokument-ID als Query-Parameter; die Funktion lädt das Dokument über einen konfigurierten HTTP-Service herunter (Base64-Antwort, Teilprojekt A) und speichert es in der Wissensdatenbank.

```javascript
var module = getModule("http");

// 1. Query-Parameter auslesen: Paperless sendet ?documentId=42
var query = JSON.parse(params["query"]);
var docId = query["documentId"] ? query["documentId"][0] : "";

if (!docId) {
    log_error("Kein documentId in den Query-Parametern gefunden.");
    return "error: missing documentId";
}

// 2. Optionaler Header-Check: prüfen ob ein X-Paperless-Event-Header gesetzt ist
var headers = JSON.parse(params["headers"]);
var eventType = headers["X-Paperless-Event"] ? headers["X-Paperless-Event"][0] : "unknown";
log_info("Paperless-Event: " + eventType + ", Dokument-ID: " + docId);

// 3. Dokument binärsicher über HTTP-Service herunterladen (Base64-Antwort-Kodierung)
//    HTTP-Service-ID 20 = "Paperless Download" (Methode GET, Antwort-Kodierung Base64)
var base64Pdf = http_service(20, { documentId: docId });

if (!base64Pdf || base64Pdf.length === 0) {
    log_error("Download fehlgeschlagen für Dokument " + docId);
    return "error: download failed";
}

// 4. Dokument in der Wissensdatenbank speichern
//    Signatur: knowledgedb_saveBinary(agentId, containerId, name, base64Content, documentId)
//    - agentId, containerId: kommen aus den konfigurierten Funktionsparametern
//    - name: Dateiendung muss angegeben sein — das Dateiformat wird daraus abgeleitet
//    - letzter Parameter "" → neues Dokument anlegen (bei Update: UUID des bestehenden Dokuments)
var savedDocId = knowledgedb_saveBinary(
    params["agentId"],
    params["containerId"],
    "paperless_dokument_" + docId + ".pdf",
    base64Pdf,
    ""
);

log_info("Dokument gespeichert, interne ID: " + savedDocId);
return savedDocId;
```

### Konfiguration der Funktionsparameter

Damit die Funktion `agentId` und `containerId` kennt, werden diese als **Funktionsparameter** in der Funktionskonfiguration definiert (nicht als Webhook-Parameter — sie kommen aus der Konfiguration, nicht aus dem eingehenden Request). Die Webhook-Parameter `body`, `headers`, `query` und `method` werden zusätzlich automatisch befüllt.

## Synchrone vs. asynchrone Ausführung

Bei einem **synchronen** Webhook wartet der Aufrufer (z.B. Paperless) auf das Ergebnis — der Rückgabewert der Funktion wird als HTTP-Antwort-Body zurückgeschickt.

Bei einem **asynchronen** Webhook antwortet die Plattform sofort mit `{"status": "accepted"}` und führt die Funktion im Hintergrund aus. Der Rückgabewert der Funktion ist dann nur im Event-Log sichtbar, nicht für den Aufrufer.

> **Empfehlung:** Für lang laufende Operationen (Dokumenten-Download + KI-Ingestion) den Webhook im **asynchronen** Modus konfigurieren.

## Weitere Informationen

- [Wissensdatenbank-Modul](./knowledgedb.md) — `knowledgedb_saveBinary` und weitere KnowledgeDB-Funktionen
- [HTTP-Modul](./http.md) — `http_service` für interne/selbst gehostete Dienste (kein Localhost-Block)
- [Administratorhandbuch Kapitel 7.4 Webhooks](../../manual/administratorhandbuch/07-http-funktionen-mcp.md#74-webhooks) — Webhooks anlegen und verwalten
