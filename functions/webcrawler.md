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

### `string webcrawler_htmlToText(html)`

Extrahiert aus einem übergebenen HTML-String den lesbaren Text und gibt ihn als leicht strukturierten Markdown-Text zurück. Alles, was kein lesbarer Text ist (HTML-Tags, `<script>`, `<style>`, `<head>`, `<noscript>`, Kommentare, Attribute), wird entfernt. Überschriften, Listen, Tabellen und Links bleiben als Markdown erhalten.

Im Gegensatz zu `webcrawler_crawlWeb` wird **keine** URL aufgerufen — die Funktion verarbeitet ausschließlich den übergebenen HTML-Text.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| html | string | Der HTML-Text, aus dem der lesbare Text extrahiert werden soll. |

**Rückgabewert**
string — der extrahierte Markdown-Text.

</details>