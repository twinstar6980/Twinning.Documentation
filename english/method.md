# Method

- [Introduction](#Introduction)

- [General configuration](#General-configuration)

- [js](#js)

- [json](#json)

- [data.hash](#datahash)

- [data.encoding](#dataencoding)

- [data.encryption](#dataencryption)

- [data.compression](#datacompression)

- [data.differentiation](#datadifferentiation)

- [image.transformation](#imagetransformation)

- [image.atlas](#imageatlas)

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

- [popcap.effect](#popcapeffect)

- [popcap.character_font_widget_2](#popcapcharacter_font_widget_2)

- [popcap.package](#popcappackage)

- [popcap.resource_stream_group](#popcapresource_stream_group)

- [popcap.resource_stream_bundle](#popcapresource_stream_bundle)

- [popcap.resource_stream_bundle_patch](#popcapresource_stream_bundle_patch)

- [pvz2.text_table](#pvz2text_table)

- [pvz2.remote_android_helper](#pvz2remote_android_helper)

## Introduction

The preset function provided by the tool generally accepts a file or directory as input and outputs the data to another file or directory after processing is complete, which by default will be output to the same level directory. Functions are generally divided into two types:

- Regular Functions

	Processes a single transaction, such as decoding a single PopCap RTON file into a JSON file.

- Batch functions

	A wrapper around a specific routine function that accepts a directory as input and processes all matching files within it, e.g. decoding all PopCap RTON files within a directory into JSON files.

	Batch functions are also optimized for efficiency for some regular functions that require frequent memory requests and releases.

Functions are grouped into function groups based on their nature and stored in `+ <home>/script/Entry/method`, where each `JS` file provides a set of functions, and a `JSON` file with the same name holds the configuration of that set of functions, which can be modified by the user as needed.

You can make the tool execute a function directly without runtime input via command-line arguments, as described in [Additional-arguments](./usage.md#additional-parameters).

The following will list the functions with their corresponding configuration rules in the following agreed format:

> ## `<method-group-id>`
>
> The group ID of the function, as a prefix to all sub-function IDs.
>
> - `<method-item-id` [ `*` ]
>
> The function's item ID, combined with the function's group ID, is the function's ID. For example, if the group ID is `popcap.rton` and the item ID is `decode`, then the function ID is `popcap.rton.decode`. If it is marked with `*`, then the function provides a batch version.
>
> - `variable-name` : `<filter-rule>`
>
> The first parameter is an input parameter, specified by `input` of the current command, always of type `string`; `<filter-rule>` specifies the input value filtering rules for the function, e.g. `*.rsb` means that the function is displayed only if the input file has extension `rsb`.
>
> - `variable-name` : `variable-type` [ ~ `<default-value>` ] = `<value>`
>
> The remaining arguments are specified by the `argument` of the current command, the second of which is usually the output argument, followed by the configuration argument.
>
> If the argument value is the string `?input`, the user will be asked to enter the argument during execution.
>
> Some parameters have a default behavior, and the user can specify the parameter value as the string `?default` to enable the default behavior, which will be identified by `~` in the documentation, followed by the value of the default behavior. Generally, output parameters have default behavior, i.e., the value of the input parameter is used to generate the value of the output parameter, e.g., the `popcap.rton.decode` function takes a `*.rton` file as the input parameter and generates a `*.json` file of the same name as the output parameter.
>
> `<value>` is the default parameter value when the user does not specify a parameter value, typically an explicit `?input` or `?default`, or the default parameter value is retrieved from the corresponding configuration file.

All of the functions listed below are regular functions. Some of the regular functions have a corresponding batch version, which is the same as the regular functions, but with the `.batch` suffix appended to the function ID and the `_directory` suffix appended to the input and output parameters.

For example, the following is the definition of the `popcap.selection_object_notation.encode` function:

> - `encode` `*`
> 	
> 	- `value_file` : `*.json`
> 	
> 	- `data_file` : `string` ~ `*.rton` = `?default`
> 	
> 	- `version_number` : `bigint` = `configuration.version_number`
> 	
> 	- `buffer_size` : `string` = `configuration.encode_buffer_size`

Then the corresponding batch version is as follows:

> - `encode.batch`
> 	
> 	- `value_file_directory` : `*`
> 	
> 	- `data_file_directory` : `string` ~ `*.rton_encode` = `?default`
> 	
> 	- `version_number` : `bigint` = `configuration.version_number`
> 	
> 	- `buffer_size` : `string` = `configuration.encode_buffer_size`

## General configuration

`- <home>/script/Entry/Entry.json` Defines the common behavior during the tool run.

- `<configuration>`

	- `language` : `string` = `Chinese`

		Interaction language. Can be `Chinese` or `English`.

	- `cli_disable_virtual_terminal_sequence` : `boolean` = `false`

		Disable command-line virtual terminal sequences. Valid only when using a command-line shell.

	- `pause_when_finish` : `boolean` = `true`

		Pause the program after all functions evaluate is done and wait for the user to close it or press enter to exit.

	- `notification_time_limit` : `null | bigint` = `15000`

		Notification time limit. If the execution time exceeds this value (in milliseconds) after a command has completed, a system notification will be pushed to alert the user to set up the configuration. Setting to null will disable notifications.

	- `thread_limit` : `bigint` = `0`

		The maximum number of thread pools. Currently has no practical effect.

	- `byte_stream_use_big_endian` : `boolean` = `false`

		Use big end-order for internal byte stream operations. This option is necessary for some big-endian file processing. It is disabled by default, because most of the time the user is dealing with little-endian files.

	- `common_buffer_size` : `string` = `64.0m`

		The size of the common buffer. Used for encoding JSON strings, encoding PNG images, etc. If the amount of memory required for encoding exceeds this size, the tool will fail to process it.

	- `json_format` : `{ ... }`

		JSON output format.

		- `disable_trailing_comma` : `boolean` = `false`

			Disable trailing comma.

		- `disable_array_wrap_line` : `boolean` = `false`

			Disable line wrapping between array elements.

	- `method_common_argument` : `{ ... }`

		File object related arguments.

		- `path_tactic_if_out_exist` : `string` = `none`

			The policy when the file object pointed to by the output path exists. Takes the following values:

			- `none`: None, selected by the user at runtime.

			- `trash`: Move existing file objects to the recycle directory `+ <home>/trash`.

			- `delete`: Delete an existing file object.

			- `override`: Overwrite existing file objects.

## `js`

- `execute`

	- `script_file` : `*.js`

## `json`

- `format` `*`

	- `source_file` : `*.json`

	- `destination_file` : `string` ~ `*.format.json` = `?default`

	- `disable_trailing_comma` : `boolean` ~ `CoreX.JSON.g_format.disable_trailing_comma` = `configuration.disable_trailing_comma`

	- `disable_array_wrap_line` : `boolean` ~ `CoreX.JSON.g_format.disable_array_wrap_line` = `configuration.disable_array_wrap_line`

- `<configuration>`

	- `disable_trailing_comma` : `boolean` = `?input`

	- `disable_array_wrap_line` : `boolean` = `?input`

## `data.hash`

- `md5`

	- `target_file` : `*`

- `<configuration>`

## `data.encoding`

- `base64.encode`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

- `base64.decode`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

- `<configuration>`

## `data.encryption`

- `xor.encrypt`

	- `plain_file` : `*`

	- `cipher_file` : `string` ~ `*.bin` = `?default`

	- `key` : `bigint` = `?input`

- `rijndael.encrypt`

	- `plain_file` : `*`

	- `cipher_file` : `string` ~ `*.bin` = `?default`

	- `mode` : `string` = `?input`

	- `block_size` : `bigint` = `?input`

	- `key` : `string` = `?input`

	- `iv` : `string` = `?input`

- `rijndael.decrypt`

	- `cipher_file` : `*`

	- `plain_file` : `string` ~ `*.bin` = `?default`

	- `mode` : `string` = `?input`

	- `block_size` : `bigint` = `?input`

	- `key` : `string` = `?input`

	- `iv` : `string` = `?input`

- `<configuration>`

## `data.compression`

- `deflate.compress`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

- `deflate.uncompress`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

	- `buffer_size` : `string` = `configuration.uncompress_buffer_size`

- `zlib.compress`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

- `zlib.uncompress`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

	- `buffer_size` : `string` = `configuration.uncompress_buffer_size`

- `gzip.compress`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

- `gzip.uncompress`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

	- `buffer_size` : `string` = `configuration.uncompress_buffer_size`

- `bzip2.compress`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

- `bzip2.uncompress`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

	- `buffer_size` : `string` = `configuration.uncompress_buffer_size`

- `lzma.compress`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

- `lzma.uncompress`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

	- `buffer_size` : `string` = `configuration.uncompress_buffer_size`

- `<configuration>`

	- `uncompress_buffer_size` : `string` = `?input`

## `data.differentiation`

- `vcdiff.encode`

	- `before_file` : `*`

	- `after_file` : `string` = `?input`

	- `patch_file` : `string` ~ `<after_file>.patch.bin` = `?default`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `vcdiff.decode`

	- `before_file` : `*`

	- `patch_file` : `string` = `?input`

	- `after_file` : `string` ~ `<patch_file>.after.bin` = `?default`

	- `buffer_size` : `string` = `configuration.decode_buffer_size`

- `<configuration>`

	- `encode_buffer_size` : `string` = `1024.0m`

	- `decode_buffer_size` : `string` = `1024.0m`

## `image.transformation`

- `flip`

	- `source_file` : `*.png`

	- `destination_file` : `string` ~ `*.flip.png` = `?default`

	- `horizontal` : `boolean` = `?input`

	- `vertical` : `boolean` = `?input`

- `scale`

	- `source_file` : `*.png`

	- `destination_file` : `string` ~ `*.scale.png` = `?default`

	- `width` : `bigint` = `?input`

	- `height` : `bigint` = `?input`

- `scale_rate`

	- `source_file` : `*.png`

	- `destination_file` : `string` ~ `*.scale.png` = `?default`

	- `size_rate` : `number` = `?input`

- `<configuration>`

## `image.atlas`

- `pack`

	- `manifest_file` : `*.atlas.json`

	- `sprite_directory` : `string` ~ `*.sprite` = `?default`

	- `atlas_file` : `string` ~ `*.atlas.png` = `?default`

- `unpack`

	- `manifest_file` : `*.atlas.json`

	- `atlas_file` : `string` ~ `*.atlas.png` = `?default`

	- `sprite_directory` : `string` ~ `*.sprite` = `?default`

- `pack_automatic`

	- `sprite_directory` : `*.sprite`

	- `manifest_file` : `string` ~ `*.atlas.json` = `?default`

	- `atlas_file` : `string` ~ `*.atlas.png` = `?default`

- `<configuration>`

## `wwise.media`

- `decode` `*`

	- `ripe_file` : `*.wem`

	- `raw_file` : `string` ~ `*.wav` = `?default`

	- `tool_ffmpeg_program_file` : `string` = `configuration.tool_ffmpeg_program_file`

	- `tool_ww2ogg_program_file` : `string` = `configuration.tool_ww2ogg_program_file`

	- `tool_ww2ogg_code_book_file` : `string` = `configuration.tool_ww2ogg_code_book_file`

- `<configuration>`

	- `tool_ffmpeg_program_file` : `string` = `~/external/ffmpeg/ffmpeg`

	- `tool_ww2ogg_program_file` : `string` = `~/external/ww2ogg/ww2ogg`

	- `tool_ww2ogg_code_book_file` : `string` = `~/external/ww2ogg/packed_codebooks_aoTuV_603.bin`

## `wwise.sound_bank`

- `encode` `*`

	- `bundle_directory` : `*.bnk.bundle`

	- `data_file` : `string` ~ `*.bnk` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.bnk`

	- `bundle_directory` : `string` ~ `*.bnk.bundle` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `<configuration>`

	- `version_number` : `bigint` = `?input`

	- `encode_buffer_size` : `string` = `64.0m`

## `marmalade.dzip`

- `pack`

	- `bundle_directory` : `*.dz.bundle`

	- `data_file` : `string` ~ `*.dz` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.pack_buffer_size`

- `unpack`

	- `data_file` : `*.dz`

	- `bundle_directory` : `string` ~ `*.dz.bundle` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `pack_automatic`

	- `resource_directory` : `*`

	- `data_file` : `string` ~ `*.dz` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `<configuration>`

	- `version_number` : `bigint` = `0`

	- `pack_buffer_size` : `string` = `256.0m`

## `popcap.zlib`

- `compress` `*`

	- `raw_file` : `*`

	- `ripe_file` : `string` ~ `*.bin` = `?default`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

- `uncompress` `*`

	- `ripe_file` : `*`

	- `raw_file` : `string` ~ `*.bin` = `?default`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

- `<configuration>`

	- `version_variant_64` : `boolean` = `?input`

## `popcap.crypt_data`

- `encrypt` `*`

	- `plain_file` : `*`

	- `cipher_file` : `string` ~ `*.cdat` = `?default`

	- `limit` : `bigint` = `configuration.limit`

	- `key` : `string` = `configuration.key`

- `decrypt` `*`

	- `cipher_file` : `*.cdat`

	- `plain_file` : `string` ~ `*` = `?default`

	- `limit` : `bigint` = `configuration.limit`

	- `key` : `string` = `configuration.key`

- `<configuration>`

	- `limit` : `bigint` = `256`

	- `key` : `string` = `AS23DSREPLKL335KO4439032N8345NF`

## `popcap.reflection_object_notation`

- `encode` `*`

	- `value_file` : `*.json`

	- `data_file` : `string` ~ `*.rton` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.rton`

	- `value_file` : `string` ~ `*.json` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

- `encrypt` `*`

	- `plain_file` : `*.rton`

	- `cipher_file` : `string` ~ `*.cipher.rton` = `?default`

	- `key` : `string` = `configuration.key`

- `decrypt` `*`

	- `cipher_file` : `*.rton`

	- `plain_file` : `string` ~ `*.plain.rton` = `?default`

	- `key` : `string` = `configuration.key`

- `encode_then_encrypt` `*`

	- `value_file` : `*.json`

	- `data_file` : `string` ~ `*.rton` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

	- `key` : `string` = `configuration.key`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decrypt_then_decode` `*`

	- `data_file` : `*.rton`

	- `value_file` : `string` ~ `*.json` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

	- `key` : `string` = `configuration.key`

- `decode_lenient`

	- `data_file` : `*.rton`

	- `value_file` : `string` ~ `*.json` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_native_string_encoding_use_utf8` : `boolean` = `configuration.version_native_string_encoding_use_utf8`

- `<configuration>`

	- `version_number` : `bigint` = `1`

	- `version_native_string_encoding_use_utf8` : `boolean` = `true`

	- `key` : `string` = `?input`

	- `encode_buffer_size` : `string` = `64.0m`

## `popcap.texture`

- `encode`

	- `image_file` : `*.png`

	- `data_file` : `string` ~ `*.ptx` = `?default`

	- `format` : `string` = `?input`

- `decode`

	- `data_file` : `*.ptx`

	- `image_file` : `string` ~ `*.png` = `?default`

	- `format` : `string` = `?input`

	- `image_width` : `bigint` = `?input`

	- `image_height` : `bigint` = `?input`

- `<configuration>`

## `popcap.u_texture`

- `encode` `*`

	- `image_file` : `*.png`

	- `data_file` : `string` ~ `*.tex` = `?default`

	- `format` : `string` = `?input`

	- `version_compress_texture_data` : `boolean` = `configuration.version_compress_texture_data`

- `decode` `*`

	- `data_file` : `*.tex`

	- `image_file` : `string` ~ `*.png` = `?default`

	- `version_compress_texture_data` : `boolean` = `configuration.version_compress_texture_data`

- `<configuration>`

	- `version_compress_texture_data` : `boolean` = `?input`

## `popcap.sexy_texture`

- `encode` `*`

	- `image_file` : `*.png`

	- `data_file` : `string` ~ `*.tex` = `?default`

	- `format` : `string` = `?input`

	- `compress_texture_data` : `boolean` = `configuration.encode_compress_texture_data`

	- `version_number` : `bigint` = `configuration.version_number`

- `decode` `*`

	- `data_file` : `*.tex`

	- `image_file` : `string` ~ `*.png` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `<configuration>`

	- `version_number` : `bigint` = `0`

	- `encode_compress_texture_data` : `boolean` = `?input`

## `popcap.animation`

- `encode` `*`

	- `manifest_file` : `*.pam.json`

	- `data_file` : `string` ~ `*.pam` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.pam`

	- `manifest_file` : `string` ~ `*.pam.json` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `convert.flash.from` `*`

	- `raw_file` : `*.pam.json`

	- `ripe_directory` : `string` ~ `*.pam.xfl` = `?default`

- `convert.flash.to` `*`

	- `ripe_directory` : `*.pam.xfl`

	- `raw_file` : `string` ~ `*.pam.json` = `?default`

- `convert.flash.resize` `*`

	- `target_directory` : `*.pam.xfl`

	- `resolution` : `bigint` = `?input`

- `convert.flash.link_media` `*`

	- `target_directory` : `*.pam.xfl`

- `<configuration>`

	- `version_number` : `bigint` = `?input`

	- `encode_buffer_size` : `string` = `8.0m`

## `popcap.re_animation`

- `encode` `*`

	- `manifest_file` : `*.reanim.json`

	- `data_file` : `string` ~ `*.reanim.compiled` = `?default`

	- `version_platform` : `string` = `configuration.version_platform`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.reanim.compiled`

	- `manifest_file` : `string` ~ `*.reanim.json` = `?default`

	- `version_platform` : `string` = `configuration.version_platform`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

- `<configuration>`

	- `version_platform` : `string` = `?input`

	- `version_variant_64` : `boolean` = `?input`

	- `encode_buffer_size` : `string` = `8.0m`

## `popcap.particle`

- `encode` `*`

	- `manifest_file` : `*.particle.json`

	- `data_file` : `string` ~ `*.xml.compiled` = `?default`

	- `version_platform` : `string` = `configuration.version_platform`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.xml.compiled`

	- `manifest_file` : `string` ~ `*.particle.json` = `?default`

	- `version_platform` : `string` = `configuration.version_platform`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

- `<configuration>`

	- `version_platform` : `string` = `?input`

	- `version_variant_64` : `boolean` = `?input`

	- `encode_buffer_size` : `string` = `8.0m`

## `popcap.trail`

- `encode` `*`

	- `manifest_file` : `*.trail.json`

	- `data_file` : `string` ~ `*.trail.compiled` = `?default`

	- `version_platform` : `string` = `configuration.version_platform`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.trail.compiled`

	- `manifest_file` : `string` ~ `*.trail.json` = `?default`

	- `version_platform` : `string` = `configuration.version_platform`

	- `version_variant_64` : `boolean` = `configuration.version_variant_64`

- `<configuration>`

	- `version_platform` : `string` = `?input`

	- `version_variant_64` : `boolean` = `?input`

	- `encode_buffer_size` : `string` = `8.0m`

## `popcap.effect`

- `encode` `*`

	- `manifest_file` : `*.popfx.json`

	- `data_file` : `string` ~ `*.popfx` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.popfx`

	- `manifest_file` : `string` ~ `*.popfx.json` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `<configuration>`

	- `version_number` : `bigint` = `1`

	- `encode_buffer_size` : `string` = `8.0m`

## `popcap.character_font_widget_2`

- `encode` `*`

	- `manifest_file` : `*.cfw2.json`

	- `data_file` : `string` ~ `*.cfw2` = `?default`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode` `*`

	- `data_file` : `*.cfw2`

	- `manifest_file` : `string` ~ `*.cfw2.json` = `?default`

- `<configuration>`

	- `encode_buffer_size` : `string` = `8.0m`

## `popcap.package`

- `pack`

	- `bundle_directory` : `*.pak.bundle`

	- `data_file` : `string` ~ `*.pak` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_compress_resource_data` : `boolean` = `configuration.version_compress_resource_data`

	- `buffer_size` : `string` = `configuration.pack_buffer_size`

- `unpack`

	- `data_file` : `*.pak`

	- `bundle_directory` : `string` ~ `*.pak.bundle` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_compress_resource_data` : `boolean` = `configuration.version_compress_resource_data`

- `pack_automatic`

	- `resource_directory` : `*`

	- `data_file` : `string` ~ `*.pak` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_compress_resource_data` : `boolean` = `configuration.version_compress_resource_data`

- `encrypt`

	- `plain_file` : `*.pak`

	- `cipher_file` : `string` ~ `*.cipher.pak` = `?default`

- `<configuration>`

	- `version_number` : `bigint` = `0`

	- `version_compress_resource_data` : `boolean` = `?input`

	- `pack_buffer_size` : `string` = `256.0m`

## `popcap.resource_stream_group`

- `pack`

	- `bundle_directory` : `*.rsg.bundle`

	- `data_file` : `string` ~ `*.rsg` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.pack_buffer_size`

- `unpack`

	- `data_file` : `*.rsg`

	- `bundle_directory` : `string` ~ `*.rsg.bundle` = `?default`

	- `version_number` : `bigint` = `configuration.version_number`

- `<configuration>`

	- `version_number` : `bigint` = `?input`

	- `pack_buffer_size` : `string` = `256.0m`

## `popcap.resource_stream_bundle`

- `pack`

	- `bundle_directory` : `*.rsb.bundle`

	- `data_file` : `string` ~ `*.rsb` = `?default`

	- `mode` : `string` = `configuration.mode`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_extended_texture_information_for_pvz2_cn` : `bigint` = `configuration.version_extended_texture_information_for_pvz2_cn`

	- `buffer_size` : `string` = `configuration.pack_buffer_size`

	- `input_packet` : `boolean` = `?input`

	- `output_new_packet` : `boolean` = `?input`

- `unpack`

	- `data_file` : `*.rsb`

	- `bundle_directory` : `string` ~ `*.rsb.bundle` = `?default`

	- `mode` : `string` = `configuration.mode`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_extended_texture_information_for_pvz2_cn` : `bigint` = `configuration.version_extended_texture_information_for_pvz2_cn`

	- `output_resource` : `boolean` = `?input`

	- `output_packet` : `boolean` = `?input`

- `resource_convert`

	- `bundle_directory` : `*.rsb.bundle`

	- `version_number` : `bigint` = `configuration.version_number`

	- `version_extended_texture_information_for_pvz2_cn` : `bigint` = `configuration.version_extended_texture_information_for_pvz2_cn`

	- `option` : `{ ... }` = `configuration.resource_convert_option`

- `repair`

	- `raw_file` : `*.rsb`

	- `ripe_file` : `string` ~ `*.repair.rsb` = `?default`

- `<configuration>`

	- `mode` : `string` = `?input`

	- `version_number` : `bigint` = `?input`

	- `version_extended_texture_information_for_pvz2_cn` : `bigint` = `?input`

	- `pack_buffer_size` : `string` = `1.2g`

	- `resource_convert_option` : `{ ... }` = `...`

## `popcap.resource_stream_bundle_patch`

- `encode`

	- `before_file` : `*.rsb`

	- `after_file` : `string` = `?input`

	- `patch_file` : `string` ~ `<after_file>.rsbpatch` = `?default`

	- `use_raw_packet` : `boolean` = `configuration.use_raw_packet`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.encode_buffer_size`

- `decode`

	- `before_file` : `*.rsb`

	- `patch_file` : `string` = `?input`

	- `after_file` : `string` ~ `<patch_file>.rsb` = `?default`

	- `use_raw_packet` : `boolean` = `configuration.use_raw_packet`

	- `version_number` : `bigint` = `configuration.version_number`

	- `buffer_size` : `string` = `configuration.decode_buffer_size`

- `<configuration>`

	- `use_raw_packet` : `boolean` = `?input`

	- `version_number` : `bigint` = `1`

	- `encode_buffer_size` : `string` = `1024.0m`

	- `decode_buffer_size` : `string` = `1024.0m`

## `pvz2.text_table`

- `convert`

	- `source_file` : `*.txt|json`

	- `destination_version` : `string` = `?input`

	- `destination_file` : `string` ~ `*.convert.txt|json` = `?default`

- `<configuration>`

## `pvz2.remote_android_helper`

- `launch`

	- `project_directory` : `*.pvz2_remote_android_helper_project`

	- `action` : `string` = `?input`

- `<configuration>`
