# 🦊 Firefox 指纹浏览器

> 专门用于 https://github.com/LoseNine/ruyipage 自动化的过检测浏览器内核

基于 Firefox 源码修改的本地指纹浏览器，通过读取本地配置文件自定义浏览器指纹信息，实现多账号隔离与反指纹检测，专门针对creepjs过了过检测。


> 支持 Windows / Linux 平台

下载请前往 GitHub release 页面

微信：`Charleval`
---

## 功能特性

- 自定义 WebRTC 本地/公网 IP
- 自定义 UserAgent
- 自定义时区
- 自定义语音列表与默认语音（本地 / 远程 TTS）
- 自定义 Canvas 指纹噪声
- 自定义 WebGL 完整参数（vendor、renderer、version、GLSL version、纹理大小等）
- 自定义 WebGPU 暴露状态、适配器身份、features、WGSL language features 与 limits
- 自定义触摸屏 / pointer / hover 画像
- 自定义屏幕分辨率
- 自定义硬件并发数
- 自定义系统字体集（Windows / Linux / Mac）
- 自定义浏览器语言
- 支持 HTTP / SOCKS5 密码代理认证
- WebDriver 检测屏蔽
- 多开互不干扰，每个实例独立指纹

---

## 指纹配置文件说明

1.创建一个纯文本文件（如 `profile1.txt`），内容格式如下，这里指定指纹：
```bash
webdriver:0

local_webrtc_ipv4:104.251.229.181

local_webrtc_ipv6:2001:db8::1234

public_webrtc_ipv4:104.251.229.181

public_webrtc_ipv6:2001:db8::5678

timezone:Asia/Tokyo

language:ja-JP,ja

speech.voices.local:Microsoft Haruka Desktop - Japanese|Microsoft Ichiro Desktop - Japanese|Microsoft Ayumi - Japanese (Japan)

speech.voices.remote:Google 日本語|Google 日本語（日本）

speech.voices.local.langs:ja-JP|ja-JP|ja-JP

speech.voices.remote.langs:ja-JP|ja-JP

speech.voices.default.name:Microsoft Haruka Desktop - Japanese

speech.voices.default.lang:ja-JP

font_system:windows

useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:119.0) Gecko/20100101 Firefox/147.0

hardwareConcurrency:32

maxTouchPoints:5

touch.enabled:true

touch.maxTouchPoints:5

touch.deviceSupportPresent:true

touch.primaryPointer:fine,hover

touch.anyPointer:coarse,fine,hover

touch.legacyApis:true

webgl.vendor:Google Inc. (AMD)

webgl.renderer:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D11 vs_5_0 ps_5_0, D3D11)

webgl.version:WebGL 1.0 (OpenGL ES 2.0 Chromium)

webgl.glsl_version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)

webgl.unmasked_vendor:Google Inc. (AMD)

webgl.unmasked_renderer:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D11 vs_5_0 ps_5_0, D3D11)

webgl.max_texture_size:16384

webgl.max_cube_map_texture_size:16384

webgl.max_texture_image_units:32

webgl.max_vertex_attribs:16

webgl.aliased_point_size_max:1024

webgl.max_viewport_dim:16384

webgl.supported_extensions:EXT_color_buffer_float,EXT_texture_filter_anisotropic,WEBGL_debug_renderer_info,WEBGL_lose_context,WEBGL_compressed_texture_s3tc,WEBGL_compressed_texture_s3tc_srgb

webgpu.enabled:true

webgpu.vendor:AMD

webgpu.architecture:rdna2

webgpu.device:AMD Radeon RX 6800 XT

webgpu.description:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D12)

webgpu.adapterName:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D12)

webgpu.subgroupMinSize:4

webgpu.subgroupMaxSize:128

webgpu.isFallbackAdapter:false

webgpu.preferredCanvasFormat:bgra8unorm

webgpu.features:core-features-and-limits,texture-compression-bc,shader-f16,bgra8unorm-storage

webgpu.wgslLanguageFeatures:readonly_and_readwrite_storage_textures,packed_4x8_integer_dot_product,pointer_composite_access

webgpu.limits.maxTextureDimension1D:16384

webgpu.limits.maxTextureDimension2D:16384

webgpu.limits.maxTextureDimension3D:2048

webgpu.limits.maxBindGroups:4

webgpu.limits.maxBufferSize:268435456

webgpu.limits.maxComputeInvocationsPerWorkgroup:256

width:551

height:500

canvas:12

httpauth.username:xxxxx

httpauth.password:xxx

socksauth.host:gate.example.com

socksauth.port:1000

socksauth.username:xxxxx

socksauth.password:xxx
```

