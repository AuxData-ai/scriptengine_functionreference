# Logging Modul

## Integration in ScriptEngine

`getModule("log");`

## Funktionen

### `void log_info(message)`
Protokolliert eine Information
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| message | string | Die Nachricht, die protokolliert werden soll. |


**Rückgabewert**
-

</details>

### `void log_warn(message)`
Protokolliert eine Warnung.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| message | string | Die Nachricht, die protokolliert werden soll. |

**Rückgabewert**
-

</details>

### `void log_error(message)`
Protokolliert einen Fehler
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| message | string | Die Nachricht, die protokolliert werden soll. |

**Rückgabewert**
-

</details>

### `void log_fatal(message)`
Protokolliert einen schlimmen Fehler
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| message | string | Die Nachricht, die protokolliert werden soll. |

**Rückgabewert**
-

</details>

### `string log_object(message, object)`
Protokolliert eine Nachricht zusammen mit allen Eigenschaften eines Objekts — je Eigenschaft eine Zeile `Name: Wert`.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| message | string | Die Nachricht, die vor den Objekt-Eigenschaften protokolliert wird. |
| object | object | Das Objekt, dessen Eigenschaften protokolliert werden. Ist der Wert kein Objekt (`null`, `undefined`, String, Zahl), wird nur die Nachricht protokolliert — das Skript läuft weiter. |

**Rückgabewert**
string - Die übergebene Nachricht.

**Beispiel**

```javascript
getModule("log");

var auftrag = {
  kunde: { name: "Contoso", ort: "Köln" },
  positionen: [ "Schrauben", "Muttern" ],
  menge: 3
};
log_object("Auftrag verarbeitet", auftrag);

// Protokoll:
// (:8:1) info: Auftrag verarbeitet
// kunde: {"name":"Contoso","ort":"Köln"}
// positionen: ["Schrauben","Muttern"]
// menge: 3
```

Verschachtelte Objekte und Arrays werden als JSON ausgegeben, einfache Werte unverändert. Die Reihenfolge der Zeilen entspricht der Reihenfolge im Objekt. Werte, die sich nicht als JSON abbilden lassen (z.B. Funktionen), werden in ihrer String-Form protokolliert.

</details>

### `bool log_serverLogs(active)`
Schaltet zusätzlich zum Skript-Protokoll die Ausgabe in das Server-Log ein oder aus (Standard: aus).
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| active | bool | `true` schreibt jede weitere Log-Ausgabe zusätzlich in das Server-Log. |

**Rückgabewert**
bool - Der gesetzte Wert.

</details>

### `string log_logs`
enthält das gesamte Logging-Protokoll in dieser Variable.
<details><summary>Details</summary>

Ist eine Vairable keine Funktion

</details>