```bash
ffmpeg \
-i input.mp4 \
-i cover.jpg \
-map 0:v \
-map 0:a \
-map 1:v \
-c copy \
-metadata:s:v:1 title="Album Cover" \
-metadata:s:v:1 comment="Cover (front)" \
-disposition:v:1 attached_pic
output.mp4
```


| 参数                                        | 释义                                                                                            |
| :---------------------------------------- | :-------------------------------------------------------------------------------------------- |
| `-i input.mp4`                            | 第一个输入文件（索引为 0）                                                                                |
| `-i cover.jpg`                            | 第二个输入文件（索引为 1）                                                                                |
| `-map 0:v`                                | 映射索引为 0 的输入文件的所有视频流（视频文件的视频流通常只有一个，即主视频）。<br/>如果该索引文件有多个视频流，可以写成 `-map 0:v:0`，`-map 0:v:1`... |
| `-map 0:a`                                | 映射索引为 0 的输入文件的所有音频流。<br/>如果该索引文件有多个音频流，可以写成 `-map 0:a:0`，`-map 0:a:1`...                      |
| `-map 1:v`                                | 映射所以为 1 的文件，并将该文件作为视频流（虽然该文件只是一个图片）                                                           |
| `-c copy`                                 | 对映射（`map`）的流只做复制操作，不重新编码                                                                      |
| `-metadata:s:v:1 title="Album Cover"`     | 给第二个视频流添加元数据 `title`                                                                          |
| `-metadata:s:v:1 comment="Cover (front)"` | 给第二个视频流添加元数据 `comment`，这个元数据非必须，但是如果你修改的是 flac 音频文件，那该元数据就是必须的                                |
| `-disposition:v:1 attached_pic`           | 将第二个视频流标记为附带图片，这个必须要设置。因为很多主流的播放器都是以 attached_pic 来判断封面                                       |
| `output.mp4`                              | 输出文件名                                                                                         |

## 参数 -disposition 说明

在 `FFmpeg` 中，所有输入文件都可以被视为一个流。对于图片输入，`FFmpeg` 通常会创建一个视频流，尽管它实际上是静态帧。就是说即使 `cover.jpg` 是一张图片，它在 `FFmpeg` 处理时会被视为一个视频流使用。

所以命令：

```bash
ffmpeg \
-i input.mp4 \
-i cover.jpg \

...
-disposition:v:1 attached_pic
...
```

有两个视频流，第一个视频流是 input.mp4，索引是0。第二个视频流是 cover.jpg，索引是1。

而 `-disposition:v` 参数可以用来设置流的属性。这里指定第二个视频做作为附加封面（attached_pic）使用。