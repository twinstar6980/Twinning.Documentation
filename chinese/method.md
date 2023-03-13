# 功能

- [介绍](#介绍)

- [通用配置](#通用配置)

- [js](#js)

- [json](#json)

- [data.hash](#datahash)

- [data.encoding](#dataencoding)

- [data.encryption](#dataencryption)

- [data.compression](#datacompression)

- [data.differentiation](#datadifferentiation)

- [image.transformation](#imagetransformation)

- [image.atlas](#imageatlas)

- [wwise.encoded_media](#wwiseencoded_media)

- [wwise.sound_bank](#wwisesound_bank)

- [marmalade.dzip](#marmaladedzip)

- [popcap.zlib](#popcapzlib)

- [popcap.reanim](#popcapreanim)

- [popcap.particle](#popcapparticle)

- [popcap.rton](#popcaprton)

- [popcap.ptx](#popcapptx)

- [popcap.pam](#popcappam)

- [popcap.pak](#popcappak)

- [popcap.rsgp](#popcaprsgp)

- [popcap.rsb](#popcaprsb)

- [popcap.rsb_patch](#popcaprsb_patch)

- [pvz2.lawn_string_text](#pvz2lawn_string_text)

- [pvz2.remote_android_helper](#pvz2remote_android_helper)

## 介绍

工具提供的预置功能一般接受一个文件或目录作为输入，并在处理完成后将数据输出至另一个文件或目录，默认情况下将输出至同级目录中。功能一般分为两种：

* 常规功能
	
	处理单项事务，例如将单个 PopCap RTON 文件解码为 JSON 文件。

* 批处理功能
	
	对某种特定常规功能的包装，接受一个目录作为输入并处理其中的所有匹配文件，例如将一个目录内的所有 PopCap RTON 文件解码为 JSON 文件。
	
	批处理功能也对一些需要频繁内存申请与释放的常规功能进行了效率优化。

各功能根据性质分为多个功能组，并存放于 `+ <home>/script/Entry/method` ，其中的每个 `JS` 文件提供一组功能，与之同名的 `JSON` 文件则保存了该组功能的配置，用户可根据需要修改配置。

用户可以通过命令行参数使工具直接执行某项功能，而无需运行时输入，具体参见 [附加参数](./usage.md#附加参数) 段落。

下面将列出各功能与对应的配置规则，约定格式如下：

> ## `<method-group-id>`
> 
> 功能的组 ID ，作为所有子功能 ID 的前缀。
> 
> * `<method-item-id` [ `*` ]
> 	
> 	功能的项 ID ，与功能的组 ID 组合成功能的 ID 。例如组 ID 为 `popcap.rton` 、项 ID 为 `decode` ，则该功能 ID 为 `popcap.rton.decode` 。如果带有 `*` 标识，则表示该功能提供批处理版本。
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
>		有些参数存在默认行为，用户可以指定参数值为字符串 `?default` 以启用默认行为，文档中将以 `~` 做出标识，并在其后跟随默认行为的取值。 一般地，输出参数具有默认行为，即以输入参数的值生成输出参数的值，例如 `popcap.rton.decode` 功能以 `*.rton` 文件作为输入参数，由此生成同名的 `*.json` 文件作为输出参数。
> 		
> 		`<value>` 则是当用户未指定参数值时的默认参数值，一般是显式的 `?input` 或 `?default` ，或者从对应的配置文件中获取默认参数值。

下面列出的所有功能都是常规功能，部分常规功能存在对应的批处理版本，批处理版本的功能与常规功能保持一致，但功能 ID 附加了 `.batch` 后缀，并且输入参数、输出参数附加了 `_directory` 后缀。

例如，以下是 `popcap.rton.encode` 功能的定义：

> * `encode` `*`
> 	
> 	* `value_file` : `*.json`
> 	
> 	* `data_file` : `string` ~ `*.rton` = `?default`
> 	
> 	* `version_number` : `bigint` = `config.version_number`
> 	
> 	* `buffer_size` : `string` = `config.encode_buffer_size`

则对应的批处理版本如下：

> * `encode.batch`
> 	
> 	* `value_file_directory` : `*`
> 	
> 	* `data_file_directory` : `string` ~ `*.rton_encode` = `?default`
> 	
> 	* `version_number` : `bigint` = `config.version_number`
> 	
> 	* `buffer_size` : `string` = `config.encode_buffer_size`

## 通用配置

`- <home>/script/Entry/Entry.json` 定义了工具运行期间的通用配置。

* `<config>`

	* `language` : `string` = `Chinese`
		
		交互语言。可以为 `Chinese` 或 `English` 。
	
	* `cli_disable_virtual_terminal_sequence` : `boolean` = `false`
		
		禁用命令行虚拟终端序列。仅当使用命令行外壳时有效。
	
	* `byte_stream_use_big_endian` : `boolean` = `false`
		
		内部字节流操作时使用大端序。这个选项对一些大端序文件的处理是必要的。默认禁用，因为大多数时候用户处理的都是小端序文件。
	
	* `common_buffer_size` : `string` = `64.0m`
		
		公用缓存区大小。用于编码 JSON 字符串、编码 PNG 图像等，如果编码时所需的内存用量超过这一大小，工具将处理失败。
	
	* `json_format` : `{ ... }`
		
		JSON 输出格式。
		
		* `disable_trailing_comma` : `boolean` = `false`
			
			禁用尾随逗号。
		
		* `disable_array_wrap_line` : `boolean` = `false`
			
			禁用数组元素间的换行。
	
	* `pause_when_finish` : `boolean` = `true`
		
		在完成所有工作后暂停程序，等待用户关闭。
	
	* `thread_limit` : `bigint` = `0`
		
		线程池上限数。目前无实际作用。

## `js`

* `execute`
	
	* `script_file` : `*.js`

## `json`

* `format` `*`
	
	* `source_file` : `*.json`
	
	* `destination_file` : `string` ~ `*.formatted.json` = `?default`
	
	* `disable_trailing_comma` : `boolean` ~ `CoreX.JSON.g_format.disable_trailing_comma` = `config.disable_trailing_comma`
	
	* `disable_array_wrap_line` : `boolean` ~ `CoreX.JSON.g_format.disable_array_wrap_line` = `config.disable_array_wrap_line`

* `<config>`
	
	* `disable_trailing_comma` : `boolean` = `?input`
	
	* `disable_array_wrap_line` : `boolean` = `?input`

## `data.hash`

* `md5`
	
	* `target_file` : `*`

* `<config>`

## `data.encoding`

* `base64.encode`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?default`

* `base64.decode`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?default`

* `<config>`

## `data.encryption`

* `xor.encrypt`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `string` ~ `*.bin` = `?default`
	
	* `key` : `bigint` = `?input`

* `rijndael.encrypt`
	
	* `plain_file` : `*`
	
	* `cipher_file` : `string` ~ `*.bin` = `?default`
	
	* `mode` : `string` = `?input`
	
	* `block_size` : `bigint` = `?input`
	
	* `key` : `string` = `?input`
	
	* `iv` : `string` = `?input`

* `rijndael.decrypt`
	
	* `cipher_file` : `*`
	
	* `plain_file` : `string` ~ `*.bin` = `?default`
	
	* `mode` : `string` = `?input`
	
	* `block_size` : `bigint` = `?input`
	
	* `key` : `string` = `?input`
	
	* `iv` : `string` = `?input`

* `<config>`

## `data.compression`

* `deflate.compress`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?default`

* `deflate.uncompress`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?default`
	
	* `buffer_size` : `string` = `config.uncompress_buffer_size`

* `zlib.compress`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?default`

* `zlib.uncompress`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?default`
	
	* `buffer_size` : `string` = `config.uncompress_buffer_size`

* `gzip.compress`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?default`

* `gzip.uncompress`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?default`
	
	* `buffer_size` : `string` = `config.uncompress_buffer_size`

* `bzip2.compress`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?default`

* `bzip2.uncompress`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?default`
	
	* `buffer_size` : `string` = `config.uncompress_buffer_size`

* `<config>`
	
	* `uncompress_buffer_size` : `string` = `?input`

## `data.differentiation`

* `vcdiff.encode`
	
	* `before_file` : `*`
	
	* `after_file` : `string` = `?input`
	
	* `patch_file` : `string` ~ `<after_file>.patch.bin` = `?default`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `vcdiff.decode`
	
	* `before_file` : `*`
	
	* `patch_file` : `string` = `?input`
	
	* `after_file` : `string` ~ `<patch_file>.after.bin` = `?default`
	
	* `buffer_size` : `string` = `config.decode_buffer_size`

* `<config>`
	
	* `encode_buffer_size` : `string` = `1024.0m`
	
	* `decode_buffer_size` : `string` = `1024.0m`

## `image.transformation`

* `flip`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `string` ~ `*.flip.png` = `?default`
	
	* `horizontal` : `boolean` = `?input`
	
	* `vertical` : `boolean` = `?input`

* `scale`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `string` ~ `*.scale.png` = `?default`
	
	* `width` : `bigint` = `?input`
	
	* `height` : `bigint` = `?input`

* `scale_rate`
	
	* `source_file` : `*.png`
	
	* `destination_file` : `string` ~ `*.scale.png` = `?default`
	
	* `size_rate` : `number` = `?input`

* `<config>`

## `image.atlas`

* `pack`
	
	* `manifest_file` : `*.atlas.json`
	
	* `sprite_directory` : `string` ~ `*.sprite` = `?default`
	
	* `atlas_file` : `string` ~ `*.atlas.png` = `?default`

* `unpack`
	
	* `manifest_file` : `*.atlas.json`
	
	* `atlas_file` : `string` ~ `*.atlas.png` = `?default`
	
	* `sprite_directory` : `string` ~ `*.sprite` = `?default`

* `pack_automatic`
	
	* `sprite_directory` : `*.sprite`
	
	* `manifest_file` : `string` ~ `*.atlas.json` = `?default`
	
	* `atlas_file` : `string` ~ `*.atlas.png` = `?default`

* `<config>`

## `wwise.encoded_media`

* `decode` `*`
	
	* `ripe_file` : `*.wem`
	
	* `raw_file` : `string` ~ `*.wav` = `?default`
	
	* `tool_ffmpeg_program_file` : `string` = `config.tool_ffmpeg_program_file`
	
	* `tool_ww2ogg_program_file` : `string` = `config.tool_ww2ogg_program_file`
	
	* `tool_ww2ogg_code_book_file` : `string` = `config.tool_ww2ogg_code_book_file`

* `<config>`
	
	* `tool_ffmpeg_program_file` : `string` = `~/external/ffmpeg/ffmpeg`
	
	* `tool_ww2ogg_program_file` : `string` = `~/external/ww2ogg/ww2ogg`
	
	* `tool_ww2ogg_code_book_file` : `string` = `~/external/ww2ogg/packed_codebooks_aoTuV_603.bin`

## `wwise.sound_bank`

* `encode` `*`
	
	* `bundle_directory` : `*.bnk.bundle`
	
	* `data_file` : `string` ~ `*.bnk` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.bnk`
	
	* `bundle_directory` : `string` ~ `*.bnk.bundle` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`

* `<config>`
	
	* `version_number` : `bigint` = `?input`
	
	* `encode_buffer_size` : `string` = `64.0m`

## `marmalade.dzip`

* `pack`
	
	* `bundle_directory` : `*.dz.bundle`
	
	* `data_file` : `string` ~ `*.dz` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.pack_buffer_size`

* `unpack`
	
	* `data_file` : `*.dz`
	
	* `bundle_directory` : `string` ~ `*.dz.bundle` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`

* `pack_automatic`
	
	* `resource_directory` : `*`
	
	* `data_file` : `string` ~ `*.dz` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`

* `<config>`
	
	* `version_number` : `bigint` = `0`
	
	* `pack_buffer_size` : `string` = `256.0m`

## `popcap.zlib`

* `compress` `*`
	
	* `raw_file` : `*`
	
	* `ripe_file` : `string` ~ `*.bin` = `?default`
	
	* `version_variant_64` : `boolean` = `config.version_variant_64`

* `uncompress` `*`
	
	* `ripe_file` : `*`
	
	* `raw_file` : `string` ~ `*.bin` = `?default`
	
	* `version_variant_64` : `boolean` = `config.version_variant_64`

* `<config>`
	
	* `version_variant_64` : `boolean` = `?input`

## `popcap.reanim`

* `encode` `*`
	
	* `manifest_file` : `*.reanim.json`
	
	* `data_file` : `string` ~ `*.reanim.compiled` = `?default`
	
	* `version_platform` : `string` = `config.version_platform`
	
	* `version_variant_64` : `boolean` = `config.version_variant_64`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.reanim.compiled`
	
	* `manifest_file` : `string` ~ `*.reanim.json` = `?default`
	
	* `version_platform` : `string` = `config.version_platform`
	
	* `version_variant_64` : `boolean` = `config.version_variant_64`

* `<config>`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.particle`

* `encode` `*`
	
	* `manifest_file` : `*.particle.json`
	
	* `data_file` : `string` ~ `*.xml.compiled` = `?default`
	
	* `version_platform` : `string` = `config.version_platform`
	
	* `version_variant_64` : `boolean` = `config.version_variant_64`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.xml.compiled`
	
	* `manifest_file` : `string` ~ `*.particle.json` = `?default`
	
	* `version_platform` : `string` = `config.version_platform`
	
	* `version_variant_64` : `boolean` = `config.version_variant_64`

* `<config>`
	
	* `version_platform` : `string` = `?input`
	
	* `version_variant_64` : `boolean` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.rton`

* `encode` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `string` ~ `*.rton` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `string` ~ `*.json` = `?default`
	
	* `native_string_encoding_use_extended_ascii` : `boolean` = `config.native_string_encoding_use_extended_ascii`
	
	* `version_number` : `bigint` = `config.version_number`

* `encrypt` `*`
	
	* `plain_file` : `*.rton`
	
	* `cipher_file` : `string` ~ `*.cipher.rton` = `?default`
	
	* `key` : `string` = `config.key`

* `decrypt` `*`
	
	* `cipher_file` : `*.rton`
	
	* `plain_file` : `string` ~ `*.plain.rton` = `?default`
	
	* `key` : `string` = `config.key`

* `encode_then_encrypt` `*`
	
	* `value_file` : `*.json`
	
	* `data_file` : `string` ~ `*.rton` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `key` : `string` = `config.key`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decrypt_then_decode` `*`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `string` ~ `*.json` = `?default`
	
	* `native_string_encoding_use_extended_ascii` : `boolean` = `config.native_string_encoding_use_extended_ascii`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `key` : `string` = `config.key`

* `decode_lenient`
	
	* `data_file` : `*.rton`
	
	* `value_file` : `string` ~ `*.json` = `?default`
	
	* `native_string_encoding_use_extended_ascii` : `boolean` = `config.native_string_encoding_use_extended_ascii`
	
	* `version_number` : `bigint` = `config.version_number`

* `<config>`
	
	* `version_number` : `bigint` = `1`
	
	* `native_string_encoding_use_extended_ascii` : `boolean` = `false`
	
	* `key` : `string` = `?input`
	
	* `encode_buffer_size` : `string` = `64.0m`

## `popcap.ptx`

* `encode`
	
	* `image_file` : `*.png`
	
	* `data_file` : `string` ~ `*.ptx` = `?default`
	
	* `format` : `string` = `?input`

* `decode`
	
	* `data_file` : `*.ptx`
	
	* `image_file` : `string` ~ `*.png` = `?default`
	
	* `format` : `string` = `?input`
	
	* `image_width` : `bigint` = `?input`
	
	* `image_height` : `bigint` = `?input`

* `<config>`

## `popcap.pam`

* `encode` `*`
	
	* `manifest_file` : `*.pam.json`
	
	* `data_file` : `string` ~ `*.pam` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decode` `*`
	
	* `data_file` : `*.pam`
	
	* `manifest_file` : `string` ~ `*.pam.json` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`

* `convert.flash.from` `*`
	
	* `raw_file` : `*.pam.json`
	
	* `ripe_directory` : `string` ~ `*.pam.xfl` = `?default`

* `convert.flash.to` `*`
	
	* `ripe_directory` : `*.pam.xfl`
	
	* `raw_file` : `string` ~ `*.pam.json` = `?default`

* `convert.flash.resize` `*`
	
	* `target_directory` : `*.pam.xfl`
	
	* `resolution` : `bigint` = `?input`

* `convert.flash.link_media` `*`
	
	* `target_directory` : `*.pam.xfl`

* `<config>`
	
	* `version_number` : `bigint` = `?input`
	
	* `encode_buffer_size` : `string` = `8.0m`

## `popcap.pak`

* `pack`
	
	* `bundle_directory` : `*.pak.bundle`
	
	* `data_file` : `string` ~ `*.pak` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `version_compress_resource_data` : `boolean` = `config.version_compress_resource_data`
	
	* `buffer_size` : `string` = `config.pack_buffer_size`

* `unpack`
	
	* `data_file` : `*.pak`
	
	* `bundle_directory` : `string` ~ `*.pak.bundle` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `version_compress_resource_data` : `boolean` = `config.version_compress_resource_data`

* `pack_automatic`
	
	* `resource_directory` : `*`
	
	* `data_file` : `string` ~ `*.pak` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `version_compress_resource_data` : `boolean` = `config.version_compress_resource_data`

* `encrypt`
	
	* `plain_file` : `*.pak`
	
	* `cipher_file` : `string` ~ `*.cipher.pak` = `?default`

* `<config>`
	
	* `version_number` : `bigint` = `0`
	
	* `version_compress_resource_data` : `boolean` = `?input`
	
	* `pack_buffer_size` : `string` = `256.0m`

## `popcap.rsgp`

* `pack`
	
	* `bundle_directory` : `*.rsgp.bundle`
	
	* `data_file` : `string` ~ `*.rsgp` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.pack_buffer_size`

* `unpack`
	
	* `data_file` : `*.rsgp`
	
	* `bundle_directory` : `string` ~ `*.rsgp.bundle` = `?default`
	
	* `version_number` : `bigint` = `config.version_number`

* `<config>`
	
	* `version_number` : `bigint` = `?input`
	
	* `pack_buffer_size` : `string` = `256.0m`

## `popcap.rsb`

* `pack`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `data_file` : `string` ~ `*.rsb` = `?default`
	
	* `mode` : `string` = `config.mode`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `version_additional_texture_information_for_pvz_2_chinese_android` : `bigint` = `config.version_additional_texture_information_for_pvz_2_chinese_android`
	
	* `buffer_size` : `string` = `config.pack_buffer_size`
	
	* `input_packet` : `boolean` = `?input`
	
	* `output_new_packet` : `boolean` = `?input`

* `unpack`
	
	* `data_file` : `*.rsb`
	
	* `bundle_directory` : `string` ~ `*.rsb.bundle` = `?default`
	
	* `mode` : `string` = `config.mode`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `version_additional_texture_information_for_pvz_2_chinese_android` : `bigint` = `config.version_additional_texture_information_for_pvz_2_chinese_android`
	
	* `output_resource` : `boolean` = `?input`
	
	* `output_packet` : `boolean` = `?input`

* `resource_convert`
	
	* `bundle_directory` : `*.rsb.bundle`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `version_additional_texture_information_for_pvz_2_chinese_android` : `bigint` = `config.version_additional_texture_information_for_pvz_2_chinese_android`
	
	* `option` : `{ ... }` = `config.resource_convert_option`

* `repair`
	
	* `raw_file` : `*.rsb`
	
	* `ripe_file` : `string` ~ `*.repair.rsb` = `?default`

* `<config>`
	
	* `mode` : `string` = `?input`
	
	* `version_number` : `bigint` = `?input`
	
	* `version_additional_texture_information_for_pvz_2_chinese_android` : `bigint` = `?input`
	
	* `pack_buffer_size` : `string` = `256.0m`
	
	* `resource_convert_option` : `{ ... }` = `...`

## `popcap.rsb_patch`

* `encode`
	
	* `before_file` : `*.rsb`
	
	* `after_file` : `string` = `?input`
	
	* `patch_file` : `string` ~ `<after_file>.rsbpatch` = `?default`
	
	* `use_raw_packet` : `boolean` = `config.use_raw_packet`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.encode_buffer_size`

* `decode`
	
	* `before_file` : `*.rsb`
	
	* `patch_file` : `string` = `?input`
	
	* `after_file` : `string` ~ `<patch_file>.rsb` = `?default`
	
	* `use_raw_packet` : `boolean` = `config.use_raw_packet`
	
	* `version_number` : `bigint` = `config.version_number`
	
	* `buffer_size` : `string` = `config.decode_buffer_size`

* `<config>`
	
	* `use_raw_packet` : `boolean` = `?input`
	
	* `version_number` : `bigint` = `1`
	
	* `encode_buffer_size` : `string` = `1024.0m`
	
	* `decode_buffer_size` : `string` = `1024.0m`

## `pvz2.lawn_string_text`

* `convert`
	
	* `source_file` : `*.LawnStrings.txt|json`
	
	* `destination_version` : `string` = `?input`
	
	* `destination_file` : `string` ~ `*.LawnStrings.converted.txt|map.json|list.json` = `?default`

* `<config>`

## `pvz2.remote_android_helper`

* `launch`
	
	* `project_directory` : `*.pvz2_remote_android_helper_project`
	
	* `action` : `string` = `?input`

* `<config>`

