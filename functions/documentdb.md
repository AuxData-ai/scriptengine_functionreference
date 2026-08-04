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


### `string[] docdb_keys()`

Gibt alle Keys zurück, die aktuell im Key Value Store der Organisation gespeichert sind.

<details><summary>Details</summary>

**Parameter:** keine

**Rückgabewert**
string[]

Liste aller gespeicherten Keys (leeres Array, wenn der Store leer ist).

**Beispiel:**
```js
getModule("docdb");
var keys = docdb_keys();
for (var i = 0; i < keys.length; i++) {
    log(keys[i] + " = " + docdb_load(keys[i]));
}
```

</details>

## Stichwortsuche

Es gibt zwölf Suchfunktionen: drei Modi (exakt, alle Wörter, mindestens ein
Wort) jeweils für Key und Value, jeweils als case-sensitive und
case-insensitive Variante. Alle geben ein Array von Objekten mit den
Eigenschaften `key` und `value` zurück (leeres Array, wenn nichts gefunden
wird). Die Suche ist organisationsweit.

**Modi**

- **Exakt** (`...Exact`): Key bzw. Value ist gleich dem Suchstring.
- **Alle Wörter** (`...AllWords`): Jedes Wort des Suchstrings (an Leerzeichen
  zerlegt) kommt als Teilstring vor (UND-Verknüpfung).
- **Mindestens ein Wort** (`...AnyWord`): Mindestens ein Wort kommt als
  Teilstring vor (ODER-Verknüpfung).

Funktionen ohne Suffix `IgnoreCase` sind case-sensitive, Funktionen mit Suffix
`IgnoreCase` ignorieren Groß-/Kleinschreibung.

| Funktion | Feld | Modus | Case |
| ------ | ------ | ------ | ------ |
| `docdb_searchKeyExact(s)` | Key | exakt | sensitiv |
| `docdb_searchKeyExactIgnoreCase(s)` | Key | exakt | insensitiv |
| `docdb_searchValueExact(s)` | Value | exakt | sensitiv |
| `docdb_searchValueExactIgnoreCase(s)` | Value | exakt | insensitiv |
| `docdb_searchKeyAllWords(s)` | Key | alle Wörter | sensitiv |
| `docdb_searchKeyAllWordsIgnoreCase(s)` | Key | alle Wörter | insensitiv |
| `docdb_searchValueAllWords(s)` | Value | alle Wörter | sensitiv |
| `docdb_searchValueAllWordsIgnoreCase(s)` | Value | alle Wörter | insensitiv |
| `docdb_searchKeyAnyWord(s)` | Key | mind. ein Wort | sensitiv |
| `docdb_searchKeyAnyWordIgnoreCase(s)` | Key | mind. ein Wort | insensitiv |
| `docdb_searchValueAnyWord(s)` | Value | mind. ein Wort | sensitiv |
| `docdb_searchValueAnyWordIgnoreCase(s)` | Value | mind. ein Wort | insensitiv |

**Parameter:**
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| s | string | der Suchstring |

**Rückgabewert**
Array von Objekten `{ key: string, value: string }`

**Beispiel**

```js
getModule("docdb");
var hits = docdb_searchValueAllWordsIgnoreCase("rechnung 2026");
for (var i = 0; i < hits.length; i++) {
    log_info(hits[i].key + " => " + hits[i].value);
}
```

