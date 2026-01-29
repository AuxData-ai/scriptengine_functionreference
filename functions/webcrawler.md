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

### `string webcrawler_liveSearch(prompt, maxPages, searchMode)`
Durchsucht das Internet nach den Informationen aus dem Prompt. Hierbei wird der Prompt analysiert und in einen optimierten Suchstring für die Internetsuche umgewandelt. Die Such wird anschließend mit dem optimierten Prompt durchgeführt.

Es existieren zwei Suchmodi:

1 - Einfach: Dort wird die Suche durchgeführt und das Ergebnis der Suche zurückgeliefert. 
2 - Erweitert: Dort werden die gefundenen Webseiten auf der Seite gecrawlt. Das Ergebnis ist der extrahierte Text aus all den Treffern.

**Hinweis:**

In der erweiterten Suche verwenden wir den Webcrawler. Der Webcrawler interpretiert kein Javascript und liest nur HTML aus. Das bedeutet, dass in Webseiten, die durch Javascript generiert werden keine Informationen gefunden werden.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| prompt | string | Der User Prompt, der automatisch über ein LLM zu einem Suchstring umgewandelt wird. Mit dem umgewandelten Suchstring erfolgt dann die Live Internetsuche |
| maxPages | number | Die maximale Anzahl an Suchergebnissen, die gefunden werden soll. |
| searchMode | number | 1: Einfacher Suchmodus 2: Erweiterter Suchmodus |

**Rückgabewert**
string der gefundene Inhalt aus dem Internet.

</details>

### `string webcrawler_liveSearchOnPage(prompt, page, maxPages, searchMode)`
Durchsucht das Internet nach den Informationen aus dem Prompt auf der konkret angegebenen Seite. Hierbei wird der Prompt analysiert und in einen optimierten Suchstring für die Internetsuche umgewandelt. Die Such wird anschließend mit dem optimierten Prompt durchgeführt.


Es existieren zwei Suchmodi:

1 - Einfach: Dort wird die Suche durchgeführt und das Ergebnis der Suche zurückgeliefert. 
2 - Erweitert: Dort werden die gefundenen Webseiten auf der Seite gecrawlt. Das Ergebnis ist der extrahierte Text aus all den Treffern.

**Hinweis:**

Es werden nur Ergebnisse geliefert, die für den Suchstring auf der konkreten Seite gefunden wurden. Andere Seiten werden nicht berücksichtigt.

In der erweiterten Suche verwenden wir den Webcrawler. Der Webcrawler interpretiert kein Javascript und liest nur HTML aus. Das bedeutet, dass in Webseiten, die durch Javascript generiert werden keine Informationen gefunden werden.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| prompt | string | Der User Prompt, der automatisch über ein LLM zu einem Suchstring umgewandelt wird. Mit dem umgewandelten Suchstring erfolgt dann die Live Internetsuche |
| page | string | Die konkrete Seite auf der die Suche ausgeführt werden soll. z.B. "wikipedia.de" |
| maxPages | number | Die maximale Anzahl an Suchergebnissen, die gefunden werden soll. |
| searchMode | number | 1: Einfacher Suchmodus 2: Erweiterter Suchmodus |

**Rückgabewert**
string der gefundene Inhalt aus dem Internet.

</details>
