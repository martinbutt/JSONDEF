# JSONDEF

JSONDEF (JSON Data Exchange Format) is a proprietary JSON specification for the exchange of data and models between two sources.

Reversed properties are prefixed with `@`

The only required properties are `@schema` and `@version`, which are in the root JSON document.

`@schema` must contain `jsondef`

`@version` must contain the schema version in semver format.

There are three other optional properties in the root JSON document: `@models`, `@data` and `@metadata`.

Additional properties are allowed in the root JSON document, as long as they do not start with `@`.

`@models` contains a list of objects representing data models. Each must contain a `@class` property, can contain an optional `@schema` property (see [Model Schemas](#model-schemas), and can contain any other properties that do not start with `@`.

`@data` contains any other properties, and can contain any other properties that do not start with `@`.

`@metadata` contains information about the request itself, and requires `@success` (boolean) and `@message` (string) properties. `@code` and `@timestamp` are optional, and can contain any other properties that do not start with `@`.

`@pagination` is optional. When used, either `@offset` or `@cursor` are required depending on if you are using offset-based or cursor-based pagination.

#### Example

```
{
    "@schema": "jsonspec",
    "@version": "0.1",

    "@models": [
        {
            "@class": "PersonModel",
            "firstName": {
                "type": "string",
                "value": "John"
            },
            "lastName": {
                "type": "string",
                "value": "Doe"
            },
            "age": {
                "type": "integer",
                "value": "22"
            },
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
## Model Schemas

JSONDEF can be used with any model schema, including custom ones

### Usage with JSON Schema

#### Example

```
{
    "@schema": "jsonspec",
    "@version": "0.1",

    "@models": [
        {
            "@class": "MyDataModel",
            "@schema": "Person",
            "firstName": "John",
            "lastName": "Doe",
            "age": 22,
        }
    ],
    "@modelSchemas": [
        {
            "$id": "https://example.com/person.schema.json",
            "$schema": "https://json-schema.org/draft/2020-12/schema",
            "title": "Person",
            "type": "object",
            "properties": {
                "firstName": {
                    "type": "string",
                    "description": "The person's first name."
                },
                "lastName": {
                    "type": "string",
                    "description": "The person's last name."
                },
                "age": {
                    "description": "Age in years which must be equal to or greater than zero.",
                    "type": "integer",
                    "minimum": 0
                }
            }
        }
    ]
    ...
}
```

### Usage with TypeSchema

#### Example

```
{
    "@schema": "jsonspec",
    "@version": "0.1",

    "@models": [
        {
            "@class": "MyDataModel",
            "@schema": "Person",
            "firstName": "John",
            "lastName": "Doe",
            "age": 22,
        }
    ],
    "@modelSchemas": [
        {
            "definitions": {
                "Person": {
                    "type": "object",
                    "properties": {
                        "firstName": {
                            "type": "string"
                        },
                        "lastName": {
                            "type": "string"
                        },
                        "age": {
                            "type": "integer"
                        }
                    }
                }
            },
        "$ref": "Person"
        }
    ]
    ...
}
```
