+++
date = '2021-04-16T19:34:42+02:00'
draft = false
title = 'Compressing Videos on macOS'
tags = ['ffmpeg', 'macOS', 'video compression', 'tips']
theme = 'PaperMod'
+++

## 📹 Compressing Videos on macOS Using `ffmpeg`

Ever wondered how to compress a video file on **macOS**?

At work, we were asked to provide a short screen recording as proof that the assigned feature we we finished is working. However, using macOS's built-in screen recording creates large video files, which aren’t ideal for sharing.

Here's a quick and effective way to compress video files using `ffmpeg`.

### 🔧 Install `ffmpeg`

If you don’t already have `ffmpeg` installed, you can install it using [Homebrew](https://brew.sh):

```sh
brew install ffmpeg
```

### 🗜️ Compress Your Videos

Use the following command to compress a video:
```sh
ffmpeg -i input-video.mp4 -crf 30 -vcodec h264 -acodec mp2 output-video-compressed.mp4
```

You can apply the same command to any number of videos:

```sh
ffmpeg -i recording1.mov -crf 30 -vcodec h264 -acodec mp2 recording1-compressed.mp4
ffmpeg -i demo_clip.mp4 -crf 30 -vcodec h264 -acodec mp2 demo_clip-compressed.mp4
ffmpeg -i screen_capture.mp4 -crf 30 -vcodec h264 -acodec mp2 screen_capture-compressed.mp4
```

### ℹ️ Notes

* `-crf` stands for Constant Rate Factor. Lower values mean higher quality and larger file sizes. A value of 30 is typically a good balance for screen recordings.
* You can tweak the `-crf` value to adjust the compression level.
* The `h264` codec is widely supported and provides efficient compression without significant quality loss.

