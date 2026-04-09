# 🦊 Firefox 指纹浏览器

> 专门用于 https://github.com/LoseNine/ruyipage 自动化的过检测浏览器内核

基于 Firefox 源码修改的本地指纹浏览器，通过读取本地配置文件自定义浏览器指纹信息，实现多账号隔离与反指纹检测，专门针对creepjs过了过检测。


> 仅支持 Windows 平台

下载见release
---

## 功能特性

- 自定义 WebRTC 本地/公网 IP
- 自定义 UserAgent
- 自定义时区
- 自定义 Canvas 指纹噪声
- 自定义 WebGL 完整参数（vendor、renderer、version、GLSL version、纹理大小等）
- 自定义屏幕分辨率
- 自定义硬件并发数
- 自定义系统字体集（Windows / Linux / Mac）
- 自定义浏览器语言
- WebDriver 检测屏蔽
- 多开互不干扰，每个实例独立指纹

---

## 指纹配置文件说明

1.创建一个纯文本文件（如 `profile1.txt`），内容格式如下，这里指定指纹：
```bash
webdriver:0

local_webrtc_ipv4:1.2.3.4

local_webrtc_ipv6:2001:db8::1234

public_webrtc_ipv4:8.8.8.8

public_webrtc_ipv6:2001:db8::5678

timezone:Asia/Taipei

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

canvas:10

language:en-US,zh-CN

httpauth.username:xxxxx

httpauth.password:xxx
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
| `font_system` | 系统字体集 | `windows` / `linux` / `mac` |
| `useragent` | 浏览器 UA 字符串 | 自定义 UA |
| `hardwareConcurrency` | CPU 逻辑核心数 | 正整数，如 `2`、`4`、`8`、`16`、`32` |
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
| `webgl.max_viewport_dim` | 最大视口尺寸 | 正整数，如 `16384` |
| `width` | 屏幕宽度 | 正整数（像素） |
| `height` | 屏幕高度 | 正整数（像素） |
| `canvas` | Canvas 指纹噪声种子 | 任意整数，不同值产生不同指纹 |
| `language` | 浏览器语言列表 | 逗号分隔，如 `en-US,zh-CN` |
| `httpauth.username` | HTTP 代理认证用户名 | 代理账号字符串 |
| `httpauth.password` | HTTP 代理认证密码 | 代理密码字符串 |

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
local_webrtc_ipv4:10.0.0.5
local_webrtc_ipv6:2001:db8::1005
public_webrtc_ipv4:103.5.140.200
public_webrtc_ipv6:2001:db8::5200
timezone:Asia/Tokyo
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:118.0) Gecko/20100101 Firefox/118.0
hardwareConcurrency:4
webgl.vendor:Intel Inc.
webgl.renderer:ANGLE (Intel, Intel(R) UHD Graphics 630 Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.version:WebGL 1.0 (OpenGL ES 2.0 Chromium)
webgl.glsl_version:WebGL GLSL ES 1.0 (OpenGL ES GLSL ES 1.0 Chromium)
webgl.unmasked_vendor:Intel Inc.
webgl.unmasked_renderer:ANGLE (Intel, Intel(R) UHD Graphics 630 Direct3D11 vs_5_0 ps_5_0, D3D11)
webgl.max_texture_size:8192
webgl.max_cube_map_texture_size:8192
webgl.max_texture_image_units:16
webgl.max_vertex_attribs:16
webgl.aliased_point_size_max:1024
webgl.max_viewport_dim:8192
width:1366
height:768
canvas:99
language:ja-JP,en-US
httpauth.username:xxxxx
httpauth.password:xxx
```
