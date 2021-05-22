---
layout: wiki
wiki: FFmpeg
order: -202102
seo_title: FFmpeg是视频处理最常用的开源软件。
title: FFmpeg
cover: true
logo:
  src: https://cdn.jsdelivr.net/gh/wejudging/wejudging.github.io/source/images/项目图片/FFmpeg/ffmpeg.webp
  small: 120px
  large: 240px
description: FFmpeg是视频处理最常用的开源软件。
---

FFmpeg功能强大，用途广泛，大量用于视频网站和商业软件（比如 Youtube 和 iTunes），也是许多音频和视频格式的标准编码/解码实现。
FFmpeg 本身是一个庞大的项目，包含许多组件和库文件，最常用的是它的命令行工具。本文介绍FFmpeg命令行如何处理视频，比桌面视频处理软件更简洁高效。

### 安装

安装homebrew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
安装FFmpeg
```bash
brew install ffmpeg
```

### 视频格式与编码

#### 容器
视频文件本身其实是一个容器（container），里面包括了视频和音频，也可能有字幕等其他内容。
常见的容器格式有以下几种。一般来说，视频文件的后缀名反映了它的容器格式。
- MP4
- MKV
- WebM
- AVI

### 编码格式

视频和音频都需要经过编码，才能保存成文件。不同的编码格式（CODEC），有不同的压缩率，会导致文件大小和清晰度的差异。
#### 视频编码
**有版权**
- H.262
- H.264
- H.265

**无版权**
- VP8
- VP9
- AV1

#### 音频编码
- MP3
- AAC

### 编码器
编码器（encoders）是实现某种编码格式的库文件。只有安装了某种格式的编码器，才能实现该格式视频/音频的编码和解码。
**FFmpeg 内置的视频编码器：**
- libx264：最流行的开源 H.264 编码器
- NVENC：基于 NVIDIA GPU 的 H.264 编码器
- libx265：开源的 HEVC 编码器
- libvpx：谷歌的 VP8 和 VP9 编码器
- libaom：AV1 编码器
**音频编码器如下：**
- libfdk-aac
- aac

### FFmpeg的使用

**命令行**
```bash
ffmpeg \
[全局参数] \
[输入文件参数] \
-i [输入文件] \
[输出文件参数] \
[输出文件]
```

**将 mp4 文件转成 webm 文件，这两个都是容器格式。输入的 mp4 文件的音频编码格式是 aac，视频编码格式是 H.264；输出的 webm 文件的视频编码格式是 VP9，音频格式是 Vorbis。**
```bash
ffmpeg \
-y \ # 全局参数
-c:a libfdk_aac -c:v libx264 \ # 输入文件参数
-i input.mp4 \ # 输入文件
-c:v libvpx-vp9 -c:a libvorbis \ # 输出文件参数
output.webm # 输出文件
```

**如果不指明编码格式，FFmpeg 会自己判断输入文件的编码。因此，上面的命令可以简单写成下面的样子。**
```
ffmpeg -i input.avi output.mp4
```



**FFmpeg 常用的命令行参数如下:**
```
-c：指定编码器
-c copy：直接复制，不经过重新编码（这样比较快）
-c:v：指定视频编码器
-c:a：指定音频编码器
-i：指定输入文件
-an：去除音频流
-vn： 去除视频流
-preset：指定输出的视频质量，会影响文件的生成速度，有以下几个可用的值 ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow。
-y：不经过确认，输出时直接覆盖同名文件。
```

### 常见用法

查看文件信息
```bash
ffmpeg -i input.mp4
```
转码
```bash
ffmpeg -i input.mkv -c:v libx264 -c:a aac 良医S01E01.mkv
```


裁剪原始视频里面的一个片段，输出为一个新视频。
可以指定开始时间（start）和持续时间（duration），也可以指定结束时间（end）。
```bash
ffmpeg -ss [start] -i [input] -t [duration] -c copy [output]
ffmpeg -ss [start] -i [input] -to [end] -c copy [output]
```




### 参考链接

- http://www.ruanyifeng.com/blog/2020/01/ffmpeg.html














