
# MS Graph Planner

## Integration in ScriptEngine
`getModule("graphplanner");`

## Benötigte Berechtigungen (Microsoft 365 / Entra ID)

Der Zugriff erfolgt über eine App-Registrierung in Entra ID (Azure AD) mit **Application-Berechtigungen** (App-only, Client-Secret). Die folgenden Microsoft-Graph-Berechtigungen müssen der App-Registrierung zugewiesen und mit **Administrator-Zustimmung (Admin Consent)** freigeschaltet werden:

| Berechtigung | Anzeigename in Entra | Typ | Wofür |
| ------ | ------ | ------ | ------ |
| `Tasks.ReadWrite.All` | Read and write all tasks | Application | Alle Plan-, Bucket- und Aufgaben-Funktionen (Lesen **und** Schreiben). Nur-Lesen genügt `Tasks.Read.All` („Read all tasks"). |
| `Group.Read.All` | Read all groups | Application | Gruppen-/Plan-Discovery: `listGroups`, `listAllPlans`, `findPlanByName`. Für `createPlan` (Plan wird an einer Gruppe angelegt) wird `Group.ReadWrite.All` benötigt. |
| `User.Read.All` | Read all users' full profiles | Application | Nur für `listPlansForUser` (Gruppenmitgliedschaften des Accounts). Alternativ genügt `GroupMember.Read.All`. |

Die beiden Berechtigungen greifen unabhängig voneinander: Ist nur `Group.Read.All` erteilt, werden die Gruppen zwar gefunden, jede Plan-Abfrage endet aber mit `403 — You do not have the required permissions to access this item.`

Ohne diese Grants antwortet Microsoft Graph mit **HTTP 403**. Die App-Registrierung liefert `tenantId`, `clientId`, `clientSecret`; `account` ist der Benutzer, in dessen Kontext benutzerbezogene Discovery (`listPlansForUser`) läuft. Diese vier Werte werden im `graphApiConfig`-Objekt übergeben.

### Wo werden die Berechtigungen gesetzt?

**Entra Admin Center → Identität → Anwendungen → App-Registrierungen → *deine App* → API-Berechtigungen → „Berechtigung hinzufügen" → „Microsoft Graph" → „Anwendungsberechtigungen" → Kategorie aufklappen (z. B. `Group`) → Berechtigung anhaken.** Anschließend zwingend **„Administratorzustimmung für *Tenant* erteilen"** — ohne erteilten Consent liefert Graph weiterhin 403, obwohl die Berechtigung in der Liste steht.

Wenn eine Berechtigung im Dialog nicht auffindbar ist, liegt es meist an einem dieser Punkte:

- Gesucht unter **Unternehmensanwendungen** statt **App-Registrierungen** — dort gibt es keine Auswahlliste, nur die bereits erteilten Berechtigungen.
- Der Tab **Delegierte Berechtigungen** ist aktiv; dieses Modul arbeitet App-only und braucht **Anwendungsberechtigungen**.
- Das Suchfeld filtert über die Kategorie-Überschriften: `Group.Read.All` komplett eingetippt liefert je nach Portal-Version keinen Treffer — stattdessen `Group` eingeben und die Kategorie aufklappen.
- Gesucht unter **Rollen und Administratoren**: dort steht es nie. Es handelt sich um Graph-API-Berechtigungen der App-Registrierung, nicht um Entra-Verzeichnisrollen.

Alternativen, falls der Tenant-Admin `Group.Read.All` nicht vergeben möchte: die Gruppen-Discovery funktioniert auch mit `GroupMember.Read.All` oder `Directory.Read.All`.

> Hinweis: Microsoft **To Do** / Outlook-Aufgaben (`graphtodo`) sind hierüber **nicht** erreichbar — diese API unterstützt nur delegierte Berechtigungen (kein App-only) und ist daher nicht Teil dieses Moduls.

## Funktionen

### `group[] graphplanner_listGroups(graphApiConfig)`

Liefert alle Microsoft-365-Gruppen des Tenants (Id + Anzeigename). Einstiegspunkt, um Gruppen- und darüber Plan-Ids zu finden.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|

**Rückgabewert**
Array von Group-Objekten `{ Id, DisplayName }`.

**Benötigte Berechtigung (Application):** `Group.Read.All`

</details>

### `planRef[] graphplanner_listPlansForGroup(graphApiConfig, groupId)`

Liefert alle Pläne, die der angegebenen M365-Gruppe gehören.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|groupId|string|Die eindeutige Id der M365-Gruppe.|

**Rückgabewert**
Array von PlanRef-Objekten `{ PlanId, Title, GroupId, GroupName }`. `GroupName` ist bei dieser Funktion leer, da hier nur die (bereits bekannte) `groupId` vorliegt.

**Benötigte Berechtigung (Application):** `Group.Read.All` + `Tasks.Read.All`

</details>

### `planRef[] graphplanner_listAllPlans(graphApiConfig)`

Läuft intern über alle Gruppen des Tenants und liefert alle Pläne mit Id, Titel und Gruppenkontext. Beantwortet „welche Planner gibt es und welche Id haben sie" in einem Aufruf. Liefert eine Gruppe keine Planner-Daten (z. B. weil Planner für sie nicht bereitgestellt ist), wird diese Gruppe übersprungen, ohne den gesamten Aufruf abzubrechen.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|

**Rückgabewert**
Array von PlanRef-Objekten `{ PlanId, Title, GroupId, GroupName }`.

**Benötigte Berechtigung (Application):** `Group.Read.All` + `Tasks.Read.All`

> **Leeres Ergebnis?** Scheitert die Plan-Abfrage bei *jeder* Gruppe, liefert die Funktion keine leere Liste, sondern einen Fehler mit der Ursache der letzten Abfrage. Ein `403` an dieser Stelle bedeutet fast immer, dass zwar `Group.Read.All` erteilt ist (die Gruppen werden ja gefunden), aber `Tasks.Read.All`/`Tasks.ReadWrite.All` fehlt oder ohne Admin-Zustimmung eingetragen wurde. Tenants mit mehr als 999 Microsoft-365-Gruppen werden nur bis zur ersten Seite durchsucht; das wird im Server-Log als Warnung vermerkt.

</details>

### `planRef[] graphplanner_listPlansForUser(graphApiConfig, account)`

Liefert alle Pläne, die der angegebene Account über seine Microsoft-365-Gruppenmitgliedschaften erreichen kann.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|account|string|Der Account (i. d. R. E-Mail-Adresse bzw. User-Id), für den die Pläne ermittelt werden sollen.|

**Rückgabewert**
Array von PlanRef-Objekten `{ PlanId, Title, GroupId, GroupName }`.

**Benötigte Berechtigung (Application):** `User.Read.All` (oder `GroupMember.Read.All`) für die Mitgliedschaften + `Tasks.Read.All` für die Pläne

> **Wie die Pläne ermittelt werden:** Zuerst werden über `/users/{account}/transitiveMemberOf` die Microsoft-365-Gruppen des Accounts gelesen (inkl. verschachtelter Mitgliedschaften, Sicherheits- und Verteilergruppen werden aussortiert), anschließend je Gruppe deren Pläne.
>
> Der naheliegende Endpunkt `/users/{id}/planner/plans` wird bewusst **nicht** verwendet: die benutzerbezogenen Planner-Endpunkte unterstützen laut Graph-Referenz ausschließlich delegierte Berechtigungen (`Application: Not supported.`) und antworten mit einem App-only-Token immer mit `403 — You do not have the required permissions to access this item.`
>
> Daraus folgt ein feiner Unterschied zur Microsoft-Semantik: geliefert werden die über Gruppen erreichbaren Pläne, nicht die individuell *mit dem Benutzer geteilten*.

</details>

### `string graphplanner_findPlanByName(graphApiConfig, groupId, name)`

Sucht innerhalb einer Gruppe den Plan mit dem angegebenen Titel und liefert dessen Id zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|groupId|string|Die eindeutige Id der M365-Gruppe, in der gesucht werden soll.|
|name|string|Der Titel des gesuchten Plans (exakter Vergleich).|

**Rückgabewert**
string - Die eindeutige Id des gefundenen Plans. Wird kein Plan mit diesem Titel gefunden, wird ein Fehler geworfen, der im Skript abgefangen werden kann.

**Benötigte Berechtigung (Application):** `Group.Read.All` + `Tasks.Read.All`

</details>

### `plan graphplanner_getPlanById(graphApiConfig, planId)`

Liest einen einzelnen Plan anhand seiner Id, inklusive dessen Etag.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans.|

**Rückgabewert**
Ein Plan-Objekt `{ Id, Title, GroupId, Etag }`.

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

</details>

### `string graphplanner_createPlan(graphApiConfig, groupId, title)`

Legt einen neuen Plan mit dem angegebenen Titel an. Jeder Plan gehört zwingend einer M365-Gruppe — `groupId` muss daher gesetzt sein.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|groupId|string|Die eindeutige Id der M365-Gruppe, der der neue Plan gehören soll. Zwingend erforderlich.|
|title|string|Der Titel des neuen Plans.|

**Rückgabewert**
string - Die Id des neu angelegten Plans.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All` + `Group.ReadWrite.All`

</details>

### `bool graphplanner_updatePlan(graphApiConfig, planId, title)`

Benennt einen bestehenden Plan um. Der aktuelle Etag des Plans wird intern vorab gelesen und für den Update-Aufruf (If-Match) verwendet.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans.|
|title|string|Der neue Titel des Plans.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `bool graphplanner_deletePlan(graphApiConfig, planId)`

Löscht einen Plan. Der aktuelle Etag des Plans wird intern vorab gelesen und für den Delete-Aufruf (If-Match) verwendet.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `bucket[] graphplanner_listBuckets(graphApiConfig, planId)`

Liefert alle Buckets (Spalten) eines Plans.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans.|

**Rückgabewert**
Array von Bucket-Objekten `{ Id, Name, PlanId, OrderHint, Etag }`.

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

</details>

### `string graphplanner_findBucketByName(graphApiConfig, planId, name)`

Sucht innerhalb eines Plans den Bucket mit dem angegebenen Namen und liefert dessen Id zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans, in dem gesucht werden soll.|
|name|string|Der Name des gesuchten Buckets (exakter Vergleich).|

**Rückgabewert**
string - Die eindeutige Id des gefundenen Buckets. Wird kein Bucket mit diesem Namen gefunden, wird ein Fehler geworfen, der im Skript abgefangen werden kann.

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

</details>

### `string graphplanner_createBucket(graphApiConfig, planId, name)`

Legt einen neuen Bucket im angegebenen Plan an.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans, in dem der Bucket angelegt werden soll.|
|name|string|Der Name des neuen Buckets.|

**Rückgabewert**
string - Die Id des neu angelegten Buckets.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `bool graphplanner_updateBucket(graphApiConfig, bucketId, name)`

Benennt einen bestehenden Bucket um. Der aktuelle Etag des Buckets wird intern vorab gelesen und für den Update-Aufruf (If-Match) verwendet.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|bucketId|string|Die eindeutige Id des Buckets.|
|name|string|Der neue Name des Buckets.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `bool graphplanner_deleteBucket(graphApiConfig, bucketId)`

Löscht einen Bucket. Der aktuelle Etag des Buckets wird intern vorab gelesen und für den Delete-Aufruf (If-Match) verwendet.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|bucketId|string|Die eindeutige Id des Buckets.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `task[] graphplanner_getTasks(graphApiConfig, planId)`

Liefert alle Tasks eines Plans (über alle Buckets hinweg).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans.|

**Rückgabewert**
Array von Task-Objekten (siehe [Task Objekt](#task-objekt)).

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

> **`The requested item is not found.`?** Das ist ein `404` von Graph und bedeutet, dass die übergebene `planId` nicht existiert. Häufigste Ursache ist ein falsch geschriebener Eigenschaftsname beim Auslesen der Id — die Objekte der Script Engine sind **case-sensitiv** und heißen je nach Herkunft unterschiedlich:
>
> | Quelle | Eigenschaft mit der Plan-Id |
> | ------ | ------ |
> | `graphplanner_listAllPlans` / `listPlansForGroup` / `listPlansForUser` | `PlanId` |
> | `graphplanner_getPlanById` | `Id` |
> | `graphplanner_findPlanByName` | Rückgabewert ist direkt die Id (string) |
>
> ```javascript
> var plaene = graphplanner_listAllPlans(cfg);
> var tasks  = graphplanner_getTasks(cfg, plaene[0].PlanId);   // richtig
> // graphplanner_getTasks(cfg, plaene[0].Id)                  // undefined -> 404
> ```

</details>

### `task[] graphplanner_getTasksInBucket(graphApiConfig, bucketId)`

Liefert alle Tasks eines einzelnen Buckets.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|bucketId|string|Die eindeutige Id des Buckets.|

**Rückgabewert**
Array von Task-Objekten (siehe [Task Objekt](#task-objekt)).

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

</details>

### `task graphplanner_getTaskById(graphApiConfig, taskId)`

Liest einen einzelnen Task anhand seiner Id, inklusive dessen Etag.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|taskId|string|Die eindeutige Id des Tasks.|

**Rückgabewert**
Ein Task-Objekt (siehe [Task Objekt](#task-objekt)).

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

</details>

### `string graphplanner_createTask(graphApiConfig, task)`

Legt einen neuen Task an. `task.PlanId` ist zwingend erforderlich, da jeder Task einem Plan zugeordnet sein muss; `task.BucketId` ist optional (fehlt er, ordnet MS Graph den Task dem Standard-Bucket des Plans zu).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|task|Objekt|Das Task Objekt mit den Feldern des neuen Tasks (siehe [Task Objekt](#task-objekt)). `PlanId` ist zwingend erforderlich, `BucketId` optional.|

**Rückgabewert**
string - Die Id des neu angelegten Tasks.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `bool graphplanner_updateTask(graphApiConfig, taskId, task)`

Aktualisiert einen bestehenden Task. Es werden alle gesetzten Felder des `task`-Objekts übertragen. Der aktuelle Etag des Tasks wird intern vorab gelesen und für den Update-Aufruf (If-Match) verwendet.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|taskId|string|Die eindeutige Id des zu aktualisierenden Tasks.|
|task|Objekt|Das Task Objekt mit den zu aktualisierenden Feldern (siehe [Task Objekt](#task-objekt)).|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `bool graphplanner_deleteTask(graphApiConfig, taskId)`

Löscht einen Task. Der aktuelle Etag des Tasks wird intern vorab gelesen und für den Delete-Aufruf (If-Match) verwendet.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|taskId|string|Die eindeutige Id des Tasks.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

**Benötigte Berechtigung (Application):** `Tasks.ReadWrite.All`

</details>

### `task[] graphplanner_searchTasks(graphApiConfig, planId, criteria)`

Liefert alle Tasks eines Plans, die die angegebenen Suchkriterien erfüllen. Da Planner keine serverseitige Filterung von Tasks unterstützt, werden intern alle Tasks des Plans gelesen (wie `getTasks`) und anschließend clientseitig gefiltert.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|planId|string|Die eindeutige Id des Plans, dessen Tasks durchsucht werden sollen.|
|criteria|Objekt|Das Kriterien-Objekt (siehe [TaskSearchCriteria Objekt](#tasksearchcriteria-objekt)). Nicht gesetzte Felder werden ignoriert.|

**Rückgabewert**
Array von Task-Objekten (siehe [Task Objekt](#task-objekt)), die allen gesetzten Kriterien entsprechen.

**Benötigte Berechtigung (Application):** `Tasks.Read.All`

</details>

### `GraphApiConfig Objekt`

Das Konfigurationsobjekt für den GraphApi Zugriff.

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

### `Group Objekt`

Eine Microsoft-365-Gruppe, wie sie von `listGroups` zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Id|string|Die eindeutige Id der Gruppe.|
|DisplayName|string|Der Anzeigename der Gruppe.|

</details>

### `PlanRef Objekt`

Ein Discovery-Ergebnis: ein Plan zusammen mit seiner Gruppe, wie es von `listPlansForGroup`, `listAllPlans` und `listPlansForUser` zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|PlanId|string|Die eindeutige Id des Plans.|
|Title|string|Der Titel des Plans.|
|GroupId|string|Die Id der Gruppe, der der Plan gehört.|
|GroupName|string|Der Anzeigename der Gruppe (nur bei `listAllPlans` befüllt, sonst leer).|

</details>

### `Plan Objekt`

Ein einzelner Plan, wie er von `getPlanById` zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Id|string|Die eindeutige Id des Plans.|
|Title|string|Der Titel des Plans.|
|GroupId|string|Die Id der Gruppe, der der Plan gehört.|
|Etag|string|Der `@odata.etag`-Wert des Plans (nur lesbar, wird intern für `updatePlan`/`deletePlan` benötigt).|

</details>

### `Bucket Objekt`

Ein Bucket (Spalte) innerhalb eines Plans.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Id|string|Die eindeutige Id des Buckets.|
|Name|string|Der Name des Buckets.|
|PlanId|string|Die Id des Plans, dem der Bucket gehört.|
|OrderHint|string|Interner Sortierhinweis von MS Graph für die Reihenfolge der Buckets.|
|Etag|string|Der `@odata.etag`-Wert des Buckets (nur lesbar, wird intern für `updateBucket`/`deleteBucket` benötigt).|

</details>

### `Task Objekt`

Das Task-Objekt, das an die GraphApi gesendet wird oder von dieser zurückgeliefert wird.

> **Hinweis:** Notizen/Beschreibung eines Tasks (`plannerTaskDetails`, ein separates Graph-Entity) werden aktuell nicht unterstützt. Ein zukünftiges Nachziehen dieser Funktionalität ist als Folgearbeit geplant.
>
> **Hinweis:** Bei `graphplanner_updateTask` gelten Felder mit Zahlenwert `0` bzw. leerem String (`PercentComplete`, `Priority`, `Title`) als "unverändert" und werden nicht an die GraphApi übertragen. Ein Task kann darüber also z. B. nicht auf 0 % Fortschritt oder Priorität 0 zurückgesetzt werden.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Id|string|Die eindeutige Id des Tasks (nur bei Rückgabewerten gefüllt).|
|PlanId|string|Die Id des Plans, dem der Task gehört. Bei `createTask` zwingend erforderlich.|
|BucketId|string|Die Id des Buckets, in dem der Task liegt. Bei `createTask` optional.|
|Title|string|Der Titel des Tasks.|
|PercentComplete|number|Fortschritt in Prozent, 0..100 (0 = nicht begonnen, 50 = in Bearbeitung, 100 = abgeschlossen).|
|Priority|number|Priorität nach Graph-Konvention, 0..10 (1 = dringend … 9 = niedrig).|
|Due|string|Fälligkeitsdatum im ISO-8601-/RFC-3339-Format, z. B. `"2026-07-15T00:00:00Z"`. Leer/nicht gesetzt = kein Fälligkeitsdatum.|
|Start|string|Startdatum im ISO-8601-/RFC-3339-Format. Leer/nicht gesetzt = kein Startdatum.|
|AssigneeIds|string[]|Die User-Ids der dem Task zugewiesenen Personen.|
|Etag|string|Der `@odata.etag`-Wert des Tasks (nur lesbar, wird intern für `updateTask`/`deleteTask` benötigt).|

</details>

### `TaskSearchCriteria Objekt`

Das Kriterien-Objekt für `graphplanner_searchTasks`. Nicht gesetzte (leere bzw. Default-)Felder werden bei der Filterung ignoriert.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|TitleContains|string|Filtert auf Tasks, deren Titel diesen Text enthält (Groß-/Kleinschreibung wird ignoriert). Leerer String = kein Filter.|
|BucketId|string|Filtert auf Tasks in genau diesem Bucket. Leerer String = kein Filter.|
|AssigneeId|string|Filtert auf Tasks, denen diese User-Id zugewiesen ist. Leerer String = kein Filter.|
|Priority|number|Filtert auf Tasks mit genau dieser Priorität (0..10). `-1` bzw. weglassen = kein Filter (Standardwert ist `-1`).|
|DueFrom|string|Filtert auf Tasks mit Fälligkeitsdatum ab diesem Zeitpunkt (ISO-8601-/RFC-3339-Format). Weglassen = kein Filter.|
|DueTo|string|Filtert auf Tasks mit Fälligkeitsdatum bis zu diesem Zeitpunkt (ISO-8601-/RFC-3339-Format). Weglassen = kein Filter.|

</details>
