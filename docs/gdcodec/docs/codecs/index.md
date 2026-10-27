# Codec Class
<b>The Codec class is the wrapper around encoding and decoding from and to buffers.</b><br>
- [List of Codecs](#list-of-codecs)
- [Codec Methods](#codec-methods)

## List of Codecs

| Codec | Description | Binary Representation | Notes |
| :--- | :--- | :--- | :--- |
| **`Codec.ENUM`** | Represents a [Godot enum](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html#enums) as a 64-bit unsigned integer | `u64` |
| **`Codec.LONG`** | Represents a 64-bit unsigned integer | `u64` |
| **`Codec.INT`** | Represents a 32-bit unsigned integer | `u32` |
| **`Codec.SHORT`** | Represents a 16-bit unsigned integer | `u16` |
| **`Codec.BYTE`** | Represents an 8-bit unsigned integer | `u8` |
| **`Codec.SIGNED_LONG`** | Represents a 64-bit signed integer | `s64` |
| **`Codec.SIGNED_INT`** | Represents a 32-bit signed integer | `s32` |
| **`Codec.SIGNED_SHORT`** | Represents a 16-bit signed integer | `s16` |
| **`Codec.SIGNED_BYTE`** | Represents an 8-bit signed integer | `s8` |
| **`Codec.DOUBLE`** | Represents a 64-bit floating-point value | `f64` |
| **`Codec.FLOAT`** | Represents a 32-bit floating-point value | `f32` |
| **`Codec.BOOL`** | Represents a Boolean (8-bit unsigned; not a bitmask) | `u8` (`0` or `1`) |
| **`Codec.STRING`** | Represents a length-prefixed [String](https://docs.godotengine.org/en/stable/classes/class_string.html) | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.STRING_NAME`** | Represents a [StringName](https://docs.godotengine.org/en/stable/classes/class_stringname.html), encoded as a string | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.LONG_ARRAY`** | Represents an array of uint64 values | `len_t`, followed by `u64` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.INT_ARRAY`** | Represents an array of uint32 values | `len_t`, followed by `u32` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.SHORT_ARRAY`** | Represents an array of uint16 values | `len_t`, followed by `u16` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.SIGNED_LONG_ARRAY`** | Represents an array of int64 values | `len_t`, followed by `s64` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.SIGNED_INT_ARRAY`** | Represents an array of int32 values | `len_t`, followed by `s32` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.SIGNED_SHORT_ARRAY`** | Represents an array of int16 values | `len_t`, followed by `s16` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.SIGNED_BYTE_ARRAY`** | Represents an array of int8 values | `len_t`, followed by `s8` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.DOUBLE_ARRAY`** | Represents an array of 64-bit float values | `len_t`, followed by `f64` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.FLOAT_ARRAY`** | Represents an array of 32-bit float values | `len_t`, followed by `f32` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.BOOL_ARRAY`** | Represents an array of Booleans | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.STRING_ARRAY`** | Represents an array of length-prefixed strings | `len_t`, followed by length-prefixed string elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.BYTE_ARRAY`** | Represents an array of uint8 values | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-length-specifier-len_t)</sup> |
| **`Codec.COLOR`** | Represents an [RGBA8](https://docs.godotengine.org/en/stable/classes/class_color.html) color | `u32` |
| **`Codec.COLOR_HDR`** | Represents an [RGBA](https://docs.godotengine.org/en/stable/classes/class_color.html) color as 32-bit floats | `[f32, f32, f32, f32]` |
| **`Codec.VECTOR2`** | Represents a [Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) | `[f32, f32]` |
| **`Codec.VECTOR2I`** | Represents a [Vector2i](https://docs.godotengine.org/en/stable/classes/class_vector2i.html) | `[s32, s32]` |
| **`Codec.VECTOR3`** | Represents a [Vector3](https://docs.godotengine.org/en/stable/classes/class_vector3.html) | `[f32, f32, f32]` |
| **`Codec.VECTOR3I`** | Represents a [Vector3i](https://docs.godotengine.org/en/stable/classes/class_vector3i.html) | `[s32, s32, s32]` |
| **`Codec.QUATERNION`** | Represents a [Quaternion](https://docs.godotengine.org/en/stable/classes/class_quaternion.html) | `[f32, f32, f32, f32]` |
| **`Codec.VARIANT`** | Represents a native Godot [Variant](https://docs.godotengine.org/en/stable/classes/class_variant.html) | [Godot Variant encoding](https://docs.godotengine.org/en/4.7/classes/class_@globalscope.html#class-globalscope-method-var-to-bytes) |
| **`Codec.VARIANT_WITH_OBJECTS`** | Represents a native Godot [Variant](https://docs.godotengine.org/en/stable/classes/class_variant.html) | [Godot Variant encoding (With Objects)](https://docs.godotengine.org/en/4.7/classes/class_@globalscope.html#class-globalscope-method-var-to-bytes-with-objects) |
| **`Codec.BITMASK_8`** | Represents an 8-bit bitmask | `u8` | [Bitmask Usage](#bitmask)
| | |

## Codec Methods

| Method Name | Description | Returns |
| :--- | :--- | :--- |
| <sup>**`static span(codec:Codec, length:int, default:Variant)`**</sup> | Constant-length codec of a given type.<br><i><sup> Note: The default value must be encodable by the codec supplied.</sup></i> | `Codec` |
| <sup>**`static byte_span(length:int)`**</sup> | Constant-length codec of bytes.<br><i><sup>Note: Does not include a length prefix</sup></i> | `Codec` |
| <sup>**`static nullable(codec:Codec)`**</sup> | Creates a codec for a "nullable" value. <br><i><sup>Note<sup>1</sup>: Wrapper for [`Codec.optional`](#codec-method-optional)<br>Note<sup>2</sup>: The value represented by this codec must be assignable by null</sup></i> | `Codec` |
| <a id="codec-method-optional"></a><sup>**`static optional(codec:Codec, default:Variant)`**</sup> | Creates a codec for an "optional" value.<br><i><sup>Note: the default value must be encodable by the codec supplied.</sup></i> | `Codec` |
| <sup>**`static constant(value:Variant)`**</sup> | Creates a non-encodable (zero-width) codec that<br>always decodes to the value supplied. | `Codec` |
| <sup>**`static padding(length:int)`**</sup> | Creates a codec that will insert `length` bytes<br>into the resulting `Codec`<br><i><sup>Note: Decoding returns null, and will ignore any encoded value.</sup></i>| `Codec` |
| <sup>**`static either(tag_codec:Codec, codecs:Array)`**</sup> | Creates a "tagged" union `Codec`. <br>Values use the form `[tag, value]` and the `tag`<br> selects the codec to use. | `Codec` |
| <sup>**`static join(codecs:Array)`**</sup> | Creates a fixed-sequence codec.<br>Values are encoded and decoded as an array<br>matching the order of codecs. | `Codec` |
| <sup>**`static limited_string(max_length:int)`**</sup> | Creates a codec that represents a length-prefixed<br>string with a max length<br><i><sup>Note: if the string exceeds the length, an error will occur.</i></sup> | `Codec` |
| <sup>**`static arrayof(codec:Codec)`**</sup> | Creates a length-prefixed array of Codecs. | `Codec`|
| <sup>**`static limited_array(codec:Codec, max_length:int)`**</sup> | Creates a length-prefixed array of Codecs that<br> reject more than `max_length` elements. | `Codec` |
| <sup>**`static mapof(key_codec:Codec, value_codec:Codec)`**</sup> | Creates a length-prefixed map `Codec` using separate<br> Codecs for keys and values.<br><i><sup>Note: Effectively just a Dictionary wrapper for codecs </sup></i> | `Codec` |
| <sup>**`static record(fields:Dictionary)`**</sup> | Creates a record codec that encodes fields in <br>the dictionary's declaration order.<br><i><sup>Note: This can be used to serialize from and to class objects</i></sup> | `Codec` |
| <sup>**`static enum_string(enum_type:Variant, default:int = -1)`**</sup> | Creates a `Codec` that represents an Enum as a String<br><i><sup>Note<sup>1</sup>: By default, forced case is disabled<br>&emsp;(ie `some_key` and `SOME_KEY` will map to the same value)<br>&emsp;To change this, use `Codec.enum_string(MyEnum, MyEnum.Default, forced_case:bool)`<br>Note<sup>2</sup>: To avoid conflicts, modify `default_key` if you have an<br>&emsp;element named `__INVALID__`:<br>&emsp;`Codec.enum_string(MyEnum, MyEnum.Default, forced_case, default_key:String)`
| <sup>**`static get_enum_string_value(enum_type:Variant, key:String, fallback:int = -1)`**</sup> | Returns the 

## Notes and Extra Info
### Codec Length Specifier (len_t)

`len_t` can be modified with `with_length_encoding` using the following flags:

| Length Encoding | Description | Binary Representation |
| :--- | :--- | :--- |
| **`Codec.LengthEncoding.INT8`** | Represents a length as a `u8` | `u8` |
| **`Codec.LengthEncoding.INT16`** | Represents a length as a `u16` | `u16` |
| **`Codec.LengthEncoding.INT32`** | Represents a length as a `u32` | `u32` |
| <a id="codec-lengthencoding-int64"></a>**`Codec.LengthEncoding.INT64`** | Represents a length as a `u64` | `u64` |
| | |

Note: **[`Codec.LengthEncoding.INT64`](#codec-lengthencoding-int64)** is the default length encoding.

### Bitmask
The `Codec.BITMASK_8` type is represented as a `u8` in binary, but will be assigned as an array of between 0 and 8 booleans.

Any missing elements in the array will be filled with the default element: `false` or `0`

Any elements past 8 (ie, index 9) will be ignored. To bypass this, you can use either of the following snippets:

```gdscript
## num_bytes should be the number of bits / 8,
## or however many bitmasks need to be joined together.
## These must be joined together in nested arrays.
CodecOpBytes.encode(
    Codec.span(Codec.BITMASK_8, num_bytes),
    [[true, false, ...], [...], ...]
)
## -> \x??\x??...
```
Example of usage:
```gdscript
CodecOpBytes.encode(Codec.BITMASK_8, [true, true, false, true, false])
## -> \xD0 or 0b11010000
```
