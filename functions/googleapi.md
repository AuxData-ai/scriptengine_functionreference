
# Google Workspace — Generic API

## Integration in ScriptEngine
`getModule("googleapi");`

Bearer-authentifizierte REST-Calls gegen beliebige `*.googleapis.com`-URLs für Funktionen, die in den Domain-Modulen (`googlemail`, `googlecalendar`, `googledrive`) nicht erstklassig abgedeckt sind. Der Token deckt automatisch die Scopes von Mail, Kalender und Drive ab.

## Funktionen

### `string googleapi_getResource(googleApiConfig, url)`

Führt einen Bearer-authentifizierten GET-Call gegen die angegebene Google-API-URL aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|url|string|Die vollständige Google-API-URL.|

**Rückgabewert**
string - Der Response-Body der Google-API.

</details>

### `string googleapi_postResource(googleApiConfig, url, body)`

Führt einen POST-Call mit JSON-Body aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|url|string|Die vollständige Google-API-URL.|
|body|string|Der JSON-Body des Requests.|

**Rückgabewert**
string - Der Response-Body der Google-API.

</details>

### `string googleapi_putResource(googleApiConfig, url, body)`

Führt einen PUT-Call mit JSON-Body aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|url|string|Die vollständige Google-API-URL.|
|body|string|Der JSON-Body des Requests.|

**Rückgabewert**
string - Der Response-Body der Google-API.

</details>

### `string googleapi_deleteResource(googleApiConfig, url)`

Führt einen DELETE-Call aus.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|url|string|Die vollständige Google-API-URL.|

**Rückgabewert**
string - Der Response-Body der Google-API.

</details>

### `GoogleApiConfig Objekt`

Siehe Beschreibung unter [Google Workspace — Mail](./googlemail.md). Im Service-Account-Modus muss die Service-Account-Client-Id im Workspace-Admin-Center für die jeweils benötigten Scopes (Mail, Kalender, Drive) autorisiert sein. Ohne diese Freigabe schlägt der Token-Mint mit `unauthorized_client` fehl.
