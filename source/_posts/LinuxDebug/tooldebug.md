---
title: ubuntu常见工具问题debug
topic: true
---

# Ubuntu Video 无法播放mkv视频
安装 h.264 decoder
sudo apt-get install h264enc
sudo apt install gstreamer1.0-libav
安装完重启播放器即可
