# Funktionsreferenz für die AuxData.ai Plattform ScriptEngine

In der AuxData.ai Plattform können mithilfe von Javascript (ES5) eigene Funktionen entwickelt werden. Diese Funktionen können dann in den Prozessschritten eines AI Workflows aufgerufen werden. 

Die FUnktionen können folgendermaßen in die AI Workflows integriert werden:
- als eigene Prozessschritte in einem AI Worklfow;
- als Vorverarbeitungsschritt in einem AI Prozessschritt;
- oder als Nachverarbeitungsschritt um das Ergebnis eines KI-Modells weiter zu verarbeiten.

Mit der Script Engine liefert die AuxData.ai Plattform die Möglichkeit, KI und Programmierung optimal zu verknüpfen um bestmögliche Ergebnisse zu erzielen.

Unterstützte Javascriptversion: ES5 (ECMA Script 5)
[Dokumentation des ECMA Script5 Standards](./ECMA-262_5.1.pdf)

AuxData.ai verwendet eine interne Javascript Engine mit dem Namen [`"Otto"`](https://github.com/robertkrimen/otto). Grundsätzlich funktionieren nahezu alle nativ in Javascript vorhandenen Operationen. 

Ausnahmen bilden reguläre Ausdrücke und zwar folgende Operatoren:
```
(?=)  // Lookahead (positive), currently a parsing error
(?!)  // Lookahead (backhead), currently a parsing error
\1    // Backreference (\1, \2, \3, ...), currently a parsing error
```

Zusätzlich zu den Standardfunktionen haben wir einige zusätzliche Module integriert, die Funktionen der AuxData.ai Plattform in die ScriptEngine integrieren und die Zusammenarbeit mit der Plattform optimieren.

## AuxData.ai Module

| Name | Beschreibung | Detailinformationen |
| ------ | ------ | ------ |
| AI Services | Aufruf von AI Services | [AI Services](./functions/aiservice.md)|
| Dokumentenleser | Liest Text aus Dokumenten aus |[Dokumentenleser](./functions/documentreader.md) |
| Dokumenten DB | Organisationsspezifischer Speicher | [Dokumenten DB](./functions/documentdb.md) |
| Mail (Imap / Smtp) | Mails auslesen und versenden | [MAIL IMAP / SMTP](./functions/mail.md) |
| HTTP Rest | HTTP Aufrufe für REST Schnittstellen o.ä. | [HTPP Aufrufe](./functions/http.md) |
| Logging | Loggingfunktionen innerhalb der Engine | [Logging](./functions/logging.md) |
| MS Graph API | Zugriff auf die MS Graph API (Mail) | [MS Graph API](./functions/msgraphapi.md) |
| MS Graph Calendar | Zugriff auf den MS Graph Kalender | [MS Graph Calendar](./functions/msgraphcalendar.md) |
| Google Workspace — Mail | Gmail-Zugriff über die Google Workspace API | [Google Mail](./functions/googlemail.md) |
| Google Workspace — Calendar | Google-Kalender-Zugriff | [Google Calendar](./functions/googlecalendar.md) |
| Google Workspace — Drive | Google-Drive-Zugriff (Datei- und Ordner-Operationen, Export) | [Google Drive](./functions/googledrive.md) |
| Google Workspace — Generic API | Generischer Bearer-Auth-Zugriff auf beliebige Google-APIs | [Google API](./functions/googleapi.md) |
| SQL | Zugriff auf externe SQL Datenbanken | [SQL Datenbanken anbinden](./functions/sql.md) |
| Webcrawler | Zugriff auf den Webcrawler | [Webcrawler](./functions/webcrawler.md) |
| Wissensdatenbank | Zugriff auf die WissensDB und KI basierter Suche | [Wissensdatenbank](./functions/knowledgedb.md) |
| Organisation | Lese-Zugriff auf die Organisations- und Connector-Konfiguration | [Organisation](./functions/organisation.md) |
| User | Lese-Zugriff auf den aktuellen Benutzer | [User](./functions/user.md) |
| HeyGen | KI-Video-Übersetzung über die HeyGen-API | [HeyGen](./functions/heygen.md) |
| Teams Bot | Proaktive Teams-Chat-Nachrichten an Benutzer | [Teams Bot](./functions/teamsbot.md) |

## Weitere Informationen
- [Umgebungsvariablen](./environmentvariables.md)
- [Übergabe Objekte anlegen](./functions/createparameterobjects.md)