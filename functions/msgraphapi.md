---
title: MS Graph API - Mail
---
## Integration in ScriptEngine

`getModule("graphapi");`

## Funktionen

### `bool send(graphApiConfig, mail)`

versendet eine Mail

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>Objekt</td>
<td>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</td>
</tr>
<tr>
<td>

### `bool send(graphApiConfig, mail)`
</td>
<td>Objekt</td>
<td>Das Mail-Objekt, das alle Informationen zum Mailversand enthält</td>
</tr>
</table>


**Rückgabewert**

Ob der Versand erfolgreich war.
</details>

### `mail[] getNewFromInbox(graphApiConfig, maxCount)`

liest maxCount neue Mails aus der Inbox aus.

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td></td>
<td>

<details>
<summary>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</summary></details>

</td>
</tr>
<tr>
<td>

### `maxCount`
</td>
<td>int</td>
<td>

<details>
<summary>Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.</summary></details>

</td>
</tr>
</table>


<details>
<summary>

**Rückgabewert**

</summary>
 Array von Mails, die gefunden wurden.
</details>


### `m`

</details>

### `mail[] getNewFromFolder(graphApiConfig, folderName, maxCount)`

Liest maxCount neue Mails aus einem Ordner des Mailaccounts aus.

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>Objekt</td>
<td>

<details>
<summary>

<details>
<summary>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</summary></details>

</summary></details>

</td>
</tr>
<tr>
<td>

### `folderId`
</td>
<td>string</td>
<td>

Die ID des Mail Ordners. Diese kann über den GraphExplorer abgefragt werden.

Posteingang = "inbox" https://developer.microsoft.com/en-us/graph/graph-explorer
</td>
</tr>
<tr>
<td>

### `maxCount`
</td>
<td>int</td>
<td>Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.</td>
</tr>
</table>


**Rückgabewert**

Array von Mails, die gefunden wurden.
</details>

### `mail[] getByCriteria(graphApiConfig, folderId, filter, maxCount)`

Liest maxCount neue Mails aus einem Ordner des Mailaccounts aus, welche die Filterkriterien erfüllen

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>Objekt</td>
<td>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</td>
</tr>
<tr>
<td>

### `folderId`
</td>
<td>string</td>
<td>

<details>
<summary>

Die ID des Mail Ordners. Diese kann über den GraphExplorer abgefragt werden.

Posteingang = "inbox" https://developer.microsoft.com/en-us/graph/graph-explorer

</summary></details>

</td>
</tr>
<tr>
<td>

### `filter`
</td>
<td>string</td>
<td>Der Filter nach graphAPI Nomenklatur Beispiel: isRead ne true (ungelesene E-Mails)</td>
</tr>
<tr>
<td>

### `maxCount`
</td>
<td>int</td>
<td>Die maximale Anzahl an Ergebnissen. Limitiert auf 1000.</td>
</tr>
</table>


**Rückgabewert** todo

</details>

### `bool move(graphApiConfig, mailId, destinationFolder)`

verschiebt die Mail mit mailId in den Zielordner

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>Objekt</td>
<td>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</td>
</tr>
<tr>
<td>

### `mailId`
</td>
<td>string</td>
<td>Die eindeutige Id der Mail</td>
</tr>
<tr>
<td>

### `destinationFolder`
</td>
<td>string</td>
<td>Der Name des ordners in den die Mail verschoben werden soll.</td>
</tr>
</table>


**Rückgabewert**

bool ob die Aktion erfolgreich war.
</details>

### `bool delete(graphApiConfig, mailId)`

löscht die Mail mit der entsprechenden ID.

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>

<details>
<summary>Objekt</summary></details>

</td>
<td>

<details>
<summary>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</summary></details>

</td>
</tr>
<tr>
<td>

### `mailId`
</td>
<td>string</td>
<td>

<details>
<summary>Die eindeutige Id der Mail</summary></details>

</td>
</tr>
</table>


**Rückgabewert**

<details>
<summary>bool ob die Aktion erfolgreich war.</summary></details>

