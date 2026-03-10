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

#### System Prompt

#### Aufgaben Prompt

##### Suchprompt

### Funktionsaufrufe

Variable können als Aufrufparameter einer Funktion verwendet werden.

### HTTP Service Aufrufe

Variablen können als Parameter in einem HTTP REST Service aufruf verwendet werden.

### MCP Service Aufrufe

Variablen können als Parameter in einem MCP Service aufruf verwendet werden.
