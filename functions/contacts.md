
# Kontakte (Microsoft 365)

## Integration in ScriptEngine
`getModule("contacts");`

Alle Funktionen erwarten als erstes Argument `contactsConfig` — ein Objekt mit den
MS-Graph-App-Zugangsdaten (`TenantId`, `ClientId`, `ClientSecret`, `Account`).
`Account` ist das Postfach (UPN/E-Mail), dessen Outlook-Kontakte verwendet werden.

Benötigte Application-Berechtigungen (Admin-Consent):
- `Contacts.ReadWrite` — Postfach-Kontakte lesen/anlegen/ändern/löschen
- `User.Read.All` — Verzeichnis-Nutzer durchsuchen (`contacts_searchDirectory`)
- `OrgContact.Read.All` — Organisations-Kontakte durchsuchen (`contacts_searchDirectory`)

Kontakt-Objekte haben die Felder:
`{Id, DisplayName, GivenName, Surname, EmailAddresses[], BusinessPhones[], MobilePhone, CompanyName, JobTitle, Source}`.
`Source` ist `"mailbox"` (Outlook-Kontakt) oder `"directory"` (Verzeichnis-Treffer).

## Postfach-Kontakte

### `array contacts_list(contactsConfig, maxCount)`
Listet die Outlook-Kontakte des Postfachs (nach `DisplayName` sortiert), max. `maxCount`.

### `array contacts_search(contactsConfig, search, maxCount)`
Volltextsuche über die Postfach-Kontakte (u.a. Name und E-Mail-Adresse), max. `maxCount`.

### `object contacts_get(contactsConfig, contactId)`
Liefert einen einzelnen Postfach-Kontakt anhand seiner `Id`.

### `object contacts_create(contactsConfig, contact)`
Legt einen Postfach-Kontakt an. `contact` ist ein Objekt mit den o.g. Feldern
(z.B. `{GivenName, Surname, EmailAddresses:["a@b.de"]}`). Liefert den angelegten Kontakt inkl. `Id`.

### `object contacts_update(contactsConfig, contactId, contact)`
Aktualisiert einen Postfach-Kontakt. Nur gesetzte Felder werden geändert. Liefert den geänderten Kontakt.

### `bool contacts_delete(contactsConfig, contactId)`
Löscht einen Postfach-Kontakt. Liefert `true`.

## Verzeichnis-Suche

### `array contacts_searchDirectory(contactsConfig, query, maxCount)`
Durchsucht organisationsweit die Entra-ID-Nutzer **und** die Organisations-Kontakte
nach `query` (Treffer in Anzeigename oder E-Mail). Ergebnisse werden zusammengeführt und
nach E-Mail-Adresse dedupliziert; jeder Treffer hat `Source = "directory"`. Das zusammengeführte
Gesamtergebnis ist auf `maxCount` Einträge begrenzt (Verzeichnis-Nutzer zuerst, dann Organisations-Kontakte).
Schlägt nur eine der beiden Quellen fehl (z.B. fehlende Berechtigung), werden die Treffer der anderen
Quelle dennoch geliefert.

## Beispiel: Kontakt per E-Mail-Adresse finden

```javascript
getModule("contacts");
var cfg = { TenantId: "...", ClientId: "...", ClientSecret: "...", Account: "team@contoso.de" };

// zuerst im Postfach suchen ...
var hits = contacts_search(cfg, "max.mustermann@contoso.de", 10);
if (!hits || hits.length === 0) {
    // ... sonst organisationsweit im Verzeichnis
    hits = contacts_searchDirectory(cfg, "max.mustermann@contoso.de", 10);
}
return hits;
```

**Fehlerbehandlung:** Wie bei allen ScriptEngine-Modulen wird im Fehlerfall ein
String zurückgegeben, der mit `ERROR` beginnt (kein Exception-Throw). Prüfen Sie das
Ergebnis vor der Weiterverarbeitung.