</details>

### `bool createDraft(graphApiConfig, mail)`

Erzeugt einen E-Mail Entwurf im Postfach. Ist die MailId richtig gesetzt, wird dies automatisch zu einer Antwort-Mail.

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>Objekt</td>
<td>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</td>
</tr>
<tr>
<td>

### `mail`
</td>
<td>Objekt</td>
<td>Das Mail Objekt. Die MailId muss so gesetzt werden, dass sie als Antwortentwurf zu einer bestehenden Mail verwendet werden kann.</td>
</tr>
</table>


**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `bool changeReadState(graphApiConfig, mailId, readState)`

Ändert den "Gelesen" Status einer Mail.

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `graphApiConfig`
</td>
<td>Objekt</td>
<td>Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.</td>
</tr>
<tr>
<td>

### `mailId`
</td>
<td>string</td>
<td>Die eindeutige Identifizierung der Mail</td>
</tr>
<tr>
<td>

### `readState`
</td>
<td>bool</td>
<td>Aufgelesen setzen = true. Auf Ungelesen setzen = false</td>
</tr>
</table>


**Rückgabewert**

Kennzeichen ob Aktion erfolgreich ausgeführt wurde.
</details>

### `GraphApiConfig Objekt`

Das Konfigurationsobjekt für den GraphApi Zugriff

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `TenantId`
</td>
<td>string</td>
<td>Die UUID des Tenant</td>
</tr>
<tr>
<td>

### `ClientId`
</td>
<td>string</td>
<td>Die UUID des Clients in MS Entra</td>
</tr>
<tr>
<td>

### `ClientSecret`
</td>
<td>string</td>
<td>Das Secret des Clients</td>
</tr>
<tr>
<td>

### `Account`
</td>
<td>string</td>
<td>Der Account, für den die Aktion durchgeführt wird. Im Normalfall eine E-Mail-Adresse</td>
</tr>
</table>

</details>

### `Mail Objekt`

Das Mailobjekt das an die GraphApi gesendet wird oder von dieser als Ergebnis zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

<table>
<tr>
<th>Name</th>
<th>Typ</th>
<th>Beschreibung</th>
</tr>
<tr>
<td>

### `From`
</td>
<td>string</td>
<td>Die UUID des Tenant</td>
</tr>
<tr>
<td>

### `To`
</td>
<td>string</td>
<td>Die UUID des Clients in MS Entra</td>
</tr>
<tr>
<td>

### `Subject`
</td>
<td>string</td>
<td>Das Secret des Clients</td>
</tr>
<tr>
<td>

### `Message`
</td>
<td>string</td>
<td>Der Account, für den die Aktion durchgeführt wird. Im Normalfall eine E-Mail-Adresse</td>
</tr>
<tr>
<td>Attachments</td>
<td>

map\[string\]string
</td>
<td>Die Anhänge der Mail Key = Dateiname; value = base64 Inhalt</td>
</tr>
<tr>
<td>Id</td>
<td>unit</td>
<td>Dieser Wert wird von der GraphApI nicht verwendet, da diese mit UUIDs arbeitet. Als Parameter ist immer die MailServerId zu übergeben.</td>
</tr>
<tr>
<td>Folder</td>
<td>string</td>
<td>Der Ordner in dem die Mail enthalten ist</td>
</tr>
<tr>
<td>Date</td>
<td>string</td>
<td>Das Sendedatum der Mail</td>
</tr>
<tr>
<td>Html</td>
<td>bool</td>
<td>Kennzeichen ob die Nachricht als HTML Dokument vorliegt</td>
</tr>
<tr>
<td>MailServerId</td>
<td>string</td>
<td>Die eindeutige Id der Mail, die zur Bearbeitung in der GraphApi verwendet werden muss.</td>
</tr>
<tr>
<td>IsRead</td>
<td>bool</td>
<td>Kennzeichen ob die Mail bereits gelesen wurde.</td>
</tr>
</table>

</details>

<details>
<summary></summary></details>

