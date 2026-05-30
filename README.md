# Firefox 指纹浏览器

> 专门用于 https://github.com/LoseNine/ruyipage 自动化的过检测浏览器内核

基于 Firefox 源码修改的本地指纹浏览器，通过读取本地配置文件自定义浏览器指纹信息，实现多账号隔离与反指纹检测。

> 支持 Windows / Linux 平台

下载请前往 GitHub release 页面

微信：`Charleval`

---

## 功能特性

- 自定义 WebRTC 本地/公网 IP
- 自定义 UserAgent、时区、语言和字体系统
- 自定义 SpeechSynthesis 本地/远程语音列表
- 自定义屏幕尺寸、Canvas 噪声、硬件并发数
- 自定义 WebGL / WebGL2 身份、能力上限、扩展、Shader 精度
- 自定义 WebGPU 适配器身份、features、WGSL language features 和 limits
- 自定义触控、pointer、hover 画像
- 支持 HTTP / SOCKS5 密码代理认证
- 支持单 userContext / 单 tab 代理池轮换
- WebDriver 检测屏蔽
- 多开互不干扰，每个实例独立指纹

---

## 指纹配置文件说明

创建一个纯文本文件，例如 `profile1.txt` 或 `fp.txt`。有效配置行支持 `key:value` 或 `key=value` 两种写法；以 `#` 开头的是注释，不参与 Firefox 指纹内核解析。

下面示例基于根目录 `fp.txt` 整理，已经把真实 IP、代理账号、密码、会话号等敏感信息替换为占位值。公开文档不要提交真实代理凭据。

