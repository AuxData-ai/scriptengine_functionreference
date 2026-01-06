# SQL Modul

## Integration in ScriptEngine
`getModule("sql");`

### `bool sql_execute(connectionConfig, sqlCommand)`

Führt ein Kommando zur Datenänderung aus(insert, update,...)

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|connectionConfig|Object|Die Verbindungskonfiguration zur Datenbank|
|sqlCommand|string|Das auszuführende SQL Kommando|


**Rückgabewert** 
bool true oder Fehlermeldung

</details>

### `queryResults sql_query(connectionConfig, sqlCommand)`

Führt die Datenbankabfrage sqlCommand durch und liefert eine JSON Struktur mit dem Ergebnis zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|connectionConfig|Object|Die Verbindungskonfiguration zur Datenbank|
|sqlCommand|string|Das auszuführende SQL Kommando|


**Rückgabewert** 
bool true oder Fehlermeldung

</details>

### Beispiel für ConnectionConfig

```
var dbConnection = new Object(); 
dbConnection["driver"] = "mysql"; 
dbConnection["dbname"] = "test";
dbConnection["host"] = "127.0.0.1";
dbConnection["port"] = "3306";
dbConnection["user"] = "root";
dbConnection["password"] = "root";
```

### Beispiel für Query

```
getModule("sql"); 
getModule("log");

var dbConnection = new Object(); 
dbConnection["driver"] = "mysql"; 
dbConnection["dbname"] = "test"; 
dbConnection["host"] = "127.0.0.1"; 
dbConnection["port"] = "3306"; 
dbConnection["user"] = "root"; 
dbConnection["password"] = "root";

var result = sql_query(dbConnection, "select quantity from storage where articleid = 2876");

if (result.Fields == undefined || result.Fields.length == 0) { 
  return "Kein Ergebnis"; 
}

log_info("Datasets found: " + result.Data.length);

for (var pos = 0; pos < result.Data.length; ++pos) { 
  var row = result.Data[pos]; 
  var dataset = "";

  for (fieldPos = 0; fieldPos < result.Fields.length; ++fieldPos) {
    dataset += "" + row[result.Fields[fieldPos]] + "|";
  }

  log_info(dataset);
}

return "OK";
```