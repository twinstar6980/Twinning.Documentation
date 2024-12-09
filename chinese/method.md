# 功能列表

- [介绍](#介绍)

- [script.js](#scriptjs)

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

- [kairosoft.game](#kairosoftgame)

## 介绍

工具提供的预置功能一般接受一个文件或目录作为输入，并在处理完成后将数据输出至另一个文件或目录，默认情况下将输出至同级目录中。功能一般分为两种：

* 常规功能
	
	处理单项事务，例如将单个 PopCap RTON 文件解码为 JSON 文件。

* 批处理功能
	
	对某种特定常规功能的包装，接受一个目录作为输入并处理其中的所有匹配文件，例如将一个目录内的所有 PopCap RTON 文件解码为 JSON 文件。
	
	批处理功能也对一些需要频繁内存申请与释放的常规功能进行了效率优化。

各功能根据性质分为多个功能组，并存放于 `<home>/script/Executor/Implement` ，其中的每个 `JS` 文件提供一组功能，与之同名的 `JSON` 文件则保存了该组功能的配置，用户可根据需要修改配置。

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
> 		首项参数是输入参数，由当前命令的 `input` 或 `argument` 指定，类型始终为 `path` ；`<filter-rule>` 指定了该功能的输入值过滤规则，如 `*.rsb` 表示只有在输入文件的扩展名为 `rsb` 时才显示该功能；可以在配置文件中修改过滤规则。
> 	
> 	* `variable-name` : `variable-type` [ ~ `<default-value>` ] = `<value>`
> 		
> 		剩余的参数则由当前命令的 `argument` 指定，第二项一般是输出参数，其后则为配置参数。
> 		
> 		参数值为字符串 `?input` 时，将在执行时要求用户输入参数。
> 		
>		有些参数可以由程序自动生成，用户可以指定参数值为字符串 `?automatic` 以启用自动生成行为，文档中将以 `~` 做出标识，并在其后跟随自动生成的值。 一般地，输出参数能够以输入参数的值自动生成，例如 `popcap.animation.decode` 功能以 `*.pam` 文件作为输入参数，并自动生成同名的 `*.pam.json` 文件作为输出参数。
> 		
> 		`<value>` 则是当用户未指定参数值时的默认值，一般是显式的 `?input` 或 `?automatic` ，可以在配置文件中修改默认值。

以下列出的所有功能都是常规功能，部分常规功能存在对应的批处理版本，则会以 `*` 符号标出。功能的批处理版本与常规版本的参数保持一致，但批处理功能的 ID 附加了 `.batch` 后缀，并且输入参数、输出参数分别指向一个用于输入、输出每次处理对象的目录。

## `script.js`

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
	
	* `key` : `string` = `?input`

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
	
	* `version_number` : `integer` = `0`

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

* `encode_cipher` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `path` ~ `*.rton` = `?automatic`
	
	* `version_number` : `integer` = `1`
	
	* `version_native_string_encoding_use_utf8` : `boolean` = `true`
	
	* `key` : `string` = `?input`
	
	* `buffer_size` : `size` = `64.0m`

* `decode_cipher` `*`
	
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
	
	* `version_number` : `integer` = `?input`

* `convert.flash.to` `*`
	
	* `ripe_directory` : `*.pam.xfl`
	
	* `raw_file` : `path` ~ `*.pam.json` = `?automatic`
	
	* `version_number` : `integer` = `?input`

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

* `unpack_lenient` `*`
	
	* `data_file` : `*.rsb`
	
	* `bundle_directory` : `path` ~ `*.rsb.bundle` = `?automatic`

* `resource_convert` `*`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `option_recase_path` : `boolean` = `?input`
	
	* `option_rton` : `boolean` = `?input`
	
	* `option_rton_version_number` : `integer` = `1`
	
	* `option_rton_version_native_string_encoding_use_utf8` : `boolean` = `true`
	
	* `option_rton_crypt` : `boolean` = `?input`
	
	* `option_rton_crypt_key` : `string` = `?input`
	
	* `option_ptx` : `boolean` = `?input`
	
	* `option_ptx_format` : `string` = `?input`
	
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

* `transpile`
	
	* `project_directory` : `*.pvz2_package_project`
	
	* `target_scope` : `string` = `?input`
	
	* `target_package` : `string` = `?input`
	
	* `option_generalize_rton` : `boolean` = `?input`
	
	* `option_generalize_ptx` : `boolean` = `?input`
	
	* `option_generalize_pam` : `boolean` = `?input`
	
	* `option_generalize_wem` : `boolean` = `?input`
	
	* `option_specialize_rton` : `boolean` = `?input`
	
	* `option_specialize_ptx` : `boolean` = `?input`
	
	* `option_specialize_pam` : `boolean` = `?input`
	
	* `option_specialize_wem` : `boolean` = `?input`
	
	* `buffer_size` : `size` = `8.0m`

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

## `kairosoft.game`

* `encrypt_record`
	
	* `target_directory` : `*`
	
	* `key` : `string` = `?input`

* `modify_program`
	
	* `target_directory` : `*`
	
	* `disable_record_encryption` : `boolean` = `?input`
	
	* `enable_debug_mode` : `boolean` = `?input`
