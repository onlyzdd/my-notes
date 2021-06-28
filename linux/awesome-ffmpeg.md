## 好用的 FFmpeg

### 获取音频/视频信息

```bash
$ ffmpeg -i input.mp4 # 使用 -i 指定输入
```

### 视频格式转换

```bash
$ ffmpeg -i input.mp4 output.flv # 自动根据后缀进行格式转换，可使用 ffmpeg -formats 查看支持的格式列表
```

### 转换视频到音频文件

```bash
$ ffmpeg -i input.mp4 -vn -ar 44100 -ac 2 -ab 320 -f mp3 output.mp3
```

相关参数：

- `-vn`：在输出文件中禁用视频录制
- `-ar`：设置输出文件的音频频率。通常使用的值是 22050、44100、48000
- `-ac`：设置音频通道的数目
- `-ab`：设置音频比特率
- `-f`：指定输出文件格式

### 更改视频文件的分辨率

```
$ ffmpeg -i input.mp4 -s 1280x720 -c:a copy output.mp4 # 更改视频分辨率为 1280x720 并存储
```

### 压缩视频文件

```bash
$ ffmpeg -i input.mp4 -vf scale=1280:-1 -c:v libx264 -preset veryslow -crf 24 output.mp4
```

### 压缩音频文件

```bash
$ ffmpeg -i input.mp3 -ab 128 output.mp3 # -ab 指定压缩音频
```

### 从视频中移除音频流

```bash
$ ffmpeg -i input.mp4 -an output.mp4 # -an 表示禁用音频录制
```

### 从媒体文件中移除视频流

```bash
$ ffmpeg -i input.mp4 -vn output.mp3 # -vn 表示禁用视频录制
```

### 从视频中提取图像

```bash
$ ffmpeg -i input.mp4 -r 1 -f image2 image-%2d.png # 从视频创建相册
```

### 裁剪视频

```bash
$ ffmpeg -i input.mp4 -filter:v "crop=w:h:x:y" output.mp4
```

### 转换一个视频的具体部分

```bash
$ ffmpeg -i input.mp4 -t 10 output.avi
```

### 设置视频的屏幕高宽比

```bash
$ ffmpeg -i input.mp4 -aspect 16:9 output.mp4
```

### 添加海报图像到音频文件

```bash
$ ffmpeg -loop 1 -i inputimage.jpg -i inputaudio.mp3 -c:v libx264 -c:a aac -strict experimental -b:a 192k -shortest output.mp4
```

### 使用开始和停止时间剪下一段媒体文件

```bash
$ ffmpeg -i input.mp4 -s 00:00:50 -codec copy -t 50 output.mp4 # -s, -t 分别指定开始时间和持续时间
```

### 切分视频文件为多个部分

```bash
$ ffmpeg -i input.mp4 -t 00:00:30 -c copy part1.mp4 -ss 00:00:30 -codec copy part2.mp4 # 更常用的一般会切分为 TS 文件，并生成 M3U8
```

### 接合或合并多个视频部分到一个

```bash
$ ffmpeg -f concat -i join.txt -c copy output.mp4 # join.txt 写入片段列表
```

### 添加字幕到一个视频文件

```bash
$ fmpeg -i input.mp4 -i subtitle.srt -map 0 -map 1 -c copy -c:v libx264 -crf 23 -preset veryfast output.mp4
```

### 预览或测试视频或音频文件

```bash
$ ffplay input.mp3 # 使用 ffplay 预览音视频文件
```

### 增加/减少视频播放速度

```bash
$ ffmpeg -i input.mp4 -vf "setpts=0.5*PTS" output.mp4 # 将双倍速度播放
```