```bash
# [WebRTC IP 指纹]
# 替换 WebRTC ICE candidate 暴露的本地/公网 IP，避免泄漏真实出口 IP。
# IPv6 不需要时可以删除 IPv6 行或留空。
local_webrtc_ipv4=203.0.113.181
local_webrtc_ipv6=2001:db8:102:1523::158
public_webrtc_ipv4=203.0.113.181
public_webrtc_ipv6=2001:db8:102:1523::158

# [时区和语言指纹]
# timezone 使用 IANA 时区 ID；language 使用 BCP47 语言标签，多个用英文逗号分隔。
timezone:Asia/Tokyo
language:ja-JP,ja

# [SpeechSynthesis 语音指纹]
# voices 使用 | 分隔多个名称；langs 使用 | 分隔语言，数量和顺序要与 voices 对齐。
speech.voices.local: Microsoft Haruka Desktop - Japanese|Microsoft Ichiro Desktop - Japanese|Microsoft Ayumi - Japanese (Japan)
speech.voices.remote: Google Japanese|Google Japanese (Japan)
speech.voices.local.langs: ja-JP|ja-JP|ja-JP
speech.voices.remote.langs: ja-JP|ja-JP
speech.voices.default.name: Microsoft Haruka Desktop - Japanese
speech.voices.default.lang: ja-JP

# [字体、User-Agent、CPU 指纹]
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:119.0) Gecko/20100101 Firefox/147.0
hardwareConcurrency:32

# [屏幕尺寸指纹]
width:551
height:500

# [Canvas 指纹]
# 整数种子；同一账号建议固定，不同账号建议不同。
canvas:12

# [HTTP 认证指纹/代理密码]
# 不用 HTTP 密码代理时可删除这两行。
httpauth.username:REPLACE_WITH_HTTP_USER
httpauth.password:REPLACE_WITH_HTTP_PASSWORD

# [SOCKS5 认证]
# 为指定 SOCKS5 host:port 提供用户名和密码，避免浏览器弹认证框。
socksauth.host=gate.example.com
socksauth.port=1000
socksauth.username=REPLACE_WITH_SOCKS_USER
socksauth.password=REPLACE_WITH_SOCKS_PASSWORD

# [单 userContext / 单 tab 轮换代理]
# proxy.rotate.enabled 开启后，不同 userContext/tab 会按顺序分配代理池条目。
# proxy.rotate.exhausted=wrap 表示列表用完后从头循环。
proxy.rotate.enabled=true
proxy.rotate.exhausted=wrap
proxy.rotate.proxy=socks5://gate.example.com:1000:REPLACE_WITH_USER:REPLACE_WITH_PASSWORD_US_00000001_5m
proxy.rotate.proxy=socks5://gate.example.com:1000:REPLACE_WITH_USER:REPLACE_WITH_PASSWORD_US_00000002_5m
proxy.rotate.proxy=socks5://gate.example.com:1000:REPLACE_WITH_USER:REPLACE_WITH_PASSWORD_US_00000003_5m

# [WebGL 基础身份指纹]
# renderer、unmasked_renderer、WebGPU device 要互相匹配。
webgl.vendor:Google Inc. (AMD)
webgl.renderer:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.version:WebGL 1.0 (OpenGL ES 2.0 Chromium)
webgl.glsl_version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)
webgl.unmasked_vendor:Google Inc. (AMD)
webgl.unmasked_renderer:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D11 vs_5_0 ps_5_0, D3D11)

# [WebGL 能力上限指纹]
# 建议使用真实 GPU/浏览器采集值，不要随意混用不同显卡档位。
webgl.max_texture_size:16384
webgl.max_cube_map_texture_size:16384
webgl.max_texture_image_units:32
webgl.max_vertex_attribs:16
webgl.aliased_point_size_max:1024
webgl.max_viewport_dim:16384
webgl.max_combined_texture_image_units:32
webgl.max_vertex_texture_image_units:32
webgl.max_renderbuffer_size:16384
webgl.max_vertex_uniform_vectors:4096
webgl.max_fragment_uniform_vectors:4096
webgl.max_varying_vectors:31
webgl.aliased_point_size_min:1
webgl.aliased_line_width_min:1
webgl.aliased_line_width_max:1
webgl.max_anisotropy:16
webgl.max_draw_buffers:8
webgl.max_color_attachments:8
webgl.max_3d_texture_size:2048
webgl.max_array_texture_layers:2048
webgl.max_uniform_buffer_bindings:90
webgl.uniform_buffer_offset_alignment:256
webgl.max_samples:4

# [WebGL 压缩纹理、Shader 精度、扩展列表]
webgl.compressed_texture_formats:COMPRESSED_RGB_S3TC_DXT1_EXT,COMPRESSED_RGBA_S3TC_DXT1_EXT,COMPRESSED_RGBA_S3TC_DXT3_EXT,COMPRESSED_RGBA_S3TC_DXT5_EXT,COMPRESSED_SRGB_S3TC_DXT1_EXT,COMPRESSED_SRGB_ALPHA_S3TC_DXT1_EXT,COMPRESSED_SRGB_ALPHA_S3TC_DXT3_EXT,COMPRESSED_SRGB_ALPHA_S3TC_DXT5_EXT
webgl.shader_precision.vertex.low_float:127,127,23
webgl.shader_precision.vertex.medium_float:127,127,23
webgl.shader_precision.vertex.high_float:127,127,23
webgl.shader_precision.fragment.low_float:127,127,23
webgl.shader_precision.fragment.medium_float:127,127,23
webgl.shader_precision.fragment.high_float:127,127,23
webgl.shader_precision.vertex.low_int:31,30,0
webgl.shader_precision.vertex.medium_int:31,30,0
webgl.shader_precision.vertex.high_int:31,30,0
webgl.shader_precision.fragment.low_int:31,30,0
webgl.shader_precision.fragment.medium_int:31,30,0
webgl.shader_precision.fragment.high_int:31,30,0

# [WebGL2 指纹]
webgl2.version:WebGL 2.0 (OpenGL ES 3.0 Chromium)
webgl2.glsl_version:WebGL GLSL ES 3.00 (OpenGL ES GLSL ES 3.00 Chromium)
webgl.max_vertex_uniform_components:16384
webgl.max_fragment_uniform_components:16384
webgl.max_varying_components:124
webgl.max_uniform_block_size:65536
webgl.max_combined_uniform_blocks:60
webgl.max_vertex_uniform_blocks:12
webgl.max_fragment_uniform_blocks:12
webgl.supported_extensions:ANGLE_instanced_arrays,EXT_blend_minmax,EXT_color_buffer_half_float,EXT_float_blend,EXT_frag_depth,EXT_sRGB,EXT_shader_texture_lod,EXT_texture_filter_anisotropic,OES_element_index_uint,OES_fbo_render_mipmap,OES_standard_derivatives,OES_texture_float,OES_texture_float_linear,OES_texture_half_float,OES_texture_half_float_linear,OES_vertex_array_object,WEBGL_color_buffer_float,WEBGL_compressed_texture_s3tc,WEBGL_compressed_texture_s3tc_srgb,WEBGL_debug_renderer_info,WEBGL_debug_shaders,WEBGL_depth_texture,WEBGL_draw_buffers,WEBGL_lose_context

# [WebGPU 适配器身份指纹]
webgpu.enabled:true
webgpu.vendor:AMD
webgpu.architecture:rdna2
webgpu.device:AMD Radeon RX 6800 XT
webgpu.description:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D12)
webgpu.subgroupMinSize:4
webgpu.subgroupMaxSize:128
webgpu.isFallbackAdapter:false
webgpu.preferredCanvasFormat:bgra8unorm

# [WebGPU 功能和限制指纹]
webgpu.features:core-features-and-limits,texture-compression-bc,shader-f16,bgra8unorm-storage
webgpu.wgslLanguageFeatures:readonly_and_readwrite_storage_textures,packed_4x8_integer_dot_product,pointer_composite_access
webgpu.limits.maxTextureDimension1D:16384
webgpu.limits.maxTextureDimension2D:16384
webgpu.limits.maxTextureDimension3D:2048
webgpu.limits.maxTextureArrayLayers:2048
webgpu.limits.maxBindGroups:4
webgpu.limits.maxBindGroupsPlusVertexBuffers:24
webgpu.limits.maxBindingsPerBindGroup:1000
webgpu.limits.maxDynamicUniformBuffersPerPipelineLayout:8
webgpu.limits.maxDynamicStorageBuffersPerPipelineLayout:4
webgpu.limits.maxSampledTexturesPerShaderStage:16
webgpu.limits.maxSamplersPerShaderStage:16
webgpu.limits.maxStorageBuffersInVertexStage:8
webgpu.limits.maxStorageBuffersInFragmentStage:8
webgpu.limits.maxStorageBuffersPerShaderStage:8
webgpu.limits.maxStorageTexturesInVertexStage:4
webgpu.limits.maxStorageTexturesInFragmentStage:4
webgpu.limits.maxStorageTexturesPerShaderStage:4
webgpu.limits.maxUniformBuffersPerShaderStage:12
webgpu.limits.maxUniformBufferBindingSize:65536
webgpu.limits.maxStorageBufferBindingSize:134217728
webgpu.limits.minUniformBufferOffsetAlignment:256
webgpu.limits.minStorageBufferOffsetAlignment:256
webgpu.limits.maxVertexBuffers:8
webgpu.limits.maxBufferSize:268435456
webgpu.limits.maxVertexAttributes:16
webgpu.limits.maxVertexBufferArrayStride:2048
webgpu.limits.maxInterStageShaderVariables:16
webgpu.limits.maxColorAttachments:8
webgpu.limits.maxColorAttachmentBytesPerSample:32
webgpu.limits.maxComputeWorkgroupStorageSize:32768
webgpu.limits.maxComputeInvocationsPerWorkgroup:256
webgpu.limits.maxComputeWorkgroupSizeX:256
webgpu.limits.maxComputeWorkgroupSizeY:256
webgpu.limits.maxComputeWorkgroupSizeZ:64
webgpu.limits.maxComputeWorkgroupsPerDimension:65535

# [触控/指针指纹]
maxTouchPoints:5
touch.enabled:true
touch.maxTouchPoints:5
touch.deviceSupportPresent:true
touch.primaryPointer:fine,hover
touch.anyPointer:coarse,fine,hover
touch.legacyApis:true
```

