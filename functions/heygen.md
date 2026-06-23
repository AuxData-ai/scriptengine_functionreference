# HeyGen Modul

Das HeyGen-Modul stellt KI-Video-Funktionen über die HeyGen-API bereit. In Schritt 1
wird die **Video-Übersetzung** unterstützt. Übersetzungen laufen asynchron: Der Aufruf
liefert sofort eine `jobId` zurück; das fertige Video wird später per `heygen_getJob` abgerufen.

> **Hinweis — No-Code-Weg über den Workflow-Editor:** Die HeyGen-Video-Übersetzung ist
> zusätzlich als **first-class AI-Workflow-Schritt** verfügbar (Schritttyp
> **„Video-Übersetzung"**). Dieser Schritt kann direkt im AI-Prozesseditor konfiguriert
> werden — ohne Scripting. Zielsprachen werden aus HeyGen geladen, das Video wird zur
> Laufzeit hochgeladen oder über eine feste Quell-URL bezogen, und das übersetzte Video
> erscheint anschließend als herunterladbares Ergebnis im Workflow. Details zur
> Konfiguration finden Sie im
> [Administratorhandbuch, Kapitel 5 – AI-Workflows bauen](../../manual/administratorhandbuch/05-ki-workflows.md#workflow-schritt-video-übersetzung-heygen).

## Initialisierung in der ScriptEngine
`getModule("heygen");`

## Funktionen

### `string heygen_translateVideo(agentId, sourceUrl, targetLanguages, options)`
Startet eine Video-Übersetzung. Gibt sofort die Job-ID zurück (asynchron).
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| agentId | number | Die ID des Agents, dem das Ergebnis-Video gehören soll. |
| sourceUrl | string | Öffentliche URL des Quell-Videos oder die HeyGen-Asset-URL aus dem Upload-Endpoint `/heygen/source-upload`. |
| targetLanguages | string[] | Zielsprachen, z.B. `["English","Spanish"]`. |
| options | object | Optional: `{ mode: "speed"|"precision", title: string }`. Standard `mode` ist `"speed"`. |

**Rückgabewert**

string die Job-ID, mit der der Status/das Ergebnis über `heygen_getJob` abgerufen wird.
</details>

### `object heygen_getJob(jobId)`
Liefert Status und Ergebnis eines Übersetzungs-Jobs.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| jobId | string | Die von `heygen_translateVideo` zurückgegebene Job-ID. |

**Rückgabewert**

object `{ jobId, status, mediaUrl, error }` — `status` ist einer von
`pending`, `processing`, `completed`, `failed`. Bei `completed` enthält
`mediaUrl` die abrufbare URL des übersetzten Videos.
</details>

### `string heygen_translateVideoAndWait(agentId, sourceUrl, targetLanguages, options, timeoutSec)`
Wie `heygen_translateVideo`, blockiert aber bis der Job fertig ist oder `timeoutSec`
abgelaufen ist. Achtung: Durch das Script-Timeout begrenzt — für lange
Übersetzungen besser `heygen_translateVideo` + späteres `heygen_getJob` verwenden.
<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| agentId | number | Die ID des Agents, dem das Ergebnis-Video gehören soll. |
| sourceUrl | string | Öffentliche URL oder HeyGen-Asset-URL des Quell-Videos. |
| targetLanguages | string[] | Zielsprachen. |
| options | object | `{ mode, title }`, siehe `heygen_translateVideo`. |
| timeoutSec | number | Maximale Wartezeit in Sekunden. |

**Rückgabewert**

string die Job-ID. Status/Ergebnis anschließend mit `heygen_getJob` abrufen.
</details>

### `string[] heygen_listLanguages()`
Liefert die von HeyGen unterstützten Zielsprachen.
<details><summary>Details</summary>

**Rückgabewert**

string[] Liste der verfügbaren Sprachnamen.
</details>

## Beispiel

```javascript
getModule("heygen");

var jobId = heygen_translateVideo(42, "https://example.com/intro.mp4", ["English","French"], {mode:"speed", title:"Intro"});

// später (z.B. in einem Intervall-Service):
var job = heygen_getJob(jobId);
if (job.status === "completed") {
    log_info("Übersetztes Video: " + job.mediaUrl);
}
```
