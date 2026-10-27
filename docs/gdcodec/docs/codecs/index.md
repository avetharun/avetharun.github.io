<a id="page_top"></a>
# Codec Class
<b>The Codec class is the wrapper around encoding and decoding to and from buffers.</b><br>

<a id="codec-links"></a>
- [List of Codecs](#list-of-codecs)
- [Codec Methods](#codec-methods)
- [Codec.LengthEncoding](#codec-length-specifier-len_t)
- [Examples](#codec-examples)
- [Encoding an Object](#codec-examples-encoding-an-object)
- [Creating a custom Codec by hand](#codec-examples-creating-a-custom-codec-by-hand)
- [Class Codec.CodecBiRef](#codec-sub-class-codecbiref)
- [Enum Codec.LengthEncoding](#codec-sub-enum-lengthencoding)
- [Enum Codec.FieldType](#codec-sub-enum-fieldtype)

<a id="codec-types"></a>
## List of Codecs

| <b>Codec</b> | <b>Description</b> | <b>Binary Representation</b> | <b>Notes</b> |
| :--- | :--- | :--- | :--- |
| <a id="codec-type-enum"></a>**`Codec.ENUM`** | Represents a [Godot enum](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html#enums) as a 64-bit unsigned integer | `u64` |
| <a id="codec-type-long"></a>**`Codec.LONG`** | Represents a 64-bit unsigned integer | `u64` |
| <a id="codec-type-int"></a>**`Codec.INT`** | Represents a 32-bit unsigned integer | `u32` |
| <a id="codec-type-short"></a>**`Codec.SHORT`** | Represents a 16-bit unsigned integer | `u16` |
| <a id="codec-type-byte"></a>**`Codec.BYTE`** | Represents an 8-bit unsigned integer | `u8` |
| <a id="codec-type-signed_long"></a>**`Codec.SIGNED_LONG`** | Represents a 64-bit signed integer | `s64` |
| <a id="codec-type-signed_int"></a>**`Codec.SIGNED_INT`** | Represents a 32-bit signed integer | `s32` |
| <a id="codec-type-signed_short"></a>**`Codec.SIGNED_SHORT`** | Represents a 16-bit signed integer | `s16` |
| <a id="codec-type-signed_byte"></a>**`Codec.SIGNED_BYTE`** | Represents an 8-bit signed integer | `s8` |
| <a id="codec-type-double"></a>**`Codec.DOUBLE`** | Represents a 64-bit floating-point value | `f64` |
| <a id="codec-type-float"></a>**`Codec.FLOAT`** | Represents a 32-bit floating-point value | `f32` |
| <a id="codec-type-bool"></a>**`Codec.BOOL`** | Represents a Boolean (8-bit unsigned; not a bitmask) | `u8` (`0` or `1`) |
| <a id="codec-type-string"></a>**`Codec.STRING`** | Represents a length-prefixed [String](https://docs.godotengine.org/en/stable/classes/class_string.html) | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-string_name"></a>**`Codec.STRING_NAME`** | Represents a [StringName](https://docs.godotengine.org/en/stable/classes/class_stringname.html), encoded as a string | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-long_array"></a>**`Codec.LONG_ARRAY`** | Represents an array of uint64 values | `len_t`, followed by `u64` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-int_array"></a>**`Codec.INT_ARRAY`** | Represents an array of uint32 values | `len_t`, followed by `u32` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-short_array"></a>**`Codec.SHORT_ARRAY`** | Represents an array of uint16 values | `len_t`, followed by `u16` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-signed_long_array"></a>**`Codec.SIGNED_LONG_ARRAY`** | Represents an array of int64 values | `len_t`, followed by `s64` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-signed_int_array"></a>**`Codec.SIGNED_INT_ARRAY`** | Represents an array of int32 values | `len_t`, followed by `s32` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-signed_short_array"></a>**`Codec.SIGNED_SHORT_ARRAY`** | Represents an array of int16 values | `len_t`, followed by `s16` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-signed_byte_array"></a>**`Codec.SIGNED_BYTE_ARRAY`** | Represents an array of int8 values | `len_t`, followed by `s8` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-double_array"></a>**`Codec.DOUBLE_ARRAY`** | Represents an array of 64-bit float values | `len_t`, followed by `f64` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-float_array"></a>**`Codec.FLOAT_ARRAY`** | Represents an array of 32-bit float values | `len_t`, followed by `f32` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-bool_array"></a>**`Codec.BOOL_ARRAY`** | Represents an array of Booleans | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-string_array"></a>**`Codec.STRING_ARRAY`** | Represents an array of length-prefixed strings | `len_t`, followed by length-prefixed string elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-byte_array"></a>**`Codec.BYTE_ARRAY`** | Represents an array of uint8 values | `len_t`, followed by `u8` elements | <sup>Notes: [len_t](#codec-info-len_t)</sup> |
| <a id="codec-type-color"></a>**`Codec.COLOR`** | Represents an [RGBA8](https://docs.godotengine.org/en/stable/classes/class_color.html) color | `u32` |
| <a id="codec-type-color_hdr"></a>**`Codec.COLOR_HDR`** | Represents an [RGBA](https://docs.godotengine.org/en/stable/classes/class_color.html) color as 32-bit floats | `[f32, f32, f32, f32]` |
| <a id="codec-type-vector2"></a>**`Codec.VECTOR2`** | Represents a [Vector2](https://docs.godotengine.org/en/stable/classes/class_vector2.html) | `[f32, f32]` |
| <a id="codec-type-vector2i"></a>**`Codec.VECTOR2I`** | Represents a [Vector2i](https://docs.godotengine.org/en/stable/classes/class_vector2i.html) | `[s32, s32]` |
| <a id="codec-type-vector3"></a>**`Codec.VECTOR3`** | Represents a [Vector3](https://docs.godotengine.org/en/stable/classes/class_vector3.html) | `[f32, f32, f32]` |
| <a id="codec-type-vector3i"></a>**`Codec.VECTOR3I`** | Represents a [Vector3i](https://docs.godotengine.org/en/stable/classes/class_vector3i.html) | `[s32, s32, s32]` |
| <a id="codec-type-quaternion"></a>**`Codec.QUATERNION`** | Represents a [Quaternion](https://docs.godotengine.org/en/stable/classes/class_quaternion.html) | `[f32, f32, f32, f32]` |
| <a id="codec-type-variant"></a>**`Codec.VARIANT`** | Represents a native Godot [Variant](https://docs.godotengine.org/en/stable/classes/class_variant.html) | [Godot Variant encoding](https://docs.godotengine.org/en/4.7/classes/class_@globalscope.html#class-globalscope-method-var-to-bytes) |
| <a id="codec-type-variant_with_objects"></a>**`Codec.VARIANT_WITH_OBJECTS`** | Represents a native Godot [Variant](https://docs.godotengine.org/en/stable/classes/class_variant.html) | [Godot Variant encoding (With Objects)](https://docs.godotengine.org/en/4.7/classes/class_@globalscope.html#class-globalscope-method-var-to-bytes-with-objects) |
| <a id="codec-type-bitmask_8"></a>**`Codec.BITMASK_8`** | Represents an 8-bit bitmask | `u8` | [Bitmask Usage](#bitmask)
| | |
<a id="codec-methods"></a>
## Codec Methods

| <b>Method Name</b> | <b>Description</b> | <b>Returns</b> |
| :--- | :--- | :--- |
| <a id="codec-method-encode"></a><sup><span style="white-space: nowrap;">**`encode(value:Variant, buffer:StreamPeerBuffer)`**</span></sup> | Encodes this `Codec` into the supplied buffer (Binary format) | `StreamPeerBuffer` |
| <a id="codec-method-decode"></a><sup><span style="white-space: nowrap;">**`decode(buffer:StreamPeerBuffer)`**</span></sup> | Decodes this `Codec` from the supplied buffer (Binary format) | `Variant` |
| <a id="codec-method-set_name"></a><sup><span style="white-space: nowrap;">**`set_name(name:String)`**</span></sup> | Sets the name of this `Codec`<br><i><sup>Note: Used when calling `instance._to_string()` or `str(instance)` | `Codec` |
| <a id="codec-method-get_name"></a><sup><span style="white-space: nowrap;">**`get_name()`** -> `default: "Unnamed"`</span></sup> | Returns the name of this `Codec` | `Codec` |
| <a id="codec-method-rmap"></a><sup><span style="white-space: nowrap;">**`rmap(decoder:Callable)`**</span></sup> | Converts the result(Decoded) into another type<br><sup><i>[View Example](#codec-example-rmap) | `Codec` |
| <a id="codec-method-smap"></a><sup><span style="white-space: nowrap;">**`smap(encoder:Callable)`**</span></sup> | Converts the source(Encoded) into another type<br><sup><i>[View Example](#codec-example-smap) | `Codec` |
| <a id="codec-method-xmap"></a><sup><span style="white-space: nowrap;">**`xmap(decoder:Callable, encoder:Callable)`**</span></sup> | Converts the result(Decoded) into another type<br>and converts it back before encoding<br><sup><i>[View Example](#codec-example-xmap) | `Codec` |
| <a id="codec-method-bicast"></a><sup><span style="white-space: nowrap;">**`bicast(bi_pairs:Array, c_type:Script\|Type)`**</span></sup> | Creates a `Codec` that maps field codecs<br>to and from instances of `type`<br><sup><i>[View Example](#codec-example-bicast)<br>- c_type must either be a `Script` or a non-script `Type` (ie. GDExtension class)<br>&emsp;- The custom constructor must be used for non-script types<br>- The `bi_pairs` param must use the following format:<br>&emsp;&emsp;- `[CODEC_TYPE, &"field_name"]`<br>&emsp;&emsp;- `[CODEC_TYPE, Codec.field(&"field_name", Codec.FieldType.Field)]`<br>&emsp;&emsp;- `[CODEC_TYPE, Codec.field(&"get_field", Codec.FieldType.Method)]` <br>&emsp;<a id="codec-method-bicast-custom-constructor"></a>- To specify a custom constructor (ie, for `Resources`)<br>&emsp;use the following function:<br>&emsp;<sup>`codec.bicast([...], MyType, MyType.my_custom_constructor)`<br>&emsp;&emsp;- See:[Using bicast on Resource-based Classes](#using-bicast-on-resource-based-classes-you-control)</sup> | `Codec` |
| <a id="codec-method-with_length_encoding"></a><sup><span style="white-space: nowrap;">**`with_length_encoding(encoding:Codec.LengthEncoding)`**</span></sup> | Sets the encoding format for length-prefixed `Codecs`<br><i><sup>See: [Codec Length Specifiers](#codec-info-len_t) | `Codec` |
| <a id="codec-method-with_optional"></a><sup><span style="white-space: nowrap;">**`with_optional(default:Variant)`**</span></sup> | Sets this codec to be Optional.<br><i><sup>Note: Identical to calling [`Codec.optional(this_codec, default)`](#codec-method-optional)<br>See: [`Codec.optional`](#codec-method-optional) | `Codec` |
| <a id="codec-method-with_nullable"></a><sup><span style="white-space: nowrap;">**`with_optional(default:Variant)`**</span></sup> | Sets this codec to be Nullable.<br><i><sup>Note: Identical to calling [`Codec.nullable(this_codec)`](#codec-method-nullable)<br>See: [`Codec.nullable`](#codec-method-nullable) and [`Codec.optional`](#codec-method-optional) | `Codec` |
| <a id="codec-method-span"></a><sup><span style="white-space: nowrap;">**`static span(codec:Codec, length:int, default:Variant)`**</span></sup> | Constant-length codec of a given type.<br><i><sup> Note: The default value must be encodable by the codec supplied.</sup></i> | `Codec` |
| <a id="codec-method-byte_span"></a><sup><span style="white-space: nowrap;">**`static byte_span(length:int)`**</span></sup> | Constant-length codec of bytes.<br><i><sup>Note: Does not include a length prefix</sup></i> | `Codec` |
| <a id="codec-method-nullable"></a><sup><span style="white-space: nowrap;">**`static nullable(codec:Codec)`**</span></sup> | Creates a codec for a "nullable" value. <br><i><sup>Note<sup>1</sup>: Wrapper for [`Codec.optional`](#codec-method-optional)<br>Note<sup>2</sup>: The value represented by this codec must be assignable by null</sup></i> | `Codec` |
| <a id="codec-method-optional"></a><sup><span style="white-space: nowrap;">**`static optional(codec:Codec, default:Variant)`**</span></sup> | Creates a codec for an "optional" value.<br><i><sup>Note<sup>1</sup>: the default value must be encodable by the codec supplied.<br>Note<sup>2</sup>: When a `Codec` is optional, it will be prefixed by a `u8` value<br> to specify whether it is present or not.</sup></i> | `Codec` |
| <a id="codec-method-constant"></a><sup><span style="white-space: nowrap;">**`static constant(value:Variant)`**</span></sup> | Creates a non-encodable (zero-width) codec that<br>always decodes to the value supplied. | `Codec` |
| <a id="codec-method-padding"></a><sup><span style="white-space: nowrap;">**`static padding(length:int)`**</span></sup> | Creates a codec that will insert `length` bytes<br>into the resulting `Codec`<br><i><sup>Note: Decoding returns null, and will ignore any encoded value.</sup></i>| `Codec` |
| <a id="codec-method-either"></a><sup><span style="white-space: nowrap;">**`static either(tag_codec:Codec, codecs:Array)`**</span></sup> | Creates a "tagged" union `Codec`. <br>Values use the form `[tag, value]` and the `tag`<br> selects the codec to use. | `Codec` |
| <a id="codec-method-join"></a><sup><span style="white-space: nowrap;">**`static join(codecs:Array)`**</span></sup> | Creates a fixed-sequence codec.<br>Values are encoded and decoded as an array<br>matching the order of codecs. | `Codec` |
| <a id="codec-method-limited_string"></a><sup><span style="white-space: nowrap;">**`static limited_string(max_length:int)`**</span></sup> | Creates a codec that represents a length-prefixed<br>string with a max length<br><i><sup>Note: if the string exceeds the length, an error will occur.</i></sup> | `Codec` |
| <a id="codec-method-arrayof"></a><sup><span style="white-space: nowrap;">**`static arrayof(codec:Codec)`**</span></sup> | Creates a length-prefixed array of Codecs. | `Codec`|
| <a id="codec-method-limited_array"></a><sup><span style="white-space: nowrap;">**`static limited_array(codec:Codec, max_length:int)`**</span></sup> | Creates a length-prefixed array of Codecs that<br> reject more than `max_length` elements. | `Codec` |
| <a id="codec-method-mapof"></a><sup><span style="white-space: nowrap;">**`static mapof(key_codec:Codec, value_codec:Codec)`**</span></sup> | Creates a length-prefixed map `Codec` using separate<br> Codecs for keys and values.<br><i><sup>Note: Effectively just a Dictionary wrapper for codecs </sup></i> | `Codec` |
| <a id="codec-method-record"></a><sup><span style="white-space: nowrap;">**`static record(fields:Dictionary)`**</span></sup> | Creates a record codec that encodes fields in <br>the dictionary's declaration order.<br><i><sup>Note: This can be used to serialize from and to class objects</i></sup> | `Codec` |
| <a id="codec-method-enum_string"></a><sup><span style="white-space: nowrap;">**`static enum_string(enum_type:Variant, default:int = -1)`**</span></sup> | Creates a `Codec` that represents an Enum as a String<br><i><sup>Note<sup>1</sup>: By default, forced case is disabled<br>&emsp;(ie `some_key` and `SOME_KEY` will map to the same value)<br>&emsp;To change this, use `Codec.enum_string(MyEnum, MyEnum.Default, forced_case:bool)`<br>Note<sup>2</sup>: To avoid conflicts, modify `default_key` if you have an<br>&emsp;element named `__INVALID__`:<br>&emsp;`Codec.enum_string(MyEnum, MyEnum.Default, forced_case, default_key:String)`
| <a id="codec-method-get_enum_string_value"></a><sup><span style="white-space: nowrap;">**`static get_enum_string_value(enum_type:Variant, key:String, fallback:int = -1)`**</span></sup> | Returns the actual `int` value represented by the `key` | `Codec` |
| <a id="codec-method-_to_string"></a><sup><span style="white-space: nowrap;">**`_to_string()`**</span></sup> | Returns the following: `"<Codec:"+get_name()+">"` | `String` |
| <a id="codec-method-field"></a><sup><span style="white-space: nowrap;">**`field(field:Variant, type:Codec.FieldType)`**</span></sup> | Returns a representation of a Field used in [`Codec.bicast`](#codec-method-bicast)<br><sup><i>Note<sup>1</sup>: `field` must either be a `Callable` (getter), a `String`, or a `StringName`<br>Note<sup>2</sup>: `type` cannot be `Method` when `field` is a `String` or a `StringName` | `Codec.CodecBiRef` |


<a id="codec-info-header"></a>
## Notes and Extra Info

<a id="codec-info-bitmask_8"></a>
### Bitmask
The `Codec.BITMASK_8` type is represented as a `u8` in binary, but will be assigned as an array of between 0 and 8 booleans.

Any missing elements in the array will be filled with the default element: `false` or `0`

Any elements past 8 (ie, index 9) will be ignored. To bypass this, you can use either of the following snippets:
<a id="codec-example-bitmask_8-nested"></a>

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
<a id="codec-example-bitmask_8"></a>
Example of usage:

```gdscript
CodecOpBytes.encode(Codec.BITMASK_8, [true, true, false, true, false])
## -> \xD0 or 0b11010000
```

<a id="codec-examples"></a>
## Examples

<a id="codec-examples-encoding-an-object"></a>
### Encoding an Object
#### Encoding an object can be done in two ways:
- Using [`xmap`](#codec-method-xmap), [`smap`](#codec-method-smap), or [`rmap`](#codec-method-rmap) to create a custom encoder and decoder mapping
- Using [`bicast`](#codec-method-bicast) to map directly to an object's `_init` function (or a custom constructor <sup><sub><i>See: [bicast's notes](#codec-method-bicast-custom-constructor)</i></sub></sup>)

Each method has its own upsides and downsides.
<br>For integral types, or types you cannot modify manually, you should use [`xmap`](#codec-method-xmap), [`smap`](#codec-method-smap), or [`rmap`](#codec-method-rmap)
<br>For types you create yourself, it's recommended to use [`bicast`](#codec-method-bicast)
<a id="codec-examples-encoding-an-object-using-xmap"></a>
#### Using [`xmap`](#codec-method-xmap), [`smap`](#codec-method-smap), or [`rmap`](#codec-method-rmap) for Encoding and Decoding

```gdscript
static var PLANE_CODEC : Codec = Codec.join(Codec.VECTOR3, Codec.FLOAT).xmap(
    func(plane:Plane) -> Array: ## Array is used for Codec.join
        return [plane.normal, plane.d],
    func(array:Array) -> Plane:
        return Plane(array[0], array[1])
)
```
<a id="codec-examples-encoding-an-object-using-bicast"></a>
#### Using [`bicast`](#codec-method-bicast) on Classes you control

To use [`bicast`](#codec-method-bicast) on Resource classes, use the [bicast Resource example](#codec-examples-encoding-an-object-using-bicast-custom-resources)

```gdscript
class MyClass extends RefCounted: # Use RefCounted or Node for this example
    var my_field_x: String
    func _init(x:String) -> void:
        self.my_field_x = x
    ## Will automatically convert to MyClass
    static var CODEC : Codec = Codec.bicast([
        Codec.STRING, Codec.field(&"my_field_x"),
    ], MyClass)
```
<a id="codec-examples-encoding-an-object-using-bicast-custom-resources"></a>
#### Using [`bicast`](#codec-method-bicast) on Resource-based Classes you control

```gdscript
class MyClass extends Resource:
    @export var my_field_x: String
    static func create_new(x:String) -> MyClass:
        var result : MyClass = MyClass.new()
        result.my_field_x = x
        return result
    ## Will automatically convert to MyClass
    static var CODEC : Codec = Codec.bicast([
        Codec.STRING, Codec.field(&"my_field_x"),
    ], MyClass, MyClass.create_new)
```
<a id="codec-examples-encoding-an-object-using-bicast-on-external-classes"></a>
#### Using [`bicast`](#codec-method-bicast) on Classes you control work very similarly to how [Resource-based bicast](#codec-examples-encoding-an-object-using-bicast-custom-resources) works.

```gdscript
static func create_new_image_packed(
	width:int,
	height:int,
	use_mipmaps:bool,
	format:int,
	data:PackedByteArray
) -> Image:
	return Image.create_from_data(width, height, use_mipmaps, format as Image.Format, data)

static var IMAGE_CODEC : Codec = Codec.bicast([
    Codec.INT, Codec.field("get_width", Codec.FieldType.Method),
    Codec.INT, Codec.field("get_height", Codec.FieldType.Method),
    Codec.BOOL, Codec.field("has_mipmaps", Codec.FieldType.Method),
    Codec.INT, Codec.field("get_format", Codec.FieldType.Method),
    Codec.PACKED_BYTE_ARRAY, Codec.field("get_data", Codec.FieldType.Method),
], Image, create_new_image_packed)
```

<a id="codec-sub-classes-and-enums"></a>

# Sub-classes and Enums
<!-- This is the most hideous thing I've ever seen in markdown, but it gets what I want to display across, unlike Markdown itself. -->
<table>
  <thead>
    <tr>
      <th style="text-align: left;"><h1>Enum</h1></th>
      <th style="text-align: left;"></th>
      <th style="text-align: left;"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left;"><h2><a id="codec-sub-enum-lengthencoding"></a>Codec.LengthEncoding<hr></h2></td>
      <td style="text-align: left;"><h3>Field<hr></h3></td>
      <td style="text-align: left;"><h3>Description<hr></h3></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"><a id="codec-lengthencoding-int8"></a><b><code class="language-plaintext highlighter-rouge">Codec.LengthEncoding.INT8</code></b></td>
      <td style="text-align: left;">Represents a length as a <code class="language-plaintext highlighter-rouge">u8</code></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"><a id="codec-lengthencoding-int16"></a><b><code class="language-plaintext highlighter-rouge">Codec.LengthEncoding.INT16</code></b></td>
      <td style="text-align: left;">Represents a length as a <code class="language-plaintext highlighter-rouge">u16</code></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"><a id="codec-lengthencoding-int32"></a><b><code class="language-plaintext highlighter-rouge">Codec.LengthEncoding.INT32</code></b></td>
      <td style="text-align: left;">Represents a length as a <code class="language-plaintext highlighter-rouge">u32</code></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"><a id="codec-lengthencoding-int64"></a><b><code class="language-plaintext highlighter-rouge">Codec.LengthEncoding.INT64</code></b></td>
      <td style="text-align: left;">Represents a length as a <code class="language-plaintext highlighter-rouge">u64</code></td>
    </tr>
    <tr>
      <td style="text-align: left;"><h2><a id="codec-sub-enum-fieldtype"></a>Codec.FieldType<hr></h2></td>
      <td style="text-align: left;"><h3>Field<hr></h3></td>
      <td style="text-align: left;"><h3>Description<hr></h3></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"><a id="codec-fieldtype-method"></a><b><code class="language-plaintext highlighter-rouge">Codec.FieldType.Method</code></b></td>
      <td style="text-align: left;">Will attempt to use the method name using <code class="language-plaintext highlighter-rouge">instance.call(field)</code></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"><a id="codec-fieldtype-field"></a><b><code class="language-plaintext highlighter-rouge">Codec.FieldType.Field</code></b></td>
      <td style="text-align: left;">Will attempt to use the field name using <code class="language-plaintext highlighter-rouge">instance.get(field)</code></td>
    </tr>
    <tr>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"></td>
      <td style="text-align: left;"></td>
    </tr>
  </tbody>
</table>