# Declarative DSL to Check Conditions in JSON Schema

This document provides an overview of a declarative Domain Specific Language (DSL) designed to check various conditions in a JSON schema. The DSL is structured as an array of conditions, with each condition containing three mandatory fields: `path`, `operation`, and `value`.

## DSL Structure

### Condition Fields

- **path**: Specifies the path to the target in the schema where the operation will be performed.
- **operation**: Specifies the type of operation to perform at the target path.
- **value**: Specifies the value to be used in the operation.

## Supported Operations

### Type Checks

- **type-is**: Checks if the type of the target matches the specified type.
- **contains-type**: Checks if an array contains elements of a specified type.

### Equality Checks

- **equals**: Checks if the target equals the specified value.
- **not-equals**: Checks if the target does not equal the specified value.
- **size-equals**: Check if the size of the array or number of properties of the object equal the specified value. 

### Presence Checks

- **has-key**: Checks if the target object has the specified key.

### Array Operations

- **contains**: Checks if the array contains the specified value.

### Object Operations

- **min-properties**: Checks if the object has a minimum number of properties.

### Numeric Operations

- **maximum**: Checks if the numeric target is less than or equal to the specified value.
- **minimum**: Checks if the numeric target is greater than or equal to the specified value.

### String Operations

- **match-pattern**: Checks if the string target matches the specified regex pattern.

## Instances of majorly used cases:

### 1. Check that an object property exists and equals a string

```json
{    
    "$anchor": "#foo"
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "$anchor"},
 {"path": ["$anchor"], "operation": "equals", "value": "#foo"}
];
```
- First condition: Checks if the root of the schema has a key `$anchor`.
- Second condition: Checks if the value of `$anchor` equals the string "#foo".

### 2. Check that an object property exists

```json
{    
    "items": {},
    "minItems": 3
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "minItems"}
]
```

- Condition: Checks if the root of the schema has a key `minItems`.

### 3. Check that an object property exists and that it has any sibling properties

```json
{    
    "prefixItems": [],
    "items": {}
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "prefixItems"},
 {"path": [], "operation": "has-key", "value": "items"}
]
```

- First condition: Checks if the root of the schema has a key `prefixItems`.
- Second condition: Checks if the root of the schema also has a key `items`.

### 4. Check that an object property exists and matches a given regex

```json
{    
    "$anchor": "te:st"
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "$anchor"},
 {"path": ["$anchor"], "operation": "match-pattern", "value": ".+:.+"}
]
```

- First condition: Checks if the root of the schema has a key `$anchor`.
- Second condition: Checks if the value of `$anchor` matches the regex pattern .+:.+.

### 5. Check that an object property exists and that it does not equal a given number/integer

```json
{    
    "exclusiveMinimum": 0
}
```
```json
[
 {"path": ["exclusiveMinimum"], "operation": "not-equals", "value": 0}
]
```

- Condition: Checks if the value of `$anchor` does not equal the value 0.

### 6. Check that an object property exists that it is of a given type

```json
{    
    "prefixItems": [{ "type": "string" }]
}
```
```json
[
 {"path": ["prefixItems"], "operation": "type-equals", "value": "array"}
]
```

- Condition: Checks if the value of `prefixItems` is of type array.

### 7. Check that an object property exists that it is of a given type and that a specific adjacent property exists

```json
{    
    "prefixItems": [],
    "items": {}
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "prefixItems"}
 {"path": ["prefixItems"], "operation": "type-equals", "value": "array"},
 {"path": [], "operation": "has-key", "value": "items"}
]
```

- First condition: Checks if the root of the schema has a key `prefixItems`. 
- Second condition: Checks if the value of `prefixItems` is of type array.
- Third condition: Checks if the root of the schema has a key `items`.

### 8. Check that an adjacent property exists and its type, and that an object property exists
 Draft 4 to Draft 6 => `exclusiveMinimum` and `exclusiveMaximum`  
SAME AS ABOVE

### 9. Check that an object property exists, that is an array, and it contains a specific string item

```json
{    
    "type": [ "any", "number" ]
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "type"},
 {"path": ["type"], "operation": "contains", "value": "any"}
]
```

- First condition: Checks if the root of the schema has a key `type`.
- Second condition: Checks if the array `type` contains the value "any".

### 10. Check that an object property exists, that is an array, and it contains an item of a given type

```json
{    
    "type": [ {"const": "hello"}, "number", "string" ]
}
```
```json
[
 {"path": ["type"], "operation": "type-equals", "value": "array"},
 {"path": ["type"], "operation": "contains-type", "value": "object"}
]
```

- First condition: Checks if the value of `type` is of type array.
- Second condition: Checks if the array `type` contains any elements of type object.

### 11. Check that an object property exists, that it is an object, and that any of its values have a specific property

```json
{
  "dependencies": {
    "foo": { "type": "object" }
  }
}
```
```json
[
 {"path": ["dependencies", {"pattern": ".*"}], "operation": "has-key", "value": "type"}
]
```

- Condition: Checks if any value within the `dependencies` object has a key `type`.

### 12. Check that an object property exists, that it is an object, and that any of its values are of a specific type

```json
{
  "dependencies": {
    "foo": { "type": "object" },
    "bar": [ "foo" ]
  }
}
```
```json
[
 {"path": ["dependencies", {"pattern": ".*"}], "operation": "type-is", "value": "array"}
]
```

- Condition: Checks if any value within the `dependencies` object is of type array.

### 13. Check that an object property exists and its size

```json
{    
    "$ref": "#/$defs/foo",
    "$defs":{ "foo": {} }
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "$ref"},
 {"path": [], "operation": "size-equals", "value": 1}
]
```

- First condition: Checks if the root of the schema has a key `$ref`.
- Second condition: Checks if the root schema has exactly 1 property.

### 14. Check that an object property exists, that it is an object, and that a value is an array of a certain size

```json
{    
    "prefixItems": []
}
```
```json
[
 {"path": [], "operation": "has-key", "value": "prefixItems"},
 {"path": ["prefixItems"], "operation": "size-equals", "value": 0}
]
```

- First condition: Checks if the root of the schema has a key `prefixItems`.
- Second condition: Checks if the array `prefixItems` has exactly 0 items.

### 15. Check that an object property is a real number that represents an integer

```json
[
 {"path": ["properties", "baz"], "operation": "type-equals", "value": "integer"}
]
```

- Condition: Checks if the value of properties.baz is an integer.

### 16.  Check that an object property exists, that it is an object, and that any of its keys represent Perl regular expressions that are not also ECMA

### 17.  Check that an object property exists and that is represents a Perl regular expression that is not ECMA

16 and 17 are to be implemented
