# Representing Schema Object as Python

OpenAPI Schema Object is equivalent of JSON Schema with some minor changes.

In theory, python objects are dictionaries with some some syntactic sugar added.
With the exception of `__dunder__` fields, a `__dict__` of python object could be directly validated against a JSON Schema.

Representation is not as simple, as the object model of JSON Schema is pretty much opposite of the object model of
type-hinted python, as enforced by mypy.

Since schema object represents only a collection of constraints, any object that meets the constraints is valid.
In particular, in case of the definition
```yaml
type: object
properties:
- field:
  type: string
required:
- field
additionalProperties: false
```
can be exactly represented as

```python
class SomeType:
    field: str | None
```

but if `additionalProperties` is skipped, then any object, that has at least this property, would've validated against the schema.

Such schema is well represented by a Protocol



## Representable keywords

### type

#### Atomic types

Atomic types are types other than object and array.

They are represented by their python equivalents:
- str
- int
- float

#### object

Objects are represented by python `class`es, and separately, where they're used, by the class name.

#TODO naming of classes

#### Any type (keyword omitted)

When omitted, it means the value may be of any type, supported by JSON Schema.

When no other keywords are present, it means any atomic, container (object or array) or `null` values are valid, and it's represented as `typing.Any`.



When serializing, the serialization method can be assumed from the data type.

- Atomic values can be serialized in the obvious way
- objects can be serialized if they are of instances of a type with a known serialization method (pydantic, dataclasses, etc),
- arrays can be serialized as long their items can be serialized.

## Pure validation fields

- title
- multipleOf
- maximum
- exclusiveMaximum
- minimum
- exclusiveMinimum
- maxLength
- minLength
- pattern
- maxItems
- minItems
- uniqueItems
- maxProperties
- minProperties

## enum



required

- enum
