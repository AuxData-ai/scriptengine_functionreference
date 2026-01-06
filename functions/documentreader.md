## Integration in ScriptEngine

`getModule("document");`

## Funktionen

### `string document_readText(agentId, filename, filecontent, readImages)`

liest ein Dokument aus und liefert den Inhalt als string zurück.

**Unterstützte Dateiformate:**

- PDF
- Word
- OpenOffice Formate
- Excel
- Text
- CSV
- MarkDown
- XML
- JSON
- JPEG
- PNG
- EML (mit Attachments)
- MSG (ohne Attachments)

**Parameter:**

- agentId : int --> der agent, der verwendet werden soll um mit seiner Konfiguration Dateien auszulesen. Relevant sind die Konfigurationen der WissensDb, wie die konfigurierten Modelle für ComputerVision und Spracherkennung.
- filename: string --> Der Dateiname mit Dateityp. Der Dateityp muss unbeidngt enthalten sein, sonst kann die Plattform nicht erkennen wie die Datei ausgelesen werden kann.
- filecontent: string --> Ein Base64 string, mit dem Inhalt der Datei.
- readImages: bool --> Falls sich in einem Pdf Bilder befinden, sollen diese Bilder ebenfalls alle ausgelesen werden? Dies ist sinnvoll, wenn gescannte Dokumente als Pdf Datei übergeben werden.

**Rückgabewert**

Der textuelle Inhalt des Dokuments

### Beispielaufruf:

`var text = document_readText(310, "test.pdf", "AKhkhuwdvbnqo...", true);`
