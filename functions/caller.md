# Caller Modul

Das Caller-Modul liefert Informationen darüber, **wer** das aktuell laufende
Skript aufgerufen hat: Aufrufart, Agent, aktive Persona und – bei AI-Services –
die Service-Kennung.

## Initialisierung in der ScriptEngine

`getModule("caller");`

## Funktionen

### `object caller_getContext()`

Gibt ein Objekt mit dem Aufrufer-Kontext zurück.

<details><summary>Details</summary>

**Parameter:** keine

**Rückgabewert**
Objekt mit den Eigenschaften:

| Feld | Typ | Beschreibung |
| ------ | ------ | ------ |
| type | string | Aufrufart: `chatbot`, `aiservice`, `intervall`, `function` oder `workflowfeedback` |
| agentId | number | ID des aufrufenden Agenten (0 wenn keiner, z.B. Funktionstest) |
| agentName | string | Name des Agenten ("" wenn keiner) |
| personaId | number | ID der aktiven Persona (0 wenn keine) |
| personaName | string | Name der Persona ("" wenn keine) |
| serviceId | number | ID des AI-Service (0 außer bei aiservice/intervall/workflowfeedback) |
| serviceName | string | Name des AI-Service ("" wenn keiner) |

**Beispiel**

```js
getModule("caller");
var ctx = caller_getContext();
if (ctx.type === "aiservice") {
    log_info("Service " + ctx.serviceName + " (#" + ctx.serviceId + "), Agent " + ctx.agentName);
}
```

</details>
