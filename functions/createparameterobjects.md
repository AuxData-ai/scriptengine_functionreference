Die ScriptEngine unterstützt ECMAScript 5. Features von ECMA Script 6 (aktuelle Javascript Version) werden nicht unterstützt, somit können nicht alle modernen Features genutzt werden.

Javascript Objekte können auf unterschiedliche Art und Weise in der ScriptEngine angelegt werden. Hier sind die unterstützten Möglichkeiten aufgeführt:

## 1. Objekt anlegen und über Array Funktion die Membervariablen definieren

```
var imap = new Object();
imap["User"] = "user";
imap["Password"] = "pwd";
imap["Server"] = "server";
```

## 2. Objekt mit Klammern anlegen

```
var imap = {
User: "user",
Password: "pwd",
Server: "server",
};
```

## 3. Objekt mit Punktnotation


```
var imap = new Object();
imap.User = "user";
imap.Password = "pwd";
imap.Server = "server";
```