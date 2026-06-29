
# MS Graph API

## Integration in ScriptEngine
`getModule("graphapi");`

## Funktionen

### `string graphapi_getResource(graphApiConfig, url)`

fragt eine Resource aus der MS Graph API an.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|url|string|Die GraphAPI Url|

**Rückgabewert**
string - Der Response der GraphAPI.

</details>

### `string graphapi_deleteResource(graphApiConfig, url)`

löscht eine Resource aus der MS Graph API (z.B. Datei im Sharepooint Ordner).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|url|string|Die GraphAPI Url|

**Rückgabewert**
string - Der Response der GraphAPI.
</details>

### `string graphapi_postResource(graphApiConfig, url, body)`

legt eine neue Resource in der MS Graph API an.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|url|string|Die GraphAPI Url|
|body|string|Der Body der an die GraphAPI übergeben wird|

**Rückgabewert**
string - Der Response der GraphAPI.

</details>

### `string graphapi_putResource(graphApiConfig, url, body)`

aktualisiert eine Resource in der MS Graph API.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|url|string|Die GraphAPI Url|
|body|string|Der Body der an die GraphAPI übergeben wird|

**Rückgabewert**
string - Der Response der GraphAPI.

</details>

### `string graphapi_send(graphApiConfig, mail)`

versendet eine Mail

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mail|Objekt|Das zu versendende [Mail Objekt](#mail-objekt). `To`, `CC` und `BCC` werden als Empfänger gesetzt.|

> **Hinweis zur Schreibweise:** Die Schlüssel des Mail-Objekts müssen exakt den Feldnamen entsprechen (`To`, `CC`, `BCC`, `Subject`, `Message`, …). Abweichende Schreibweisen (z. B. `cc` oder `bcc`) werden ohne Fehlermeldung ignoriert, sodass die betroffenen Empfänger nicht angeschrieben werden.

**Beispiel**

```javascript
var mail = {
    To:      ["empfaenger@example.com"],
    CC:      ["kopie@example.com"],
    BCC:     ["blindkopie@example.com"],
    Subject: "Test",
    Message: "Hallo",
    Html:    false
};
graphapi_send(graphApiConfig, mail);
```

**Rückgabewert**
bool - Flag ob Operation erfolgreich oder nicht

</details>

### `mail[] graphapi_getNewFromInbox(graphApiConfig, maxCount)`

liest maxCount neue Mails aus der Inbox aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|maxCount|number|Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.|

> **Hinweis zu Empfängern:** In den ausgelesenen Mails sind `To` und `CC` als String-Arrays befüllt. `BCC` ist bei empfangenen Mails immer leer, da die GraphApi BCC-Empfänger nicht zurückliefert.

**Beispiel**

```javascript
var mails = graphapi_getNewFromInbox(graphApiConfig, 10);
for (var i = 0; i < mails.length; i++) {
    var to = mails[i].To;  // Array der To-Empfänger
    var cc = mails[i].CC;  // Array der CC-Empfänger
    // mails[i].BCC -> bei empfangenen Mails immer leer
}
```

**Rückgabewert**

</summary>
 Array von Mails, die gefunden wurden.
</details>

### `mail[] graphapi_getNewFromFolder(graphApiConfig, folderId, maxCount)`

Liest maxCount neue Mails aus einem Ordner des Mailaccounts aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|folderId|string| Die ID des Mail Ordners. Diese kann über den GraphExplorer abgefragt werden Posteingang = "inbox" https://developer.microsoft.com/en-us/graph/graph-explorer|
|maxCount|number|Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.|

**Rückgabewert**
Array von Mails, die gefunden wurden.
</details>

### `mail[] graphapi_getByCriteria(graphApiConfig, filter, folderId, maxCount)`

Lies maxCount Mails aus, die dem Filterkriterium entsprechen.

> **Hinweis:** Die Reihenfolge ist `filter` **vor** `folderId` — abweichend von `googlemail_getByCriteria` (dort zuerst der Ordner). Das Datum im Filter im UTC-Format mit `Z` angeben (z. B. `receivedDateTime ge 2026-06-24T00:00:00Z`).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|filter|string|Der Filter nach graphAPI Nomenklatur Beispiel: isRead ne true (ungelesene E-Mails). Datumswerte im UTC-Format mit `Z`, z. B. `receivedDateTime ge 2026-06-24T00:00:00Z and receivedDateTime le 2026-06-24T23:59:59Z`.|
|folderId|string| Die ID des Mail Ordners. Diese kann über den GraphExplorer abgefragt werden Posteingang = "inbox" https://developer.microsoft.com/en-us/graph/graph-explorer|
|maxCount|number|Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.|

**Beispiele für Filter**

| Ziel | Filter |
| ------ | ------ |
| Ungelesene Mails | `isRead ne true` |
| Mails eines Tages | `receivedDateTime ge 2026-06-24T00:00:00Z and receivedDateTime le 2026-06-24T23:59:59Z` |
| Mails eines Absenders | `startswith(from/emailAddress/address, 'max@firma.de')` |

> **Absender-Filter — `startswith` statt `eq`:** Microsoft Graph liefert bei `from/emailAddress/address eq '…'` häufig **null Treffer**, obwohl passende Mails existieren (bekanntes serverseitiges Graph-Verhalten). Verwende stattdessen `startswith(from/emailAddress/address, '…')` mit der vollständigen Adresse — das wirkt wie ein exakter Treffer, nutzt aber den Operator, den Graph zuverlässig auswertet.

> **Empfänger-Filter:** Das Filtern nach `toRecipients`/`ccRecipients` über `$filter` ist bei Graph unzuverlässig (liefert oft Fehler oder keine Treffer) und wird von dieser Funktion nicht zuverlässig unterstützt.

> **Sortierung:** Standardmäßig werden die Treffer nach `receivedDateTime` absteigend (neueste zuerst) sortiert. Bei Filtern auf `from`, `toRecipients` oder `ccRecipients` lehnt Graph diese Sortierung ab ("The restriction or sort order is too complex to evaluate."); für diese Filter wird daher **ohne** feste Sortierung abgefragt.

**Rückgabewert**
Array von Mails, die gefunden wurden.
</details>

### `mail[] graphapi_searchMails(graphApiConfig, searchTerm, folderId, maxCount)`

Volltextsuche über die Nachrichten eines Ordners. Im Gegensatz zu `getByCriteria` (das nur `$filter` nutzt und den Body **nicht** durchsuchen kann) verwendet diese Funktion Graphs `$search` und durchsucht standardmäßig u. a. **Betreff und Body** (sowie Absender und Empfänger).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|searchTerm|string|Der Suchbegriff. Eine einfache Phrase durchsucht Betreff, Body, Absender und Empfänger. Gezielt sind auch KQL-Eigenschaften möglich, z. B. `subject:Rechnung` oder `body:Vertrag`. Die Anführungszeichen um den Suchwert setzt die Funktion selbst.|
|folderId|string|Die ID des Mail Ordners (Posteingang = "inbox").|
|maxCount|number|Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.|

**Beispiele**

```javascript
// Volltextsuche in Betreff + Body
var mails = graphapi_searchMails(graphConfig, "Rechnung März", "inbox", 100);

// gezielt nur im Betreff
var betreff = graphapi_searchMails(graphConfig, "subject:Angebot", "inbox", 100);
```

> **Sortierung:** `$search` lässt sich nicht mit `$orderby` kombinieren — Graph liefert die Treffer nach **Relevanz** sortiert, nicht nach Datum.

> **`$search` vs. `$filter`:** Für exakte Kriterien (ungelesen, Datum, Absender) nutze `getByCriteria`. Für eine Textsuche im Mailinhalt nutze `searchMails`. Beides gleichzeitig (Suche **und** Filter) unterstützt Graph für Mails nicht.

**Rückgabewert**
Array von Mails, die gefunden wurden.
</details>

### `bool graphapi_move(graphApiConfig, mailId, destinationFolder)`

verschiebt die Mail mit mailId in den Zielordner

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string| Die eindeutige Id der Mail|
|destinationFolder|string|Der Name des ordners in den die Mail verschoben werden soll.|

**Rückgabewert**
bool ob die Aktion erfolgreich war.
</details>

### `bool graphapi_delete(graphApiConfig, mailId)`

löscht die Mail mit der entsprechenden ID.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string| Die eindeutige Id der Mail|

**Rückgabewert**
bool ob die Aktion erfolgreich war.

</details>

### `bool graphapi_createDraft(graphApiConfig, mail)`

Erzeugt einen E-Mail Entwurf im Postfach. Ist im Mail-Objekt eine `MailServerId` (Id einer bestehenden Nachricht) gesetzt, wird der Entwurf automatisch als Antwort auf diese Nachricht angelegt. Ohne `MailServerId` wird ein komplett neuer Entwurf (mit `Subject`, `Message`, `To`/`CC`/`BCC`) im Drafts-Ordner erstellt.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mail|Objekt|Das Mail Objekt. Optional kann `MailServerId` auf die Id einer bestehenden Nachricht gesetzt werden, um einen Antwortentwurf zu erzeugen; ist sie leer/ungesetzt, wird ein neuer Entwurf aus `Subject`, `Message` und den Empfängerfeldern erstellt.|

**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `bool graphapi_changeReadState(graphApiConfig, mailId, readState)`

Ändert den "Gelesen" Status einer Mail.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string| Die eindeutige Id der Mail|
|readState|bool|Aufgelesen setzen = true. Auf Ungelesen setzen = false|

**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `bool graphapi_setLabel(graphApiConfig, mailId, label)`

Setzt die Outlook-Kategorie (Label) einer Mail. Vorhandene Kategorien werden überschrieben — die Mail trägt anschließend genau diese eine Kategorie. Existiert die Kategorie noch nicht in der Master Category List (MCL) des Users, wird sie automatisch dort angelegt (best effort, mit einer aus dem Namen abgeleiteten Farbe). Dadurch wird die Kategorie in Outlook — auch im Posteingang — zuverlässig angezeigt. Schlägt das Anlegen fehl (z.B. fehlende Berechtigung `MailboxSettings.ReadWrite`), wird die Kategorie dennoch gesetzt und der Fehler nur protokolliert.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string|Die eindeutige Id der Mail|
|label|string|Name der Outlook-Kategorie, die gesetzt werden soll. Darf nicht leer sein.|

**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `bool graphapi_addLabel(graphApiConfig, mailId, label)`

Fügt der Mail eine Outlook-Kategorie (Label) zusätzlich hinzu. Vorhandene Kategorien bleiben erhalten. Ist die Kategorie bereits gesetzt, ist der Aufruf ein No-op (Vergleich case-sensitive). Existiert die Kategorie noch nicht in der Master Category List (MCL) des Users, wird sie automatisch dort angelegt (best effort, mit einer aus dem Namen abgeleiteten Farbe). Dadurch wird die Kategorie in Outlook — auch im Posteingang — zuverlässig angezeigt. Schlägt das Anlegen fehl (z.B. fehlende Berechtigung `MailboxSettings.ReadWrite`), wird die Kategorie dennoch gesetzt und der Fehler nur protokolliert.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string|Die eindeutige Id der Mail|
|label|string|Name der Outlook-Kategorie, die hinzugefügt werden soll. Darf nicht leer sein.|

**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `bool graphapi_removeLabel(graphApiConfig, mailId, label)`

Entfernt eine Outlook-Kategorie (Label) von der Mail. Andere Kategorien bleiben erhalten. Ist die angegebene Kategorie nicht gesetzt, ist der Aufruf ein No-op und gibt erfolgreich zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string|Die eindeutige Id der Mail|
|label|string|Name der Outlook-Kategorie, die entfernt werden soll. Darf nicht leer sein.|

**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `string[] graphapi_getLabels(graphApiConfig, mailId)`

Liest die gesetzten Outlook-Kategorien (Labels) einer Mail aus. Gegenstück zu `setLabel`/`addLabel`/`removeLabel`. Hinweis: Bei Mails, die über `getNewFromInbox`/`getNewFromFolder`/`getByCriteria` geladen wurden, sind die Labels bereits im Feld `Labels` des Mail-Objekts enthalten — diese Funktion wird nur benötigt, wenn lediglich die `mailId` vorliegt.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mailId|string|Die eindeutige Id der Mail (MailServerId).|

**Rückgabewert**

string[] - Die Liste der gesetzten Outlook-Kategorien (Labels). Hat die Mail keine Kategorien, wird ein leeres Array zurückgegeben.
</details>

### `bool graphapi_ensureCategory(graphApiConfig, name, color)`

Stellt sicher, dass eine Kategorie in der Master Category List (MCL) des Users existiert. Fehlt sie, wird sie angelegt; existiert sie bereits, ist der Aufruf ein No-op (case-sensitiver Namensvergleich). Wird benötigt, damit Outlook die Kategorie — insbesondere im Posteingang — farbig anzeigt. Erfordert die Berechtigung `MailboxSettings.ReadWrite` (Application).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|name|string|Name der Kategorie, die in der MCL sichergestellt werden soll. Darf nicht leer sein.|
|color|string|Outlook-Farbe (`"none"`, `"preset0"` … `"preset24"`). Bei leerem String (`""`) wird eine deterministische, aus dem Namen abgeleitete Preset-Farbe vergeben.|

**Rückgabewert**

bool - `true` bei Erfolg. Bei API-Fehlern wird ein Fehler geworfen, der im Skript abgefangen werden kann.
</details>

### `MasterCategory[] graphapi_getMasterCategories(graphApiConfig)`

Liest die Master Category List (MCL) des Users — alle dort definierten Kategorien mit Namen und Farbe. Erfordert mindestens `MailboxSettings.Read` (Application).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|

**Rückgabewert**

Array von MasterCategory-Objekten (siehe unten). Ist die MCL leer: leeres Array. Bei API-Fehlern wird ein Fehler geworfen, der im Skript abgefangen werden kann.
</details>

### `MasterCategory Objekt`

Das Ergebnis-Objekt eines einzelnen MCL-Eintrags aus `graphapi_getMasterCategories`.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Name|string|Anzeigename der Kategorie.|
|Color|string|Outlook-Farbe der Kategorie (`"none"`, `"preset0"` … `"preset24"`).|
</details>

### `string graphapi_findFolderId(graphApiConfig, folderPath)`

Löst einen punktseparierten Ordner-Pfad (z.B. `"Posteingang.Info"`) zur entsprechenden Folder-ID auf. Das erste Segment ist der Top-Level-Ordner, weitere Segmente werden als Unterordner aufgelöst.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|folderPath|string|Der Folder-Pfad mit `.` als Trennzeichen (z.B. `"Posteingang.Info"`). Erstes Segment = Top-Level-Ordner, weitere Segmente = Unterordner.|

**Rückgabewert**
string - Die eindeutige ID des Folders. Wird ein Segment des Pfades nicht gefunden, wird ein Fehler geworfen, der im Skript abgefangen werden kann.

</details>

### `MailFolderInfo[] graphapi_listFolders(graphApiConfig)`

Liefert alle Mail-Folder des Postfachs rekursiv als flache Liste in Depth-First-Reihenfolge (Eltern vor Kindern). Jeder Eintrag enthält die Folder-ID, den Anzeigenamen und den vollständigen Pfad in Punktnotation. Bekannte Einschränkung: pro Ebene werden maximal 999 Folder ausgelesen.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|

**Rückgabewert**
Array von MailFolderInfo-Objekten (siehe unten). Bei leerem Postfach: leeres Array. Bei API-Fehlern wird ein Fehler geworfen, der im Skript abgefangen werden kann.

</details>

### `MailFolderInfo Objekt`

Das Ergebnis-Objekt eines einzelnen Folders aus `graphapi_listFolders`.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Id|string|Die eindeutige MS-Graph-Folder-ID. Direkt nutzbar für andere graphapi_*-Funktionen.|
|DisplayName|string|Der Anzeigename des Folders.|
|Path|string|Der vollständige Pfad vom Root mit `.` als Trennzeichen (z.B. `"Posteingang.Info"`). Konsistent zu graphapi_findFolderId.|

</details>

### `GraphApiConfig Objekt`

Das Konfigurationsobjekt für den GraphApi Zugriff

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|TenantId|string|Die UUID des Tenant|
|ClientId|string|Die UUID des Clients in MS Entra|
|ClientSecret|string|Das Secret des Clients|
|Account|string|Der Account, für den die Aktion durchgeführt wird. Im Normalfall eine E-Mail-Adresse|

</details>

### `Mail Objekt`

Das Mailobjekt das an die GraphApi gesendet wird oder von dieser als Ergebnis zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|From|string|Die E-Mail-Adresse des Absenders der Mail|
|FromName|string|Der Anzeigename des Absenders (wie in Outlook angezeigt). Wird beim Auslesen von Mails automatisch befüllt, sofern der Absender einen Anzeigenamen gesetzt hat – andernfalls leer.|
|To|string[]|Die Empfänger der Mail. Wird beim Auslesen von Mails automatisch befüllt.|
|CC|string[]|Die CC-Empfänger der Mail. Wird beim Versenden berücksichtigt und beim Auslesen von Mails automatisch befüllt.|
|BCC|string[]|Die BCC-Empfänger der Mail. Wird beim Versenden berücksichtigt. Beim Auslesen empfangener Mails nicht verfügbar, da die GraphApi BCC-Empfänger nicht zurückliefert.|
|Subject|string|Der Betreff der Mail|
|Message|string|Die Nachricht der Mail|
|Attachments|map[string] string |Die Anhänge der Mail Key = Dateiname; value = base64 Inhalt|
|Id|number|Dieser Wert wird von der GraphApI nicht verwendet, da diese mit UUIDs arbeitet. Als Parameter ist immer die MailServerId zu übergeben.|
|Folder|string|Der Ordner in dem die Mail enthalten ist|
|Date|string|Das Sendedatum der Mail|
|Html|bool|Kennzeichen ob die Nachricht als HTML Dokument vorliegt|
|MailServerId|string|Die eindeutige Id der Mail, die zur Bearbeitung in der GraphApi verwendet werden muss.|
|IsRead|bool|Kennzeichen ob die Mail bereits gelesen wurde.|
|Labels|string[]|Die gesetzten Outlook-Kategorien (Labels) der Mail. Wird beim Auslesen von Mails automatisch befüllt.|

</details>
