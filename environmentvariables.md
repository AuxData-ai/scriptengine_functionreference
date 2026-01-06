Hier befinden sich die Beschreibungen zu allen Funktionen des ScriptEngine.

## Umgebungsvariablen

Es ist möglich, Umgebungsvariablen an der Organisation und an der Funktion zu setzen. Umgebungsvariablen sind dann sinnvoll zu setzen, wenn es Werte sind, die man nicht hart codiert, in der Funktion enthalten haben will, aber keine Funktionsparameter sind. Sinnvolle Einsatzzwecke sind zum Beispiel API-Tokens. Werden Umgebungsvariablen nur von einer Funktion benötigt, können diese an der Funktion selbst definiert werden. Die Funktion kann auch exportiert werden, es werden dann zwar die benötigen Umgebungsvariablen Deklaration mit exportiert, aber nicht die konkreten Werte, sodass diese in einer anderen Organisation bedenkenlos importiert werden können, ohne dass vertrauliche Informationen übertragen werden.

Benötige ich eine Umgebungsvariable in mehreren Funktionen, so kann ich diese an der Organisation speichern. Alle Umgebungsvariablen, die an der Organisation hinterlegt sind, werden in jeden Funktionsaufruf eingetragen und können dort verwendet werden.

Werden an der Funktion und in der Organisation Umgebungsvariablen verwendet, die exakt gleich heißen, so werden diese von der Organisation verwendet. Die Organisationseinstellungen überschreiben die Funktionseinstellungen.

## Vordefinierte Umgebungsvariablen

Es gibt vordefinierte Umgebungsvariablen, die ein Admin über die Umgebungsvariablen setzen kann um die Ausführung der Funktion anzupassen.


| Name der Umgebungsvariable | Typ | Beschreibung |
| ------ | ------ | ------ |
|timeout|int|Hier kann der timeout einer Funktion überschrieben werden. Der Wert wird als Sekunden übergeben. überschreitet die Dauer der Ausführung einer Funktion diesen Wert in Sekunden, so wird die Funktion abgebrochen. Wird dieser Wert nicht gesetzt, so wird der interne Wert von 600 Sekunden (10 Minuten) verwendet. Mit dieser Konfiguration können lang laufende Funktionen länger arbeiten. Dies ist vor allem dann sinnvoll, wenn es Funktionen sind die sehr viele Ai Services aufrufen oder sehr aufwendige AI Services aufrufen. Der Wert der Variable kann innerhalb einer Funktion nicht abgerufen werden. Der Wert des timeouts wird nicht an die Funktion weitergegeben. **Wichtig!**
Der timeout kann nur an Funktionen gesetzt werden. Wird die timeout Variable an der Organisation gesetzt, wird dies ignoriert.
|
