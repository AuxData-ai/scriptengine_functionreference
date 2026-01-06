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

Zusätzlich zu den Standardfunktionen haben wir einige zusätzliche Featrues integriert, die Funktionen der AuxData.ai Plattform in die ScriptEngine integrieren und die Zusammenarbeit mit der Plattform optimieren.

Hier findest Du die Funktionen:

- [AI Services](./functions/aiservice.md)
- [Dokumentenleser](./functions/documentreader.md)
- [Dokumenten DB](./functions/DocumentDb.md)
- [MAIL IMAP / SMTP](./functions/mail.md)
- [HTPP Aufrufe](./functions/http.md)
- [Logging](./functions/logging.md)
- [MS Graph API](./functions/msgraphapi.md)
- [SQL Datenbanken anbinden](./functions/sql.md)
- [Webcrawler](./functions/webcrawler.md)
- [Wissensdatenbank](./functions/knowledgedb.md)
- [Übergabe Objekte anlegen](./functions/createparameterobjects.md)