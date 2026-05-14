# Variablen in der AuxData.ai Plattform

## Syntax
In AI Services können Variablen genutzt werden. Diese Variablen können selbstständig vergeben werden. In den AI Services gibt es derzeit nur zwei feste vordefinierte Variablen.

Eine Variable beginnt immer mit "${" und endet mit "}"

Syntax:
${variableNname}

**Beispiel:**

${myValue}

Bei der Benennung der Variable gelten folgende Regeln:

1. keine Leerzeichen
2. Muss mit einem Buchstaben beginnen
3. Kein Bindestrich
4. keine Sonderzeichen
5. Folgende Zeichen sind erlaubt: a-z, A-Z, 0-9, _

## Vordefinierte Variablen

Es existieren derzeit zwei vordefinierte Variablen

### answer

Diese Variable hat das Ergebnis des vorhergehenden AI Serviceschritt enthalten.

Einsatz in den Anweisungen: ${answer}

**Beispiel:**

Fasse mir folgenden Text zusammen:
${answer}

### result

Diese Variable kann als mit 1 beginnendes Array verwendet werden. Das die Ergebnisse aller vorheriger Schritte enthält.

Einsatz der Anweisung ${result[<Prozessschritt Nr.>]} 

**Beispiel:**

Kombiniere folgende beiden Texte und generiere ein einheitliches Ergebnis.
1. Text:
${result[1]}

2. Text
${result[2]}

## Setzen von Variablen
Variablen können auf unterschiedliche Art und Weise mit Inhalten befüllt sein. Eine Variable ist immer vom Datentyp Text. Zahlen die einer Variable mitgegebene werden müssen in der ScriptEngine in eine Zahl umgewandelt werden. 

### AI Service Definition

Der einfachste Fall ist die Befüllung einer Variable durch die Eingabe des Anwenders über das Formular, welches dargestellt wird, wenn der AI Service aufgerufen wird. Bei der Anlage eines neuen AI Service werden alle Variablen im ausgewählten Worklfow gesucht und automatisch in das Formular eignetragen. Nun kann der Anwender jeder Variable ein mögliches Eingabefeld zuweisen.

### Funktionsergebnis

Wird eine Funktion ausgeführt, so gibt man für die Funktion auch den Variablennamen an, in dem die Ergebnisse der Funktion gespeichert werden. Diese könnnen anschließend im Prompt verwendet werden. (siehe Verwenden von Variablen)

### REST Serviceaufruf Ergebnis

Wird ein REST Service ausgeführt, so gibt man für diesen Service auch den Variablennamen an, in dem die Ergebnisse des Service gespeichert werden. Diese könnnen anschließend im Prompt verwendet werden. (siehe Verwenden von Variablen)

### MCP Service Aufruf

Wird ein MCP Service ausgeführt, so gibt man für diesen Service auch den Variablennamen an, in dem die Ergebnisse des Service gespeichert werden. Diese könnnen anschließend im Prompt verwendet werden. (siehe Verwenden von Variablen)

## Verwenden von Variablen

Die Variablen können an unterschiedlichen Stellen eingesetzt werden um Informationen an die KI oder anderen Services zu übertragen, wo sie benötigt werden um bestmögliche Ergebnisse zu erzielen.

### AI Workflow Prompts

#### Beschreibung der Rahmenbedinungen für die Ausführung des Prompts (System Prompt Ergänzung)

Eingabe in diesem Bereich werden als Rahmenbedingungen für die Ausführung des Prompts deklariert und an das LLM Übergeben. Die dort verwendeten Variablen werden aufgelöst und durch den dahinterstehenden Text ersetzt.

<img width="1617" height="823" alt="grafik" src="https://github.com/user-attachments/assets/c50ebda5-dce8-4107-baef-739a5d86d643" />

#### Prompt

Hier wird die tatsächlich Anweisung beschrieben, welche das KI Modell asuführen soll. Auch hier werden die ort verwendeten Variablen werden aufgelöst und durch den dahinterstehenden Text ersetzt.

Beispiel mit Verwendung der Variable Task
<img width="1630" height="840" alt="grafik" src="https://github.com/user-attachments/assets/0e2ea9bd-3e09-412a-a28d-ce73be90d1ab" />


##### Nach welchen Informationen soll in der Wissensdatenbank und/oder in den Parametern gesucht werden? (Suchprompt)

Hier werden Variablen verwendet um die Suche in der Wissensdatenbank durchzuführen. EIngaben in diesem Feld haben keine Auswirkung auf die Ausführung des Prompts sondern nur auf die Datenbeschaffung aus der Wissensdatenbank über die semantische Suche.

<img width="1629" height="834" alt="grafik" src="https://github.com/user-attachments/assets/6c9be325-311d-4d12-8ffb-986df5f911c6" />


### Funktionsaufrufe

Variable können als Aufrufparameter einer Funktion verwendet werden.

In diesem Screenshot wird das Ergebnis der Funktion infoCrawler in der Variable info gespeichert (Da dies ein Eingabefeld ist, welches nur den Variablennamen erwartet müssen hier ${ und } weggelassen werden. 

Die Funktion hat ebenfalls eine Variable search. In diesem Fall wird sie über ein Eingabefeld des AI Service befüllt.
<img width="1627" height="859" alt="grafik" src="https://github.com/user-attachments/assets/dbc60fcb-3dcd-4176-8608-ca63ab2839ba" />

### HTTP Service Aufrufe

Variablen können als Parameter in einem HTTP REST Service aufruf verwendet werden.

In diesem Screenshot wird das Ergebnis der Google Suche in der Variable searchResult gespeichert (Da dies ein Eingabefeld ist, welches nur den Variablennamen erwartet müssen hier ${ und } weggelassen werden. 

Die Goggle Suche hat ebenfalls eine Variable search. In diesem Fall wird sie über ein Eingabefeld des AI Service befüllt.
<img width="1633" height="825" alt="grafik" src="https://github.com/user-attachments/assets/8c08a31e-564a-4de7-abc4-008f3951dfea" />


### MCP Service Aufrufe

Variablen können als Parameter in einem MCP Service aufruf verwendet werden.