2.在浏览器启动的时候指定fpfile参数，可以在cmd或者任何脚本程序中做，然后就会使用这个txt里边的指纹：
```bash
firefox.exe --fpfile=C:\fingerprints\profile1.txt
```

### 字段说明

| 字段 | 说明 | 可选值 / 格式 |
|------|------|---------------|
| `webdriver` | 是否暴露 WebDriver 标记 | `0` 隐藏 / `1` 暴露 |
| `local_webrtc_ipv4` | WebRTC 本地 IPv4 | 任意 IPv4 地址 |
| `local_webrtc_ipv6` | WebRTC 本地 IPv6 | 任意 IPv6 地址 |
| `public_webrtc_ipv4` | WebRTC 公网 IPv4 | 任意 IPv4 地址 |
| `public_webrtc_ipv6` | WebRTC 公网 IPv6 | 任意 IPv6 地址 |
| `timezone` | 时区 | IANA 时区名，如 `Asia/Taipei`、`America/New_York` |
| `language` | 浏览器语言列表 | 逗号分隔，如 `ja-JP,ja`、`en-US,zh-CN` |
| `speech.voices.local` | 本地语音列表 | 用 `|` 分隔的语音名称列表 |
| `speech.voices.remote` | 远程语音列表 | 用 `|` 分隔的语音名称列表 |
| `speech.voices.local.langs` | 本地语音语言列表 | 与本地语音一一对应，使用 `|` 分隔 |
| `speech.voices.remote.langs` | 远程语音语言列表 | 与远程语音一一对应，使用 `|` 分隔 |
| `speech.voices.default.name` | 默认语音名称 | 需与语音列表中的某一项一致 |
| `speech.voices.default.lang` | 默认语音语言 | 如 `ja-JP`、`en-US` |
| `font_system` | 系统字体集 | `windows` / `linux` / `mac` |
| `useragent` | 浏览器 UA 字符串 | 自定义 UA |
| `hardwareConcurrency` | CPU 逻辑核心数 | 正整数，如 `2`、`4`、`8`、`16`、`32` |
| `maxTouchPoints` | `navigator.maxTouchPoints` 触摸点数量 | 正整数，如 `0`、`1`、`5` |
| `touch.enabled` | 是否启用触摸相关 API | `true` / `false` |
| `touch.maxTouchPoints` | 触摸点数量 | 正整数，通常与 `maxTouchPoints` 保持一致 |
| `touch.deviceSupportPresent` | 是否声明设备存在触摸支持 | `true` / `false` |
| `touch.primaryPointer` | 主 pointer / hover 画像 | 逗号分隔，如 `fine,hover` |
| `touch.anyPointer` | 任意 pointer / hover 画像 | 逗号分隔，如 `coarse,fine,hover` |
| `touch.legacyApis` | 是否启用旧触摸 API 暴露 | `true` / `false` |
| `webgl.vendor` | WebGL 厂商信息（masked） | 自定义字符串，如 `Google Inc. (AMD)` |
| `webgl.renderer` | WebGL 渲染器信息（masked） | 自定义字符串，如 `ANGLE (AMD, ...)` |
| `webgl.version` | WebGL 版本 | 如 `WebGL 1.0 (OpenGL ES 2.0 Chromium)` |
| `webgl.glsl_version` | GLSL 着色器语言版本 | 如 `WebGL GLSL ES 1.0 (...)` |
| `webgl.unmasked_vendor` | WebGL 真实厂商信息 | 自定义字符串 |
| `webgl.unmasked_renderer` | WebGL 真实渲染器信息 | 自定义字符串 |
| `webgl.max_texture_size` | 最大纹理尺寸 | 正整数，如 `16384`、`8192` |
| `webgl.max_cube_map_texture_size` | 最大立方体贴图尺寸 | 正整数，如 `16384` |
| `webgl.max_texture_image_units` | 最大纹理单元数 | 正整数，如 `32`、`16` |
| `webgl.max_vertex_attribs` | 最大顶点属性数 | 正整数，如 `16` |
| `webgl.aliased_point_size_max` | 最大点大小 | 正整数，如 `1024` |
| `webgl.aliased_point_size_range` | 点大小范围 | `最小值,最大值`，如 `1,1024` |
| `webgl.max_viewport_dim` | 最大视口尺寸 | 正整数，如 `16384` |
| `webgl.supported_extensions` | WebGL 扩展列表 | 逗号分隔，如 `EXT_color_buffer_float,WEBGL_debug_renderer_info` |
| `webgpu.enabled` | 是否暴露 WebGPU | `true` / `false` |
| `webgpu.vendor` | WebGPU 适配器厂商 | 如 `AMD`、`NVIDIA`、`Intel` |
| `webgpu.architecture` | WebGPU 适配器架构 | 如 `rdna2` |
| `webgpu.device` | WebGPU 设备名称 | 如 `AMD Radeon RX 6800 XT` |
| `webgpu.description` | WebGPU 适配器描述 | 如 `ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D12)` |
| `webgpu.adapterName` | WebGPU 适配器名称 | 自定义字符串；部分 Firefox API 版本可能不暴露 `adapter.info.name` |
| `webgpu.subgroupMinSize` | WebGPU subgroup 最小值 | 正整数，如 `4` |
| `webgpu.subgroupMaxSize` | WebGPU subgroup 最大值 | 正整数，如 `128` |
| `webgpu.isFallbackAdapter` | 是否 fallback adapter | `true` / `false` |
| `webgpu.preferredCanvasFormat` | WebGPU 首选 canvas 格式 | 如 `bgra8unorm` |
| `webgpu.features` | WebGPU features 列表 | 逗号分隔，如 `texture-compression-bc,shader-f16` |
| `webgpu.wgslLanguageFeatures` | WGSL language features 列表 | 逗号分隔 |
| `webgpu.limits.*` | WebGPU limits | 使用 `webgpu.limits.字段名:数值`，如 `webgpu.limits.maxTextureDimension2D:16384` |
| `width` | 屏幕宽度 | 正整数（像素） |
| `height` | 屏幕高度 | 正整数（像素） |
| `canvas` | Canvas 指纹噪声种子 | 任意整数，不同值产生不同指纹 |
| `httpauth.username` | HTTP 代理认证用户名 | 代理账号字符串 |
| `httpauth.password` | HTTP 代理认证密码 | 代理密码字符串 |
| `socksauth.host` | SOCKS5 代理主机 | 域名或 IP，如 `gate.example.com` |
| `socksauth.port` | SOCKS5 代理端口 | 正整数，如 `1000` |
| `socksauth.username` | SOCKS5 代理认证用户名 | 代理账号字符串 |
| `socksauth.password` | SOCKS5 代理认证密码 | 代理密码字符串 |

