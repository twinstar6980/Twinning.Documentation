# 功能列表

- [介绍](#介绍)

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

## 介绍

工具提供的预置功能一般接受一个文件或文件夹作为输入，并在处理完成后将数据输出至另一个文件或文件夹，默认情况下将输出至同级文件夹中。功能一般分为两种：

* 常规功能
	
	处理单项事务，例如将单个 PopCap RTON 文件解码为 JSON 文件。

* 批处理功能
	
	对某种特定常规功能的包装，接受一个文件夹作为输入并处理其中的所有匹配文件，例如将一个文件夹内的所有 PopCap RTON 文件解码为 JSON 文件。
	
	批处理功能也对一些需要频繁内存申请与释放的常规功能进行了效率优化。

各功能根据性质分为多个功能组，并存放于 `+ <home>/script/Executor/Implement` ，其中的每个 `JS` 文件提供一组功能，与之同名的 `JSON` 文件则保存了该组功能的配置，用户可根据需要修改配置。

用户可以通过命令行参数使工具直接执行某项功能，而无需运行时输入，具体参见 [附加参数](./usage.md#附加参数) 段落。

下面将列出各功能与对应的配置规则，约定格式如下：

> ## `<method-group-id>`
> 
> 功能的组 ID ，作为所有子功能 ID 的前缀。
> 
> * `<method-item-id` [ `*` ]
> 	
> 	功能的项 ID ，与功能的组 ID 组合成功能的 ID 。例如组 ID 为 `popcap.animation` 、项 ID 为 `decode` ，则该功能 ID 为 `popcap.animation.decode` 。如果带有 `*` 标识，则表示该功能提供批处理版本。
> 	
> 	* `variable-name` : `<filter-rule>`
> 		
> 		首项参数是输入参数，由当前命令的 `input` 指定，类型始终为 `string` ；`<filter-rule>` 指定了该功能的输入值过滤规则，如 `*.rsb` 表示只有在输入文件的扩展名为 `rsb` 时才显示该功能。
> 	
> 	* `variable-name` : `variable-type` [ ~ `<default-value>` ] = `<value>`
> 		
> 		剩余的参数则由当前命令的 `argument` 指定，第二项一般是输出参数，其后则为配置参数。
> 		
> 		参数值为字符串 `?input` 时，将在执行时要求用户输入参数。
> 		
>		有些参数可以由程序自动生成，用户可以指定参数值为字符串 `?automatic` 以启用自动生成行为，文档中将以 `~` 做出标识，并在其后跟随自动生成的值。 一般地，输出参数能够以输入参数的值自动生成，例如 `popcap.animation.decode` 功能以 `*.pam` 文件作为输入参数，并自动生成同名的 `*.pam.json` 文件作为输出参数。
> 		
> 		`<value>` 则是当用户未指定参数值时的默认参数值，一般是显式的 `?input` 或 `?automatic` ，或者从对应的配置文件中获取默认参数值。

以下列出的所有功能都是常规功能，部分常规功能存在对应的批处理版本，则会以 `*` 符号标出。功能的批处理版本与常规版本的参数保持一致，但批处理功能的 ID 附加了 `.batch` 后缀，并且输入参数、输出参数分别指向一个用于输入、输出每次处理对象的文件夹。

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
