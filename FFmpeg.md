# FFmpeg

## 常用命令

### 分离视频

```
# 输出图像 去掉声音
ffmpeg -i pig.mp4 -vcodec copy -an out.mp4
```



### RTMP推流

```
ffmpeg -re -i pig.mp4 -f flv rtmp://127.0.0.1:1935/live/123
```

