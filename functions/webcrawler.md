# Webcrawler Modul

## Integration in ScriptEngine
`getModule("webcrawler");`

### `string webcrawler_crawlWeb(url)`

Crawlt den Inhalt einer konkreten Webseite. Die Linktiefe ist auf 3 festgelegt und es werden maximal 10 Seite gecrawlt.

**Hinweis:**

Der Webcrawler interpretiert kein Javascript und liest nur HTML aus. Das bedeutet, dass in Webseiten, die durch Javascript generiert werden keine Informationen gefunden werden.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | Die Url der Webseite die gecrawlt werden soll. |

**Rückgabewert**
Der Inhalt der Webseite

</details>

### `string webcrawler_crawlWebEnhanced(url, depth, pageCount)`
Crawlt den Inhalt einer konkreten Webseite. Die maximale Linktiefe kann übergeben werden und auch die maximale Anzahl an Seite die gecrawlt werden sollen.

**Hinweis:**

Der Webcrawler interpretiert kein Javascript und liest nur HTML aus. Das bedeutet, dass in Webseiten, die durch Javascript generiert werden keine Informationen gefunden werden.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| url | string | Die Url der Webseite die gecrawlt werden soll. |
| depth | number | Die maximale Linktiefe (Es werden nur Links verfoglt, die sich innerhalb der gleichen Domän befinden.) |
| pageCount | number | Die maximale Anzahl an Unterseiten die gecrawlt werden sollen.  |

**Rückgabewert**
string der Inhalt der gecrawltern Webseiten.

</details>