### JSON
JaveScript Object Notation
-  Standard for "serializing" data objects in human-readable format
-  Useful for data interchange, and for representing and stroing semistructured data
![RB vs JSON](https://i.imgur.com/gHCEo4n.png)
![XML vs JSON](https://i.imgur.com/INYQWqV.png)
- Basic constrcuts (recursive)
1. Base values
  - numbers, string, boolean
2. Object {label:value}
  - sets of label-value pairs
3. Arrays []
  - lists of values
```json
{ "Books":
  [
    { "ISBN":"ISBN-0-13-713526-2",
      "Price":85,
      "Edition":3,
      "Title":"A First Course in Database Systems",
      "Authors":[ {"First_Name":"Jeffrey", "Last_Name":"Ullman"},
                  {"First_Name":"Jennifer", "Last_Name":"Widom"} ] }
    ,
    { "ISBN":"ISBN-0-13-815504-6",
      "Price":100,
      "Remark":"Buy this book bundled with 'A First Course' - a great deal!",
      "Title":"Database Systems:The Complete Book",
      "Authors":[ {"First_Name":"Hector", "Last_Name":"Garcia-Molina"},
                  {"First_Name":"Jeffrey", "Last_Name":"Ullman"},
                  {"First_Name":"Jennifer", "Last_Name":"Widom"} ] }
  ],
  "Magazines":
  [
    { "Title":"National Geographic",
      "Month":"January",
      "Year":2009 }
    ,
    { "Title":"Newsweek",
      "Month":"February",
      "Year":2009 }
  ]
}
{ "type":"object",
  "properties": {
     "Books": {
        "type":"array",
        "items": {
           "type":"object",
           "properties": {
              "ISBN": { "type":"string", "pattern":"ISBN*" },
              "Price": { "type":"integer",
                         "minimum":0, "maximum":200 },
              "Edition": { "type":"integer", "optional": true },
              "Remark": { "type":"string", "optional": true },
              "Title": { "type":"string" },
              "Authors": { 
                 "type":"array",
                 "minItems":1,
                 "maxItems":10,
                 "items": {
                    "type":"object",
                    "properties": {
                       "First_Name": { "type":"string" },
                       "Last_Name": { "type":"string" }}}}}}},
     "Magazines": {
        "type":"array",
        "items": {
           "type":"object",
           "properties": {
              "Title": { "type":"string" },
              "Month": { "type":"string",
                         "enum":["January","February"] },
              "Year": { "type":"integer" }}}}
}}
```
- You can have items that are not specified in schema
  - but we can add "additionalProperties":false to forbid that 