### 配置规划建议

适合 `ruyipage` 自动化场景的一份指纹文件，建议按下面顺序规划：

1. 先固定地区相关字段：`timezone`、`language`、`speech.voices.*` 保持同一国家和语言环境。
2. 再固定网络暴露字段：`local_webrtc_ipv4`、`local_webrtc_ipv6`、`public_webrtc_ipv4`、`public_webrtc_ipv6` 与目标出口环境保持一致。
3. 再固定硬件渲染字段：`useragent`、`hardwareConcurrency`、`webgl.*`、`webgpu.*`、`touch.*`、`font_system` 之间不要相互冲突。
4. 最后补充界面尺寸与噪声：`width`、`height`、`canvas`，让不同实例有轻微差异但不要偏离常见设备画像。

语音字段建议保持成套一致：

1. `speech.voices.local` 与 `speech.voices.local.langs` 数量要一致。
2. `speech.voices.remote` 与 `speech.voices.remote.langs` 数量要一致。
3. `speech.voices.default.name` 和 `speech.voices.default.lang` 需要与语音列表自洽。

---

## 使用方法

### 基本启动

```bash
firefox.exe --fpfile=C:\fingerprints\profile1.txt
```

### HTTP 密码代理

`fpfile` 已支持 HTTP 代理认证字段：

```bash
httpauth.username:xxxxx
httpauth.password:xxx
```

如果要让指定 `profile` 走 HTTP/HTTPS 代理，按下面步骤配置即可。

1. 在 `fpfile` 中写入代理认证账号和密码。
2. 在对应 `profile` 目录下的 `user.js` 中写入代理地址和端口。

`user.js` 示例：

```js
user_pref("network.proxy.type", 1);
user_pref("network.proxy.http", "xxx");
user_pref("network.proxy.http_port", xxx);
user_pref("network.proxy.ssl", "xxx");
user_pref("network.proxy.ssl_port", xxx);
```

将上面的 `xxx` 替换为实际代理地址和端口。

