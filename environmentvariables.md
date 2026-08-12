Hier befinden sich die Beschreibungen zu allen Funktionen des ScriptEngine.

## Umgebungsvariablen

Es ist möglich, Umgebungsvariablen an der Organisation und an der Funktion zu setzen. Umgebungsvariablen sind dann sinnvoll zu setzen, wenn es Werte sind, die man nicht hart codiert, in der Funktion enthalten haben will, aber keine Funktionsparameter sind. Sinnvolle Einsatzzwecke sind zum Beispiel API-Tokens. Werden Umgebungsvariablen nur von einer Funktion benötigt, können diese an der Funktion selbst definiert werden. Die Funktion kann auch exportiert werden, es werden dann zwar die benötigen Umgebungsvariablen Deklaration mit exportiert, aber nicht die konkreten Werte, sodass diese in einer anderen Organisation bedenkenlos importiert werden können, ohne dass vertrauliche Informationen übertragen werden.

Benötige ich eine Umgebungsvariable in mehreren Funktionen, so kann ich diese an der Organisation speichern. Alle Umgebungsvariablen, die an der Organisation hinterlegt sind, werden in jeden Funktionsaufruf eingetragen und können dort verwendet werden.

Werden an der Funktion und in der Organisation Umgebungsvariablen verwendet, die exakt gleich heißen, so werden diese von der Organisation verwendet. Die Organisationseinstellungen überschreiben die Funktionseinstellungen.

## Vordefinierte Umgebungsvariablen

Es gibt vordefinierte Umgebungsvariablen, die ein Admin über die Umgebungsvariablen setzen kann um die Ausführung der Funktion anzupassen.


| Name der Umgebungsvariable | Typ | Beschreibung |
| ------ | ------ | ------ |
|timeout|int|Hier kann der timeout einer Funktion überschrieben werden. Der Wert wird als Sekunden übergeben. überschreitet die Dauer der Ausführung einer Funktion diesen Wert in Sekunden, so wird die Funktion abgebrochen. Wird dieser Wert nicht gesetzt, so wird der interne Wert von 600 Sekunden (10 Minuten) verwendet. Mit dieser Konfiguration können lang laufende Funktionen länger arbeiten. Dies ist vor allem dann sinnvoll, wenn es Funktionen sind die sehr viele Ai Services aufrufen oder sehr aufwendige AI Services aufrufen. Der Wert der Variable kann innerhalb einer Funktion nicht abgerufen werden. Der Wert des timeouts wird nicht an die Funktion weitergegeben. **Wichtig!** Der timeout kann nur an Funktionen gesetzt werden. Wird die timeout Variable an der Organisation gesetzt, wird dies ignoriert.
|

## Ergebnisse vorheriger Workflow-Schritte (`aiserviceresults`)

Wird eine Funktion als Prozessschritt eines AI-Workflows ausgeführt, stellt die Plattform die Ergebnisse aller vorhergehenden Schritte als globale Variable `aiserviceresults` zur Verfügung. Es ist kein `getModule(...)`-Aufruf nötig — die Variable ist in jeder Workflow-Funktion direkt verfügbar.

| Eigenschaft | Wert |
| ------ | ------ |
| Typ | `Array<string>` oder `undefined` (falls die Funktion nicht innerhalb eines Workflows oder ohne vorhergehenden Schritt läuft) |
| Reihenfolge | Chronologisch — `aiserviceresults[0]` enthält das Ergebnis des ältesten vorhergehenden Schritts, `aiserviceresults[aiserviceresults.length - 1]` das des direkt davorliegenden Schritts |
| Inhalt | Jedes Element ist das `Result`-Feld des entsprechenden vorhergehenden AI-Service-Ergebnisses, als String |

**Wichtig:** Außerhalb eines Workflows (bzw. ohne vorhergehenden Schritt) ist
`aiserviceresults` **nicht `null`, sondern `undefined`**. Der belastbare Guard
ist `typeof aiserviceresults === "undefined"` — ein Vergleich mit `null`
erkennt diesen Fall nicht. (Das unterscheidet sich bewusst von
`aiservicedocuments` weiter unten, das im leeren Fall echtes `null` liefert —
siehe dort.)

**Beispiel:**

```javascript
function executorFunctionWrapper() {
    if (typeof aiserviceresults === "undefined") {
        return "Kein vorheriger Schritt vorhanden";
    }

    // letztes vorheriges Ergebnis weiterverarbeiten
    var letztesErgebnis = aiserviceresults[aiserviceresults.length - 1];
    return "Vorheriges Ergebnis war: " + letztesErgebnis;
}
```

## Dokumente vorheriger Workflow-Schritte (`aiservicedocuments`)

Erzeugt ein vorhergehender Schritt ein Word- oder Excel-Dokument (Ergebnistyp
„Word (.docx)" bzw. „Excel-Tabelle (.xlsx)"), stellt die Plattform dessen
URL im Global `aiservicedocuments` bereit. Auch hier ist kein
`getModule(...)`-Aufruf nötig.

| Eigenschaft | Wert |
| ------ | ------ |
| Typ | Array von Arrays — ein Eintrag je vorhergehendem Schritt |
| Reihenfolge | identisch zu `aiserviceresults`: `aiservicedocuments[i]` gehört zu `aiserviceresults[i]` |
| Schritt ohne Dokument | leeres Array (kein Fehler) |
| Außerhalb eines Workflows | `null` (**anders als `aiserviceresults`**, das dort `undefined` ist — hier ist der Vergleich `aiservicedocuments !== null` der belastbare Guard) |

Die eigentliche Datei holt `aiservice_getDocument(url)` base64-kodiert.

```
getModule("aiservice");

if (aiservicedocuments !== null && aiservicedocuments[0].length > 0) {
    var doc = aiservice_getDocument(aiservicedocuments[0][0]);
    log_info(doc.filename + " hat " + doc.size + " Bytes");
}
```
