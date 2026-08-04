
# SharePoint (Microsoft 365)

## Integration in ScriptEngine
`getModule("sharepoint");`

Alle Funktionen erwarten als erstes Argument `spConfig` — ein Objekt mit den
MS-Graph-App-Zugangsdaten (`TenantId`, `ClientId`, `ClientSecret`, `Account`).
Der App-Registrierung müssen im Tenant `Sites.ReadWrite.All` gewährt sein.

Zusätzlich kennt `spConfig` das optionale Feld `Region` — den Geo-Code des
Tenants (z.B. `"EUR"`, `"NAM"`, `"DEU"`). Nur `sharepoint_search` benötigt ihn,
alle übrigen Funktionen ignorieren ihn. Die Feldnamen sind
**case-sensitiv**: `Region` wird übernommen, `region` stillschweigend ignoriert.

```javascript
var spConfig = {
  TenantId: "...", ClientId: "...", ClientSecret: "...",
  Account: "team@contoso.de",
  Region: "EUR"
};
```

## Navigation

### `object sharepoint_getSite(spConfig, hostname, sitePath)`
Löst eine SharePoint-Site auf und liefert `{Id, Name, WebUrl}`.
`hostname` z.B. `contoso.sharepoint.com`, `sitePath` z.B. `marketing`.

### `array sharepoint_listDrives(spConfig, siteId)`
Listet die Dokumentbibliotheken (Drives) einer Site als `[{Id, Name, WebUrl}]`.

### `array sharepoint_listFolder(spConfig, driveId, path)`
Listet Dateien/Ordner eines Drive-Ordners (eine Ebene). `path=""` = Root.

### `array sharepoint_listFilesRecursive(spConfig, driveId)`
Liefert rekursiv alle Dateien eines Drives.

## Dateien

### `string sharepoint_downloadFile(spConfig, driveId, itemId)`
Lädt eine Datei und liefert den Inhalt Base64-kodiert.

### `object sharepoint_uploadFile(spConfig, driveId, folderPath, name, base64Content)`
Legt eine Datei an (einfacher Upload, max. 4 MiB). `folderPath=""` = Root.
Größere Dateien werden mit Fehler abgelehnt.

### `object sharepoint_createFolder(spConfig, driveId, parentPath, name)`
Legt einen Ordner an. `parentPath=""` = Root.

### `bool sharepoint_deleteItem(spConfig, driveId, itemId)`
Löscht Datei oder Ordner.

### `object sharepoint_moveItem(spConfig, driveId, itemId, newParentId, newName)`
Verschiebt/benennt ein Item um. `newName=""` behält den Namen, `newParentId=""`
behält den Ordner.

## Suche

### `array sharepoint_searchDrive(spConfig, driveId, query)`
Sucht Dateien/Ordner nach Name/Inhalt innerhalb eines Drives. Zuverlässig mit
App-only-Zugang.

### `string sharepoint_search(spConfig, query, entityTypes)`
Microsoft-Search über `entityTypes` (z.B. `["driveItem","listItem"]`), liefert
rohen Graph-JSON mit Microsofts Relevanz-Ranking.

**`spConfig.Region` ist hier Pflicht.** Microsoft verlangt bei Suchen mit
Application-Berechtigungen die Angabe der Region; fehlt sie, antwortet Graph mit
`400 — Region is required when request with application permission.` Die
Funktion bricht deshalb schon vorher mit einer entsprechenden Fehlermeldung ab.
Den passenden Wert liefert bei Multi-Geo-Tenants
`siteCollection.dataLocationCode` (`GET /sites/root?$select=siteCollection`); bei
Single-Geo-Tenants ist das Feld leer und der Geo-Code des Tenants ist direkt
einzutragen (europäische Tenants üblicherweise `"EUR"`).

Eine Suche mit Application-Berechtigungen durchsucht geteilte und öffentliche
Inhalte; private OneDrive-Inhalte bleiben außen vor.

**Einschränkung:** Für `driveItem`/`listItem` sind je nach Tenant delegierte
Rechte nötig; mit App-only-Zugang kann die Anfrage `403` liefern.

## Listen

### `array sharepoint_listLists(spConfig, siteId)`
Listet die Listen einer Site als `[{Id, Name, WebUrl}]`.

### `string sharepoint_getListItems(spConfig, siteId, listId, filter)`
Liest Listen-Items inkl. Feldern (roher Graph-JSON). `filter=""` = ohne
OData-`$filter`.

### `string sharepoint_createListItem(spConfig, siteId, listId, fields)`
Legt ein Item an. `fields` ist ein Objekt Spaltenname→Wert.

### `string sharepoint_updateListItem(spConfig, siteId, listId, itemId, fields)`
Aktualisiert die Felder eines Items.

### `bool sharepoint_deleteListItem(spConfig, siteId, listId, itemId)`
Löscht ein Listen-Item.
