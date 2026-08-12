# AI-Service Modul

## Initialisierung in der ScriptEngine
`getModule("aiservice");`

## Funktionen 


### `resultObj aiservice_runWithToken(token, agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit Token Authentifizierung auf.

<details><summary>Details</summary>

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|token|string| Das AccessToken, dass mit dem Aufruf verwendet werden soll.|
|agentId|number|Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.|
|serviceId|number|Die Id des Service, der aufgerufen werden soll.|
|params|Objekt Das Parameter Objekt|

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

**Rückgabewert**
Objekt

**Verarbeitung des Rückgabewerts:**

Besitzt der aufgerufen AI Service nur einen Prozessschritt. Kann das ergebnis des Prozessschritts mit folgendem Kommando abgerufen werden:

`resultObj.MultiResults.Results[0].Result;`

Das Ergebnis ist ein string.
</details>

### `resultObj aiservice_runWithTokenAsync(token, agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit Token Authentifizierung auf. Dies ist ein asnyhroner Aufruf, der im Hintergrund durchgeführt wird.

<details><summary>Details</summary>

**Parameter:**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|token|string| Das AccessToken, dass mit dem Aufruf verwendet werden soll.|
|agentId|number|Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.|
|serviceId|number|Die Id des Service, der aufgerufen werden soll.|
|params|Objekt Das Parameter Objekt|

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

**Rückgabewert**
Objekt

Da dieser AI-Service ansynchron aufgerufen wird, wird als Ergebnis nur ein leeres Ergebnisobjekt zurückgeschickt.   

Ob der Service wirklich im Hintergrund aufgerufen wurde, kann über `resultObj.BackgroundMode` abgefragt werden. Dies ist ein Bool.
</details>

### `resultObj aiservice_run(agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit User Authentifizierung auf. Der aufrufende Anwender, muss berechtigt sein den AI-Service am Agenten aufzurufen. Ansonsten schlägt die Authentifizierung fehl. Der User wird intern ermittelt, so dass kein zusätzlicher Parameter übergeben werden muss.

Geprüft wird gegen den Agenten, dem der Service laut Datenbank gehört — nicht
gegen den übergebenen `agentId`-Parameter. Als berechtigt gilt, wer Service-Admin
ist, Organisations-Admin des Agenten (auch über eine übergeordnete Organisation),
oder wer dem Agenten direkt bzw. über eine Benutzergruppe zugeordnet ist. Fehlt
die Berechtigung, nennt die Fehlermeldung den betroffenen Agenten (Id und Name).

<details><summary>Details</summary>

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|agentId|number|Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.|
|serviceId|number|Die Id des Service, der aufgerufen werden soll.|
|params|Objekt Das Parameter Objekt|

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

**Rückgabewert**
Objekt

**Verarbeitung des Rückgabewerts:**

Besitzt der aufgerufen AI Service nur einen Prozessschritt. Kann das ergebnis des Prozessschritts mit folgendem Kommando abgerufen werden:

`resultObj.MultiResults.Results[0].Result;`

Das Ergebnis ist ein string.
</details>

### `resultObj aiservice_runAsync(agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit User Authentifizierung auf. Der aufrufende Anwender, muss berechtigt sein den AI-Service am Agenten aufzurufen. Ansonsten schlägt die Authentifizierung fehl. Der User wird intern ermittelt, so dass kein zusätzlicher Parameter übergeben werden muss. Über diese Funktion wird der AI-Service asynchron aufgerufen. 

<details><summary>Details</summary>

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|agentId|number|Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.|
|serviceId|number|Die Id des Service, der aufgerufen werden soll.|
|params|Objekt Das Parameter Objekt|

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

**Rückgabewert**
Objekt

**Verarbeitung des Rückgabewerts:**
Da dieser AI-Service ansynchron aufgerufen wird, wird als Ergebnis nur ein leeres Ergebnisobjekt zurückgeschickt.  
Ob der Service wirklich im Hintergrund aufgerufen wurde, kann über `resultObj.BackgroundMode` abgefragt werden. Dies ist ein Bool.
</details>

### `docObj aiservice_getDocument(url);`

Liefert ein von einem AI-Service-Schritt erzeugtes Word- oder Excel-Dokument
base64-kodiert. Erzeugt ein Schritt mit dem Ergebnistyp „Word (.docx)" oder
„Excel-Tabelle (.xlsx)" eine Datei, steht deren URL in `Documents` des
Schrittergebnisses bzw. im Global `aiservicedocuments`.

<details><summary>Details</summary>

**Parameter:**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|url|string|Die Dokument-URL aus `aiservicedocuments[...]` oder aus `resultObj.MultiResults.Results[n].Documents[m]`|

**Rückgabewert**
Objekt mit den Feldern:

| Feld | Typ | Beschreibung |
| ------ | ------ | ------ |
|url|string|die übergebene URL|
|filename|string|Dateiname inklusive Endung|
|mimetype|string|MimeType der Datei|
|kind|string|`docx` oder `xlsx`|
|size|number|Rohgröße der Datei in Bytes|
|base64|string|Dateiinhalt base64-kodiert|

