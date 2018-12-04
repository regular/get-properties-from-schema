```
const getProperties = require('get-properties-from-schema')

const schema = {
  "description": "A 2D/3D rectangle or box with position, anchor (origin) and rotation",
  "type": "object",
  "required": [
    "size",
    "type"
  ],
  "properties": {
    "type": {
      "const": "transform"
    },
    "size": {
      "$ref": "#/definitions/vec3f"
    }
  },
  "definitions": {
    "vec3f": {
      "type": "object",
      "required": [
        "x",
        "y"
      ],
      "properties": {
        "x": {
          "type": "number"
        },
        "y": {
          "type": "number"
        }
      }
    }
  }
}

test('get root properties', t => {
  const props = getProperties(schema)
  t.equal(props.length, 2)
  t.deepEqual(props[0], {key: 'type', value: {"const": "transform"}})
  t.deepEqual(props[1], {key: 'size', value: schema.definitions.vec3f})
  console.log(props[1])
  const size_props = getProperties(props[1].value)
  t.equal(size_props.length, 2)
  t.deepEqual(size_props[0], {key: 'x', value: {type: 'number'}})
  t.deepEqual(size_props[1], {key: 'y', value: {type: 'number'}})
  t.end()
})
```

Note: Only local $ref references are supported