启动时指定 `--fpfile` 参数：

```bash
firefox.exe --fpfile=C:\fingerprints\profile1.txt
```

---

## 字段说明

| 字段 | 说明 | 可选值 / 格式 |
|------|------|---------------|
| `local_webrtc_ipv4` / `public_webrtc_ipv4` | WebRTC 本地/公网 IPv4 | 合法 IPv4 地址 |
| `local_webrtc_ipv6` / `public_webrtc_ipv6` | WebRTC 本地/公网 IPv6 | 合法 IPv6 地址；不需要可删除 |
| `timezone` | JS/Intl 使用的浏览器时区 | IANA 时区名，如 `Asia/Tokyo`、`America/Los_Angeles` |
| `language` | `navigator.language` / Accept-Language | BCP47 语言标签，多个用英文逗号分隔 |
| `speech.voices.*` | `speechSynthesis.getVoices()` 语音指纹 | voices 与 langs 数量、顺序要一致 |
| `font_system` | 字体系统预设 | `windows` / `mac` / `linux` |
| `useragent` | 浏览器 UA 字符串 | 完整 UA；版本和平台信息应互相匹配 |
| `hardwareConcurrency` | CPU 逻辑核心数 | 正整数，如 `4`、`8`、`16`、`32` |
| `width` / `height` | 屏幕尺寸 | 正整数 CSS 像素 |
| `canvas` | Canvas 噪声种子 | 整数；同账号固定，不同账号建议不同 |
| `httpauth.username` / `httpauth.password` | HTTP 代理或 Basic Auth 自动认证 | 代理服务商提供的账号密码 |
| `socksauth.*` | SOCKS5 host、port、username、password | host 为域名/IP，port 为 `1-65535` |
| `proxy.rotate.enabled` | 开启按 userContext/tab 分配代理 | `true` / `false`、`1` / `0`、`yes` / `no`、`on` / `off` |
| `proxy.rotate.exhausted` | 代理池用完后的行为 | `wrap` 循环；`direct` / `none` / `stop` 表示不再使用代理 |
| `proxy.rotate.proxy` | 代理池条目，可写多行 | `http://host:port:user:pass` 或 `socks5://host:port:user:pass` |
| `webgl.vendor` / `webgl.renderer` | WebGL masked 厂商和渲染器 | 与目标 GPU/浏览器组合一致 |
| `webgl.unmasked_vendor` / `webgl.unmasked_renderer` | `WEBGL_debug_renderer_info` 暴露值 | 与 renderer、WebGPU device 一致 |
| `webgl.*` 能力字段 | WebGL `getParameter()` 能力上限 | 正整数或浮点数，建议使用真实采集值 |
| `webgl.shader_precision.*` | `getShaderPrecisionFormat()` 返回值 | `rangeMin,rangeMax,precision` |
| `webgl.supported_extensions` | WebGL 扩展列表 | 英文逗号分隔 |
| `webgl2.*` | WebGL2 版本和能力字段 | 与 WebGL/GPU 组合一致 |
| `webgpu.enabled` | 是否启用 WebGPU 指纹覆盖 | `true` / `false` |
| `webgpu.vendor` / `architecture` / `device` / `description` | WebGPU 适配器身份 | 与 WebGL renderer 保持一致 |
| `webgpu.features` | WebGPU features 列表 | 英文逗号分隔 |
| `webgpu.wgslLanguageFeatures` | WGSL language features 列表 | 英文逗号分隔 |
| `webgpu.limits.*` | `GPUDevice.limits` 资源上限 | 正整数，建议使用真实 adapter 采集值 |
| `maxTouchPoints` / `touch.*` | 触控、PointerEvent、hover 画像 | `true` / `false`、正整数或逗号分隔画像值 |

