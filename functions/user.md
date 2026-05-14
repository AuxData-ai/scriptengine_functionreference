# User Modul

## Integration in ScriptEngine
`getModule("user");`

### `user user_getUser()`

liefert den Anwender zurück, der diese Funktion gerade ausgeführt hat mit allen relevanten Informationen. Ein User Objekt ist ein Objekt zu Informationszwecken. Zum Beispiel um die Mail Adresse des aktuellen Nutzers und seinen Namen auszulesen. Änderungen am User Objekt können nicht gespeichert werden. 

<details><summary>Details</summary>

**Parameter**

| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| - | - | - |

**Rückgabewert**
Das User Objekt

</details>

### User Objekt

Hier sind die wichtigsten Informationen, die als User Objekt zurückgeliefert werden:
<details><summary>Details</summary>
| Name | Typ | Beschreibung |
| ------ | ------ | ------ |
| Id | number | Die eindeutige ID des Anwenders in der Plattform. |
| FirstName | string | Der Vorname des Anwenders. |
| LastName | string | Der Nachname des Anwenders. |
| OrganisationId | number | Die  eindeutige ID der Organisation, zu der der Anwender gehört. |
| Role | number | Die Rolle des Anwenders die ihm zugeordnet ist. 1 = User, 2 = AgentAdmin, 3 = OrganisationAdmin |
| Email | string | Die E-Mail Adresse des Users |
</details>
