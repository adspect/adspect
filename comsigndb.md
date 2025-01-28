# ComsignDB

ComsginDB is an ad-hoc NoSQL content database software with on-disk persistence and Redis-like access semantics.

Copyright &copy; 2020&ndash;2024 by [Comsign](https://www.comsign.io/).  All rights reserved.

## Usage

Running ComsignDB from command line:

```
# comsigndb --config /etc/comsigndb.json
# comsigndb --help
```

## HTTP API

Available HTTP API endpoints are detailed below.

Some endpoints either expect JSON-encoded data in request body, or return JSON-encoded data in response body; in the latter case,
ComsignDB will set the `Content-Type: application/json` response header.

### List Databases

```
GET /
```

Response example:

```json
[
  "database1",
  "database2"
]
```

### Save All Databases to Disk

```
SAVE /
```

### Load All Databases from Disk

```
LOAD /
```

### Save Database to Disk

```
SAVE /<database>
```

### Load Database from Disk

```
LOAD /<database>
```

### List Keys

```
GET /<database>
```

This endpoint returns an object with key types as property names and arrays of keys as property values.

Response example:

```json
{
  "set": [
    "key1",
    "key2"
  ],
  "fileset": [
    "key1",
    "key2"
  ]
}
```

### List Keys by Type

```
GET /<database>/{set|fileset}
```

Response example:

```json
[
  "key1",
  "key2"
]
```

### Create Keys

Unlike Redis, keys are not implicitly created by insertion operations and must be created manually before they can be used.

```
PUT /<database>/{set|fileset}
[
  "key1",
  "key2"
]
```

### Delete Keys

```
DELETE /<database>/{set|fileset}
[
  "key1",
  "key2"
]
```

### Set String

```
PUT /<database>/string/<key>
Hello, world!
```

### Get String

```
GET /<database>/string/<key>
```

### Add Set Elements

```
PUT /<database>/{set|fileset}/<key>
[
  "element1",
  "element2"
]
```

### Remove Set Elements

```
DELETE /<database>/{set|fileset}/<key>
[
  "element1",
  "element2"
]
```

### Get Set Cardinality

```
SIZE /<database>/{set|fileset}/<key>
```

Response example:

```json
42
```

### Get All Set Elements

```
GET /<database>/{set|fileset}/<key>
```

Response example:

```json
[
  "element1",
  "element2",
  "element3"
]
```

### Test Set Membership

```
TEST /<database>/{set|fileset}/<key>
[
  "element1",
  "element2",
  "element3"
]
```

This endpoint accepts an array of elements and returns an array of those elements that are present in the set.  The output array
preserves the order of elements in the input array.  If the same element appears several times in the input array, then it will
appear the same number of times in the ouput array, given that it is present in the set.

Response example:

```json
[
  "element1",
  "element3"
]
```

### Get Random Set Elements

```
RANDOM /<database>/{set|fileset}/<key>?n=<number>&seed=<seed>
```

This endpoint returns `<number>` of randomly picked set elements (defaults to 1.)  Uniqueness is not guaranteed, i.e. the resulting
array may contain the same elements multiple times.

Optionally, you may seed the random number generator with an arbitrary string `<seed>`.  If not specified, the RNG will be seeded
from the system entropy source.

Response example:

```json
[
  "element4",
  "element27",
  "element11"
]
```
