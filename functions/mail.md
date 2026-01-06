# Mail Modul (Imap + SMTP)

## Integration in ScriptEngine

`getModule("email");`

### `bool send(mail, smtpConfig)`
sendet eine Mail per SMTP mit der übergebenen SMTP Konfiguration
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| mail | Objekt | Das Mail Objekt, das gesendet werden soll |
| smtpConfig | Objekt | Die Smtp Konfiguration, die herangezogen werden soll um die Mail zu versenden. |

**Rückgabewert**
bool - Operation erfolgreich oder nicht

</details>

### `mail[] getNewFromInbox(imapConfig)`
Liest ein Imap Postfach aus und liefert alle ungelesenen Mails zurück.

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| imapConfig | Objekt | Die IMAP Konfiguration um das Postfach abzufragen |

**Rückgabewert**
Array von Mail Objekten

</details>

### `mail[] getNewFromFolder(imapConfig, folderName)`
Liest ein Imap Postfach aus und liefert alle ungelesenen Mails aus dem angegebenen Ordner zurück.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| imapConfig | Objekt | Die IMAP Konfiguration um das Postfach abzufragen |
|  folderName| string | Der Name des Ordners, der ausgelesen werden soll |

**Rückgabewert**
Array von Mail Objekten

</details>

### `mail[] getByCriteria(imapConfig, criteria)`
Liefert eine Liste aller Mails, die das übergebene Kriterium erfüllen.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| imapConfig | Objekt | Die IMAP Konfiguration um das Postfach abzufragen |
| criteria | string | Das Kriterium das die Mails erfüllen müssen |

**Rückgabewert**
Array von Mail Objekten

</details>

### `bool move(mailId, originFolder, destinationFolder, imapConfig)`
Verschiebt eine Mail im Postfach von einem Ordner in einen anderen.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| mailId | string | Die ID der Mail, die verschoben werden soll |
| originFolder | string | Der Ordner in dem sich die Mail gerade befindet |
| destinationFolder| string | Der Ordner in dem die Mail verschoben werden soll |
| imapConfig| Object | Die IMAP Konfiguration |

**Rückgabewert**
Operation erfolgreich oder nicht

</details>

### `bool delete(mailId, folder, imapConfig)`
todo
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| mailId | string | Die ID der Mail, die gelöscht werden soll |
| folder | string | Der Ordner in dem sich die Mail befindet |
| imapConfig| Object | Die IMAP Konfiguration |

**Rückgabewert**
Operation erfolgreich oder nicht

</details>

### `bool addFlag(mailId, folder, flag, imapConfig)`
Fügt der Mail ein Kennzeichen hinzu.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| mailId | string | Die ID der Mail, die gelöscht werden soll |
| folder | string | Der Ordner in dem sich die Mail befindet |
| flag| string | Kennzeichen, welches der Mail hinzugefügt werden soll (z.B. read) |
| imapConfig| Object | Die IMAP Konfiguration |

**Rückgabewert**
Operation erfolgreich oder nicht

</details>

### Beispiel für SMTP Config

```
var smtpConfig = new Object();
smtpConfig["user"] = "user";
smtpConfig["Password"] = "pwd";
smtpConfig["Smtpserver"] = "127.0.0.1";
smtpConfig["Smtpport"] = "587";
smtpConfig["SenderAddress"] = "sender@sender.de";
```

### Beispiel für IMAP Config

```
var imap = new Object();
imap["User"] = "user";
imap["Password"] = "pwd";
imap["Server"] = "127.0.0.1";
```

### Beispiel für ein MAil Objekt

```
var mailObj = new Object();
var to = new Array();
to.push("receiver@receiver.de");
var from = "%s";
mailObj["From"] = "sender@sender.de";
mailObj["To"] = to;
mailObj["Subject"] = "Das ist der Betreff";
mailObj["Message"] = "Hallo wie geht es Dir?";
```