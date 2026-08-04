# Andere Functions aufrufen

Eine Function kann andere Functions der eigenen Organisation aufrufen und so wiederverwendbare Bausteine kombinieren. Die `call`-Funktionen stehen **ohne** vorheriges `getModule` zur Verfügung.

## Funktionen

### `string call(functionId, params)`

Ruft eine andere Function über deren numerische ID auf.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| functionId | number | die numerische ID der aufzurufenden Function (sichtbar in der URL des Editors) |
| params | object | key, value Pair der Übergabeparameter |

**Rückgabewert:** das Ergebnis der aufgerufenen Function als string.

```javascript
var result = call(34, { kundennummer: "4711" });
log_info(result);
```
</details>

### `string call_by_name(functionName, params)`

Wie `call`, jedoch wird die Function über ihren **Namen** statt ihrer ID aufgelöst. Das macht den Code lesbarer und robuster gegenüber ID-Wechseln. Die Namens-Auflösung ist nicht case-sensitiv und ignoriert führende/abschließende Leerzeichen.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| functionName | string | der Name der Function, wie im Editor angezeigt |
| params | object | key, value Pair der Übergabeparameter |

**Rückgabewert:** das Ergebnis der aufgerufenen Function als string. Wird der Name in der Organisation nicht gefunden, liefert die Funktion einen Fehler-String zurück.

```javascript
var result = call_by_name("Kundendaten laden", { kundennummer: "4711" });
log_info(result);
```
</details>

### Wrapper-Modul `functions` — `func_<Name>(params)`

Mit `getModule("functions")` wird für **jede Function der Organisation automatisch eine benannte Wrapper-Funktion** erzeugt. Der Funktionsname besteht aus dem Präfix `func_` und dem bereinigten Function-Namen (Sonderzeichen werden zu `_`, deutsche Umlaute transliteriert, z.B. „Kundendaten laden" → `func_Kundendaten_laden`).

<details><summary>Details</summary>

Das Modul wird bei `getModule("functions")` einmalig geladen und injiziert die Wrapper. Jeder Wrapper nimmt genau ein Parameter-Objekt entgegen und gibt das Ergebnis als string zurück.

```javascript
getModule("functions");

// entspricht call_by_name("Kundendaten laden", {...})
var result = func_Kundendaten_laden({ kundennummer: "4711" });
log_info(result);
```
</details>
