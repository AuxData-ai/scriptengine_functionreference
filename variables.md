# Variablen in der ScriptEngine

## Syntax
In AI Services können Variablen genutzt werden. Diese Variablen können selbstständig vergeben werden. In den AI Services gibt es derzeit nur zwei feste vordefinierte Variablen.

Eine Variable beginnt immer mit "${" und endet mit "}"

Syntax:
${variableNname}

Beispiel:
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

Beispiel:

Fasse mir folgenden Text zusammen:
${answer}

### result

Diese Variable kann als mit 1 beginnendes Array verwendet werden. Das die Ergebnisse aller vorheriger Schritte enthält.

Einsatz der Anweisung ${result[<Prozessschritt Nr.>]} 

Beispiel:

Kombiniere folgende beiden Texte und generiere ein einheitliches Ergebnis.
1. Text:
${result[1]}

2. Text
${result[2]}

## Verwendung von Variablen
###
