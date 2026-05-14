
# Google Workspace — Calendar

## Integration in ScriptEngine
`getModule("googlecalendar");`

Das Modul bietet die gleiche Funktionssurface wie `graphcalendar`. Hinweis: `findMeetingTimes` ist clientseitig auf Basis der Google-Freebusy-API implementiert (Schnittmenge der freien Slots aller Teilnehmer); `Confidence` ist konstant `100`.

## Funktionen

### `string googlecalendar_findCalendarByName(googleApiConfig, name)`

Sucht den Kalender des Accounts mit dem angegebenen Anzeigenamen und liefert dessen Id zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|name|string|Der Anzeigename (Summary) des Kalenders.|

**Rückgabewert**
string - Die eindeutige Id des Kalenders. Eine leere Id (`""`) verweist in den anderen Funktionen auf den Standard-Kalender (`"primary"`) des Accounts.

</details>

### `event[] googlecalendar_getEventsInRange(googleApiConfig, calendarId, fromIso, toIso)`

Liefert alle Termine des Kalenders im angegebenen Zeitfenster (inklusive aufgelöster Serien-Instanzen).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|calendarId|string|Die Kalender-Id (leerer String = `"primary"`).|
|fromIso|string|Beginn des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|toIso|string|Ende des Zeitfensters im ISO-8601-/RFC-3339-Format.|

**Rückgabewert**
Array von Event Objekten.

</details>

### `event googlecalendar_getEventById(googleApiConfig, calendarId, eventId)`

Liest einen einzelnen Termin anhand seiner Id.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|calendarId|string|Die Kalender-Id (leerer String = `"primary"`).|
|eventId|string|Die eindeutige Id des Termins.|

**Rückgabewert**
Ein Event Objekt.

</details>

### `slot[] googlecalendar_findFreeSlots(googleApiConfig, calendarId, fromIso, toIso, minutes)`

Liefert die freien Zeitfenster im angegebenen Zeitraum, die mindestens `minutes` Minuten lang sind. Verwendet die Freebusy-API.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|calendarId|string|Die Kalender-Id (leerer String = `"primary"`).|
|fromIso|string|Beginn des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|toIso|string|Ende des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|minutes|number|Mindestlänge eines freien Slots in Minuten.|

**Rückgabewert**
Array von Slot Objekten.

</details>

### `slot[] googlecalendar_findMeetingTimes(googleApiConfig, attendees, fromIso, toIso, minutes)`

Liefert gemeinsame freie Zeitfenster der angegebenen Teilnehmer im Zeitraum, die mindestens `minutes` Minuten lang sind. Implementierung: Schnittmenge aller Freebusy-Antworten. `Confidence` ist konstant `100`. Teilnehmer ohne Freebusy-Sicht (z. B. externe Adressen) werden geloggt und als „frei" behandelt.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|attendees|string[]|E-Mail-Adressen der Teilnehmer.|
|fromIso|string|Beginn des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|toIso|string|Ende des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|minutes|number|Gewünschte Meeting-Länge in Minuten.|

**Rückgabewert**
Array von Slot Objekten inklusive `Confidence` (immer `100`) und der berücksichtigten Teilnehmer.

</details>

### `string googlecalendar_createEvent(googleApiConfig, calendarId, event)`

Legt einen neuen Termin im Kalender an. Ist `event.OnlineMeeting` gesetzt, wird ein Google-Meet-Link automatisch erzeugt.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|calendarId|string|Die Kalender-Id (leerer String = `"primary"`).|
|event|Objekt|Das Event Objekt mit Titel, Zeit, Teilnehmern usw.|

**Rückgabewert**
string - Die Id des neu angelegten Termins.

</details>

### `bool googlecalendar_updateEvent(googleApiConfig, calendarId, eventId, event)`

Aktualisiert einen bestehenden Termin (Patch). Nicht gesetzte Felder bleiben unverändert.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|calendarId|string|Die Kalender-Id (leerer String = `"primary"`).|
|eventId|string|Die eindeutige Id des Termins.|
|event|Objekt|Das Event Objekt mit den zu aktualisierenden Feldern.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlecalendar_deleteEvent(googleApiConfig, calendarId, eventId)`

Löscht einen Termin.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|calendarId|string|Die Kalender-Id (leerer String = `"primary"`).|
|eventId|string|Die eindeutige Id des Termins.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlecalendar_acceptEvent(googleApiConfig, eventId, comment, sendResponse)`

Sagt eine Termineinladung zu.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|eventId|string|Die eindeutige Id des Termins.|
|comment|string|Optionaler Kommentar.|
|sendResponse|bool|`true` = alle Teilnehmer benachrichtigen, `false` = nur eigenen Status setzen.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlecalendar_declineEvent(googleApiConfig, eventId, comment, sendResponse)`

Lehnt eine Termineinladung ab.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|eventId|string|Die eindeutige Id des Termins.|
|comment|string|Optionaler Kommentar.|
|sendResponse|bool|`true` = alle Teilnehmer benachrichtigen, `false` = nur eigenen Status setzen.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool googlecalendar_tentativelyAcceptEvent(googleApiConfig, eventId, comment, sendResponse)`

Sagt eine Termineinladung mit Vorbehalt zu.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|googleApiConfig|Objekt|Die Konfiguration für den Google Workspace API Zugriff.|
|eventId|string|Die eindeutige Id des Termins.|
|comment|string|Optionaler Kommentar.|
|sendResponse|bool|`true` = alle Teilnehmer benachrichtigen, `false` = nur eigenen Status setzen.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `GoogleApiConfig Objekt`

Siehe Beschreibung unter [Google Workspace — Mail](./googlemail.md). Identische Struktur — Modus A (Service Account) verwendet hier den Scope `https://www.googleapis.com/auth/calendar`.

### `Event Objekt`

Das Termin-Objekt für Google Calendar. Identisch zur Struktur des [MS Graph Calendar Event Objekts](./msgraphcalendar.md), so dass Skripte zwischen beiden Anbietern portierbar bleiben.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|EventServerId|string|Die eindeutige Id des Termins (vom Server vergeben).|
|CalendarId|string|Die Id des Kalenders, in dem der Termin liegt (optional).|
|Subject|string|Der Titel des Termins.|
|Body|string|Der Inhalt / die Beschreibung des Termins.|
|Html|bool|Kennzeichen ob `Body` als HTML interpretiert werden soll. Für Google Calendar wird der Body immer als Text gesendet.|
|Location|string|Die Location-Beschreibung des Termins.|
|Start|string|Startzeitpunkt im ISO-8601-/RFC-3339-Format.|
|End|string|Endzeitpunkt im ISO-8601-/RFC-3339-Format.|
|IsAllDay|bool|Kennzeichen ob es sich um einen ganztägigen Termin handelt.|
|Organizer|string|E-Mail-Adresse des Organisators.|
|Attendees|EventAttendee[]|Liste der Teilnehmer.|
|OnlineMeeting|bool|Kennzeichen ob ein Google-Meet-Link angelegt werden soll.|
|OnlineMeetingUrl|string|Die generierte URL des Google-Meet-Meetings (nur in Lese-Antworten gefüllt).|

</details>

### `EventAttendee Objekt`

Identische Struktur zu MS Graph: `Email`, `Name`, `Type` (`"required"` | `"optional"`), `Response` (`"none"` | `"accepted"` | `"declined"` | `"tentative"`).

### `Slot Objekt`

Identische Struktur zu MS Graph: `Start`, `End`, `Confidence`, `Attendees`.
