# MCP-Server (Model Context Protocol)

Über das `mcp`-Modul können die in der eigenen Organisation konfigurierten **MCP-Server** direkt aus einer Function heraus aufgerufen werden. Damit lassen sich die Tools eines MCP-Servers (z.B. Suche, Datenabruf, Aktionen) als Baustein in eigene Abläufe einbinden.

## Initialisierung in der ScriptEngine
`getModule("mcp");`

> Die aufgerufenen MCP-Server müssen zur Organisation gehören, in der die Function erstellt wurde. Die Namens-Auflösung ist nicht case-sensitiv und ignoriert führende/abschließende Leerzeichen.

## Funktionen

### `string mcp_call(mcpServerId, toolName, params)`

Ruft ein Tool eines MCP-Servers über dessen numerische ID auf.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| mcpServerId | number | die numerische ID des MCP-Servers |
| toolName | string | der Name des aufzurufenden Tools |
| params | object | key, value Pair der Tool-Argumente |

**Rückgabewert:** das Tool-Ergebnis als string.

```javascript
getModule("mcp");

var result = mcp_call(7, "search", { query: "Quartalsbericht" });
log_info(result);
```
</details>

### `string mcp_call_by_name(serverName, toolName, params)`

Wie `mcp_call`, jedoch wird der MCP-Server über seinen **Namen** statt seiner ID aufgelöst.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| serverName | string | der Name des MCP-Servers, wie im Editor angezeigt |
| toolName | string | der Name des aufzurufenden Tools |
| params | object | key, value Pair der Tool-Argumente |

**Rückgabewert:** das Tool-Ergebnis als string. Wird der Name in der Organisation nicht gefunden, liefert die Funktion einen Fehler-String zurück.

```javascript
getModule("mcp");

var result = mcp_call_by_name("Wissens-Server", "search", { query: "Quartalsbericht" });
log_info(result);
```
</details>

### Wrapper-Modul `mcpservices` — `mcpsvc_<Server>(toolName, params)`

Mit `getModule("mcpservices")` wird für **jeden MCP-Server der Organisation automatisch eine benannte Wrapper-Funktion** erzeugt. Der Funktionsname besteht aus dem Präfix `mcpsvc_` und dem bereinigten Server-Namen (Sonderzeichen werden zu `_`, deutsche Umlaute transliteriert).

<details><summary>Details</summary>

Jeder Wrapper nimmt den Tool-Namen und ein Parameter-Objekt entgegen und gibt das Tool-Ergebnis als string zurück.

```javascript
getModule("mcpservices");

// entspricht mcp_call_by_name("Wissens-Server", "search", {...})
var result = mcpsvc_Wissens_Server("search", { query: "Quartalsbericht" });
log_info(result);
```

> **Hinweis:** Das Präfix `mcpsvc_` ist bewusst vom Modul-Präfix `mcp_` verschieden, damit es nicht mit den eingebauten `mcp_*`-Funktionen kollidiert.
</details>
