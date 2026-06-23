# Teams Bot Modul

Das Teams-Bot-Modul versendet proaktiv Microsoft-Teams-Chat-Nachrichten an einen Benutzer über das Teams Bot Framework. Die Bot-Zugangsdaten werden automatisch aus der Organisations-Konfiguration (`TeamsBotConfig` + `ADConfig.MsGraphTenantId`) gelesen — es müssen keine Credentials übergeben werden.

Modul-Name für `getModule("teamsbot")`

## Voraussetzung

Eine proaktive Nachricht kann nur an Benutzer gesendet werden, die dem Bot **mindestens einmal selbst geschrieben** haben. Erst dadurch wird die nötige Conversation-Referenz gespeichert. Ist keine Referenz vorhanden, liefert die Funktion einen Fehler zurück.

## Funktionen

### `teamsbot_sendMessage(recipient, text [, html])`

Sendet eine Nachricht an einen Benutzer (per UPN/E-Mail identifiziert).

**Parameter:**

| Parameter | Typ | Bedeutung |
| --- | --- | --- |
| `recipient` | string | UPN / E-Mail des Empfängers, z.B. `"max.mustermann@firma.de"` |
| `text` | string | Nachrichtentext |
| `html` | bool | optional (Default `false`); `true` → HTML-Subset (Teams `textFormat: "xml"`), sonst Plaintext |

**Rückgabe:** `true` bei Erfolg; im Fehlerfall eine Fehlermeldung (z.B. Bot inaktiv, Empfänger unbekannt, keine Conversation gespeichert).

### `teamsbot_sendMessageToConversation(conversationId, serviceUrl, text [, html])`

Sendet eine Nachricht direkt in eine bereits bekannte Teams-Conversation.

**Parameter:**

| Parameter | Typ | Bedeutung |
| --- | --- | --- |
| `conversationId` | string | Teams Conversation-Id |
| `serviceUrl` | string | Bot-Framework Service-URL der Conversation |
| `text` | string | Nachrichtentext |
| `html` | bool | optional (Default `false`); siehe oben |

**Rückgabe:** `true` bei Erfolg; sonst Fehlermeldung.

## Beispiel

```javascript
function notifyUser() {
    getModule("teamsbot");
    var ok = teamsbot_sendMessage("max.mustermann@firma.de", "Dein Report ist fertig ✅");
    return ok;
}
notifyUser();
```

## Sicherheitshinweis

Sende nur Nachrichten, die für Menschen bestimmt sind. Die Nutzung von Teams als Log-/Datenablage ist nicht zulässig.
