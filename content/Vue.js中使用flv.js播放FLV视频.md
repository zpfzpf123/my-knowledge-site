---
title: "Vue.js中使用flv.js播放FLV视频"
date: 2025-08-13
tags:
  - vue2
---

# 使用 flv.js 实现 FLV 视频播放的 Vue.js 示例文档
该文档介绍了如何在 Vue.js 应用中使用 flv.js 来播放 FLV 格式的视频。

## 下载
```
npm install --save flv.js
```

## 代码示例
### 模板部分
```html
<template>
  <div>
    <!-- 定义一个 video 标签，用于显示视频。id 为 videoElement，方便在 JavaScript 中获取该元素。
         controls 属性表示显示视频的控制条，如播放、暂停、音量等按钮。
         autoplay 属性表示视频自动播放，muted 属性表示视频静音播放。
         width 和 height 属性分别设置视频的宽度和高度为 300px 和 200px。 -->
    <video id="videoElement" controls autoplay muted width="300px" height="200px">    
    </video>
    <!-- 定义一个按钮，绑定了 click 事件，当点击按钮时会调用名为 play 的方法。 -->
    <button @click="play">播放</button>
  </div>
</template>
```

### 脚本部分
```javascript
<script>
    // 引入 flv.js 库
    import flvjs from 'flv.js'

    export default {
        // 定义组件的数据属性
        data () {
            return {
                // 用于存储 flv.js 创建的播放器实例，初始值为 null
                flvPlayer:null
            }
        },
        // 组件挂载完成后的生命周期钩子函数
        mounted() {
            // 检查当前环境是否支持 flv.js 播放 FLV 视频
            if (flvjs.isSupported()) {
                // 通过 document.getElementById 获取页面中 id 为 videoElement 的 video 元素
                var videoElement = document.getElementById('videoElement');
                // 使用 flvjs.createPlayer 创建一个 FLV 视频播放器实例
                // type: 'flv' 表示视频类型为 FLV。
                // isLive: true 表示视频是直播流（这里可能根据实际情况调整）。
                // hasAudio: false 表示视频没有音频（这里可能根据实际情况调整）。
                // url: 'http://1011.hlsplay.aodianyun.com/demo/game.flv' 是视频的播放地址。
                this.flvPlayer = flvjs.createPlayer({
                    type: 'flv',
                    isLive: true,
                    hasAudio: false,
                    url: 'http://1011.hlsplay.aodianyun.com/demo/game.flv'
                });
                // 将创建的播放器实例与页面中的 video 元素关联起来
                this.flvPlayer.attachMediaElement(videoElement);
                try {
                    // 加载视频资源
                    this.flvPlayer.load()
                    // 播放视频
                    this.flvPlayer.play()
                } catch (e) {
                    // 如果出现错误，打印错误信息
                    console.log(e)
                }
            }
        },
        // 定义组件的方法
        methods:{
            // 定义名为 play 的方法，用于播放视频
            play () {
                this.flvPlayer.play();
            }
        },
        // 组件销毁前的生命周期钩子函数
        beforeDestroy(){
            // 如果播放器实例存在，调用其 destory 方法销毁播放器
            this.flvPlayer && this.flvPlayer.destory()
        }
    }
</script>
```

## 组件属性
| 属性名 | 类型 | 描述 |
| ---- | ---- | ---- |
| 无 | 无 | 该 Vue 组件未定义对外的属性 |

## 组件方法
| 方法名 | 参数 | 描述 |
| ---- | ---- | ---- |
| play | 无 | 调用 flv.js 播放器实例的 play 方法，实现视频播放功能 |

希望这个文档能帮助你理解如何在 Vue.js 中使用 flv.js 来播放 FLV 视频。如有疑问，请随时查阅相关文档或进行进一步的测试。 