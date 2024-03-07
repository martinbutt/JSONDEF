# JSONDEF

JSONDEF (JSON Data Exchange Format) is a proprietary JSON specification for the exchange of data and models between two sources.

Reversed properties are prefixed with `@`

The only required properties are `@schema` and `@version`, which are in the root JSON document.

`@schema` must contain `jsondef`

`@version` must contain the schema version in semver format.

There are three other optional properties in the root JSON document: `@models`, `@data` and `@metadata`.

Additional properties are allowed in the root JSON document, as long as they do not start with `@`.

`@models` contains a list of objects representing data models. Each must contain a `@class` property, and can contain any other properties that do not start with `@`.

`@data` contains any other properties, and can contain any other properties that do not start with `@`.

`@metadata` contains information about the request itself, and requires `@success` (boolean) and `@message` (string) properties. `@code` and `@timestamp` are optional, and can contain any other properties that do not start with `@`.

`@pagination` is optional. When used, either `@offset` or `@cursor` are required depending on if you are using offset-based or cursor-based pagination.

```
{
    "@schema": "jsonspec",
    "@version": "0.1",

    "@models": [
        {
            "@class": "MyDataModel"
        }
    ],

    "@data": {
        "MyData": "Some data"
    },

    "@metadata": {
        "@success": true,
        "@message": "The request was successful",
        "@code": 123,
        "@timestamp": 123123123
    },
    
    "@pagination": {
        "@offset": {
            "@page": 1,
            "@pageSize": 10,
            "@total": 150
        },
        "@cursor": {
            "@previous": 123
            "@current": 234,
            "@next": 345,
            "@pageSize": 10,
        }
    }
}
```
