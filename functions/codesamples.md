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
getModule("http");
getModule("log");

function getLinks(jsonData, pageCount) {
  // Überprüfe, ob das JSON-Objekt eine Eigenschaft "items" hat
  if (jsonData.items) {
    // Verwende die Methode "slice", um die ersten pagecount Elemente aus dem Array "items" herauszulesen
    var websites = jsonData.items.slice(0, pageCount);
    var links = new Array();
    
    log_info("found entries: " + websites.length);
    log_info("pages to crawl count: " + pageCount);
    
    for (var pos = 0; pos < websites.length && pos < pageCount; pos++) {
        var item = websites[pos];
        log_info("found website " + item.link);
        links.push(item.link);
    }
    
    // Rückgabe der Links
    return links;
  } else {
    // Wenn das JSON-Objekt keine Eigenschaft "items" hat, wird ein leerer Array zurückgegeben
    return [];
  }
}

log_info("Pages Parameter: " + pages);
var pageCount = parseInt(pages);
log_info("Extracted pageCount: " + pageCount);

var webSearch = "https://www.googleapis.com/customsearch/v1?key=" + apikey + "&q=" + http_queryEscape("\""+ search + "\""); 

log_info(webSearch);

var result = "";

if (webSearch != "") {
    jsonString = http_get(webSearch, "");    
    //log_info(jsonString);
    jsonObj = JSON.parse(jsonString);
    var links = getLinks(jsonObj, pageCount);

    if (links != null) {
        for (var pos = 0; pos < links.length; ++pos) {
            log_info("Parse website " + links[pos]);
             result += http_get(links[pos]);    + "\n";        
        }
    }
}

return result;
```
