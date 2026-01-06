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
| MS Graph API | Zugriff auf die MS Graph API | [MS Graph API](./functions/msgraphapi.md) |
| SQL | Zugriff auf externe SQL Datenbanken | [SQL Datenbanken anbinden](./functions/sql.md) |
| Webcrawler | Zugriff auf den Webcrawler | [Webcrawler](./functions/webcrawler.md) |
| Wissensdatenbank | Zugriff auf die WissensDB und KI basierter Suche | [Wissensdatenbank](./functions/knowledgedb.md) |

## Weitere Informationen
- [Umgebungsvariablen](./environmentvariables.md)
- [Übergabe Objekte anlegen](./functions/createparameterobjects.md)