---

## 使用方法

### 基本启动

```bash
firefox.exe --fpfile=C:\fingerprints\profile1.txt
```

### HTTP 密码代理

`fpfile` 中写入认证账号和密码：

```bash
httpauth.username:REPLACE_WITH_HTTP_USER
httpauth.password:REPLACE_WITH_HTTP_PASSWORD
```

然后在对应 `profile` 目录的 `user.js` 中写入 HTTP/HTTPS 代理地址和端口：

```js
user_pref("network.proxy.type", 1);
user_pref("network.proxy.http", "proxy.example.com");
user_pref("network.proxy.http_port", 1000);
user_pref("network.proxy.ssl", "proxy.example.com");
user_pref("network.proxy.ssl_port", 1000);
```

启动示例：

```bash
foxprint.exe --fpfile=C:\fingerprints\profile1.txt --profile=C:\profiles\user1
```

### SOCKS5 密码代理

SOCKS5 认证账号写在 `fpfile` 中：

```bash
socksauth.host:gate.example.com
socksauth.port:1000
socksauth.username:REPLACE_WITH_SOCKS_USER
socksauth.password:REPLACE_WITH_SOCKS_PASSWORD
```

同时在对应 `profile` 目录的 `user.js` 中写入 SOCKS5 代理地址和端口：

