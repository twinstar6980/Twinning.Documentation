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

- [texture.encoding](#textureencoding)

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

- [pvz2.package_project](#pvz2package_project)

- [pvz2.remote_project](#pvz2remote_project)

- [kairosoft.user_data](#kairosoftuser_data)

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
> 		The first argument is an input argument, specified by `input` or `argument` of the current command, always of type `path`; `<filter-rule>` specifies the input value filtering rules for the method, e.g. `*.rsb` means that the method is displayed only if the input file has extension `rsb`. Filter rule can be modified in the configuration file.
> 	
> 	* `variable-name` : `variable-type` [ ~ `<default-value>` ] = `<value>`
> 		
> 		The remaining arguments are specified by the `argument` of the current command, the second of which is usually the output argument, followed by the configuration argument.
> 		
> 		If the argument value is the string `?input`, the user will be asked to enter the argument during execution.
> 		
> 		Some arguments can generate automatic by program, and the user can specify the argument value as the string `?automatic` to enable the automatic behavior, which will be identified by `~` in the documentation, followed by the value of the automatic behavior. Generally, output arguments have default behavior, i.e., the value of the input argument is used to generate the value of the output argument, e.g., the `popcap.animation.decode` method takes a `*.pam` file as the input argument and generates a `*.pam.json` file of the same name as the output argument.
> 		
> 		`<value>` is the default value when the user does not specify a argument value, typically an explicit `?input` or `?automatic`, the default value can be modified in the configuration file.

All of the methods listed below are regular method. Some of the regular method have a batch version, which is the same as the regular method, but with the `.batch` suffix appended to the method ID .

## `js`

* `execute` `*`
	
	* `script_file` : `*.js`
	
	* `is_module` : `boolean` = `?input`

## `data.hash`

* `md5` `*`
	
	* `target_file` : `*`

## `data.encoding`

* `base64.encode` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`

* `base64.decode` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`

## `data.encryption`

* `xor.encrypt` `*`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `path` ~ `*.bin` = `?automatic`
	
	* `key` : `integer` = `?input`

* `rijndael.encrypt` `*`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `path` ~ `*.bin` = `?automatic`
	
	* `mode` : `string` = `?input`
	
	* `block_size` : `integer` = `?input`
	
	* `key` : `string` = `?input`
	
	* `iv` : `string` = `?input`

* `rijndael.decrypt` `*`
	
	* `cipher_file` : `*`
	
	* `plain_file` : `path` ~ `*.bin` = `?automatic`
	
	* `mode` : `string` = `?input`
	
	* `block_size` : `integer` = `?input`
	
	* `key` : `string` = `?input`
	
	* `iv` : `string` = `?input`

## `data.compression`

* `deflate.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`

* `deflate.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `size` = `?input`

* `zlib.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`

* `zlib.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `size` = `?input`

* `gzip.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`

* `gzip.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `size` = `?input`

* `bzip2.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`

* `bzip2.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `size` = `?input`

* `lzma.compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`

* `lzma.uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`
	
	* `buffer_size` : `size` = `?input`

## `data.differentiation`

* `vcdiff.encode`
	
	* `after_file` : `*`
	
	* `patch_file` : `path` ~ `*.patch.bin` = `?automatic`
	
	* `before_file` : `path` = `?input`
	
	* `buffer_size` : `size` = `1024.0m`

* `vcdiff.decode`
	
	* `patch_file` : `*`
	
	* `after_file` : `path` ~ `*.after.bin` = `?automatic`
	
	* `before_file` : `path` = `?input`
	
	* `buffer_size` : `size` = `1024.0m`

## `text.json`

* `format` `*`
	
	* `source_file` : `*.json`
	
	* `destination_file` : `path` ~ `*.format.json` = `?automatic`
	
	* `disable_array_trailing_comma` : `boolean` ~ `KernelX.JSON.g_format.disable_array_trailing_comma` = `?automatic`
	
	* `disable_array_line_breaking` : `boolean` ~ `KernelX.JSON.g_format.disable_array_line_breaking` = `?automatic`
	
	* `disable_object_trailing_comma` : `boolean` ~ `KernelX.JSON.g_format.disable_object_trailing_comma` = `?automatic`
	
	* `disable_object_line_breaking` : `boolean` ~ `KernelX.JSON.g_format.disable_object_line_breaking` = `?automatic`

## `text.xml`

* `format` `*`
	
	* `source_file` : `*.xml`
	
	* `destination_file` : `path` ~ `*.format.xml` = `?automatic`

## `texture.transformation`

* `flip` `*`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `path` ~ `*.flip.png` = `?automatic`
	
	* `horizontal` : `boolean` = `?input`
	
	* `vertical` : `boolean` = `?input`

* `scale` `*`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `path` ~ `*.scale.png` = `?automatic`
	
	* `size_width` : `integer` = `?input`
	
	* `size_height` : `integer` = `?input`

* `scale_rate` `*`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `path` ~ `*.scale.png` = `?automatic`
	
	* `size_rate` : `floater` = `?input`

## `texture.encoding`

* `encode`
	
	* `image_file` : `*.png`
	
	* `data_file` : `path` ~ `*.bin` = `?automatic`
	
	* `format` : `string` = `?input`

* `decode`
	
	* `data_file` : `*.bin`
	
	* `image_file` : `path` ~ `*.png` = `?automatic`
	
	* `format` : `string` = `?input`
	
	* `image_width` : `integer` = `?input`
	
	* `image_height` : `integer` = `?input`

## `texture.atlas`

* `pack`
	
	* `definition_file` : `*.atlas.json`
	
	* `sprite_directory` : `path` ~ `*.sprite` = `?automatic`
	
	* `atlas_file` : `path` ~ `*.atlas.png` = `?automatic`

* `unpack`
	
	* `definition_file` : `*.atlas.json`
	
	* `atlas_file` : `path` ~ `*.atlas.png` = `?automatic`
	
	* `sprite_directory` : `path` ~ `*.sprite` = `?automatic`

* `pack_automatic`
	
	* `sprite_directory` : `*.sprite`
	
	* `atlas_file` : `path` ~ `*.atlas.png` = `?automatic`
	
	* `definition_file` : `path` ~ `*.atlas.json` = `?automatic`

## `wwise.media`

* `encode` `*`
	
	* `raw_file` : `*.wav`
	
	* `ripe_file` : `path` ~ `*.wem` = `?automatic`
	
	* `format` : `string` = `?input`

* `decode` `*`
	
	* `ripe_file` : `*.wem`
	
	* `raw_file` : `path` ~ `*.wav` = `?automatic`

## `wwise.sound_bank`

* `encode` `*`
	
	* `bundle_directory` : `*.bnk.bundle`
	
	* `data_file` : `path` ~ `*.bnk` = `?automatic`
	
	* `version_number` : `integer` = `?input`
	
	* `buffer_size` : `size` = `64.0m`

* `decode` `*`
	
	* `data_file` : `*.bnk`
	
	* `bundle_directory` : `path` ~ `*.bnk.bundle` = `?automatic`
	
	* `version_number` : `integer` = `?input`

## `marmalade.dzip`

* `pack` `*`
	
	* `bundle_directory` : `*.dz.bundle`
	
	* `data_file` : `path` ~ `*.dz` = `?automatic`
	
	* `version_number` : `integer` = `0`
	
	* `buffer_size` : `size` = `256.0m`

* `unpack` `*`
	
	* `data_file` : `*.dz`
	
	* `bundle_directory` : `path` ~ `*.dz.bundle` = `?automatic`
	
	* `version_number` : `integer` = `0`

* `pack_automatic`
	
	* `resource_directory` : `*`
	
	* `data_file` : `path` ~ `*.dz` = `?automatic`
	
	* `version_number` : `integer` = 0`

## `popcap.zlib`

* `compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `path` ~ `*.bin` = `?automatic`
	
	* `version_variant_64` : `boolean` = `?input`

* `uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `path` ~ `*.bin` = `?automatic`
	
	* `version_variant_64` : `boolean` = `?input`

## `popcap.crypt_data`

* `encrypt` `*`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `path` ~ `*.cdat` = `?automatic`
	
	* `limit` : `integer` = `256`
	
	* `key` : `string` = `AS23DSREPLKL335KO4439032N8345NF`

* `decrypt` `*`
	
	* `cipher_file` : `*.cdat`
	
	* `plain_file` : `path` ~ `*` = `?automatic`
	
	* `limit` : `integer` = `256`
	
	* `key` : `string` = `AS23DSREPLKL335KO4439032N8345NF`

## `popcap.reflection_object_notation`

* `encode` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `path` ~ `*.rton` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`
	
	* `buffer_size` : `size` = `64.0m`

* `decode` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `path` ~ `*.json` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`

* `encrypt` `*`
	
	* `plain_file` : `*.rton`
	
	* `cipher_file` : `path` ~ `*.cipher.rton` = `?automatic`
	
	* `key` : `string` = `?input`

* `decrypt` `*`
	
	* `cipher_file` : `*.rton`
	
	* `plain_file` : `path` ~ `*.plain.rton` = `?automatic`
	
	* `key` : `string` = `?input`

* `encode_then_encrypt` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `path` ~ `*.rton` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`
	
	* `key` : `string` = `?input`
	
	* `buffer_size` : `size` = `64.0m`

* `decrypt_then_decode` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `path` ~ `*.json` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`
	
	* `key` : `string` = `?input`

* `decode_lenient` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `path` ~ `*.json` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`

## `popcap.texture`

* `encode`
	
	* `image_file` : `*.png`
	
	* `data_file` : `path` ~ `*.ptx` = `?automatic`
	
	* `format` : `string` = `?input`

* `decode`
	
	* `data_file` : `*.ptx`
	
	* `image_file` : `path` ~ `*.png` = `?automatic`
	
	* `format` : `string` = `?input`
	
	* `image_width` : `integer` = `?input`
	
	* `image_height` : `integer` = `?input`

## `popcap.u_texture`

* `encode` `*`
	
	* `image_file` : `*.png`
	
	* `data_file` : `path` ~ `*.tex` = `?automatic`
	
	* `version_compress_texture_data` : `boolean` = `?input`
	
	* `format` : `string` = `?input`

* `decode` `*`
	
	* `data_file` : `*.tex`
	
	* `image_file` : `path` ~ `*.png` = `?automatic`
	
	* `version_compress_texture_data` : `boolean` = `?input`

## `popcap.sexy_texture`

* `encode` `*`
	
	* `image_file` : `*.png`
	
	* `data_file` : `path` ~ `*.tex` = `?automatic`
	
	* `version_number` : `integer` = `0`
	
	* `format` : `string` = `?input`
	
	* `compress_texture_data` : `boolean` = `?input`

* `decode` `*`
	
	* `data_file` : `*.tex`
	
	* `image_file` : `path` ~ `*.png` = `?automatic`
	
	* `version_number` : `integer` = `0`

## `popcap.animation`

* `encode` `*`
	
	* `definition_file` : `*.pam.json`
	
	* `data_file` : `path` ~ `*.pam` = `?automatic`
	
	* `version_number` : `integer` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.pam`
	
	* `definition_file` : `path` ~ `*.pam.json` = `?automatic`
	
	* `version_number` : `integer` = `?input`

* `convert.flash.from` `*`
	
	* `raw_file` : `*.pam.json`
	
	* `ripe_directory` : `path` ~ `*.pam.xfl` = `?automatic`

* `convert.flash.to` `*`
	
	* `ripe_directory` : `*.pam.xfl`
	
	* `raw_file` : `path` ~ `*.pam.json` = `?automatic`

* `convert.flash.resize` `*`
	
	* `target_directory` : `*.pam.xfl`
	
	* `resolution` : `integer` = `?input`

* `convert.flash.link_media` `*`
	
	* `target_directory` : `*.pam.xfl`

## `popcap.re_animation`

* `encode` `*`
	
	* `definition_file` : `*.reanim.json`
	
	* `data_file` : `path` ~ `*.reanim.compiled` = `?automatic`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.reanim.compiled`
	
	* `definition_file` : `path` ~ `*.reanim.json` = `?automatic`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`

## `popcap.particle`

* `encode` `*`
	
	* `definition_file` : `*.particle.json`
	
	* `data_file` : `path` ~ `*.xml.compiled` = `?automatic`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.xml.compiled`
	
	* `definition_file` : `path` ~ `*.particle.json` = `?automatic`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`

## `popcap.trail`

* `encode` `*`
	
	* `definition_file` : `*.trail.json`
	
	* `data_file` : `path` ~ `*.trail.compiled` = `?automatic`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.trail.compiled`
	
	* `definition_file` : `path` ~ `*.trail.json` = `?automatic`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`

## `popcap.particle_effect`

* `encode` `*`
	
	* `definition_file` : `*.ppf.json`
	
	* `data_file` : `path` ~ `*.ppf` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.ppf`
	
	* `definition_file` : `path` ~ `*.ppf.json` = `?automatic`
	
	* `version_number` : `integer` = `1`

## `popcap.render_effect`

* `encode` `*`
	
	* `definition_file` : `*.popfx.json`
	
	* `data_file` : `path` ~ `*.popfx` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_variant` : `integer` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.popfx`
	
	* `definition_file` : `path` ~ `*.popfx.json` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_variant` : `integer` = `?input`

## `popcap.character_font_widget_2`

* `encode` `*`
	
	* `definition_file` : `*.cfw2.json`
	
	* `data_file` : `path` ~ `*.cfw2` = `?automatic`
	
	* `buffer_size` : `size` = `8.0m`

* `decode` `*`
	
	* `data_file` : `*.cfw2`
	
	* `definition_file` : `path` ~ `*.cfw2.json` = `?automatic`

## `popcap.package`

* `pack` `*`
	
	* `bundle_directory` : `*.pak.bundle`
	
	* `data_file` : `path` ~ `*.pak` = `?automatic`
	
	* `version_number` : `integer` = `0`
	
	* `version_compress_resource_data` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `256.0m`

* `unpack` `*`
	
	* `data_file` : `*.pak`
	
	* `bundle_directory` : `path` ~ `*.pak.bundle` = `?automatic`
	
	* `version_number` : `integer` = `0`
	
	* `version_compress_resource_data` : `boolean` = `?input`

* `pack_automatic`
	
	* `resource_directory` : `*`
	
	* `data_file` : `path` ~ `*.pak` = `?automatic`
	
	* `version_number` : `integer` = `0`
	
	* `version_compress_resource_data` : `boolean` = `?input`

* `encrypt` `*`
	
	* `plain_file` : `*.pak`
	
	* `cipher_file` : `path` ~ `*.cipher.pak` = `?automatic`

## `popcap.resource_stream_group`

* `pack` `*`
	
	* `bundle_directory` : `*.rsg.bundle`
	
	* `data_file` : `path` ~ `*.rsg` = `?automatic`
	
	* `version_number` : `integer` = `?input`
	
	* `buffer_size` : `size` = `256.0m`

* `unpack` `*`
	
	* `data_file` : `*.rsg`
	
	* `bundle_directory` : `path` ~ `*.rsg.bundle` = `?automatic`
	
	* `version_number` : `integer` = `?input`

## `popcap.resource_stream_bundle`

* `pack` `*`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `data_file` : `path` ~ `*.rsb` = `?automatic`
	
	* `version_number` : `integer` = `?input`
	
	* `version_extended_texture_information_for_pvz2_cn` : `integer` = `?input`
	
	* `layout_mode` : `string` = `?input`
	
	* `input_packet` : `boolean` = `?input`
	
	* `output_new_packet` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `1.25g`

* `unpack` `*`
	
	* `data_file` : `*.rsb`
	
	* `bundle_directory` : `path` ~ `*.rsb.bundle` = `?automatic`
	
	* `version_number` : `integer` = `?input`
	
	* `version_extended_texture_information_for_pvz2_cn` : `integer` = `?input`
	
	* `layout_mode` : `string` = `?input`
	
	* `output_resource` : `boolean` = `?input`
	
	* `output_packet` : `boolean` = `?input`

* `resource_convert` `*`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `option_recase_path` : `boolean` = `?input`
	
	* `option_rton` : `boolean` = `?input`
	
	* `option_rton_version_number` : `integer` = `1`
	
	* `option_rton_version_native_string_encoding_use_utf8` : `boolean` = `?input`
	
	* `option_rton_crypt` : `boolean` = `?input`
	
	* `option_rton_crypt_key` : `string` = `?input`
	
	* `option_ptx` : `boolean` = `?input`
	
	* `option_ptx_texture_format_map_name` : `string` = `?input`
	
	* `option_ptx_atlas` : `boolean` = `?input`
	
	* `option_ptx_atlas_resize` : `boolean` = `?input`
	
	* `option_ptx_sprite` : `boolean` = `?input`
	
	* `option_pam` : `boolean` = `?input`
	
	* `option_pam_version_number` : `integer` = `6`
	
	* `option_pam_json` : `boolean` = `?input`
	
	* `option_pam_flash` : `boolean` = `?input`
	
	* `option_bnk` : `boolean` = `?input`
	
	* `option_bnk_version_number` : `integer` = `?input`
	
	* `option_wem` : `boolean` = `?input`

* `unpack_lenient` `*`
	
	* `data_file` : `*.rsb`
	
	* `bundle_directory` : `path` ~ `*.rsb.bundle` = `?automatic`

## `popcap.resource_stream_bundle_patch`

* `encode`
	
	* `after_file` : `*.rsb`
	
	* `patch_file` : `path` ~ `*.rsbpatch` = `?automatic`
	
	* `before_file` : `path` = `?input`
	
	* `version_number` : `integer` = `1`
	
	* `use_raw_packet` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `1024.0m`

* `decode`
	
	* `patch_file` : `*.rsbpatch`
	
	* `after_file` : `path` ~ `*.rsb` = `?automatic`
	
	* `before_file` : `path` = `?input`
	
	* `version_number` : `integer` = `1`
	
	* `use_raw_packet` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `1024.0m`

## `pvz2.text_table`

* `convert`
	
	* `source_file` : `*.txt|json`
	
	* `destination_version` : `string` = `?input`
	
	* `destination_file` : `path` ~ `*.convert.txt|json` = `?automatic`

## `pvz2.resource_manifest`

* `new_type_object_notation.encode`
	
	* `definition_file` : `*.json`
	
	* `data_file` : `path` ~ `*.newton` = `?automatic`
	
	* `buffer_size` : `size` = `64.0m`

* `new_type_object_notation.decode`
	
	* `data_file` : `*.newton`
	
	* `definition_file` : `path` ~ `*.json` = `?automatic`

* `regular.from`
	
	* `raw_file` : `*.json`
	
	* `ripe_file` : `path` ~ `*.regular.json` = `?automatic`

* `regular.to`
	
	* `ripe_file` : `*.json`
	
	* `raw_file` : `path` ~ `*.official.json` = `?automatic`
	
	* `use_array_style_path` : `boolean` = `?input`

## `pvz2.package_project`

* `compile`
	
	* `project_directory` : `*.pvz2_package_project`
	
	* `target_scope` : `string` = `?input`
	
	* `target_package` : `string` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

* `link`
	
	* `project_directory` : `*.pvz2_package_project`
	
	* `target_package` : `string` = `?input`
	
	* `remake_manifest` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `1024.0m`

* `parse`
	
	* `project_directory` : `*.pvz2_package_project`
	
	* `package_directory` : `path` = `?input`
	
	* `package_name` : `string` = `?automatic`
	
	* `package_version_number` : `integer` = `?input`
	
	* `package_version_extended_texture_information_for_pvz2_cn` : `integer` = `?input`

## `pvz2.remote_project`

* `execute`
	
	* `project_directory` : `*.pvz2_remote_project`
	
	* `action` : `string` = `?input`
	
	* `target` : `string` = `?input`

## `kairosoft.user_data`

* `recrypt`
	
	* `old_directory` : `*`
	
	* `new_directory` : `path` ~ `*.recrypt` = `?automatic`
	
	* `old_key` : `string` = `?input`
	
	* `new_key` : `string` = `?input`
