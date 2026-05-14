
# MS Graph Calendar

## Integration in ScriptEngine
`getModule("graphcalendar");`

## Funktionen

### `string graphcalendar_findCalendarByName(graphApiConfig, name)`

Sucht den Kalender des Accounts mit dem angegebenen Anzeigenamen und liefert dessen Id zurück.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|name|string|Der Anzeigename des Kalenders, der gesucht werden soll.|

**Rückgabewert**
string - Die eindeutige Id des Kalenders. Bei einer leeren Id („") werden in den anderen Funktionen die Methoden gegen den Standard-Kalender des Accounts ausgeführt.

</details>

### `event[] graphcalendar_getEventsInRange(graphApiConfig, calendarId, fromIso, toIso)`

Liefert alle Termine des Kalenders im angegebenen Zeitfenster (inklusive aufgelöster Serien-Instanzen).

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|calendarId|string|Die Kalender-Id (leerer String = Standard-Kalender des Accounts).|
|fromIso|string|Beginn des Zeitfensters im ISO-8601-/RFC-3339-Format, z. B. `"2026-05-01T00:00:00Z"`.|
|toIso|string|Ende des Zeitfensters im ISO-8601-/RFC-3339-Format.|

**Rückgabewert**
Array von Event Objekten.

</details>

### `event graphcalendar_getEventById(graphApiConfig, calendarId, eventId)`

Liest einen einzelnen Termin anhand seiner Id.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|calendarId|string|Die Kalender-Id (leerer String = Standard-Kalender des Accounts).|
|eventId|string|Die eindeutige Id des Termins.|

**Rückgabewert**
Ein Event Objekt.

</details>

### `slot[] graphcalendar_findFreeSlots(graphApiConfig, calendarId, fromIso, toIso, minutes)`

Liefert die freien Zeitfenster im angegebenen Zeitraum, die mindestens `minutes` Minuten lang sind. Aktuell wird hierfür immer der Hauptkalender des Accounts ausgewertet — der Parameter `calendarId` ist für API-Symmetrie reserviert.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|calendarId|string|Die Kalender-Id (aktuell ignoriert, immer Hauptkalender).|
|fromIso|string|Beginn des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|toIso|string|Ende des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|minutes|number|Mindestlänge eines freien Slots in Minuten.|

**Rückgabewert**
Array von Slot Objekten.

</details>

### `slot[] graphcalendar_findMeetingTimes(graphApiConfig, attendees, fromIso, toIso, minutes)`

Liefert MS-Graph-Vorschläge für gemeinsame Meeting-Zeiten der angegebenen Teilnehmer im Zeitfenster mit der gewünschten Mindestlänge.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|attendees|string[]|E-Mail-Adressen der Teilnehmer.|
|fromIso|string|Beginn des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|toIso|string|Ende des Zeitfensters im ISO-8601-/RFC-3339-Format.|
|minutes|number|Gewünschte Meeting-Länge in Minuten.|

**Rückgabewert**
Array von Slot Objekten inklusive `Confidence` (0–100) und der berücksichtigten Teilnehmer.

</details>

### `string graphcalendar_createEvent(graphApiConfig, calendarId, event)`

Legt einen neuen Termin im Kalender an. Ist `event.OnlineMeeting` gesetzt, generiert MS Graph automatisch den Teams-Link.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|calendarId|string|Die Kalender-Id (leerer String = Standard-Kalender des Accounts).|
|event|Objekt|Das Event Objekt mit Titel, Zeit, Teilnehmern usw.|

**Rückgabewert**
string - Die Id des neu angelegten Termins.

</details>

### `bool graphcalendar_updateEvent(graphApiConfig, calendarId, eventId, event)`

Aktualisiert einen bestehenden Termin. Es werden alle gesetzten Felder des `event`-Objekts übertragen.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|calendarId|string|Die Kalender-Id (leerer String = Standard-Kalender des Accounts).|
|eventId|string|Die eindeutige Id des Termins.|
|event|Objekt|Das Event Objekt mit den zu aktualisierenden Feldern.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool graphcalendar_deleteEvent(graphApiConfig, calendarId, eventId)`

Löscht einen Termin.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|calendarId|string|Die Kalender-Id (leerer String = Standard-Kalender des Accounts).|
|eventId|string|Die eindeutige Id des Termins.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool graphcalendar_acceptEvent(graphApiConfig, eventId, comment, sendResponse)`

Sagt eine Termineinladung zu.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|eventId|string|Die eindeutige Id des Termins.|
|comment|string|Optionaler Kommentar, der mit der Antwort gesendet wird.|
|sendResponse|bool|`true` = Antwort an den Organisator senden, `false` = nur eigenen Status setzen.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool graphcalendar_declineEvent(graphApiConfig, eventId, comment, sendResponse)`

Lehnt eine Termineinladung ab.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|eventId|string|Die eindeutige Id des Termins.|
|comment|string|Optionaler Kommentar, der mit der Antwort gesendet wird.|
|sendResponse|bool|`true` = Antwort an den Organisator senden, `false` = nur eigenen Status setzen.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `bool graphcalendar_tentativelyAcceptEvent(graphApiConfig, eventId, comment, sendResponse)`

Sagt eine Termineinladung mit Vorbehalt zu.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|graphApiConfig|Objekt|Die Konfiguration für den GraphApi Zugriff, die für die Durchführung von graphApi Calls notwendig sind.|
|eventId|string|Die eindeutige Id des Termins.|
|comment|string|Optionaler Kommentar, der mit der Antwort gesendet wird.|
|sendResponse|bool|`true` = Antwort an den Organisator senden, `false` = nur eigenen Status setzen.|

**Rückgabewert**
bool - Kennzeichen ob die Aktion erfolgreich war.

</details>

### `Event Objekt`

Das Termin-Objekt, das an die GraphApi gesendet wird oder von dieser zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|EventServerId|string|Die eindeutige Id des Termins (vom Server vergeben).|
|CalendarId|string|Die Id des Kalenders, in dem der Termin liegt (optional).|
|Subject|string|Der Titel des Termins.|
|Body|string|Der Inhalt / die Beschreibung des Termins.|
|Html|bool|Kennzeichen ob `Body` als HTML interpretiert werden soll.|
|Location|string|Die Location-Beschreibung des Termins.|
|Start|string|Startzeitpunkt im ISO-8601-/RFC-3339-Format.|
|End|string|Endzeitpunkt im ISO-8601-/RFC-3339-Format.|
|IsAllDay|bool|Kennzeichen ob es sich um einen ganztägigen Termin handelt.|
|Organizer|string|E-Mail-Adresse des Organisators.|
|Attendees|EventAttendee[]|Liste der Teilnehmer.|
|OnlineMeeting|bool|Kennzeichen ob ein Online-Meeting (Teams) angelegt werden soll.|
|OnlineMeetingUrl|string|Die generierte URL des Online-Meetings (nur in Lese-Antworten gefüllt).|

</details>

### `EventAttendee Objekt`

Ein einzelner Teilnehmer eines Termins.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Email|string|Die E-Mail-Adresse des Teilnehmers.|
|Name|string|Der Anzeigename des Teilnehmers (optional).|
|Type|string|`"required"` oder `"optional"`.|
|Response|string|Antwortstatus: `"none"`, `"accepted"`, `"declined"` oder `"tentative"`.|

</details>

### `Slot Objekt`

Ein Zeitfenster, das von `findFreeSlots` oder `findMeetingTimes` zurückgeliefert wird.

<details>
<summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|Start|string|Startzeitpunkt des Fensters.|
|End|string|Endzeitpunkt des Fensters.|
|Confidence|number|Konfidenzwert (0–100, nur bei `findMeetingTimes` gefüllt).|
|Attendees|string[]|Berücksichtigte Teilnehmer (nur bei `findMeetingTimes` gefüllt).|

</details>
