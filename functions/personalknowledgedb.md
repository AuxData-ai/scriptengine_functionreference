# Personal Knowledge-DB Modul

Speichert Dokumente in der **persönlichen Wissensdatenbank** des ausführenden Users
und durchsucht sie semantisch. Im Gegensatz zur Agent-Wissensdatenbank gibt es weder
`agentId` noch `containerId` — alle Funktionen sind user-scoped.

**Quota:** maximal 100 Dokumente, maximal 500 MB pro Datei.
**Pipeline:** Chunking + Embedding. Kein Context-Enrichment, kein LightRAG, keine
Bild-Extraktion (im Unterschied zur Agent-Wissensdatenbank).

## Integration in ScriptEngine

`getModule("personalknowledgedb");`

## Funktionen

### `string saveText(name, content, documentId)`

Speichert ein Textdokument in der persönlichen Wissensdatenbank. Der Inhalt wird
direkt gechunkt, ohne durch den Datei-Parser zu laufen.

| Name | Typ | Beschreibung |
|------|-----|--------------|
| name | string | Name des Dokuments |
| content | string | textueller Inhalt |
| documentId | string | leer = neues Dokument; gesetzt = bestehendes Dokument ersetzen (UUID des persönlichen Dokuments) |

**Rückgabewert:** documentId des neuen oder ersetzten Dokuments.

### `string saveBinary(name, content, documentId)`

Speichert ein Binärdokument (PDF, DOCX, Bild, Audio, ...). `content` ist ein
Base64-String, der durch den internen Datei-Parser läuft.

| Name | Typ | Beschreibung |
|------|-----|--------------|
| name | string | Name inklusive Dateiendung |
| content | string (base64) | Base64-codierter Dateiinhalt |
| documentId | string | leer = neu; gesetzt = ersetzen |

**Rückgabewert:** documentId.

### `string saveTextByToken(token, name, content, documentId)`

Wie `saveText`, der User wird aus dem Token aufgelöst.

| Name | Typ | Beschreibung |
|------|-----|--------------|
| token | string | Access-Token zur User-Resolution |
| name | string | Name des Dokuments |
| content | string | textueller Inhalt |
| documentId | string | leer = neu; gesetzt = ersetzen |

### `string saveBinaryByToken(token, name, content, documentId)`

Wie `saveBinary`, mit Token-basierter User-Resolution.

| Name | Typ | Beschreibung |
|------|-----|--------------|
| token | string | Access-Token zur User-Resolution |
| name | string | Name inklusive Dateiendung |
| content | string (base64) | Base64-codierter Dateiinhalt |
| documentId | string | leer = neu; gesetzt = ersetzen |

### `string[] find(searchText, qualityGate, resultLimit)`

Semantische Suche im User-Scope.

| Name | Typ | Beschreibung |
|------|-----|--------------|
| searchText | string | Suchanfrage |
| qualityGate | number | Mindest-Score (0..1) |
| resultLimit | int | maximale Anzahl Treffer |

**Rückgabewert:** Array der gefundenen Chunk-Texte.

> Hinweis: Eine `findByKeyword`-Funktion ist im persönlichen Modul **nicht** verfügbar.
