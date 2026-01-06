
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

### `mail[] graphapi_getByCriteria(graphApiConfig, folderId, filter, maxCount)`

Liest maxCount neue Mails aus einem Ordner des Mailaccounts aus, welche die Filterkriterien erfüllen

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|folderId|string| Die ID des Mail Ordners. Diese kann über den GraphExplorer abgefragt werden Posteingang = "inbox" https://developer.microsoft.com/en-us/graph/graph-explorer|
|filter|string|Der Filter nach graphAPI Nomenklatur Beispiel: isRead ne true (ungelesene E-Mails)|
|maxCount|number|Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.|

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

Erzeugt einen E-Mail Entwurf im Postfach. Ist die MailId richtig gesetzt, wird dies automatisch zu einer Antwort-Mail.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|mail|Objekt|Das Mail Objekt. Die MailId muss so gesetzt werden, dass sie als Antwortentwurf zu einer bestehenden Mail verwendet werden kann.|

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
|From|string|Der Absender der Mail|
|To|string[]|Die Empfägner der Mail|
|Subject|string|Der Betreff der Mail|
|Message|string|Die Nachricht der Mail|
|Attachments|map[string] string |Die Anhänge der Mail Key = Dateiname; value = base64 Inhalt|
|Id|number|Dieser Wert wird von der GraphApI nicht verwendet, da diese mit UUIDs arbeitet. Als Parameter ist immer die MailServerId zu übergeben.|
|Folder|string|Der Ordner in dem die Mail enthalten ist|
|Date|string|Das Sendedatum der Mail|
|Html|bool|Kennzeichen ob die Nachricht als HTML Dokument vorliegt|
|MailServerId|string|Die eindeutige Id der Mail, die zur Bearbeitung in der GraphApi verwendet werden muss.|
|IsRead|bool|Kennzeichen ob die Mail bereits gelesen wurde.|

</details>