# 🦊 Firefox 指纹浏览器

基于 Firefox 源码修改的本地指纹浏览器，通过读取本地配置文件自定义浏览器指纹信息，实现多账号隔离与反指纹检测，专门针对creepjs过了过检测。


> 仅支持 Windows 平台

百度网盘下载压缩包直接使用：
通过网盘分享的文件：fingerprintbrowser-firefox
链接: https://pan.baidu.com/s/1TXocn1792hzNAfLqAj0L0Q 提取码: axn7 

---

## 功能特性

- 自定义 WebRTC 本地/公网 IP
- 自定义 UserAgent
- 自定义时区
- 自定义 Canvas 指纹噪声
- 自定义 WebGL 渲染器信息
- 自定义屏幕分辨率
- 自定义硬件并发数
- 自定义系统字体集（Windows / Linux / Mac）
- WebDriver 检测屏蔽
- 多开互不干扰，每个实例独立指纹

---

## 指纹配置文件说明

创建一个纯文本文件（如 `profile1.txt`），内容格式如下：

webdriver:0
local_webrtc:108.151.173.203
public_webrtc:119.151.173.203
timezone:Asia/Taipei
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:119.0) Gecko/20100101 Firefox/119.0
hardwareConcurrency:32
webglrenderer:Google Inc. (Microsoft)
webglvendor:ANGLE (Microsoft, Microsoft Basic Render Driver (0x0000008C) Direct3D11 vs_5_0 ps_5_0, D3D11)
width:550
height:500
canvas:2


### 字段说明

| 字段 | 说明 | 可选值 / 格式 |
|------|------|---------------|
| `webdriver` | 是否暴露 WebDriver 标记 | `0` 隐藏 / `1` 暴露 |
| `local_webrtc` | WebRTC 本地 IP | 任意 IPv4 地址 |
| `public_webrtc` | WebRTC 公网 IP | 任意 IPv4 地址 |
| `timezone` | 时区 | IANA 时区名，如 `Asia/Taipei`、`America/New_York` |
| `font_system` | 系统字体集 | `windows` / `linux` / `mac` |
| `useragent` | 浏览器 UA 字符串 | 自定义 UA |
| `hardwareConcurrency` | CPU 逻辑核心数 | 正整数，如 `2`、`4`、`8`、`16`、`32` |
| `webglrenderer` | WebGL 渲染器名称 | 自定义字符串 |
| `webglvendor` | WebGL 厂商信息 | 自定义字符串 |
| `width` | 屏幕宽度 | 正整数（像素） |
| `height` | 屏幕高度 | 正整数（像素） |
| `canvas` | Canvas 指纹噪声种子 | 任意整数，不同值产生不同指纹 |

---

## 使用方法

### 基本启动

```bash
firefox.exe --fpfile=C:\fingerprints\profile1.txt

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
webdriver:0
local_webrtc:192.168.1.100
public_webrtc:45.33.32.156
timezone:America/New_York
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:120.0) Gecko/20100101 Firefox/120.0
hardwareConcurrency:8
webglrenderer:NVIDIA Corporation (NVIDIA)
webglvendor:ANGLE (NVIDIA, NVIDIA GeForce GTX 1660 SUPER Direct3D11 vs_5_0 ps_5_0, D3D11)
width:1920
height:1080
canvas:42

日本用户
webdriver:0
local_webrtc:10.0.0.5
public_webrtc:103.5.140.200
timezone:Asia/Tokyo
font_system:windows
useragent:Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:118.0) Gecko/20100101 Firefox/118.0
hardwareConcurrency:4
webglrenderer:Intel Inc. (Intel)
webglvendor:ANGLE (Intel, Intel(R) UHD Graphics 630 Direct3D11 vs_5_0 ps_5_0, D3D11)
width:1366
height:768
canvas:99


