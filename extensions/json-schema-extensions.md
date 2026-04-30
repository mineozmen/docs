---
description: Standard JSON schema is extended mainly for advanced validation use cases
icon: table
---

# JSON Schema

The following keywords can be used in addition to JSON schema standards:

## errorMessage

Supported both on client and server sides, "errorMessage" keyword allows displaying custom error messages when certain validation rules fail. This keyword expects an object outlining messages to display for different types of validation rules, such as:

```json
"title": "Base Price"
"type": "number",
"errorMessage": {
    "type": "Price needs to be numerical",
    "required": "Missing price information"
}
```

## x-validation

On server side, "x-validation" keyword allows applying a set of validation rules to all or select fields at once, such as:

```json
"title": "Product",
"type": "object",
"x-validation": {
    "validator-1": { //label for validator
        "scope": { //either type or fields to apply validation
            "type": "string",
            "fields": ["data.name", "data.surname"]
        },
        "validator": "com.networknt.schema.MinLengthValidator", //validator class
        "props": 3 //props used by the validator
    }
}
```

## x-jmespath

On server side, "x-jmespath" keyword allows applying JMESPath patterns for validating input data. The pattern can either return boolean (false representing invalid data) or an object with valid and message fields, such as:

```json
"title": "Prices",
"type": "object",
"x-jmespath": "basePrice>salePrice"
```

## x-jsoup

On server side, "x-jsoup" keyword allows detection of HTML tags with XSS risk, with the option to select safety level (none, basic, basicWithImages, relaxed or simpleText), such as:

```json
"title": "Description",
"type": "string",
"x-jsoup": "basic"
```

## x-lazy

On server side, "x-lazy" keyword with boolean value allows returning a validation process immediately when a validation error is discovered, skipping evaluation for the remaining rules.

