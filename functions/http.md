## HTTP Modul

`getModule("http");`

## Funktionen

### `string http_get(url, header)`
Führt einen Http-Get Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>

### `string http_post(url, contentType, header, body)`
Führt einen Http-Post Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| contentType | string | der ContentType des Bodies. z.B. "application/json" |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |
| body | string | der Body der mitgeschickt wird. In der Regeln ein JSON oder XML Objekt das in einen string umgewandelt wurde. |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>

### `string http_put(url, contentType, header, body)`

Führt einen Http-Put Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| contentType | string | der ContentType des Bodies. z.B. "application/json" |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |
| body | string | der Body der mitgeschickt wird. In der Regeln ein JSON oder XML Objekt das in einen string umgewandelt wurde. |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>

### `string http_delete(url, header)`
Führt einen Http-Delete Aufruf auf "url" durch und liefert das Ergebnis des Aufrufs zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | die aufzurufende Url des HTTP Kommandos |
| header | object | key, value Pair der Header Informationen. Wie solche Objekte angelegt werden findest Du unter [Übergabe Objekte anlegen](https://gitlab.com/auxdata/auxdata.ai/-/wikis/Funktionsreferenz/%C3%9Cbergabe-Objekte-anlegen) |

**Rückgabewert**

Das Ergebnis des Aufrufs als string. Um die Weiterverarbeitung des strings (zum Beispiel Konvertierung in ein JSON Objekt) muss sich die aufrufende Funktionen kümmern.
</details>


### `string http_queryEscape(value)`

Optimiert den übergegebenen string für einen Http Aufruf. Es werden Sonderzeichen durch Http-konforme Steuerzeichen ersetzt. Wie zum Beispiel Leerzeichen. Das Ergebnis wird zurückgegeben
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| value | string | der string der für einen http Aufruf optimiert werden soll. |

**Rückgabewert**

Der optimierte string, der dann zum Beispiel als Parameter an einen http-Request angehängt werden kann.</details>

### `string http_search(search, exactMatch)`
Führt eine Google Custom Search aus und liefert das Ergebnis zurück. 
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| search | string | der Suchbegriff wonach gesucht werden soll. **Wichtig**: Hier wird keine Optimierung und Anpassung des Suchbegriffs druchgeführt. Besteht die Gefahr, dass Leer - bzw. Sonderzeichen im Suchbegriff enthalten sind, muss http_queryEscape vorher auf den Suchbegriff ausgeführt werden. Ansonsten wird ein fehlerhaftes Ergebnis zurückgeliefert. |
| exactMatch | bool | Soll genau nach diesem Suchbegriff gesucht werden, oder sind Ergebnisse zu teilen des Suchstrings möglich |

**Rückgabewert**

das Google Suchergebnis als string. Hierbei handelt es sich um ein JSON DOkument das in ein JSON Objekt umgewandelt und anschließend durchsucht werden kann.
</details>
