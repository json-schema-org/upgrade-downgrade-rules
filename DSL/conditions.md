# The Condition DSL

This document provides an overview of a declarative Domain Specific Language (DSL) designed to check various conditions for upgrade and downgrade rules in this repository.

## Why Not Use JSON Schema Validation?

There are several reasons for opting against JSON Schema as the schema language for these conditions:

- **Simplicity**: Implementers do not need a full-blown JSON Schema implementation to support upgrade/downgrade rules in their language of choice. This reduces the complexity and learning curve for developers.
- **Performance**: JSON Schema validation can be slow, especially on Alterschema. By focusing on a subset of operations, we achieve much better performance.
- **Specific Needs**: The modifications needed are typically only 1-2 levels deep, and only 20-30% of JSON Schema constraints are necessary to satisfy the conditions for upgrades/downgrades.

## DSL Structure

The DSL is structured as an array of conditions, with each condition containing three mandatory fields: `path`, `operation`, and `value`.

### Condition Fields

- `path`: Specifies the path to the target in the schema where the operation will be performed. It takes an array. Each element of the array can be:
  - A string
  - An object which can have pre-defined properties. Which is: `pattern`.
    Given a pattern, all the keywords matching the pattern undergo condition check and transformation iteratively.  
    Example: 

    #### Schema
    ```json
    {    
      "$schema": "https://json-schema.org/draft-06/schema",
      "properties": {
        "foo": {
            "required": true
        },
        "bar": {
            "required": false
        }
      }
    }
    ```
    #### Condition
    ```json
    [
      { "path": [ "properties", { "pattern": ".*" } ], "operation": "has-key", "value": "required" }
    ]
    ```

- `operation`: Specifies the type of operation to perform at the target path. Should be a string and take one of the supported operations mentioned below.

- `value`: Specifies the value to be used in the operation. Should take a value supported by the respective operation chosen.

## Operations

### Any

#### Type Checks

`type-is`: Checks if the type of the target matches the specified type.
 - **Value**: Should be a string with one of the primitive types: `integer`, `boolean`, `array`, `object`, `string`, `number`.

##### Schema
```json
{    
  "$schema": "https://json-schema.org/draft-03/schema",
  "type": [ { "const": "hello" }, "number", "string" ]
}
```
##### Condition
```json
[
  { "path": [ "type" ], "operation": "type-is", "value": "array" }
]
```

#### Equality Checks

`equals`: Checks if the target equals the specified value.
 - **Value**: Can be anything.

##### Schema
```json
{
  "$schema": "https://json-schema.org/draft-06/schema",
  "exclusiveMinimum": 0
}
```

##### Condition
```json
[
  { "path": [ "exclusiveMinimum" ], "operation": "equals", "value": 0 }
]
```

`not-equals`: Checks if the target does not equal the specified value.
 - **Value**: Can be anything.

##### Condition
```json
[
  { "path": [ "exclusiveMinimum" ], "operation": "not-equals", "value": -1 }
]
```

`size-equals`: Checks if the size of the array or number of properties of the object equals the specified value.
 - **Value**: Should be a positive integer.

##### Schema
```json
{    
  "$schema": "https://json-schema.org/draft-06/schema",
  "required": [ ]
}
```
##### Condition
```json
[
  { "path": [ "required" ], "operation": "size-equals", "value": 0 }
]
```

### Array Operations

Presence check:

`contains`: Checks if the array contains the specified value.
 - **Value**: Can be anything.

#### Schema
```json
{    
  "$schema": "https://json-schema.org/draft-03/schema",
  "type": [ "any", { } ]
}
```
#### Condition
```json
[
  { "path": [ "type" ], "operation": "contains", "value": "any" }
]
```

`contains-type`: Checks if an element in the array contains a specified type.
 - **Value**: Should be a string with one of the primitive types: `integer`, `boolean`, `array`, `object`, `string`, `number`.

#### Condition
```json
[
  { "path": [ "type" ], "operation": "contains-type", "value": "object" }
]
```

### Object Operations

Presence Checks:  

`has-key`: Checks if the target object has the specified key.
 - **Value**: Should be a string as the objects will always have keys of type string.

#### Schema
```json
{
  "$schema": "https://json-schema.org/draft-03/schema",
  "properties": {
    "foo": {
      "required": true
    }
  }
}
```
#### Condition
```json
[
  { "path": [ "properties", { "pattern": ".*" } ], "operation": "has-key", "value": "required" }
]
```

Property Count Check:

`min-properties`: Checks if the object has a minimum number of properties.
 - **Value**: Should be a positive integer.

#### Schema
```json
{ 
  "$schema": "https://json-schema.org/draft-07/schema",
  "type": "array",
  "items": {
    "$ref": "#",
    "type": "string"
  }
}
```
#### Condition
```json
[
  { "path": [ "items" ], "operation": "min-properties", "value": 2 }
]
```

### Numeric Operations

Numeric Bound Check:

`maximum`: Checks if the numeric target is less than or equal to the specified value.
 - **Value**: Should be a number.

#### Schema
```json
{
  "$schema": "https://json-schema.org/draft-03/schema",
  "divisibleBy": -1
}
```
#### Condition
```json
[
  { "path": [ "divisibleBy" ], "operation": "maximum", "value": -1 }
]
```

### String Operations

`match-pattern`: Checks if the string target matches the specified regex pattern.
 - **Value**: Should be a regular expression as per ECMA 262.

#### Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$anchor": "te:st"
}
```
#### Condition
```json
[
  { "path": [ "$anchor" ], "operation": "math-pattern", "value": ".*:.*" }
]
```