
# Google Workspace — Drive

## Integration in ScriptEngine
`getModule("googledrive");`

Native Google-Doc-Formate (Docs, Sheets, Slides) liefern bei `getFileContent` einen Fehler — verwende stattdessen `exportFile` mit dem gewünschten Ziel-MIME-Type (z. B. `"application/pdf"`).

## Funktionen

### `drivefile[] googledrive_listFiles(googleApiConfig, folderId, query, maxCount)`

Listet die Dateien im angegebenen Ordner auf, optional gefiltert über die Drive-`q=`-Syntax.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|folderId|string|Die Id des Ordners (`""` = root).|
|query|string|Optionaler Drive-Query, z. B. `"mimeType='text/plain'"`.|
|maxCount|number|Die maximale Anzahl an Ergebnissen.|

**Rückgabewert**
Array von DriveFile Objekten.

</details>

### `string googledrive_findFolderByPath(googleApiConfig, path)`

Löst einen Slash-getrennten Pfad zu einer Ordner-Id auf. `""` liefert `"root"` zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|path|string|Der Pfad, z. B. `"Documents/Reports/2026"`.|

**Rückgabewert**
string - Die Id des gefundenen Ordners. Fehler, wenn ein Pfadsegment nicht existiert.

</details>

### `drivefile googledrive_getFileMetadata(googleApiConfig, fileId)`

Liefert die Metadaten einer Datei.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|fileId|string|Die eindeutige Id der Datei.|

**Rückgabewert**
Ein DriveFile Objekt.

</details>

### `string googledrive_getFileContent(googleApiConfig, fileId)`

Lädt den binären Inhalt der Datei und liefert ihn Base64-codiert zurück. Native Google-Doc-Formate werden mit Fehler abgelehnt — verwende `exportFile`.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|fileId|string|Die eindeutige Id der Datei.|

**Rückgabewert**
string - Base64-codierter Inhalt der Datei.

</details>

### `string googledrive_uploadFile(googleApiConfig, folderId, name, base64Content, mimeType)`

Lädt eine neue Datei in den angegebenen Ordner hoch.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|folderId|string|Die Id des Zielordners (`""` = root).|
|name|string|Der Dateiname.|
|base64Content|string|Der Inhalt der Datei als Base64-String.|
|mimeType|string|Der MIME-Type der Datei (z. B. `"text/plain"`).|

**Rückgabewert**
string - Die Id der neu angelegten Datei.

</details>

### `bool googledrive_updateFileContent(googleApiConfig, fileId, base64Content)`

Ersetzt den Inhalt einer bestehenden Datei.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|fileId|string|Die eindeutige Id der Datei.|
|base64Content|string|Der neue Inhalt der Datei als Base64-String.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googledrive_deleteFile(googleApiConfig, fileId)`

Löscht die Datei dauerhaft (Drive verschiebt nicht in den Papierkorb).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|fileId|string|Die eindeutige Id der Datei.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `string googledrive_createFolder(googleApiConfig, parentId, name)`

Legt einen neuen Ordner unter dem angegebenen Eltern-Ordner an.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|parentId|string|Die Id des Eltern-Ordners (`""` = root).|
|name|string|Der Anzeigename des neuen Ordners.|

**Rückgabewert**
string - Die Id des neu angelegten Ordners.

</details>

### `string googledrive_exportFile(googleApiConfig, fileId, exportMimeType)`

Exportiert ein natives Google-Dokument (Doc, Sheet, Slide) in das gewünschte Format und liefert das Ergebnis Base64-codiert zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|fileId|string|Die eindeutige Id der Datei.|
|exportMimeType|string|Ziel-MIME-Type, z. B. `"application/pdf"` oder `"application/vnd.openxmlformats-officedocument.wordprocessingml.document"`.|

**Rückgabewert**
string - Base64-codierter Inhalt der exportierten Datei.

</details>

### `GoogleApiConfig Objekt`

Siehe Beschreibung unter [Google Workspace — Mail](./googlemail.md). Identische Struktur — Modus A (Service Account) verwendet hier den Scope `https://www.googleapis.com/auth/drive`.

### `DriveFile Objekt`

Das Datei-/Ordner-Objekt, das von Google Drive zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Id|string|Die eindeutige Id der Datei oder des Ordners.|
|Name|string|Der Dateiname.|
|MimeType|string|Der MIME-Type der Datei. Ordner haben den Wert `"application/vnd.google-apps.folder"`.|
|Size|number|Die Größe der Datei in Bytes.|
|ParentId|string|Die Id des Eltern-Ordners (erste Eintrag in `Parents`).|
|CreatedTime|string|Erstellungszeitpunkt im ISO-8601-/RFC-3339-Format.|
|ModifiedTime|string|Letzter Änderungszeitpunkt im ISO-8601-/RFC-3339-Format.|
|WebViewLink|string|URL zum Öffnen der Datei im Drive-Web-UI.|

</details>
