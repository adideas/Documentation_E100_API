# E100 API PUBLIC

# Authentication ([View](https://publicapi.e100.eu/#api-Authentication)) (OAuth2.0)

Authentication - Obtaining an access token To work with most API methods, you must pass an access-token - a special access token - in the request header. It is a string of Latin letters and numbers. In the future, the received token must be specified in the headers (headers) of requests requiring authorization.

[![](https://github.com/adideas/E100API/blob/master/ru/postman.JPG)]()

```sh
	https://publicapi.e100.eu/token
```

| ... | ... | ... |
| ------ | ------ | ------ |
| grant_type | password | Connection type Default: password |
| username | '' | User login (company manager: login or email; driver: card number (9 digits)). Valid values: latin, letters, numbers, symbols, '@' ', and,'. ', (Period) |
| password | '' | Password (head of the company: password to the site's LC; driver: password to the mobile application E100App (set by the client in the LC)) |


```sh
{
    "grant_type": "password",
    "username": "user1",
    "password": "somesecretpassword"
}
```

| ... | ... |
| ------ | ------ |
| Content-Type | application/json |

Status 200

```sh
"data": {
  "access_token": "33fb89523d56118f838b8857debde4b6f683930d", // TOKEN!!! -> Headers (access-token)
  "expires_in": 32400,
  "token_type": "Bearer",
  "scope": null,
  "refresh_token": "734a3d23b7f87ecf9b40f399eb398ad2a2c33f11",
  "user_id": "epk",
  "first_name": null,
  "last_name": null,
  "code": "19-42",
  "email": "mail@company.com",
  "tarif": "A",
  "country": "pl",
  "fullname": "EPK",
  "group_id": "2",
  "defcur": "EUR",
  "client_type": "1",
  "REFNumber": null
}
```

Status 401 (Unauthorized)

```sh
"data": {
  "error": "invalid_grant",
  "error_description": "Invalid username and password combination"
}
```

# Transactions ([View](https://publicapi.e100.eu/#api-Transactions))

```sh
	https://publicapi.e100.eu/transactions
```

| ... | ... |
| ------ | ------ |
| Content-Type | application/json |
| access-token | 33fb89523d56118f838b8857debde4b6f683930d |

| ... | ... | ... |
| ------ | ------ | ------ |
| page_number | Number / Integer | Page number, default value 1, allowed values ​​(1 -> n). By default: 1. Allowed values: (1 -> n) |
| page_size | Number / Integer | The number of transactions per page, the default value is 100, the valid values ​​are 1… 2000 Default: 100 The valid values ​​are: 1..2000 |
| lang | String | Language Default: en Restrictions: 2 Allowed Values: "en, pl, ru" |
| from | DateTima | Period start date By default: 1. First day of the current month; 2. If only the date, time is explicitly specified: '00: 00: 00 'Allowed values: YYYY: 2000-2100 ;, mm: 01-12 ;, dd: 01-31 ;, HH: 00-23 ;, ii: 00-59 ;,: 00-59 |
| to | DateTime | Period end date (1. The “to” parameter must be later than “from”; 2. The difference between the time “to” and “from” must not exceed 3 months (error code 4)) By default: 1. Current time; 2. If only the date, time is explicitly specified: '23: 59: 59 'Allowed values: YYYY: 2000-2100 ;, mm: 01-12 ;, dd: 01-31 ;, HH: 00-23 ;, ii: 00-59 ;,: 00-59 |
| regions | Array [] | Array of countries (empty value - “all countries”) Default: [] Valid values: ['at', 'am', 'az', 'by', 'be', 'bg', 'ba', 'hu ',' de ',' nl ',' ge ',' dk ',' es', 'it', 'kz', 'lv', 'lt', 'lu', 'md', 'pl', 'ru', 'ro', 'rs', 'sk', 'si', 'ua', 'fr', 'cz', 'se', 'ee'] |
| services | Array [] | service id Default: [] Valid values: 1..n |
| cards | Array [] | Array of card numbers (full 19 digits) Default: [] Limitations: 19 |
| auto_numbers | Array [] | Array of car numbers Default: [] Allowed values: "Letters and numbers" |
| stations | Array [] | Array of service points (E100 code) Default: [] Allowed values: "latin letters, numbers, space, comma" |
| exposed | Boolean | Flag "only exposed" and "only current (not exposed)" Default: if not specified - all transactions Allowed values: "'true' - exposed, 'false' - not exposed" |
| delay_hours | Number / Integer | Late transactions - in which the difference between the date of completion and the date of adding to processing is greater than or equal to delay_hours (in hours). Only late transactions will be returned in response. Valid values: 1..n |
| invoice_id | Array [] | Billing number / Billing number Allowed values: numbers |


Example responce
```sh
{
    "page_number": 1, // Current page
    "page_count": 2, // Number of pages
    "page_size": 100, // The number of transactions on the page
    "row_count": 133, // Total number of transactions
    "transactions": [
        {
            "datetime_insert": "2020-08-31T09: 29: 34", // Date the transaction was added to the database (GMT +3)
            "confirmed": true, // confirmed / unconfirmed transaction
            "auto": "********", // Vehicle number
            "card": "*******", // Full card number
            "card_shortname": "*******", // Short card number
            "ticket": "4396", // Check number
            "currency": "RUR", // Transaction currency
            "date": "2020-08-31T12: 29: 04 + 03: 00", // Date of the transaction
            "card_driver": null, // Driver's license number
            "excise": null, // Excise
            "exposed": false, // exposed / not exposed transaction
            "invoice_date": null, // Invoice date
            "invoice_id": null, // Invoice number
            "price": 43.6515, // Transaction price
            "volume": 370, // Number of liters
            "service_id": 21, // Service id
            "service_name": "Diesel fuel", // Service name
            "station_id": "RU6696", // Service point code E100
            "category": 2, // Benefits of service at the service point E100
            "address": "Moscow - Voronezh, 200 km, v. Kukuy, white and green, to the left", // Address of the service point Е100, where the transaction was made
            "brand": "TATNEFT", // Brand of the service point E100, where the transaction was made
            "sum": 16150.5, // Transaction amount
            "driver": "" // Driver surname
        },
...
    ]
}
```
