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

### `string log_logs()`
liefert das Ergebnis des Loggings zurück.
<details><summary>Details</summary>

**Parameter**
keine

**Rückgabewert**
Alle Loggingausgaben für die Funktion.

</details>