# Konwledge-DB Modul

## Integration in ScriptEngine

`getModule("knowledgedb");`

## Funktionen

### `string saveText(agentId, containerId, name, content, documentId)`

Speichert ein Textdokument in der Wissensdatenbank. Das Textdokument wird nicht über die Reader ausgelesen, sondern direkt gechunkt und in die Wissensdatenbank hochgeladen werden.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| agentId | number | Die ID des Agent, in dem das Dokument gespeichert werden soll. |
| containerId | number | Die ID des Containers, in dem das Dokument gespeichert werden soll. |
| name | string | der Name des Dokuments |
| content | string | der textuelle Inhalt des Dokuments |
| documentId | string (uuid) | Soll ein bestehendes Dokument aktualisiert werden, so kann hier die DokumentenID übergeben werden. Ist der Wert ein Leerstring, wird automatisch ein neues Dokument angelegt. |


**Rückgabewert** 

string die DokumentendId des Dokuments in dem der Text gespeichert wurde.

</details>

### `string saveBinary(agentId, containerId, name, content, documentId)`

speichert ein Binärdokument in der Wissensdatenbank. Der Content ist ein Base64 string, der an den internen Document Parser übergeben wird. Der DocumentParser liest die textuellen Inhalten aus dem Dokument aus. Handelt es sich um Dateiformate, welche über die Wissensdatenbankskonfiguration am Agent konfiguriert sind, werden die dort definierten Konfigurationen verwendet um das Dokument auszulesen (Computer Vision, Transkription).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| agentId | number | Die ID des Agent, in dem das Dokument gespeichert werden soll. |
| containerId | number | Die ID des Containers, in dem das Dokument gespeichert werden soll. |
| name | string | der Name des Dokuments. **Wichtig:** Es muss die Dateiendung mit übergeben werden, ansonsten kann die Plattform nicht auslesen um welches Dokumentenformat es sich handelt. |
| content | string (bae64) | Der Inhalt des Dokuments als Base64 string |
| documentId | string (uuid) | Soll ein bestehendes Dokument aktualisiert werden, so kann hier die DokumentenID übergeben werden. Ist der Wert ein Leerstring, wird automatisch ein neues Dokument angelegt. |


**Rückgabewert** 

string die DokumentendId des Dokuments in dem der Text gespeichert wurde.

</details>

### `string saveTextByToken(token, agentId, containerId, name, content, documentId)`

Speichert ein Textdokument in der Wissensdatenbank. Das Textdokument wird nicht über die Reader ausgelesen, sondern direkt gechunkt und in die Wissensdatenbank hochgeladen werden.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| token | string | Ein spezielles Access Token, das übergeben wird um die Berechtigung zur Verarbeitung zu prüfen |
| agentId | number | Die ID des Agent, in dem das Dokument gespeichert werden soll. |
| containerId | number | Die ID des Containers, in dem das Dokument gespeichert werden soll. |
| name | string | der Name des Dokuments |
| content | string | der textuelle Inhalt des Dokuments |
| documentId | string (uuid) | Soll ein bestehendes Dokument aktualisiert werden, so kann hier die DokumentenID übergeben werden. Ist der Wert ein Leerstring, wird automatisch ein neues Dokument angelegt. |


**Rückgabewert** 

string die DokumentendId des Dokuments in dem der Text gespeichert wurde.

</details>

### `string saveBinaryByToken(token, agentId, containerId, name, content, documentId)`

speichert ein Binärdokument in der Wissensdatenbank. Der Content ist ein Base64 string, der an den internen Document Parser übergeben wird. Der DocumentParser liest die textuellen Inhalten aus dem Dokument aus. Handelt es sich um Dateiformate, welche über die Wissensdatenbankskonfiguration am Agent konfiguriert sind, werden die dort definierten Konfigurationen verwendet um das Dokument auszulesen (Computer Vision, Transkription).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| token | string | Ein spezielles Access Token, das übergeben wird um die Berechtigung zur Verarbeitung zu prüfen |
| agentId | number | Die ID des Agent, in dem das Dokument gespeichert werden soll. |
| containerId | number | Die ID des Containers, in dem das Dokument gespeichert werden soll. |
| name | string | der Name des Dokuments. **Wichtig:** Es muss die Dateiendung mit übergeben werden, ansonsten kann die Plattform nicht auslesen um welches Dokumentenformat es sich handelt. |
| content | string (bae64) | Der Inhalt des Dokuments als Base64 string |
| documentId | string (uuid) | Soll ein bestehendes Dokument aktualisiert werden, so kann hier die DokumentenID übergeben werden. Ist der Wert ein Leerstring, wird automatisch ein neues Dokument angelegt. |


**Rückgabewert** 

string die DokumentendId des Dokuments in dem der Text gespeichert wurde.

</details>

### `string[] find(searchText, agentId, coontainerId, qualityGate, resultlimit)`

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

### `string[] findByKeyword(searchText, agentId, coontainerId)`

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
| resultlimit | int | die maximale Anzahl an Ergebnissen |

<details>
<summary>

**Rückgabewert** 

</summary>
Array von strings der Suchergebnisse
</details>
