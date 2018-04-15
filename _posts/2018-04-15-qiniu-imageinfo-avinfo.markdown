---
layout: post
title:  "七牛云图片和视频上传后的处理"
date:   2018-04-15 15:50:00
tags: 七牛 imageInfo avinfo
---
### 一、背景介绍
#### 1、目前系统保存图片和视频的逻辑
项目所有图片和视频都是保存在七牛云的。
图片上传后，后台数据库保存的是图片链接地址。
视频上传后，app端是本地截取的图片作为视频缩略图并上传到七牛云。数据库中保存的是此缩略图的链接地址。

#### 2、存在的问题
小程序因为不好处理视频截图，目前后台保存的是视频地址。为了能在app上正确的展示视频截图，需要做一些额外的处理工作。

下面先了解一下七牛云提供的获取图片和视频额外信息的接口。
官方文档参考：[https://developer.qiniu.com/kodo/manual/1235/vars](https://developer.qiniu.com/kodo/manual/1235/vars)

### 二、图片处理
通过在图片地址后面附加imageInfo，可以查询该图片的基本信息，这里我们关心的是图片的宽和高。

比如：图片上传后的地址为：[http://images.yipuyanyi.com/1523723760119](http://images.yipuyanyi.com/1523723760119)

附加imageInfo后的地址： [http://images.yipuyanyi.com/1523723760119?imageInfo](http://images.yipuyanyi.com/1523723760119?imageInfo)

返回JSON格式数据如下：

    { 
    "size":69400,
    "format":"jpeg",
    "width":960,
    "height":540,
    "colorModel":"ycbcr"
    }

在展示图片之前获取图片的高宽信息，正确展示占位图片的尺寸。可以避免出现横向的图片用了竖向的占位图片。

### 三、视频信息获取
通过在后面附加avinfo，可以查询该视频的基本信息，这里我们还是关心视频的宽和高。
官方文档参考：[https://developer.qiniu.com/kodo/manual/1235/vars](https://developer.qiniu.com/kodo/manual/1235/vars)

比如：视频上传后的地址为：
[http://images.yipuyanyi.com/tmp_c0e798ff2c5a3286ff2fcc1dd0e4d68d.mp4](http://images.yipuyanyi.com/tmp_c0e798ff2c5a3286ff2fcc1dd0e4d68d.mp4)

附加avinfo后的地址：
[http://images.yipuyanyi.com/tmp_c0e798ff2c5a3286ff2fcc1dd0e4d68d.mp4?avinfo](http://images.yipuyanyi.com/tmp_c0e798ff2c5a3286ff2fcc1dd0e4d68d.mp4?avinfo)

返回JSON格式数据如下：

    {
        "streams": [
            {
                "index": 0,
                "codec_name": "h264",
                "codec_long_name": "H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10",
                "profile": "High",
                "codec_type": "video",
                "codec_time_base": "1200139/55158784",
                "codec_tag_string": "avc1",
                "codec_tag": "0x31637661",
                "width": 640,
                "height": 368,
                "coded_width": 640,
                "coded_height": 368,
                "has_b_frames": 2,
                "sample_aspect_ratio": "1:1",
                "display_aspect_ratio": "40:23",
                "pix_fmt": "yuv420p",
                "level": 30,
                "chroma_location": "left",
                "refs": 1,
                "is_avc": "true",
                "nal_length_size": "4",
                "r_frame_rate": "23/1",
                "avg_frame_rate": "27579392/1200139",
                "time_base": "1/11776",
                "start_pts": 0,
                "start_time": "0.000000",
                "duration_ts": 1199115,
                "duration": "101.827021",
                "bit_rate": "489497",
                "bits_per_raw_sample": "8",
                "nb_frames": "2342",
                "disposition": {
                    "default": 1,
                    "dub": 0,
                    "original": 0,
                    "comment": 0,
                    "lyrics": 0,
                    "karaoke": 0,
                    "forced": 0,
                    "hearing_impaired": 0,
                    "visual_impaired": 0,
                    "clean_effects": 0,
                    "attached_pic": 0,
                    "timed_thumbnails": 0
                },
                "tags": {
                    "creation_time": "2018-04-14T22:41:33.000000Z",
                    "language": "und",
                    "handler_name": "Core Media Video"
                }
            },
            {
                "index": 1,
                "codec_name": "aac",
                "codec_long_name": "AAC (Advanced Audio Coding)",
                "profile": "LC",
                "codec_type": "audio",
                "codec_time_base": "1/48000",
                "codec_tag_string": "mp4a",
                "codec_tag": "0x6134706d",
                "sample_fmt": "fltp",
                "sample_rate": "48000",
                "channels": 2,
                "channel_layout": "stereo",
                "bits_per_sample": 0,
                "r_frame_rate": "0/0",
                "avg_frame_rate": "0/0",
                "time_base": "1/48000",
                "start_pts": 0,
                "start_time": "0.000000",
                "duration_ts": 4898784,
                "duration": "102.058000",
                "bit_rate": "157406",
                "max_bit_rate": "157406",
                "nb_frames": "4787",
                "disposition": {
                    "default": 1,
                    "dub": 0,
                    "original": 0,
                    "comment": 0,
                    "lyrics": 0,
                    "karaoke": 0,
                    "forced": 0,
                    "hearing_impaired": 0,
                    "visual_impaired": 0,
                    "clean_effects": 0,
                    "attached_pic": 0,
                    "timed_thumbnails": 0
                },
                "tags": {
                    "creation_time": "2018-04-14T22:41:33.000000Z",
                    "language": "und",
                    "handler_name": "Core Media Audio"
                }
            }
        ],
        "format": {
            "nb_streams": 2,
            "nb_programs": 0,
            "format_name": "mov,mp4,m4a,3gp,3g2,mj2",
            "format_long_name": "QuickTime / MOV",
            "start_time": "0.000000",
            "duration": "102.058000",
            "size": "8299780",
            "bit_rate": "650593",
            "probe_score": 100,
            "tags": {
                "major_brand": "mp42",
                "minor_version": "1",
                "compatible_brands": "mp41mp42isom",
                "creation_time": "2018-04-14T22:41:33.000000Z"
            }
        }
    }

在展示缩略图片之前获取图片的高宽信息，正确展示占位图片的尺寸。可以避免出现横向的图片用了竖向的占位图片。

### 四、视频的缩略图获取

参考官方文档: [https://developer.qiniu.com/dora/manual/1313/video-frame-thumbnails-vframe](https://developer.qiniu.com/dora/manual/1313/video-frame-thumbnails-vframe)
七牛提供视频帧访问的能力，格式如下：

    vframe/<Format>/offset/<Second>/w<Width>/h/<Height>/rotate/<Degree>

例如：

    vframe/png/offset/0/w/720/h/480 
    
表示以png格式提取0秒的视频帧，图片尺寸为宽720*高480像素。给定的高宽比例如果跟原视频比例不一致，生成的图片会变形。

根据官方文档解释，这种方式产生的图片只有第一次访问时自动生成，后面再访问就直接访问缓存。该缓存不占用用户资源，访问流量免费。

[http://images.yipuyanyi.com/tmp_c0e798ff2c5a3286ff2fcc1dd0e4d68d.mp4?vframe/png/offset/0/w/720/h/480](http://images.yipuyanyi.com/tmp_c0e798ff2c5a3286ff2fcc1dd0e4d68d.mp4?vframe/png/offset/0/w/720/h/480)

此处指定的高、宽尺寸根据之前获取的视频基本信息来定。