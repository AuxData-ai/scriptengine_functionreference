# DocumentDB Modul

Die DocumentDb ist nicht zu verwechseln mit der KnowledgeDb. Das DOkumentDB Modul speichert Text zu einer ID in der Datenbank zu einer Organisation ab. Am ehesten gleicht sie dem localstorage in Webbrowsern. Es stehen drei Funktionen zur Verfügung um Dokumente zu speichern, laden oder zu löschen.

## Initialisierung in der ScriptEngine

`getModule("docdb");`

## Funktionen

### `string docdb_load(documentId)`

Lädt das Dokument mit der documentId aus der Datenbank und gibt es als string zurück.

<details><summary>Details</summary>

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| documentId | string |die Id des Dokuments|

**Rückgabewert**
String

Der Inhalt des geladenen Dokuments

</details>


### `bool docdb_save(documentId, document)`

speichert das Dokument unter der angegebenen documentId. Ob das Speichern erfolgreich war, kann über den Rückgabewert ausgelesen werden.


<details><summary>Details</summary>

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|documentId|string | die Id des Dokuments|
|document| string | das Textdokument |

**Rückgabewert**
bool

Kennzeichen ob die Operation erfolgreich durchgeführt wurde.

</details>


### `bool docdb_delete(documentId)`

Löscht das Dokument unter der angegebenen documentId. Ob das Löschen erfolgreich war, kann über den Rückgabewert ausgelesen werden.


<details><summary>Details</summary>

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
|documentId|string | die Id des Dokuments|

**Rückgabewert**
bool

Kennzeichen ob die Operation erfolgreich durchgeführt wurde.

</details>

