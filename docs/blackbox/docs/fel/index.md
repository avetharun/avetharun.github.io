# BlackBox Filter Expression Language

The Filter Expression Language (FEL) is used in BlackBox for triggers, buttons, pickables, etc

## How it works ##
The expression language has only four* tokens (excluding the filters themselves):
- `and`, `or`, `(`, `)`
- \* Note: `true` and `false` are considered filters internally, they just always return a constant value


Filters themselves are expanded into the following syntax:

`filter_type` or `filter_type(...)` where anything in parentheses directly proceeding a filter acts like arguments to the filter (ie, `game_value(a=="some string")` )

A fully completed expression could be as follows: `is_player or (is_not_player and node_property(identifier=="fake_player"))`


## Default Filters ##
- `false` : Always false
- `true` : Always true
- `is_player` : Whether the passed node inherits from `EntityPlayer`
- `is_not_player` : Inverse of `is_player`
- `is_entity` : Whether the passed node either inherits from `Entity` or has a `identifier : StringName` property
- `is_not_entity` : Inverse of `is_entity` (does not inherit from `Entity` or have `identifier : StringName` as a property)
- `entity_name(name)` : Checks whether the passed node is an entity and whether it's `identifier`/FGD:`targetname` field is `name`
- `game_value(sub_expression)` : Checks whether a game value exists or passes an operation. (See: [GameValue expressions](#gamevalue-expressions))
- `node_property(sub_expression)` : Checks whether the passed node has a given property. Same syntax as `game_value(...)`  (See: [GameValue expressions](#gamevalue-expressions))
### Internal Filters ###
- `invalid_expression` : Returned when the FEL parser either fails, the expression is invalid, or some error occurs (See console if this is returned!)

### Parameters ###
Filters, upon parsing an expression, are duplicated and given their respective parameters, if needed

## Predicates ##
Predicates are wrapper classes specifically for the expression system. While they can be used externally, it's not reccomended.

The basic structure of the Predicate class is as follows:
```
class Predicate:
  func set_params(Array[String])
  func get_params() -> Array[String]
  virtual func passes(Node)
```
### Existing predicate classes ###
`FuncPredicate` : Constructor takes a `Callable` and automatically calls it when needed

`InvertedPredicate` : Wrapper for an existing predicate, constructor takes a `Predicate` and inverts the result of `passes(...)`

`AlwaysTruePredicate` : Always returns true

`AlwaysFalsePredicate` : Always returns false


## Custom Filters ##
Custom filters can be created via passing a `TriggerFilter` into `TriggerFilter.register_filter(name, filter)`


Ensure the method registering this is called properly!
```
static var _MyCustomFilter := TriggerFilter.new("custom_filter", FuncPredicate.new(func(node, params):
	if (params.size() == 0:
		return false
	else:
		...
)
static func _static_init() -> void:
  TriggerFilter.register_filter("custom_filter", _MyCustomFilter)
```

## GameValue Expressions ##
The `game_value(key ...)` and `node_property(key ...)` filters have their own syntax with comparison and prescence operators.

When using this filter, the type of the param and the type of the value must match, otherwise it will return false (as Godot doesn't have `try: catch:` statements)



### Comparison Operators ###
- `==`
- `!=`
- `<`
- `<`
- `<=`
- `>=`
- `is_null`
- `not_null`
### Prescence Operators ###
- `is_absent`
- `is_present`

The `is_absent` and `is_present` operators will check whether the `key` exists or not.

If an operator isn't present, it's implicitly assumed `is_present` is used

### Type Casting ###

Since types must be exact for the expression, various casting methods are included. They can be used as follows: `game_value(some_key==bool("false"))`

However, in most cases this isn't needed as the following are implicitly casted:

`true` and `false` are implicitly casted to their boolean counterparts

Integers (no decimal point) will implicitly be casted to an integer

Floats (ie, 1.0) will be implicitly casted to a floating point number

### Syntax for the GameValue Expressions: ###

`game_value(some_key)`, `game_value(some_key==some_string_value)`, `game_value(some_key not_null)`
