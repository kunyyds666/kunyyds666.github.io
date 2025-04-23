双向流式对话事件

上行事件

更新对话配置

- **事件类型**：chat.update

- **事件说明**：此事件可以更新当前对话连接的配置项，若更新成功，会收到 chat.updated 的下行事件，否则，会收到 error 下行事件。

- **事件结构**：

| **参数**                                                 |      **类型**       | **是否必选** | **说明**                                                     |
| :------------------------------------------------------- | :-----------------: | ------------ | :----------------------------------------------------------- |
| id                                                       |       String        | 必选         | 客户端自行生成的事件 ID，方便定位问题。                      |
| event_type                                               |       String        | 必选         | 固定为 chat.update。                                         |
| data                                                     |       Object        | 可选         | 事件数据，包含对话配置的详细信息。                           |
| data.chat_config                                         |       Object        | 可选         | 对话配置。                                                   |
| data.chat_config.meta_data                               | Map<String, String> | 可选         | 附加信息，通常用于封装一些业务相关的字段。查看对话消息详情时，系统会透传此附加信息。自定义键值对，应指定为 Map 对象格式。长度为 16 对键值对，其中键（key）的长度范围为 1～64 个字符，值（value）的长度范围为 1～512 个字符。 |
| data.chat_config.custom_variables                        | Map<String, String> | 可选         | 智能体中定义的变量。在智能体 prompt 中设置变量 {{key}} 后，可以通过该参数传入变量值，同时支持 Jinja2 语法。详细说明可参考变量示例。变量名只支持英文字母和下划线。 |
| data.chat_config.extra_params                            | Map<String, String> | 可选         | 附加参数，通常用于特殊场景下指定一些必要参数供模型判断，例如指定经纬度，并询问智能体此位置的天气。自定义键值对格式，其中键（key）仅支持设置为： latitude（纬度，此时值（Value）为纬度值，例如 39.9800718）。 longitude（经度，此时值（Value）为经度值，例如 116.309314）。 |
| data.chat_config.user_id                                 |       String        | 可选         | 标识当前与智能体的用户，由使用方自行定义、生成与维护。user_id 用于标识对话中的不同用户，不同的 user_id，其对话的上下文消息、数据库等对话记忆数据互相隔离。如果不需要用户数据隔离，可将此参数固定为一个任意字符串，例如 123，abc 等。 |
| data.chat_config.conversation_id                         |       String        | 可选         | 标识对话发生在哪一次会话中。会话是智能体和用户之间的一段问答交互。一个会话包含一条或多条消息。对话是会话中对智能体的一次调用，智能体会将对话中产生的消息添加到会话中。可以使用已创建的会话，会话中已存在的消息将作为上下文传递给模型。创建会话的方式可参考创建会话。对于一问一答等不需要区分 conversation 的场合可不传该参数，系统会自动生成一个会话。不传的话会默认创建一个新的 conversation。 |
| data.chat_config.auto_save_history                       |       Boolean       | 可选         | 是否保存本次对话记录。 true：（默认）会话中保存本次对话记录，包括本次对话的模型回复结果、模型执行中间结果。 false：会话中不保存本次对话记录，后续也无法通过任何方式查看本次对话信息、消息详情。在同一个会话中再次发起对话时，本次会话也不会作为上下文传递给模型。 |
| data.chat_config.parameters                              |  Map<String, any>   | 可选         | 设置对话流的输入参数。  对话流的输入参数 USER_INPUT 应在 additional_messages 中传入，在 parameters 中的 USER_INPUT 不生效。  如果 parameters 中未指定 CONVERSATION_NAME 或其他输入参数，则使用参数默认值运行对话流；如果指定了这些参数，则使用指定值。 |
| data.input_audio                                         |       Object        | 可选         | 输入音频格式。                                               |
| data.input_audio.format                                  |       String        | 可选         | 输入音频的格式，支持 pcm、wav、ogg。默认为 wav。             |
| data.input_audio.codec                                   |       String        | 可选         | 输入音频的编码，支持 pcm、opus、g711a、g711u。默认为 pcm。 如果音频编码格式为 g711a 或 g711u，format 请设置为 pcm。 |
| data.input_audio.sample_rate                             |       Integer       | 可选         | 输入音频的采样率，默认 24000。 如果音频编码格式 codec 为 g711a 或 g711u，音频采样率请设置为 8000。 |
| data.input_audio.channel                                 |       Integer       | 可选         | 输入音频的声道数，默认是 1（单声道）。                       |
| data.input_audio.bit_depth                               |       Integer       | 可选         | 输入音频的位深，默认是 16。                                  |
| data.output_audio                                        |       Object        | 可选         | 输出音频格式。                                               |
| data.output_audio.codec                                  |       String        | 可选         | 输出音频编码，支持 pcm、opus。默认是 pcm。                   |
| data.output_audio.pcm_config                             |       Object        | 可选         | 当 codec 设置为 opus 时，不需要设置此字段。 当 codec 设置为 pcm 时，返回的 PCM 数据将固定为单声道，采样深度为 16 位。 |
| data.output_audio.pcm_config.sample_rate                 |       Integer       | 可选         | 输出 pcm 音频的采样率，默认 24000。                          |
| data.output_audio.pcm_config.frame_size_ms               |        Float        | 可选         | 输出每个 pcm 包的时长，单位 ms，默认不限制。                 |
| data.output_audio.pcm_config. limit_config               |       Object        | 可选         | 输出音频限流配置，默认不限制。                               |
| data.output_audio.pcm_config. limit_config.period        |       Integer       | 可选         | 周期的时长，单位为秒。例如设置为 10 秒，则以 10 秒作为一个周期。 |
| data.output_audio.pcm_config. limit_config.max_frame_num |       Integer       | 可选         | 周期内，最大返回 pcm 包数量。                                |
| data.output_audio.opus_config                            |       Object        | 可选         | 当 codec 设置为 pcm 时，不需要设置此字段。                   |
| data.output_audio.opus_config.bitrate                    |       Integer       | 可选         | 输出 opus 的码率，默认 48000。                               |
| data.output_audio.opus_config.use_cbr                    |       Boolean       | 可选         | 输出 opus 是否使用 CBR 编码，默认为 false。                  |
| data.output_audio.opus_config.frame_size_ms              |        Float        | 可选         | 输出 opus 的帧长，默认是 10。可选值： 2.5、5、10、20、40、60 |
| data.output_audio.opus_config.limit_config               |       Object        | 可选         | 输出音频限流配置，默认不限速。                               |
| data.output_audio.opus_config.limit_config.period        |       Integer       | 可选         | 周期的时长，单位为秒。例如设置为 10 秒，则以 10 秒作为一个周期。 |
| data.output_audio.opus_config.limit_config.max_frame_num |       Integer       | 可选         | 周期内最大返回的 Opus 帧数量。                               |
| data.output_audio.speech_rate                            |       Integer       | 可选         | 输出音频的语速，取值范围 [-50, 100]，默认为 0。-50 表示 0.5 倍速，100 表示 2 倍速。 |
| data.output_audio.voice_id                               |       String        | 可选         | 输出音频的音色 ID，默认是柔美女友音色。你可以调用[查看音色列表](https://www.coze.cn/open/docs/developer_guides/list_voices) API 查看当前可用的所有音色 ID。 |
| data.event_subscriptions                                 |    Array<String>    | 可选         | 需要订阅下行事件的事件类型列表。不设置或者设置为空为订阅所有下行事件。 |
| data.need_play_prologue                                  |        bool         | 可选         | 是否需要播放开场白，默认为 false。                           |
| data.turn_detection                                      |       object        | 可选         | 转检测配置。                                                 |
| data.turn_detection.type                                 |       string        | 可选         | 用户演讲检测模式，包括： **server_vad** ：语音数据会传输到服务器端进行实时分析，服务器端的语音活动检测算法会判断用户是否在说话。 **client_interrupt**：（默认）客户端实时分析语音数据，并检测用户是否已停止说话。 |
| data.turn_detection.prefix_padding_ms                    |       Integer       | 可选         | server_vad 模式下，VAD 检测到语音之前要包含的音频量，单位为 ms。默认为 600ms。 |
| data.turn_detection.silence_duration_ms                  |       Integer       | 可选         | server_vad 模式下，检测语音停止的静音持续时间，单位为 ms。默认为 500ms。 |
| data.asr_config                                          |       Object        | 可选         | 语音识别配置，包括热词和上下文信息，以便优化语音识别的准确性和相关性。 |
| data.asr_config.hot_words                                |    Array<String>    | 可选         | 请输入热词列表，以便提升这些词汇的识别准确率。 所有热词加起来最多100个 Tokens，超出部分将自动截断。 |
| data.asr_config.context                                  |       String        | 可选         | 请输入上下文信息。 最多输入 800 个 Tokens，超出部分将自动截断。 |

- **事件示例**：





流式上传音频片段

- **事件类型**：input_audio_buffer.append

- **事件说明**：流式向服务端提交音频的片段。

- **事件结构**：

- 

- **事件示例**：



提交音频

- **事件类型**：input_audio_buffer.complete

- **事件说明**：客户端发送 input_audio_buffer.complete 事件来告诉服务端提交音频缓冲区的数据。服务端提交成功后会返回 input_audio_buffer.completed 事件。在 server_vad 模式下，提交此事件无效。

- **事件结构**：

- 

- **事件示例**：



清除缓冲区音频

- **事件类型**：input_audio_buffer.clear

- **事件说明**：客户端发送 input_audio_buffer.clear 事件来告诉服务端清除缓冲区的音频数据。服务端清除完后将返回 input_audio_buffer.cleared 事件。在 server_vad 模式下，提交此事件无效。

- **事件结构**：

- 

- **事件示例**：



手动提交对话内容

- **事件类型**：conversation.message.create

- **事件说明**：若 role=user，提交事件后就会生成语音回复，适合如下的场景，比如帮我解析 xx 链接，帮我分析这个图片的内容等。若 role=assistant，提交事件后会加入到对话的上下文。

- **事件结构**：

- 

- **事件示例**：



清除上下文

- **事件类型**：conversation.clear

- **事件说明**：清除上下文，会在当前 conversation 下新增一个 section，服务端处理完后会返回 conversation.cleared 事件。

- **事件结构**：

- 

- **事件示例**：

- 

提交端插件执行结果

- **事件类型**：conversation.chat.submit_tool_outputs

- **事件说明**：你可以将需要客户端执行的操作定义为插件，对话中如果触发这个插件，会收到一个 event_type = "conversation.chat.requires_action" 的下行事件，此时需要执行客户端的操作后，通过此上行事件来提交插件执行后的结果。

- **事件结构**：



- **事件示例**：



打断智能体输出

- **事件类型**：conversation.chat.cancel

- **事件说明**：发送此事件可取消正在进行的对话，中断后，服务端将会返回 conversation.chat.canceled 事件。

- **事件结构**：

- 

- **事件示例**：

- 

下行事件

对话连接成功

- **事件类型**：chat.created

- **事件说明**：流式对话接口成功建立连接后服务端会发送此事件。

- **事件结构**：

- 

- 事件示例：



对话配置成功

- **事件类型**：chat.updated

- **事件说明**：对话配置更新成功后，会返回最新的配置。

- **事件结构**：



- **事件示例**：



对话开始

- **事件类型**：conversation.chat.created

- **事件说明**：创建对话的事件，表示对话开始。

- **事件结构**：



- **事件示例**：



对话正在处理 

- **事件类型**：conversation.chat.in_progress

- **事件说明**：服务端正在处理对话。

- **事件结构**：



- **事件示例**：



增量消息

- **事件类型**：conversation.message.delta

- **事件说明**：增量消息，通常是 type=answer 时的增量消息。

- **事件结构**：



- **事件示例**：



增量语音 

- **事件类型**：conversation.audio.delta

- **事件说明**：增量消息，通常是 type=answer 时的增量消息。

- **事件结构**：



- **事件示例**：



消息完成

- **事件类型**：conversation.message.completed

- **事件说明**：消息已回复完成。此时事件中带有所有 message.delta 的拼接结果，且每个消息均为 completed 状态。

- **事件结构**：



- **事件示例**：



语音回复完成

- **事件类型**：conversation.audio.completed

- **事件说明**：音频回复完成。

- **事件结构**：



- **事件示例**：



对话完成

- **事件类型**：conversation.chat.completed

- **事件说明**：表示对话已完成。

- **事件结构**：



- **事件示例**：



对话失败

- **事件类型**：conversation.chat.failed

- **事件说明**：此事件用于标识对话失败。

- **事件结构**：

- 

- **事件示例**：

- 

发生错误

- **事件类型**：error

- **事件说明**：对话过程中的错误事件。

- **事件结构**：

- 

- **事件示例**：

- 

input_audio_buffer 提交成功

- **事件类型**：input_audio_buffer.completed

- **事件说明**：流式提交的音频完成后，返回此事件。

- **事件结构**：

- 

- **事件示例**：

- 

input_audio_buffer 清除成功

- **事件类型**：input_audio_buffer.cleared

- **事件说明**：清除缓冲区音频成功后，返回此事件。

- **事件结构**：

- 

- **事件示例**：

- 

上下文清除完成

- **事件类型**：conversation.cleared

- **事件说明**：清除上下文成功后，返回此事件。

- **事件结构**：

- 

- **事件示例**：

- 

智能体输出中断

- **事件类型**：conversation.chat.canceled

- **事件说明**：客户端提交 conversation.chat.cancel 事件，服务端完成中断后，将返回此事件。

- **事件结构**：

- 

- **事件示例**：



用户语音识别字幕

- **事件类型**：conversation.audio_transcript.update

- **事件说明**：用户语音识别的中间值，每次返回都是全量文本。

- **事件结构**：

- 

- **事件示例**：

- 

用户语音识别完成

- **事件类型**：conversation.audio_transcript.completed

- **事件说明**：用户语音识别完成。

- **事件结构**：

- 

- **事件示例**：

- 

端插件请求

- **事件类型**：conversation.chat.requires_action

- **事件说明**：对话中断，需要使用方上报工具的执行结果。

- **事件结构**：

- 

  <iframe src="https://about:blank/" frameborder="0" class="tb-scrollable-stunt" style="--tw-border-spacing-x: 0; --tw-border-spacing-y: 0; --tw-translate-x: 0; --tw-translate-y: 0; --tw-rotate: 0; --tw-skew-x: 0; --tw-skew-y: 0; --tw-scale-x: 1; --tw-scale-y: 1; --tw-pan-x: ; --tw-pan-y: ; --tw-pinch-zoom: ; --tw-scroll-snap-strictness: proximity; --tw-gradient-from-position: ; --tw-gradient-via-position: ; --tw-gradient-to-position: ; --tw-ordinal: ; --tw-slashed-zero: ; --tw-numeric-figure: ; --tw-numeric-spacing: ; --tw-numeric-fraction: ; --tw-ring-inset: ; --tw-ring-offset-width: 0px; --tw-ring-offset-color: #fff; --tw-ring-color: rgba(59,130,246,.5); --tw-ring-offset-shadow: 0 0 #0000; --tw-ring-shadow: 0 0 #0000; --tw-shadow: 0 0 #0000; --tw-shadow-colored: 0 0 #0000; --tw-blur: ; --tw-brightness: ; --tw-contrast: ; --tw-grayscale: ; --tw-hue-rotate: ; --tw-invert: ; --tw-saturate: ; --tw-sepia: ; --tw-drop-shadow: ; --tw-backdrop-blur: ; --tw-backdrop-brightness: ; --tw-backdrop-contrast: ; --tw-backdrop-grayscale: ; --tw-backdrop-hue-rotate: ; --tw-backdrop-invert: ; --tw-backdrop-opacity: ; --tw-backdrop-saturate: ; --tw-backdrop-sepia: ; -webkit-font-smoothing: antialiased; box-sizing: border-box; font-family: &quot;PingFang SC&quot;, &quot;Noto Sans SC&quot;, sans-serif; outline: none; height: 1732.99px; left: -831.979px; position: absolute; top: -1732.99px; user-select: none; visibility: hidden; width: 831.979px;"></iframe>

  | **参数**                                                     | **类型**            | **是否必选** | **说明**                                                     |
  | ------------------------------------------------------------ | ------------------- | ------------ | ------------------------------------------------------------ |
  | id                                                           | String              | 必选         | 服务端生成的唯一 ID。                                        |
  | event_type                                                   | String              | 必选         | 必填 conversation.chat.requires_action。                     |
  | data                                                         | Object              | 必选         | 事件数据，包含对话的详细信息。                               |
  | data.id                                                      | String              | 必选         | 对话 ID，即对话的唯一标识。                                  |
  | data.conversation_id                                         | String              | 必选         | 会话 ID，即会话的唯一标识。                                  |
  | data.bot_id                                                  | String              | 必选         | 要进行会话聊天的智能体 ID。                                  |
  | data.created_at                                              | Integer             | 可选         | 对话创建的时间。格式为 10 位的 Unixtime 时间戳，单位为秒。   |
  | data.completed_at                                            | Integer             | 可选         | 对话完成的时间。格式为 10 位的 Unixtime 时间戳，单位为秒。   |
  | data.last_error                                              | Object              | 可选         | 对话运行异常时，此字段中返回详细的错误信息，包括：Code：错误码。Integer 类型。0 表示成功，其他值表示失败。Msg：错误信息。String 类型。 |
  | data.meta_data                                               | Map<String, String> | 可选         | 创建消息时的附加消息，用于传入使用方的自定义数据，获取消息时也会返回此附加消息。自定义键值对，应指定为 Map 对象格式。长度为 16 对键值对，其中键（key）的长度范围为 1～64 个字符，值（value）的长度范围为 1～512 个字符。 |
  | data.status                                                  | String              | 可选         | 对话的运行状态。取值为 requires_action，表示对话需要用户操作。 |
  | data.usage                                                   | Object              | 可选         | 对话的 Token 使用情况。                                      |
  | data.usage.token_count                                       | Integer             | 可选         | 本次对话消耗的 Token 总数，包括 input 和 output 部分的消耗。 |
  | data.usage.output_count                                      | Integer             | 可选         | output 部分消耗的 Token 总数。                               |
  | data.usage.input_count                                       | Integer             | 可选         | input 部分消耗的 Token 总数。                                |
  | data.required_action                                         | Object              | 可选         | 需要执行的额外操作。                                         |
  | data.required_action.type                                    | String              | 可选         | 额外操作的类型，枚举值为 submit_tool_outputs。               |
  | data.required_action.submit_tool_outputs                     | Object              | 可选         | 需要提交的结果详情，通过提交接口上传，并可以继续聊天。       |
  | data.required_action.submit_tool_outputs.tool_calls          | Array               | 可选         | 具体上报信息详情。                                           |
  | data.required_action.submit_tool_outputs.tool_calls[].id     | String              | 可选         | 上报运行结果的 ID。                                          |
  | data.required_action.submit_tool_outputs.tool_calls[].type   | String              | 可选         | 工具类型，枚举值为 function。                                |
  | data.required_action.submit_tool_outputs.tool_calls[].function | Object              | 可选         | 执行方法 function 的定义。                                   |
  | data.required_action.submit_tool_outputs.tool_calls[].function.name | String              | 可选         | 方法名。                                                     |
  | data.required_action.submit_tool_outputs.tool_calls[].function.arguments | String              | 可选         | 方法参数。                                                   |
  | detail.logid                                                 | String              | 必选         | 本次请求的日志 ID。如果遇到异常报错场景，且反复重试仍然报错，可以根据此 logid 及错误码联系扣子团队获取帮助。详细说明可参考获取帮助和技术支持。 |

  

- **事件示例**：

- 

  

  {

  ​    "id": "7446668538246561821",

  ​    "event_type": "conversation.chat.requires_action",

  ​    "data": {

  ​        "id": "7434369946098090003",

  ​        "conversation_id": "7434368393420996671",

  ​        "bot_id": "7379165428687536166",

  ​        "created_at": 1730949143,

  ​        "completed_at": 1730949146,

  ​        "last_error": {

  ​            "code": 0,

  ​            "msg": ""

  ​        },

  ​        "status": "requires_action",

  ​        "usage": {

  ​            "token_count": 0,

  ​            "output_count": 0,

  ​            "input_count": 0

  ​        },

  ​        "required_action": {

  ​            "type": "submit_tool_outputs",

  ​            "submit_tool_outputs": {

  ​                "tool_calls": [

  ​                    {

  ​                        "id": "BUJJS0JKQ0dHR0VeFkNCQF5HEEZBXhJFEUJeEhFCRkESSktDREISSUI=",

  ​                        "type": "function",

  ​                        "function": {

  ​                            "name": "get_current_temperature",

  ​                            "arguments":"{\'location\":\'深圳\'}"

  ​                        },

  ​                        "require_info": null

  ​                    }

  ​                ]

  ​            }

  ​        },

  ​        "section_id": "7434368393420996671"

  ​    },

  ​    "detail": {

  ​        "logid": "20241210152726467C48D89D6DB2F3***"

  ​    }

  }

  

用户开始说话

- **事件类型**：input_audio_buffer.speech_started

- **事件说明**：此事件表示服务端识别到用户正在说话。只有在 server_vad 模式下，才会返回此事件。

- **事件结构**：

- 

- **事件示例**：



用户结束说话

- **事件类型**：input_audio_buffer.speech_stopped

- **事件说明**：此事件表示服务端识别到用户已停止说话。只有在 server_vad 模式下，才会返回此事件。

- **事件结构**：

- 

- **事件示例**：





















