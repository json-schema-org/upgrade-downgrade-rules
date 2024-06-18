# Transformation DSL

The Transformation DSL is a concise language inspired by JSON Patch for specifying schema modifications using operations. It consists of an array of objects, each having the following keys:

## Tranform fields

| Property  | Description                                                      | Required | Data Type         |
|-----------|------------------------------------------------------------------|----------|-------------------|
| operation | Specifies the operation to perform                               | Yes      | String            |
| path      | Specifies the path to the target in the schema                   | Yes      | Array of Strings  |
| value     | Specifies the value to be used in the operation                  | No       | Any               |
| from      | Specifies the source path for operations involving move/copy     | No       | Array of Strings  |
| to        | Specifies the destination path for operations involving move/copy| No       | Array of Strings  |

## Operations

### `append-property`

Appends property in the provided `from` path to the `to` path. The `to` path should be an array, or it will be created as an empty array before appending.

- **Takes**: `from`, `to`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "properties": {
        "foo": {
            "required": true
        }
    },
    "required": []
}
```

**Transform**:

```json
[
    { "operation": "append-property", "to": [ "required" ], "from": [ "properties", "foo" ] }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "properties": {
        "foo": { 
            "required": true
        }
    },
    "required": [ "foo" ]
}
```

### `prefix-until-unique`

Prefixes matching keyword or keywords in the path with the provided value until it is unique.

- **Takes**: `path`, `value`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft-07/schema",
    "if": {},
    "x-if": {},
    "x-x-if": {},
    "$ref": "#foo"
}
```

**Transform**:

```json
[
    { "operation": "prefix-until-unique", "path": [ { "not": "$ref" } ], "value": "x-" }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft/2019-09/schema",
    "x-x-x-if":{},
    "x-x-x-x-if": {},
    "x-x-x-x-x-if" :{},
    "$ref": "#foo"
}
```

### `replace`

Replaces the existing value at the given path with the new value.

- **Takes**: `path`, `value`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft/2019-09/schema"
}
```

**Transform**:

```json
[
    { "operation": "replace", "path": [ "$schema" ], "value": "https://json-schema.org/draft/2020-12/schema" }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema"
}
```

### `replace-with-absolute`

Replaces the value at the given path with an absolute value.

- **Takes**: `path`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "divisibleBy": -2
}
```

**Transform**:

```json
[
    { "operation": "replace-with-absolute", "path": [ "divisibleBy" ] }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "divisibleBy": 2
}
```

### `remove`

Removes the key and its respective value at the given path.

- **Takes**: `path`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "type": "any"
}
```

**Transform**:

```json
[
    { "operation": "remove", "path": [ "type" ] }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft-04/schema"
}
```

### `remove-substring`

Removes all occurrences of a given substring in the string at the specified path.

- **Takes**: `path`, `value`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft/2019-09/schema",
    "$anchor": "fo:o"
}
```

**Transform**:

```json
[
    { "operation": "remove-substring", "path": [ "$anchor" ], "value": ":" }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$anchor": "foo"
}
```

### `remove-and-append`

Removes the value from the `from` path and appends it to the `to` path. The `to` path should be an array, or it will be created as an empty array before appending.

- **Takes**: `from`, `to`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "type": [ { "minimum": 3 } ]
}
```

**Transform**:

```json
[
    { "operation": "remove-and-append", "to": [ "anyOf" ], "from": [ "type",  {} ] }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft-04/schema",
    "anyOf": [ { "minimum": 3 } ]
}
```

### `move`

Moves the value from the `from` path to the `to` path. The `to` can be an array or object.

- **Takes**: `from`, `to`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft-03/schema",
    "extends": [ { "type": "string" } ]
}
```

**Transform**:

```json
[
    { "operation": "move", "to": [ "allOf" ], "from": [ "extends" ] }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft-04/schema",
    "allOf": [ { "type": "string" } ]
}
```

### `add`

Adds a new value at the specified path. If the path already exists, it behaves like `replace`. You can add to an array at the last index by specifying `-1`.

- **Takes**: `path`, `value`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft/2019-09/schema",
    "contains": { "type": "string" },
    "not": { "type": "number" },
    "allOf": [],
    "unevaluatedItems": false
}
```

**Transform**:

```json
[
    { "operation": "add", "path": [ "allOf", "-" ], "value": { "not": { "not": { "contains": { "type": "string" } } } } }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "contains": { "type": "string" },
    "not": { "type": "number" },
    "allOf": [
        { "not": { "not": { "contains": { "type": "string" } } } }
    ],
    "unevaluatedItems": false
}
```

### `copy`

Copies the value from the `from` path to the `to` path. If there is an existing value at the `to` path, it will be replaced; otherwise, a new value will be created.

- **Takes**: `from`, `to`

**Before**:

```json
{
    "$schema": "https://json-schema.org/draft/2019-09/schema",
    "contains": { "type": "string" },
    "maxContains": 3,
    "allOf": [
        { "not": { "not": { "contains": { "type": "string" }, "maxContains": "temp" } } }
    ]
}
```

**Transform**:

```json
[
    { "operation": "copy", "to" : [ "allOf", 0, "not", "not", "contains", "maxContains" ], "from": [ "maxContains" ] }
]
```

**After**:

```json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "contains": { "type": "string" },
    "maxContains": 3,
    "allOf": [
        { "not": { "not": { "contains": { "type": "string" }, "maxContains": 3 } } }
    ]
}
```
