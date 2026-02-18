# Code Beispiele der ScriptEngine 


## Excelsheet auslesen

```
function removeMetaDataFromDocument(str) {
    // Find the index of the first '[' character
    var bracketIndex = str.indexOf('[');
    
    // If '[' is found, return substring starting from that position
    // If not found (indexOf returns -1), return empty string
    return bracketIndex !== -1 ? str.substring(bracketIndex) : '';
}

document = removeMetaDataFromDocument(document);


try {
    jsonObj = JSON.parse(document);
} catch (e) {
    log_error("could not parse json document. Expected xlsx File. Error: "+ e)
    return "Konnte Exceldatei nicht auslesen.\n ExcelDatei:\n" + document;
}

return "erfolgreich";
```

## Dokument in Wissensdatenbank speichern

```
getModule("knowledgedb");
var document = "Base64String...==";
var docId = knowledgedb_saveBinary(agentId, containerId, "Test.pdf", document, "");
return docId;
```

## Email versenden
```
getModule("mail")
var mailObj = new Object();
var to = new Array();
var from = "mail.werner.meyer@web.de";
to.push(receiver);
mailObj["From"] = from;
mailObj["To"] = to;
mailObj["Subject"] = subject;
mailObj["Message"] = message;

var smtp = new Object();
smtp["User"] = smtp_user;
smtp["Password"] = smtp_passwort;
smtp["Smtpserver"] = smtp_server;
smtp["Smtpport"] = parseInt(smtp_port);
smtp["SenderAddress"] = from;


result = mail_send(mailObj, smtp)
return result;
```

## Internetreceherche mit Google

```
getModule("mail")
var mailObj = new Object();
var to = new Array();
var from = "mail.werner.meyer@web.de";
to.push(receiver);
mailObj["From"] = from;
mailObj["To"] = to;
mailObj["Subject"] = subject;
mailObj["Message"] = message;

var smtp = new Object();
smtp["User"] = smtp_user;
smtp["Password"] = smtp_passwort;
smtp["Smtpserver"] = smtp_server;
smtp["Smtpport"] = parseInt(smtp_port);
smtp["SenderAddress"] = from;


result = mail_send(mailObj, smtp)
return result;
```
