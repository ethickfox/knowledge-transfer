---
Duration: Invalid date
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Ready To Discuss
Topic: []
---
## JavaScript Object Notation

It is a popular format used for sharing data online. It’s syntax is based on the JavaScript language, but might be used with any language. Mostly it used for data exchange including REST Api requests and responses or application configurations. One of the main advantage of JSON between it’s nearest competitor - XML, is an ease of reading and amount of characters.

```JavaScript
{
  type : 'renault',
  name : 'name'
}
```

```JSON
{
  "type": "renault",
  "name": "name"
}
```

```XML
<car>
  <type>renault</type>
  <name>name</name>
</car>
```

### Data Types

|   |   |
|---|---|
|string|String in ””|
|number|integer value|
|float|decimal value|
|array||
|object|Object with properties inside {}|
|boolean|true/false|
|empty|null|

### Numbers

No leading zeros or Trailing Decimals are allowed

### Comments

In JSON no comments allowed

### Arrays

Arrays support only values, without keys inside

```JSON
[
  {
    "brand": "bmw",
    "color": "yellow",
    "model_year": 2020
  },
  {
    "brand": "renault",
    "color": "green",
    "model_year": 2030
  }
]
```

```JSON
{
  "owners": [
    "Ivan",
    "Anton",
    "Maxim"
  ]
}
```

## Schema

It is a blueprint for what data in your JSON should look like. You can create schema of what date you are expecting to receive. And than use it in your program for verification of JSON correctness

  

```JSON
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "A product in the catalog",
  "type": "object",
  "properties": {
    "id": {
      "type": "number"
    },
    "name": {
      "type": "string"
    }
  },
  "required": [
    "id",
    "name"
  ]
}
```

1. **"$schema"** Both used as a JSON Schema dialect identifier and as the identifier of a resource which is itself a JSON Schema, which describes the set of valid schemas written for this particular dialect. May refer to the current schema
2. **"$id"** defines a URI for the schema, and the base URI that other URI references within the schema are resolved against. Note that this URI is an identifier and not necessarily a network locator. In the case of a network-addressable URL, a schema need not be downloadable from its canonical URI
3. **"title" and “description”** are descriptive only. They do not add constraints to the data being validated.
4. **"type"** data type indication for value
5. **“properties”** list of available inner keys
6. **"id", "name"** key names
7. **"type"** valid value type for this name
8. **“required”** list of required fields

Example of valid JSON for this schema

```JSON
{
  "id": 900,
  "name": "Ivan"
}
```

Invalid, because starts from array

```JSON
[
  {
    "id": 901,
    "name": "Max"
  },
  {
    "id": 900,
    "name": "Ivan"
  }
]
```

  

To generate schema, use

> [!info] JSON Schema Generator  
> JSON Schema Generator - automatically generate JSON schema from JSON.  
> [https://jsonschema.net/app/schemas/0](https://jsonschema.net/app/schemas/0)


[https://jsonapi.org](https://jsonapi.org/)