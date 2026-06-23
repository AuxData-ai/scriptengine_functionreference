# Organisation Modul

Das Organisation-Modul stellt die komplette Konfiguration der ausführenden Organisation lesend zur Verfügung. Es eignet sich besonders für den direkten Zugriff auf Connector-Konfigurationen (Microsoft Graph, Teams Bot) ohne zusätzliche Umgebungsvariablen.

Modul-Name für `getModule("organisation")`

## Funktionen

### `organisation_getOrganisation()`

**Parameter:** keine

**Rückgabe:** Ein Objekt mit allen Feldern der Organisation. Die wichtigsten:

| Feld | Typ | Bedeutung |
| --- | --- | --- |
| `Id` | number | Eindeutige Organisations-Id |
| `Name` | string | Anzeigename |
| `EMail`, `Phone`, `Web` | string | Kontaktdaten |
| `Street`, `Postcode`, `City`, `Country` | string | Adresse |
| `Active` | bool | Organisation aktiv |
| `Env` | object | Organisationsweite Umgebungsvariablen (Key → Value) |
| `EnvConfidential` | array | Namen der vertraulichen Env-Keys |
| `ADConfig` | object | Active-Directory- und Microsoft-Graph-Konfiguration |
| `ADConfig.MsGraphTenantId` | string | Microsoft Graph Tenant Id |
| `ADConfig.MsgraphClientId` | string | Microsoft Graph Client Id |
| `ADConfig.MsgraphClientSecret` | string | Microsoft Graph Client Secret |
| `TeamsBotConfig` | object | Microsoft Teams Bot Konfiguration |
| `TeamsBotConfig.MsAppId` | string | Teams App Id |
| `TeamsBotConfig.MsAppPassword` | string | Teams App Password |


## Beispiel

```javascript
function myFunc() {
    getModule("organisation");
    var org = organisation_getOrganisation();

    var tenantId  = org.ADConfig.MsGraphTenantId;
    var clientId  = org.ADConfig.MsgraphClientId;
    var clientSec = org.ADConfig.MsgraphClientSecret;

    // direkt für einen MS-Graph-Aufruf verwenden, ohne Umweg über
    // organisationsweite Umgebungsvariablen
    return tenantId + "/" + clientId;
}
myFunc();
```

## Sicherheitshinweis

`MsgraphClientSecret`, `MsAppPassword` sowie alle als vertraulich markierten Env-Werte (`EnvConfidential`) sind im zurückgegebenen Objekt im Klartext enthalten. Schreibe diese Werte nicht in Logs, Antworten oder ungesicherte externe Aufrufe.

## Read-only

Das Objekt ist eine Kopie der internen Organisation. Veränderungen im Skript (`org.Name = "..."`) wirken sich nicht auf die Datenbank aus.