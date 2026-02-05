# Blackbox ECS

This mod of Godot's engine supports a slightly more advanced ECS system, such that entities/nodes can be fetched via global IDs

Any object of Node type with a property "var identifier : StringName" is considered a valid Entity

Entities made this way must be manually added to the entitylist however, but objects extending Entity will have this managed automatically.
