# Częstolot - API

## Endpoints


 ### Logowanie
 |type|endpoint|
 |---|---|
 |HTTP POST|/login||

 ##### Parameters:
  - email
 ##### Responce:
 - token - token used in every future request
**or**
 - error register - ask to register
 
 Example 1: 
```json
{
    "token": "wr31283"
}
```
Example 2:
```
register
```

**Important!**
In every future request API can drop error like eg. wrong token. Error is dropped wrapped in json:

```json
{
    "error": "tokenNotFound"
}
```
 Errors are  described in last part of document.
 
### O użytkowniku

 |type|endpoint|
 |---|---|
 |HTTP POST|/me||

 ##### Parameters:
  - token
 ##### Responce:

|key|type|value description|
|-|-|-|
|name|string|user real name|
|email|string|user email|
|status|enum("not", "silver", "gold")|user status|
|totalMiles|int|total miles with class bonus|

Example:
 ```json
 {
    "name": "John Smith",
    "email": "example@example.com",
    "status": "not",
    "totalMiles": 123
 }
 ```

 ### Dodaj lot
 
  |type|endpoint|
 |---|---|
 |HTTP POST|/add||

 ##### Parameters:
  - token
  - from
  - to
  - date
  - class [^flight-class-enum]
  - type [^flight-type-enum]
 
 ##### Responce:

|key|type|value description|
|-|-|-|
|status|enum("error", "ok")|indicate if operation success|
|extraMiles|int|only when **status=ok**, how many miles added|
|description|string|only when **status=error**, error description|

Example:
 ```json
 {
    "status": "ok",
    "extraMiles": 125,
 }
 ```

 ### Wyświetl loty
  
  |type|endpoint|
 |---|---|
 |HTTP POST|/show|

 ##### Parameters:
  - token
  - limit *(not mandatory)* - int
  - start *(not mandatory)* - int (index of first element)
 
 ##### Responce:

All parameters in array. See example.

|key|type|value description|
|-|-|-|
|from|enum [(IATA codes)][IATA-codes]|from airport code|
|to|enum [(IATA codes)][IATA-codes]|to airport code|
|date|date object|date of flight|
|realMiles|int|miles in aircraft|
|class|[^flight-class-enum]|class|
|type|[^flight-type-enum]|One-way or Round Trip|
|miles|int|miles after class bonus|

Example 1:
 ```json
 {
    "flights": [
        {
            "from": "LUZ",
            "to": "JFK",
            "date": "2016-01-31",
            "realMiles": 127,
            "class": "economy",
            "type": "one",
            "miles": 321
        },
        {
            "from": "WAW",
            "to": "KRK",
            "date": "2017-01-14",
            "realMiles": 12,
            "class": "economy",
            "type": "round",
            "miles": 32
        }
    ]
 }
 ```
 
 ## Data types
 
 #### Date
 string -> yyyy-mm-dd
 *ex:*
 ```"2017-10-28"```
 
 ## Errors
 
 ####
 ```tokenNotFound``` - user should re-login

 [^flight-class-enum]: enum(“economy”,“premium”,“buisness”,“first”)
 [^flight-type-enum]:  	enum(“one”,“round”)
 
 [date-spec]: <https://stackoverflow.com/a/15952652>
 [IATA-codes]: <https://www.world-airport-codes.com/alphabetical/airport-code/a.html>
