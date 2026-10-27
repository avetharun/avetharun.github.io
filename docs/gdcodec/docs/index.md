# GDCodec - Codec-based binary serialization for Godot
[Godot Asset Library Link](https://godotengine.org/asset-library/asset/5465)

GDCodec is a library that bundles binary serialization, json serialization, and packet-based networking into a single easy to use library.

## How it works:

Codec(s) are a wrapper around Godot's [StreamPeerBuffer](https://docs.godotengine.org/en/stable/classes/class_streampeerbuffer.html) class

## Terminology

### Bicoders

Bicoders (Encoder & Decoder mapper(s)) are a way to "convert" types from one instance to another, whether it's an array of types, or an actual instance generated using `Codec.X.bicast(...)`