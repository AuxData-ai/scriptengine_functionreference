
# Google Workspace — Mail (Gmail)

## Integration in ScriptEngine
`getModule("googlemail");`

Der Folder-Begriff entspricht in Gmail einem Label. `move` legt nicht existierende Ziel-Labels automatisch an. `delete` verschiebt die Mail in den Papierkorb (reversibel).

## Funktionen

### `bool googlemail_send(googleApiConfig, mail)`

Versendet eine Mail über das Gmail-Konto des konfigurierten Accounts.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|mail|Objekt|Das Mail Objekt, das versendet werden soll.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `mail[] googlemail_getNewFromInbox(googleApiConfig, maxCount)`

Liest bis zu `maxCount` ungelesene Mails aus der INBOX aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|maxCount|number|Die maximale Anzahl an Ergebnissen.|

**Rückgabewert**
Array von Mail Objekten.

</details>

### `mail[] googlemail_getNewFromFolder(googleApiConfig, label, maxCount)`

Liest bis zu `maxCount` ungelesene Mails aus dem angegebenen Label aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|label|string|Das Gmail-Label (Anzeigename, z. B. `"Processed"` oder `"INBOX"`).|
|maxCount|number|Die maximale Anzahl an Ergebnissen.|

**Rückgabewert**
Array von Mail Objekten.

</details>

### `mail[] googlemail_getByCriteria(googleApiConfig, label, query, maxCount)`

Sucht Mails über die Gmail `q=`-Suchsyntax, eingeschränkt auf das angegebene Label.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|label|string|Das Gmail-Label, auf das die Suche eingeschränkt werden soll.|
|query|string|Filter im Gmail-Format, z. B. `"from:billing@example.com is:unread"`.|
|maxCount|number|Die maximale Anzahl an Ergebnissen.|

**Rückgabewert**
Array von Mail Objekten.

</details>

### `bool googlemail_move(googleApiConfig, mailId, destinationLabel)`

Verschiebt die Mail aus der INBOX in das angegebene Label. Existiert das Ziel-Label noch nicht, wird es automatisch angelegt.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|mailId|string|Die eindeutige Id der Mail (`MailServerId`).|
|destinationLabel|string|Der Anzeigename des Ziel-Labels.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlemail_delete(googleApiConfig, mailId)`

Verschiebt die Mail in den Gmail-Papierkorb (reversibel).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|mailId|string|Die eindeutige Id der Mail (`MailServerId`).|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlemail_changeReadState(googleApiConfig, mailId, read)`

Setzt den Lese-Status einer Mail (toggelt das `UNREAD`-Label).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|mailId|string|Die eindeutige Id der Mail (`MailServerId`).|
|read|bool|`true` = als gelesen markieren, `false` = als ungelesen markieren.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlemail_createDraft(googleApiConfig, mail)`

Erzeugt einen Antwort-Entwurf zu einer bestehenden Mail. `mail.MailServerId` muss die Id der Original-Mail enthalten.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|mail|Objekt|Das Mail Objekt. `MailServerId` referenziert die Original-Mail.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `GoogleApiConfig Objekt`

Das Konfigurationsobjekt für den Google Workspace API Zugriff. Es unterstützt zwei Authentifizierungs-Modi: Service Account mit Domain-wide Delegation oder OAuth2 mit Refresh-Token.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Account|string|Die E-Mail-Adresse des Workspace-Users, in dessen Namen Aktionen ausgeführt werden.|
|ServiceAccountJson|string|Inhalt der JSON-Key-Datei eines Service-Accounts (Modus A: Domain-wide Delegation).|
|ClientId|string|OAuth2 Client-Id (Modus B).|
|ClientSecret|string|OAuth2 Client-Secret (Modus B).|
|RefreshToken|string|OAuth2 Refresh-Token (Modus B).|

Im Service-Account-Modus muss die Service-Account-Client-Id im Workspace-Admin-Center unter „Domain-wide delegation" für den Scope `https://www.googleapis.com/auth/gmail.modify` autorisiert sein.

</details>

### `Mail Objekt`

Das Mailobjekt, das an Gmail gesendet wird oder von Gmail zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|From|string|Der Absender der Mail (in Antwort-Objekten gefüllt; beim Senden ignoriert — Gmail nutzt den authentifizierten User).|
|To|string[]|Die Empfänger der Mail.|
|Subject|string|Der Betreff der Mail.|
|Message|string|Die Nachricht der Mail.|
|Attachments|map[string] string|Die Anhänge der Mail. Key = Dateiname; Value = Base64-Inhalt.|
|Folder|string|Das Gmail-Label, in dem die Mail enthalten ist (z. B. `"INBOX"`).|
|Date|string|Das Sendedatum der Mail.|
|Html|bool|Kennzeichen ob die Nachricht als HTML-Dokument vorliegt.|
|MailServerId|string|Die eindeutige Gmail-Id der Mail.|
|IsRead|bool|Kennzeichen ob die Mail bereits gelesen wurde.|

</details>
