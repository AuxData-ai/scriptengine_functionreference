## Integration in ScriptEngine

`getModule("knowledgedb");`

## Funktionen

### `string saveText(agentId, containerId, name, content, documentId)`

todo

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
|  |  |  |
|  |  |  |


**Rückgabewert** todo

</details>

### `string saveBinary(agentId, containerId, name, content, documentId)`

todo

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
|  |  |  |
|  |  |  |


**Rückgabewert** todo

</details>

### `string saveTextByToken(token, agentId, containerId, name, content, documentId)`

todo

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
|  |  |  |
|  |  |  |


**Rückgabewert** 

string --\> Die DokumentenId
</details>

### `string saveBinaryByToken(token, agentId, containerId, name, content, documentId)`

todo

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| token | string | Das Accesstoken entweder eines Users oder der externen Schnittstellenfreigabe |
| agentId | int | Der Agent in dem das Dokument gespeichert werden soll |
| containerId | int | Der Container in dem das Dokument gespeichert werden soll |
| name | string | Der Name des Dokuments |
| content | base64 string | Der base64 string des Dokuments |
| documentId | string | die Id eines Dokuments, welches aktualisiert werden soll. falls es sich um einen leeren string handelt, wird ein neues Dokument angelegt. |


**Rückgabewert** 

<details>
<summary>

string --\> Die DokumentenId
</summary></details>

</details>

### `string find(searchText, agentId, coontainerId, qualityGate, resultlimit)`

findet die Chunks in denen der Suchstring vorkommt. Die Suche findet zweistufig statt. Im ersten Schritt ist es eine semantische Suche. Wird die Anzahl der resultlimits nicht erreicht, wird die Suche um eine Stichwortsuche ergänzt um noch weitere Treffer zu finden.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| searchText | string |  |
| agentId | int |  |
| containerId | int | Die Id des Containers in dem gesucht werden soll. Falls 0, werden in allen Container des Agents gesucht |
| qualityGate | int | Das qualityGate, welches eingehalten werden soll |
| resultlimit | int | die maximale Anzahl an Ergebnissen |


**Rückgabewert** 

<details>
<summary>Array von strings der Suchergebnisse</summary></details>

</details>

### `string findByKeyword(searchText, agentId, coontainerId)`

sucht in der Wissensdatenbank nach dem expliziten Suchstring. Es wird keine semantische Suhe durchgeführt. Die Treffer müssen eindeutige Teilstrings innerhalb des Textbausteins sein. Groß und Kleinschreibung wird hierbei ignoriert.

<details>
<summary>Details</summary>

**Parameter**

</details>

| Name | Typ | Beschreibung |
|------|-----|--------------|
| searchText | string | Der text nachdem gesucht werden  |
| agentId | int | Der Agent in dem gesucht werden soll |
| containerId | int | Die Id des Containers in dem gesucht werden soll. Falls 0, werden in allen Container des Agents gesucht. |

<details>
<summary>

**Rückgabewert** 

</summary>
Array von strings der Suchergebnisse
</details>

