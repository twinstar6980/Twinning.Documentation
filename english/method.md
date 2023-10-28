# Method

- [Introduction](#Introduction)

- [js](#js)

- [data.hash](#datahash)

- [data.encoding](#dataencoding)

- [data.encryption](#dataencryption)

- [data.compression](#datacompression)

- [data.differentiation](#datadifferentiation)

- [text.json](#textjson)

- [text.xml](#textxml)

- [texture.transformation](#texturetransformation)

- [texture.atlas](#textureatlas)

- [wwise.media](#wwisemedia)

- [wwise.sound_bank](#wwisesound_bank)

- [marmalade.dzip](#marmaladedzip)

- [popcap.zlib](#popcapzlib)

- [popcap.crypt_data](#popcapcrypt_data)

- [popcap.reflection_object_notation](#popcapreflection_object_notation)

- [popcap.texture](#popcaptexture)

- [popcap.u_texture](#popcapu_texture)

- [popcap.sexy_texture](#popcapsexy_texture)

- [popcap.animation](#popcapanimation)

- [popcap.re_animation](#popcapre_animation)

- [popcap.particle](#popcapparticle)

- [popcap.trail](#popcaptrail)

- [popcap.particle_effect](#popcapparticle_effect)

- [popcap.render_effect](#popcaprender_effect)

- [popcap.character_font_widget_2](#popcapcharacter_font_widget_2)

- [popcap.package](#popcappackage)

- [popcap.resource_stream_group](#popcapresource_stream_group)

- [popcap.resource_stream_bundle](#popcapresource_stream_bundle)

- [popcap.resource_stream_bundle_patch](#popcapresource_stream_bundle_patch)

- [pvz2.text_table](#pvz2text_table)

- [pvz2.resource_manifest](#pvz2resource_manifest)

- [pvz2.remote_android_helper](#pvz2remote_android_helper)

## Introduction

The preset method provided by the tool generally accepts a file or directory as input and outputs the data to another file or directory after processing is complete, which by default will be output to the same level directory. Methods are generally divided into two types:

* Regular Method
	
	Processes a single transaction, such as decoding a single PopCap RTON file into a JSON file.

* Batch Method
	
	A wrapper around a specific regular method that accepts a directory as input and processes all matching files within it, e.g. decoding all PopCap RTON files within a directory into JSON files.
	
	Batch method are also optimized for efficiency for some regular method that require frequent memory requests and releases.

Methods are grouped into method groups based on their nature and stored in `<home>/script/Executor/Implement`, where each `JS` file provides a set of methods, and a `JSON` file with the same name holds the configuration of that set of methods, which can be modified by the user as needed.

You can make the tool execute a method directly without runtime input via command-line arguments, as described in [Additional-arguments](./usage.md#additional-arguments).

The following will list the methods with their corresponding configuration rules in the following agreed format:

> ## `<method-group-id>`
>
> The ID of method group, as a prefix to all method's item ID.
>
> * `<method-item-id` [ `*` ]
>	
> 	The method's item ID, combined with the method's group ID, is the method's ID. For example, if the group ID is `popcap.animation` and the item ID is `decode`, then the method ID is `popcap.animation.decode`. If it is marked with `*`, then the method provides a batch version.
> 	
> 	* `variable-name` : `<filter-rule>`
> 		
> 		The first argument is an input argument, specified by `input` of the current command, always of type `string`; `<filter-rule>` specifies the input value filtering rules for the method, e.g. `*.rsb` means that the method is displayed only if the input file has extension `rsb`.
> 		
> 	* `variable-name` : `variable-type` [ ~ `<default-value>` ] = `<value>`
> 		
> 		The remaining arguments are specified by the `argument` of the current command, the second of which is usually the output argument, followed by the configuration argument.
> 		
> 		If the argument value is the string `?input`, the user will be asked to enter the argument during execution.
> 		
> 		Some arguments can generate automatic by program, and the user can specify the argument value as the string `?automatic` to enable the automatic behavior, which will be identified by `~` in the documentation, followed by the value of the automatic behavior. Generally, output arguments have default behavior, i.e., the value of the input argument is used to generate the value of the output argument, e.g., the `popcap.animation.decode` method takes a `*.pam` file as the input argument and generates a `*.pam.json` file of the same name as the output argument.
> 		
> 		`<value>` is the default argument value when the user does not specify a argument value, typically an explicit `?input` or `?automatic`, or the default argument value is retrieved from the corresponding configuration file.

All of the methods listed below are regular method. Some of the regular method have a batch version, which is the same as the regular method, but with the `.batch` suffix appended to the method ID .

## `js`

* `execute` `*`
	
	* `script_file` : `*.js`
	
	* `is_module` : `boolean` = `?input`

## `data.hash`

* `md5` `*`
	
	* `target_file` : `*`

* `<configuration>`

## `data.encoding`

* `base64.encode` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`

* `base64.decode` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`

* `<configuration>`

## `data.encryption`

* `xor.encrypt` `*`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `string` ~ `*.bin` = `?automatic`
	
	* `key` : `bigint` = `?input`

* `rijndael.encrypt` `*`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `string` ~ `*.bin` = `?automatic`
	
	* `mode` : `string` = `?input`
	
	* `block_size` : `bigint` = `?input`
	
	* `key` : `string` = `?input`
	
	* `iv` : `string` = `?input`

* `rijndael.decrypt` `*`
	
	* `cipher_file` : `*`
	
	* `plain_file` : `string` ~ `*.bin` = `?automatic`
	
	* `mode` : `string` = `?input`
	
	* `block_size` : `bigint` = `?input`
	
	* `key` : `string` = `?input`
	
	* `iv` : `string` = `?input`

* `<configuration>`

## `data.compression`

* `deflate.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`

* `deflate.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.uncompress_buffer_size`

* `zlib.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`

* `zlib.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.uncompress_buffer_size`

* `gzip.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`

* `gzip.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.uncompress_buffer_size`

* `bzip2.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`

* `bzip2.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.uncompress_buffer_size`

* `lzma.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`

* `lzma.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.uncompress_buffer_size`

* `<configuration>`
	
	* `uncompress_buffer_size` : `string` = `?input`

## `data.differentiation`

* `vcdiff.encode`
	
	* `after_file` : `*`
	
	* `patch_file` : `string` ~ `*.patch.bin` = `?automatic`
	
	* `before_file` : `string` = `?input`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `vcdiff.decode`
	
	* `patch_file` : `*`
	
	* `after_file` : `string` ~ `*.after.bin` = `?automatic`
	
	* `before_file` : `string` = `?input`
	
	* `buffer_size` : `string` = `configuration.decode_buffer_size`

* `<configuration>`
	
	* `encode_buffer_size` : `string` = `1024.0m`
	
	* `decode_buffer_size` : `string` = `1024.0m`

## `text.json`

* `format` `*`
	
	* `source_file` : `*.json`
	
	* `destination_file` : `string` ~ `*.format.json` = `?automatic`
	
	* `disable_trailing_comma` : `boolean` ~ `KernelX.JSON.g_format.disable_trailing_comma` = `configuration.disable_trailing_comma`
	
	* `disable_array_wrap_line` : `boolean` ~ `KernelX.JSON.g_format.disable_array_wrap_line` = `configuration.disable_array_wrap_line`

* `<configuration>`
	
	* `disable_trailing_comma` : `boolean` = `?automatic`
	
	* `disable_array_wrap_line` : `boolean` = `?automatic`

## `text.xml`

* `format` `*`
	
	* `source_file` : `*.xml`
	
	* `destination_file` : `string` ~ `*.format.xml` = `?automatic`

* `<configuration>`

## `texture.transformation`

* `flip` `*`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `string` ~ `*.flip.png` = `?automatic`
	
	* `horizontal` : `boolean` = `?input`
	
	* `vertical` : `boolean` = `?input`

* `scale` `*`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `string` ~ `*.scale.png` = `?automatic`
	
	* `size_width` : `bigint` = `?input`
	
	* `size_height` : `bigint` = `?input`

* `scale_rate` `*`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `string` ~ `*.scale.png` = `?automatic`
	
	* `size_rate` : `number` = `?input`

* `<configuration>`

## `texture.atlas`

* `pack`
	
	* `definition_file` : `*.atlas.json`
	
	* `sprite_directory` : `string` ~ `*.sprite` = `?automatic`
	
	* `atlas_file` : `string` ~ `*.atlas.png` = `?automatic`

* `unpack`
	
	* `definition_file` : `*.atlas.json`
	
	* `atlas_file` : `string` ~ `*.atlas.png` = `?automatic`
	
	* `sprite_directory` : `string` ~ `*.sprite` = `?automatic`

* `pack_automatic`
	
	* `sprite_directory` : `*.sprite`
	
	* `atlas_file` : `string` ~ `*.atlas.png` = `?automatic`
	
	* `definition_file` : `string` ~ `*.atlas.json` = `?automatic`

* `<configuration>`

## `wwise.media`

* `encode` `*`
	
	* `raw_file` : `*.wav`
	
	* `ripe_file` : `string` ~ `*.wem` = `?automatic`
	
	* `format` : `string` = `?input`

* `decode` `*`
	
	* `ripe_file` : `*.wem`
	
	* `raw_file` : `string` ~ `*.wav` = `?automatic`

* `<configuration>`

## `wwise.sound_bank`

* `encode` `*`
	
	* `bundle_directory` : `*.bnk.bundle`
	
	* `data_file` : `string` ~ `*.bnk` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.bnk`
	
	* `bundle_directory` : `string` ~ `*.bnk.bundle` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `<configuration>`
	
	* `version_number` : `bigint` = `?input`
	
	* `encode_buffer_size` : `string` = `64.0m`

## `marmalade.dzip`

* `pack` `*`
	
	* `bundle_directory` : `*.dz.bundle`
	
	* `data_file` : `string` ~ `*.dz` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `buffer_size` : `string` = `configuration.pack_buffer_size`

* `unpack` `*`
	
	* `data_file` : `*.dz`
	
	* `bundle_directory` : `string` ~ `*.dz.bundle` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `pack_automatic`
	
	* `resource_directory` : `*`
	
	* `data_file` : `string` ~ `*.dz` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `<configuration>`
	
	* `version_number` : `bigint` = `0`
	
	* `pack_buffer_size` : `string` = `256.0m`

## `popcap.zlib`

* `compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?automatic`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`

* `uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?automatic`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`

* `<configuration>`
	
	* `version_variant_64` : `boolean` = `?input`

## `popcap.crypt_data`

* `encrypt` `*`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `string` ~ `*.cdat` = `?automatic`
	
	* `limit` : `bigint` = `configuration.limit`
	
	* `key` : `string` = `configuration.key`

* `decrypt` `*`
	
	* `cipher_file` : `*.cdat`
	
	* `plain_file` : `string` ~ `*` = `?automatic`
	
	* `limit` : `bigint` = `configuration.limit`
	
	* `key` : `string` = `configuration.key`

* `<configuration>`
	
	* `limit` : `bigint` = `256`
	
	* `key` : `string` = `AS23DSREPLKL335KO4439032N8345NF`

## `popcap.reflection_object_notation`

* `encode` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `string` ~ `*.rton` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `string` ~ `*.json` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

* `encrypt` `*`
	
	* `plain_file` : `*.rton`
	
	* `cipher_file` : `string` ~ `*.cipher.rton` = `?automatic`
	
	* `key` : `string` = `configuration.key`

* `decrypt` `*`
	
	* `cipher_file` : `*.rton`
	
	* `plain_file` : `string` ~ `*.plain.rton` = `?automatic`
	
	* `key` : `string` = `configuration.key`

* `encode_then_encrypt` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `string` ~ `*.rton` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`
	
	* `key` : `string` = `configuration.key`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decrypt_then_decode` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `string` ~ `*.json` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`
	
	* `key` : `string` = `configuration.key`

* `decode_lenient` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `string` ~ `*.json` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

* `<configuration>`
	
	* `version_number` : `bigint` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`
	
	* `key` : `string` = `?input`
	
	* `encode_buffer_size` : `string` = `64.0m`

## `popcap.texture`

* `encode`
	
	* `image_file` : `*.png`
	
	* `data_file` : `string` ~ `*.ptx` = `?automatic`
	
	* `format` : `string` = `?input`

* `decode`
	
	* `data_file` : `*.ptx`
	
	* `image_file` : `string` ~ `*.png` = `?automatic`
	
	* `format` : `string` = `?input`
	
	* `image_width` : `bigint` = `?input`
	
	* `image_height` : `bigint` = `?input`

* `<configuration>`

## `popcap.u_texture`

* `encode` `*`
	
	* `image_file` : `*.png`
	
	* `data_file` : `string` ~ `*.tex` = `?automatic`
	
	* `version_compress_texture_data` : `boolean` = `configuration.version_compress_texture_data`
	
	* `format` : `string` = `?input`

* `decode` `*`
	
	* `data_file` : `*.tex`
	
	* `image_file` : `string` ~ `*.png` = `?automatic`
	
	* `version_compress_texture_data` : `boolean` = `configuration.version_compress_texture_data`

* `<configuration>`
	
	* `version_compress_texture_data` : `boolean` = `?input`

## `popcap.sexy_texture`

* `encode` `*`
	
	* `image_file` : `*.png`
	
	* `data_file` : `string` ~ `*.tex` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `format` : `string` = `?input`
	
	* `compress_texture_data` : `boolean` = `configuration.encode_compress_texture_data`

* `decode` `*`
	
	* `data_file` : `*.tex`
	
	* `image_file` : `string` ~ `*.png` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `<configuration>`
	
	* `version_number` : `bigint` = `0`
	
	* `encode_compress_texture_data` : `boolean` = `?input`

## `popcap.animation`

* `encode` `*`
	
	* `definition_file` : `*.pam.json`
	
	* `data_file` : `string` ~ `*.pam` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.pam`
	
	* `definition_file` : `string` ~ `*.pam.json` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `convert.flash.from` `*`
	
	* `raw_file` : `*.pam.json`
	
	* `ripe_directory` : `string` ~ `*.pam.xfl` = `?automatic`

* `convert.flash.to` `*`
	
	* `ripe_directory` : `*.pam.xfl`
	
	* `raw_file` : `string` ~ `*.pam.json` = `?automatic`

* `convert.flash.resize` `*`
	
	* `target_directory` : `*.pam.xfl`
	
	* `resolution` : `bigint` = `?input`

* `convert.flash.link_media` `*`
	
	* `target_directory` : `*.pam.xfl`

* `<configuration>`
	
	* `version_number` : `bigint` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.re_animation`

* `encode` `*`
	
	* `definition_file` : `*.reanim.json`
	
	* `data_file` : `string` ~ `*.reanim.compiled` = `?automatic`
	
	* `version_platform` : `string` = `configuration.version_platform`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.reanim.compiled`
	
	* `definition_file` : `string` ~ `*.reanim.json` = `?automatic`
	
	* `version_platform` : `string` = `configuration.version_platform`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`

* `<configuration>`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.particle`

* `encode` `*`
	
	* `definition_file` : `*.particle.json`
	
	* `data_file` : `string` ~ `*.xml.compiled` = `?automatic`
	
	* `version_platform` : `string` = `configuration.version_platform`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.xml.compiled`
	
	* `definition_file` : `string` ~ `*.particle.json` = `?automatic`
	
	* `version_platform` : `string` = `configuration.version_platform`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`

* `<configuration>`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.trail`

* `encode` `*`
	
	* `definition_file` : `*.trail.json`
	
	* `data_file` : `string` ~ `*.trail.compiled` = `?automatic`
	
	* `version_platform` : `string` = `configuration.version_platform`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.trail.compiled`
	
	* `definition_file` : `string` ~ `*.trail.json` = `?automatic`
	
	* `version_platform` : `string` = `configuration.version_platform`
	
	* `version_variant_64` : `boolean` = `configuration.version_variant_64`

* `<configuration>`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.particle_effect`

* `encode` `*`
	
	* `definition_file` : `*.ppf.json`
	
	* `data_file` : `string` ~ `*.ppf` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.ppf`
	
	* `definition_file` : `string` ~ `*.ppf.json` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `<configuration>`
	
	* `version_number` : `bigint` = `1`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.render_effect`

* `encode` `*`
	
	* `definition_file` : `*.popfx.json`
	
	* `data_file` : `string` ~ `*.popfx` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_variant` : `bigint` = `configuration.version_variant`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.popfx`
	
	* `definition_file` : `string` ~ `*.popfx.json` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_variant` : `bigint` = `configuration.version_variant`

* `<configuration>`
	
	* `version_number` : `bigint` = `1`
	
	* `version_variant` : `bigint` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.character_font_widget_2`

* `encode` `*`
	
	* `definition_file` : `*.cfw2.json`
	
	* `data_file` : `string` ~ `*.cfw2` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.cfw2`
	
	* `definition_file` : `string` ~ `*.cfw2.json` = `?automatic`

* `<configuration>`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.package`

* `pack` `*`
	
	* `bundle_directory` : `*.pak.bundle`
	
	* `data_file` : `string` ~ `*.pak` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_compress_resource_data` : `boolean` = `configuration.version_compress_resource_data`
	
	* `buffer_size` : `string` = `configuration.pack_buffer_size`

* `unpack` `*`
	
	* `data_file` : `*.pak`
	
	* `bundle_directory` : `string` ~ `*.pak.bundle` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_compress_resource_data` : `boolean` = `configuration.version_compress_resource_data`

* `pack_automatic`
	
	* `resource_directory` : `*`
	
	* `data_file` : `string` ~ `*.pak` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_compress_resource_data` : `boolean` = `configuration.version_compress_resource_data`

* `encrypt` `*`
	
	* `plain_file` : `*.pak`
	
	* `cipher_file` : `string` ~ `*.cipher.pak` = `?automatic`

* `<configuration>`
	
	* `version_number` : `bigint` = `0`
	
	* `version_compress_resource_data` : `boolean` = `?input`
	
	* `pack_buffer_size` : `string` = `256.0m`

## `popcap.resource_stream_group`

* `pack` `*`
	
	* `bundle_directory` : `*.rsg.bundle`
	
	* `data_file` : `string` ~ `*.rsg` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `buffer_size` : `string` = `configuration.pack_buffer_size`

* `unpack` `*`
	
	* `data_file` : `*.rsg`
	
	* `bundle_directory` : `string` ~ `*.rsg.bundle` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`

* `<configuration>`
	
	* `version_number` : `bigint` = `?input`
	
	* `pack_buffer_size` : `string` = `256.0m`

## `popcap.resource_stream_bundle`

* `pack` `*`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `data_file` : `string` ~ `*.rsb` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_extended_texture_information_for_pvz2_cn` : `bigint` = `configuration.version_extended_texture_information_for_pvz2_cn`
	
	* `layout_mode` : `string` = `configuration.layout_mode`
	
	* `input_packet` : `boolean` = `?input`
	
	* `output_new_packet` : `boolean` = `?input`
	
	* `buffer_size` : `string` = `configuration.pack_buffer_size`

* `unpack` `*`
	
	* `data_file` : `*.rsb`
	
	* `bundle_directory` : `string` ~ `*.rsb.bundle` = `?automatic`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `version_extended_texture_information_for_pvz2_cn` : `bigint` = `configuration.version_extended_texture_information_for_pvz2_cn`
	
	* `layout_mode` : `string` = `configuration.layout_mode`
	
	* `output_resource` : `boolean` = `?input`
	
	* `output_packet` : `boolean` = `?input`

* `resource_convert` `*`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `option_recase_path` : `boolean` = `configuration.resource_convert_option.recase_path`
	
	* `option_rton` : `boolean` = `configuration.resource_convert_option.rton`
	
	* `option_rton_version_number` : `integer` = `configuration.resource_convert_option.rton_version_number`
	
	* `option_rton_version_native_string_encoding_use_utf8` : `boolean` = `configuration.resource_convert_option.rton_version_native_string_encoding_use_utf8`
	
	* `option_rton_crypt` : `boolean` = `configuration.resource_convert_option.rton_crypt`
	
	* `option_rton_crypt_key` : `string` = `configuration.resource_convert_option.rton_crypt_key`
	
	* `option_ptx` : `boolean` = `configuration.resource_convert_option.ptx`
	
	* `option_ptx_texture_format_map_name` : `string` = `configuration.resource_convert_option.ptx_texture_format_map_name`
	
	* `option_ptx_atlas` : `boolean` = `configuration.resource_convert_option.ptx_atlas`
	
	* `option_ptx_atlas_resize` : `boolean` = `configuration.resource_convert_option.ptx_atlas_resize`
	
	* `option_ptx_sprite` : `boolean` = `configuration.resource_convert_option.ptx_sprite`
	
	* `option_pam` : `boolean` = `configuration.resource_convert_option.pam`
	
	* `option_pam_version_number` : `integer` = `configuration.resource_convert_option.pam_version_number`
	
	* `option_pam_json` : `boolean` = `configuration.resource_convert_option.pam_json`
	
	* `option_pam_flash` : `boolean` = `configuration.resource_convert_option.pam_flash`
	
	* `option_bnk` : `boolean` = `configuration.resource_convert_option.bnk`
	
	* `option_bnk_version_number` : `integer` = `configuration.resource_convert_option.bnk_version_number`
	
	* `option_wem` : `boolean` = `configuration.resource_convert_option.wem`

* `unpack_lenient` `*`
	
	* `data_file` : `*.rsb`
	
	* `bundle_directory` : `string` ~ `*.rsb.bundle` = `?automatic`

* `<configuration>`
	
	* `version_number` : `bigint` = `?input`
	
	* `version_extended_texture_information_for_pvz2_cn` : `bigint` = `?input`
	
	* `layout_mode` : `string` = `?input`
	
	* `pack_buffer_size` : `string` = `1.25g`
	
	* `resource_convert_option` : `{ ... }` = `...`

## `popcap.resource_stream_bundle_patch`

* `encode`
	
	* `after_file` : `*.rsb`
	
	* `patch_file` : `string` ~ `*.rsbpatch` = `?automatic`
	
	* `before_file` : `string` = `?input`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `use_raw_packet` : `boolean` = `configuration.use_raw_packet`
	
	* `buffer_size` : `string` = `configuration.encode_buffer_size`

* `decode`
	
	* `patch_file` : `*.rsbpatch`
	
	* `after_file` : `string` ~ `*.rsb` = `?automatic`
	
	* `before_file` : `string` = `?input`
	
	* `version_number` : `bigint` = `configuration.version_number`
	
	* `use_raw_packet` : `boolean` = `configuration.use_raw_packet`
	
	* `buffer_size` : `string` = `configuration.decode_buffer_size`

* `<configuration>`
	
	* `version_number` : `bigint` = `1`
	
	* `use_raw_packet` : `boolean` = `?input`
	
	* `encode_buffer_size` : `string` = `1024.0m`
	
	* `decode_buffer_size` : `string` = `1024.0m`

## `pvz2.text_table`

* `convert`
	
	* `source_file` : `*.txt|json`
	
	* `destination_version` : `string` = `?input`
	
	* `destination_file` : `string` ~ `*.convert.txt|json` = `?automatic`

* `<configuration>`

## `pvz2.resource_manifest`

* `new_type_object_notation.encode`
	
	* `definition_file` : `*.json`
	
	* `data_file` : `string` ~ `*.newton` = `?automatic`
	
	* `buffer_size` : `string` = `configuration.new_type_object_notation_encode_buffer_size`

* `new_type_object_notation.decode`
	
	* `data_file` : `*.newton`
	
	* `definition_file` : `string` ~ `*.json` = `?automatic`

* `regular.from`
	
	* `raw_file` : `*.json`
	
	* `ripe_file` : `string` ~ `*.regular.json` = `?automatic`

* `regular.to`
	
	* `ripe_file` : `*.json`
	
	* `raw_file` : `string` ~ `*.official.json` = `?automatic`
	
	* `use_array_style_path` : `boolean` = `configuration.official_use_array_style_path`

* `<configuration>`
	
	* `new_type_object_notation_encode_buffer_size` : `string` = `64.0m`
	
	* `official_use_array_style_path` : `boolean` = `?input`

## `pvz2.remote_android_helper`

* `launch`
	
	* `project_directory` : `*.pvz2_remote_android_helper_project`
	
	* `action` : `string` = `?input`

* `<configuration>`
