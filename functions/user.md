# User Modul

Das User-Modul stellt den aktuellen Benutzer (den Auslöser der Funktion) lesend zur Verfügung. Es eignet sich für personalisierte Anreden, kontextabhängige Berechtigungslogik und das Mitloggen, wer eine Funktion ausgelöst hat.

Modul-Name für `getModule()`: `"user"`

## Funktionen

### `user_getUser()`

**Parameter:** keine

**Rückgabe:** Ein Objekt mit den Feldern des aktuellen Benutzers:

| Feld | Typ | Bedeutung |
| --- | --- | --- |
| `Id` | number | Eindeutige Benutzer-Id |
| `KeycloakId` | string | Keycloak-Benutzer-Id (UUID) |
| `FirstName` | string | Vorname |
| `LastName` | string | Nachname |
| `Email` | string | E-Mail-Adresse |
| `OrganisationId` | number | Id der Organisation, der der Benutzer angehört |
| `Role` | number | Rolle des Benutzers (1 = User, 2 = AgentAdmin, 3 = OrgAdmin, 4 = ServiceAdmin) |
| `Language` | string | Sprach-Code (z. B. `"de"`, `"en"`) |
| `Active` | bool | Benutzer aktiv |
| `DeactivationDate` | string | Deaktivierungsdatum (leer, falls aktiv) |
| `TwoFA` | bool | Zwei-Faktor-Authentifizierung aktiviert |
| `ThirdPartyId` | string | Externe Benutzer-Id (z. B. aus einem angebundenen IdP) |
| `Groups` | array | Liste der Gruppen, denen der Benutzer angehört |
| `Config` | object | Benutzerspezifische Konfiguration |
| `Config.Role` | string | Rolle/Beschreibung im freien Textformat |
| `Config.Behavior` | string | Verhaltens-Beschreibung für KI-Interaktion |
| `Config.TextSample` | string | Schreibstil-Textprobe |
| `Config.Language` | string | Bevorzugte Sprache |
| `Config.PersonalKnowledgeDbEnabled` | bool | Persönliche Wissensdatenbank aktiviert |

**Hinweis:** Will man die Organisation des Benutzers (Connector-Daten etc.) abrufen, ist nicht `OrganisationId` der richtige Weg — die ausführende Funktion gehört einer Organisation an, die direkt über das [Organisation-Modul](./organisation.md) verfügbar ist.

## Beispiel

```javascript
function myFunc() {
    getModule("user");
    var user = user_getUser();

    var greeting = "Hallo " + user.FirstName + " " + user.LastName + "!";
    var lang     = user.Language;

    if (user.Role >= 3) {
        // OrgAdmin oder höher
        return greeting + " (Admin-Modus)";
    }

    return greeting;
}
myFunc();
```

## Read-only

Das Objekt ist eine Kopie des aktuellen Benutzers. Änderungen im Skript (`user.Email = "..."`) wirken sich nicht auf die Datenbank aus.