例如：

```bash
foxprint.exe --fpfile=C:\fingerprints\profile1.txt --profile=C:\profiles\user1
```

对应的代理配置文件路径为：`C:\profiles\user1\user.js`

### SOCKS5 密码代理

SOCKS5 代理认证账号写在 `fpfile` 中：

```bash
socksauth.host:gate.example.com
socksauth.port:1000
socksauth.username:xxxxx
socksauth.password:xxx
```

同时在对应 `profile` 目录下的 `user.js` 中写入 SOCKS5 代理地址和端口：

```js
user_pref("network.proxy.type", 1);
user_pref("network.proxy.socks", "gate.example.com");
user_pref("network.proxy.socks_port", 1000);
user_pref("network.proxy.socks_version", 5);
user_pref("network.proxy.socks_remote_dns", true);
```

如果使用 `ruyipage`，可以用 `set_proxy('socks5://gate.example.com:1000')` 写入 SOCKS5 主机和端口，账号密码仍由 `fpfile` 中的 `socksauth.*` 字段提供。

多开方案
每个浏览器实例使用不同的指纹文件和独立的 --profile 目录即可实现多开，各实例之间数据完全隔离。

准备多份配置文件：
C:\fingerprints\profile1.txt
C:\fingerprints\profile2.txt
C:\fingerprints\profile3.txt

分别启动：
:: 实例 1
foxprint.exe --fpfile=C:\fingerprints\profile1.txt --profile=C:\profiles\user1

:: 实例 2
foxprint.exe --fpfile=C:\fingerprints\profile2.txt --profile=C:\profiles\user2

:: 实例 3
foxprint.exe --fpfile=C:\fingerprints\profile3.txt --profile=C:\profiles\user3

配置文件示例

美国用户
```bash
webdriver:0
local_webrtc_ipv4:192.168.1.100
local_webrtc_ipv6:2001:db8::1100
public_webrtc_ipv4:45.33.32.156
public_webrtc_ipv6:2001:db8::3156
timezone:America/New_York
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:120.0) Gecko/20100101 Firefox/120.0
hardwareConcurrency:8
webgl.vendor:NVIDIA Corporation
webgl.renderer:ANGLE (NVIDIA, NVIDIA GeForce GTX 1660 SUPER Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.version:WebGL 1.0 (OpenGL ES 2.0 Chromium)
webgl.glsl_version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)
webgl.unmasked_vendor:NVIDIA Corporation
webgl.unmasked_renderer:ANGLE (NVIDIA, NVIDIA GeForce GTX 1660 SUPER Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.max_texture_size:16384
webgl.max_cube_map_texture_size:16384
webgl.max_texture_image_units:32
webgl.max_vertex_attribs:16
webgl.aliased_point_size_max:1024
webgl.max_viewport_dim:16384
width:1920
height:1080
canvas:42
language:en-US
httpauth.username:xxxxx
httpauth.password:xxx
```

日本用户
```bash
webdriver:0
local_webrtc_ipv4:104.251.229.181
local_webrtc_ipv6:2001:db8::1234
public_webrtc_ipv4:104.251.229.181
public_webrtc_ipv6:2001:db8::5678
timezone:Asia/Tokyo
language:ja-JP,ja
speech.voices.local:Microsoft Haruka Desktop - Japanese|Microsoft Ichiro Desktop - Japanese|Microsoft Ayumi - Japanese (Japan)
speech.voices.remote:Google 日本語|Google 日本語（日本）
speech.voices.local.langs:ja-JP|ja-JP|ja-JP
speech.voices.remote.langs:ja-JP|ja-JP
speech.voices.default.name:Microsoft Haruka Desktop - Japanese
speech.voices.default.lang:ja-JP
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:119.0) Gecko/20100101 Firefox/147.0
hardwareConcurrency:32
webgl.vendor:Google Inc. (AMD)
webgl.renderer:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.version:WebGL 1.0 (OpenGL ES 2.0 Chromium)
webgl.glsl_version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)
webgl.unmasked_vendor:Google Inc. (AMD)
webgl.unmasked_renderer:ANGLE (AMD, AMD Radeon RX 6800 XT Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.max_texture_size:16384
webgl.max_cube_map_texture_size:16384
webgl.max_texture_image_units:32
webgl.max_vertex_attribs:16
webgl.aliased_point_size_max:1024
webgl.max_viewport_dim:16384
width:551
height:500
canvas:12
httpauth.username:xxxxx
httpauth.password:xxx
```
