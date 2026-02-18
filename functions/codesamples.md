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