**Berechtigung**

Abrufbar sind ausschließlich Dokumente, die die Funktion in diesem Aufruf
erhalten hat: die Dokumente der vorhergehenden Schritte des laufenden Workflows
und alles, was ein `aiservice_run`- bzw. `aiservice_runWithToken`-Aufruf
zurückgeliefert hat. Eine selbst zusammengebaute URL wird abgelehnt. Die
asynchronen Varianten (`aiservice_runAsync`, `aiservice_runWithTokenAsync`)
liefern kein Ergebnis und damit auch keine abrufbaren Dokumente.

**Fehler**

| Fall | Meldung |
| ------ | ------ |
| URL nicht aus diesem Aufruf | `document not accessible from this execution` |
| Dokument unbekannt oder aus fremder Organisation | `document not found` |
| Datei nicht mehr vorhanden | `document content unavailable` |
| Datei größer als 25 MB | `document too large: ...` |

Im Fehlerfall liefert `aiservice_getDocument` — wie jede ScriptEngine-Funktion —
einen **String** (`"ERROR aiservice_getDocument at ...: <Meldung>"`) statt des
`docObj`; `doc.base64` und die übrigen Felder sind dann `undefined`.

**Beispiel:**

```
getModule("aiservice");
getModule("sharepoint");

if (aiservicedocuments !== null && aiservicedocuments[0].length > 0) {
    var doc = aiservice_getDocument(aiservicedocuments[0][0]);
    sharepoint_uploadFile(spConfig, driveId, "/Berichte", "Bericht.docx", doc.base64);
}
```
</details>

### Verarbeitung des Ergebnis Objekts

#### Ein Prozessschritt

Besitzt der aufgerufen AI Service nur einen Prozessschritt. Kann das Ergebnis des Prozessschritts mit folgendem Kommando abgerufen werden:
`resultObj.MultiResults.Results[0].Result;`

#### Mehrere Prozessschritte im Ergebnis

Beispiel Code zur Umwandlung eines AI-ServiceErgebnisses in einen Html Tabelle.


```
getModule("log");
getModule("aiservice");

// This function does NOT support multidimensional results.
// It expects results without lists.
function getSubArray(multiResult) {
    var resultArray = new Array();
    
    if (multiResult != null) {
            
        var results = multiResult.Results;
        
        if (results != null && results.length > 0) {
            if (results[0].ServiceStepCommand.Displaytype != "Hidden") {
                resultArray.push(results[0].Result);    
            }
        
            var currentResults = getSubArray(results[0].NextServiceResult);
            
            for (var pos = 0; pos < currentResults.length; ++pos) {
                resultArray.push(currentResults[pos]);
            }
        }
    }
    
    return resultArray;
}

function getSubHeaderArray(multiResult) {
    var resultArray = new Array();
    
    if (multiResult != null) {
            
        var results = multiResult.Results;
        
        if (results != null && results.length > 0) {
            
            if (results[0].ServiceStepCommand.Displaytype != "Hidden") {
                resultArray.push(results[0].ServiceStepCommand.Title);    
            }
            
        
            var currentResults = getSubHeaderArray(results[0].NextServiceResult);
            
            for (var pos = 0; pos < currentResults.length; ++pos) {
                resultArray.push(currentResults[pos]);
            }
        }
    }
    
    return resultArray;
}

function extractArrayFromResult(result) {
    
    var resultArray = new Array();
    
    if (result != null) {
        var multiResult = result.MultiResults;
        
        if (multiResult != null) {
            resultArray = getSubArray(multiResult);
        }
    }
    
    return resultArray;
}

function extractHeaderArrayFromResult(result) {
    
    var resultArray = new Array();
    
    if (result != null) {
        var multiResult = result.MultiResults;
        
        if (multiResult != null) {
            resultArray = getSubHeaderArray(multiResult);
        }
    }
    
    return resultArray;
}

function convertResultArrayToHtmlTableRow(resultArray, elementName) {
    
    var row = "<tr>"
    
    
    for (var pos = 0; pos < resultArray.length; ++pos) {
        var content = resultArray[pos];
        row += "<";
        row += elementName;
        row += ">";
        row += content;
        row +=  "</";
        row += elementName;
        row += ">";
    }
    
    row += "</tr>";
    return row;
}

for (var pos = 0; pos < articles.length; ++pos) {
    log_info("processing " + articles[pos] + " at " + pos);
    
    if (agentId == "" || serviceId == "") {
        continue;
    }
    
    if (articles[pos] == "") {
        continue;
    }
     
    var aiServiceObj = new Object();
    aiServiceObj["search"] = articles[pos];
    var result = aiservice_run(agentId, serviceId, aiServiceObj);
    var resultArray = extractArrayFromResult(result);
    
    if (!addedHeader) {
        var headerArray = extractHeaderArrayFromResult(result);
        resultDocument += "\n" + convertResultArrayToHtmlTableRow(headerArray, "th");
        addedHeader = true;
    }
    
    resultDocument += "\n" + convertResultArrayToHtmlTableRow(resultArray, "td");
}
```
