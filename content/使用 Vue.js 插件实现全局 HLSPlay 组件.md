---
title: "使用 Vue.js 插件实现全局 HLSPlay 组件"
date: 2025-08-13
tags:
  - vue2
  - 视频
---

# 使用 HLSPlay 组件的 Vue.js 插件使用说明文档
该插件允许你在 Vue.js 应用中全局使用 HLSPlay 组件。

## 下载
```
npm install vue-video-player@5.0.2
npm install videojs-contrib-hls@5.14.1
```

## 代码
### 模板部分
```html
<template>
  <section class="HLSPlay-component">
    <!-- video-player 是 vue-video-player 插件提供的视频播放器组件 -->
    <!-- ref 用于在 Vue 实例中访问该组件实例，这里命名为 videoPlayer -->
    <!-- class 为视频播放器的样式类名 -->
    <!-- :options 绑定了播放器的配置选项，通过 playerOptions 数据属性传递 -->
    <!-- :playsinline 用于设置视频在移动端是否支持内联播放，设置为 true -->
    <!-- custom-event-name 自定义了一个状态改变事件的名称 -->
    <video-player
      ref="videoPlayer"
      class="video-player-box"
      :options="playerOptions"
      :playsinline="true"
      custom-event-name="customstatechangedeventname"
    />
  </section>
</template>
```

### 脚本部分
```javascript
<script>
// 引入 video.js 的默认样式文件，用于视频播放器的样式
import 'video.js/dist/video-js.css'
// 引入 videojs-contrib-hls 插件，用于支持 HLS 格式视频播放
import 'videojs-contrib-hls'
// 从 vue-video-player 中导入 videoPlayer 组件
import { videoPlayer } from 'vue-video-player'

export default {
  // 组件名称为 HLSPlay
  name: 'HLSPlay',
  // 注册 videoPlayer 组件，使其在当前组件中可用
  components: {
    videoPlayer
  },
  // 定义组件接收的属性
  props: {
    // 视频源的 URL，类型为字符串，未设置默认值
    // eslint-disable-next-line vue/require-default-prop
    src: {
      type: String
    },
    // 视频封面的 URL，类型为字符串，未设置默认值
    // eslint-disable-next-line vue/prop-name-casing,vue/require-default-prop
    cover_url: {
      type: String
    }
  },
  // 组件的数据属性
  data() {
    return {
      // 视频播放器的配置选项
      playerOptions: {
        // 自动播放，设置为 true
        autoplay: true,
        // 静音，设置为 true
        muted: true,
        // 预加载的时长，设置为 '63:25'
        preload: '63:25',
        // 语言设置为中文
        language: 'zh-CN',
        // 使视频播放器自适应容器大小，设置为 true
        fluid: true,
        // 播放速度选项，这里注释掉了，可按需启用
        // playbackRates: [0.7, 1.0, 1.5, 2.0],
        // 视频源数组，这里只有一个 HLS 格式的视频源
        sources: [
          {
            // 视频源类型为 application/x-mpegURL（HLS 格式）
            type: 'application/x-mpegURL',
            // 视频源的路径，使用组件属性 src 的值
            src: this.src 
            // 这里是一个示例视频源 URL，可替换为实际的
            // src: 'https://cdn.theguardian.tv/webM/2015/07/20/150716YesMen_synd_768k_vp8.webm'
          }
        ],
        // 视频封面的地址，使用组件属性 cover_url 的值
        poster: this.cover_url, 
        // 当视频不支持播放时显示的提示信息
        notSupportedMessage: '此视频暂无法播放，请稍后再试'
        // 控制栏的配置选项，这里注释掉了，可按需启用
        // controlBar: {
        //   timeDivider: true,
        //   durationDisplay: true,
        //   remainingTimeDisplay: false,
        //   fullscreenToggle: true, // 全屏按钮
        //   currentTimeDisplay: true, // 当前时间
        //   volumeControl: false, // 声音控制键
        //   playToggle: false, // 暂停和播放键
        //   // progressControl: true // 进度条
        // }
      }
    }
  },
  // 计算属性，返回视频播放器的实例
  computed: {
    player() {
      return this.$refs.videoPlayer.player
    }
  },
  // 组件挂载完成后的生命周期钩子函数
  mounted() {
    // 这里原本有播放的调用，注释掉了，可按需启用
    // this.play()
    // 原本有打印播放器实例的操作，注释掉了，可按需启用
    // console.log('this is current player instance object', this.player)
  },
  // 组件销毁前的生命周期钩子函数
  beforeDestroy() {
    // 如果播放器实例存在，则销毁它
    this.player && this.player.dispose()
  },
  // 组件的方法
  methods: {
    // 设置新的视频源的方法
    setSrc(src) {
      // 使用播放器实例的 src 方法设置新的视频源和类型
      this.player.src({ src: src, type: 'application/x-mpegURL' })
    },
    // 播放视频的方法
    play() {
      try {
        // 确保 DOM 更新完成后执行播放操作
        this.$nextTick(() => {
          // 调用播放器实例的 play 方法播放视频
          this.player.play().then(res => {
            // 播放成功时，这里原本有一个提示，可按需完善
            this.$message('获取视频成功')
          }).catch(e => {
            // 播放失败时，打印错误信息
            console.log(e, '失败')
          }) 
        })
      } catch (e) {
        // 捕获异常并打印错误信息
        console.log(e)
      }
      // 手动触发播放
    }
  }

}
</script>
```

### 样式部分
```css
<style lang="scss" scoped>
.HLSPlay-component{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  ::v-deep.video-player-box{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    #vjs_video_1{
      height: 100%;
    }
   #vjs_video_3 {
      height: inherit;
      padding-top: 36.25% !important;
    }
  }
}
::v-deep.video-player {
  //.vjs_video_3-dimensions {
  //  width: 100%;
  //  height: 96%;
  //}
  width: 100%;
  height: 100%;
  // 初始化，暂停按钮居中
  .vjs-big-play-button {
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  .video-js .vjs-tech {
    object-fit: fill;
    height: 100%;
  }
  .vjs-fluid{
    padding-top: 2vw !important;
    height: inherit;
  }
}
</style>
```

## 使用
在你的 Vue 组件中，你可以使用 HLSPlay 组件来创建一个 HLS 视频播放器。

### 组件属性
| 属性名 | 类型 | 描述 |
| ---- | ---- | ---- |
| src | String | 视频源的 URL |
| cover_url | String | 视频封面的 URL |

### 组件方法
| 方法名 | 参数 | 描述 |
| ---- | ---- | ---- |
| setSrc | src（字符串类型的新视频源 URL） | 设置新的视频源 |
| play | 无 | 播放视频 |

## 示例
以下是一个如何在 Vue 组件中使用 HLSPlay 组件的示例：

```html
<template>
  <div>
    <HLSPlay class="video-item" :src="videoSrc" :cover_url="coverUrl" ref="hlsPlayer" />
    <button @click="changeVideo">Change Video</button>
  </div>
</template>

<script>
import HLSPlay from './HLSPlay'

export default {
  components: {
    HLSPlay
  },
  data() {
    return {
      videoSrc: 'https://path/to/your/video.m3u8',
      coverUrl: 'https://path/to/your/cover.jpg'
    }
  },
  methods: {
    changeVideo() {
      this.videoSrc = 'https://path/to/another/video.m3u8'
      this.$refs.hlsPlayer.setSrc(this.videoSrc)
      this.$refs.hlsPlayer.play()
    }
  }
}
</script>
<style lang="scss" scoped>
  .video-item {
    width: 100%;
    height: 100%;
  }
</style>
```