```js
user_pref("network.proxy.type", 1);
user_pref("network.proxy.socks", "gate.example.com");
user_pref("network.proxy.socks_port", 1000);
user_pref("network.proxy.socks_version", 5);
user_pref("network.proxy.socks_remote_dns", true);
```

如果使用 `ruyipage`，可以用 `set_proxy('socks5://gate.example.com:1000')` 写入 SOCKS5 主机和端口，账号密码仍由 `fpfile` 中的 `socksauth.*` 字段提供。

### 代理轮换

`proxy.rotate.*` 适用于一个 tab 使用一个密码代理的轮换场景。把每个可用代理写成一行 `proxy.rotate.proxy`，浏览器会按 userContext/tab 顺序取一个代理条目。

```bash
proxy.rotate.enabled=true
proxy.rotate.exhausted=wrap
proxy.rotate.proxy=socks5://gate.example.com:1000:REPLACE_WITH_USER:REPLACE_WITH_PASSWORD_US_00000001_5m
proxy.rotate.proxy=socks5://gate.example.com:1000:REPLACE_WITH_USER:REPLACE_WITH_PASSWORD_US_00000002_5m
proxy.rotate.proxy=socks5://gate.example.com:1000:REPLACE_WITH_USER:REPLACE_WITH_PASSWORD_US_00000003_5m
```

`proxy.rotate.exhausted` 可选值：

- `wrap`：代理列表用完后从第一条继续循环。
- `direct` / `none` / `stop`：代理列表用完后不再分配代理。

### 多开方案

每个浏览器实例使用不同的指纹文件和独立的 `--profile` 目录即可实现多开，各实例之间数据完全隔离。

```bash
foxprint.exe --fpfile=C:\fingerprints\profile1.txt --profile=C:\profiles\user1
foxprint.exe --fpfile=C:\fingerprints\profile2.txt --profile=C:\profiles\user2
foxprint.exe --fpfile=C:\fingerprints\profile3.txt --profile=C:\profiles\user3
```

---

## 配置规划建议

1. 先固定地区相关字段：`timezone`、`language`、`speech.voices.*` 保持同一国家和语言环境。
2. 再固定网络暴露字段：`local_webrtc_ipv4`、`local_webrtc_ipv6`、`public_webrtc_ipv4`、`public_webrtc_ipv6` 与目标出口环境保持一致。
3. 再固定硬件渲染字段：`useragent`、`hardwareConcurrency`、`webgl.*`、`webgpu.*`、`touch.*`、`font_system` 之间不要相互冲突。
4. 最后补充界面尺寸与噪声：`width`、`height`、`canvas`，让不同实例有轻微差异但不要偏离常见设备画像。

语音字段建议保持成套一致：

1. `speech.voices.local` 与 `speech.voices.local.langs` 数量要一致。
2. `speech.voices.remote` 与 `speech.voices.remote.langs` 数量要一致。
3. `speech.voices.default.name` 和 `speech.voices.default.lang` 需要与语音列表自洽。

代理和敏感信息建议：

1. 不要把真实代理账号、密码、会话号或出口 IP 提交到公开仓库。
2. README、示例截图、issue 内容统一使用 `REPLACE_WITH_*`、`203.0.113.0/24`、`2001:db8::/32` 等占位值。
3. 真正运行用的 `fp.txt` 建议放在私有目录，并按账号或任务分开管理。
