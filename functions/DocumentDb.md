---
title: Document DB
---

Die DocumentDb ist nicht zu verwechseln mit der KnowledgeDb. Die Dokument speichert Text zu einer ID in der Datenbank zu einer Organisation ab. Am ehesten gleicht sie dem localstorage in Webbrowsern. Es stehen drei Funktionen zur Verfügung um Dokumente zu speichern, laden oder zu löschen.

## Integration in ScriptEngine

getModule("docdb");

## Funktionen

### `string docdb_load(documentId)`

lädt das Dokument mit der documentId aus der Datenbank und gibt es als string zurück.

**Parameter:**

- documentId : string die Id des Dokuments

### `bool docdb_save(documentId, document)`

speichert das Dokument unter der angegebenen documentId. Ob das Speichern erfolgreich war, kann über den Rückgabewert ausgelesen werden.


**Parameter:**

- documentId: string --> die Id des Dokuments
- document: string --> das Textdokument 



### `bool docdb_delete(documentId)`

Löscht das Dokument unter der angegebenen documentId. Ob das Löschen erfolgreich war, kann über den Rückgabewert ausgelesen werden.


**Parameter:**

- documentId: string --> die Id des Dokuments