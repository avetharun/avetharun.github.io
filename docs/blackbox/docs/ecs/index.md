# Blackbox ECS

This mod of Godot's engine supports a slightly more advanced ECS system, such that entities/nodes can be fetched via global IDs

Any object of Node type with a property "var identifier : StringName" is considered a valid Entity

Entities made this way must be manually added to the entitylist however, but objects extending Entity will have this managed automatically.


## Inputs and Outputs
The IO system for the ECS implementation is automated upon loading the map, so long as the node attached has an `outputs` and `targetname` key attached to the properties

The MapLoader will scan all entities upon loading to check for Input### and Output### functions to add. Any function starting with `Input` or `Output` will be used accordingly

Inputs can have parameters for functions, which will be automatically inserted when required
### Creating Inputs
Inputs can be created using the following syntax:
`func InputName(...):...` where `Name` is the actual input you would invoke through Outputs

### Creating Outputs
Inputs can be created using the following syntax:
`func OutputName():...` where `Name` is the Output's name

### Using Outputs
The provided FGD (use the `export_fgd` console command to generate) contains the `outputs` key by default

Outputs are defined in the property pane as:
`<output_name>,<target>,<input>,<[params, ...]>,<delay>,<fire_multiple_times>`

or in the case of single parameters:
`<output_name>,<target>,<input>,<param>,<delay>,<fire_multiple_times>`

* Note: the `fire_multiple_times` option is formatted as `0=once` and `1=indefinite`
#### Output Targets
- `!self` | `!caller` : Invokes the input on the object outputting from
- `!activator` : Unused, however branches and buttons will eventually use this to determine what triggered it
- `<entity_id>` : Invokes the input on all entities with the `targetname: <entity_id>`
