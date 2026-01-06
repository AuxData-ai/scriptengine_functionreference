---
title: AI-Service Aufrufe
---




## Integration in Scriptengine

`getModule("aiservice");`

## Funktionen 


### `resultObj aiservice_runWithToken(token, agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit Token Authentifizierung auf.

**Parameter:**

- token: Das AccessToken, dass mit dem Aufruf verwendet werden soll.
- agentId: Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.
- serviceId: Die Id des Service, der aufgerufen werden soll.
- params: Das Parameter

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

Verarbeitung des Rückgabewerts

Besitzt der aufgerufen AI Service nur einen Prozessschritt. Kann das ergebnis des Prozessschritts mit folgendem Kommando abgerufen werden:
`resultObj.MultiResults.Results[0].Result;`

Das Ergebnis ist ein string.

### `resultObj aiservice_runWithTokenAsync(token, agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit Token Authentifizierung auf. Dies ist ein asnyhroner Aufruf, der im Hintergrund durchgeführt wird.

**Parameter:**

- token: Das AccessToken, dass mit dem Aufruf verwendet werden soll.
- agentId: Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.
- serviceId: Die Id des Service, der aufgerufen werden soll.
- params: 

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

Da dieser AI-Service ansynchron aufgerufen wird, wird als Ergebnis nur ein leeres Ergebnisobjekt zurückgeschickt.   
Ob der Service wirklich im Hintergrund aufgerufen wurde, kann über `resultObj.BackgroundMode` abgefragt werden. Dies ist ein Bool.

### `resultObj aiservice_run(agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit User Authentifizierung auf. Der aufrufende Anwender, muss berechtigt sein den AI-Service am Agenten aufzurufen. Ansonsten schlägt die Authentifizierung fehl. Der User wird intern ermittelt, so dass kein zusätzlicher Parameter übergeben werden muss.

**Parameter:**

- agentId: Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.
- serviceId: Die Id des Service, der aufgerufen werden soll.
- params: Ist ein zu erzeugendes Objekt, wo die Platzhalter befüllt werden müssen, die der aufzurufende AI-Service zur Bearbeitung benötigt.

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

**Verarbeitung des Rückgabewerts**

Besitzt der aufgerufen AI Service nur einen Prozessschritt. Kann das Ergebnis des Prozessschritts mit folgendem Kommando abgerufen werden:
`resultObj.MultiResults.Results[0].Result;`

Das Ergebnis ist ein string.

### `resultObj aiservice_runAsync(agentId, serviceId, params);`

Diese Funktion ruft den übergebenen Service mit User Authentifizierung auf. Der aufrufende Anwender, muss berechtigt sein den AI-Service am Agenten aufzurufen. Ansonsten schlägt die Authentifizierung fehl. Der User wird intern ermittelt, so dass kein zusätzlicher Parameter übergeben werden muss. Über diese Funktion wird der AI-Service asynchron aufgerufen. 

**Parameter:**

- agentId: Die Id des Agenten, in dem sich der aufzurufende AI Service befindet.
- serviceId: Die Id des Service, der aufgerufen werden soll.
- params: Ist ein zu erzeugendes Objekt, wo die Platzhalter befüllt werden müssen, die der aufzurufende AI-Service zur Bearbeitung benötigt.

```
var params = new Object();
params.placeholder1 = "value1";
params.placeholder2 = "value2";
params.placeholder3 = "value3";
```

**Verarbeitung des Rückgabewerts**

Da dieser AI-Service ansynchron aufgerufen wird, wird als Ergebnis nur ein leeres Ergebnisobjekt zurückgeschickt.  
Ob der Service wirklich im Hintergrund aufgerufen wurde, kann über `resultObj.BackgroundMode` abgefragt werden. Dies ist ein Bool.

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
