---
title: "滚动条自定义CSS样式指南"
date: 2025-08-13
tags:
  - 样式
---

# 滚动条样式自定义说明文档

## 一、滚动条样式代码及注释
```css
/* 选择器用于匹配滚动条的整体部分，这里使用了 & 表示当前选择器的上下文元素，
   然后通过 ::-webkit-scrollbar 伪元素来设置滚动条整体的样式，
   适用于 Webkit 内核的浏览器（如 Chrome、Safari 等） */
&::-webkit-scrollbar {
    // 设置垂直滚动条的宽度为 6px
    width: 6px;
    // 设置水平滚动条的高度为 6px
    height: 6px;
}
/* 选择器用于匹配滚动条轨道两端的按钮，这些按钮允许通过点击微调小方块的位置，
   同样使用了 & 和 ::-webkit-scrollbar-button 伪元素 */
&::-webkit-scrollbar-button {
    // 将滚动条轨道两端的按钮设置为不显示
    display: none;
}
/* 选择器用于匹配滚动条的轨道（里面装有滚动条滑块 Thumb），
   使用 & 和 ::-webkit-scrollbar-track 伪元素 */
&::-webkit-scrollbar-track {
    // 将滚动条轨道的背景设置为透明
    background: transparent;
}
/* 选择器用于匹配滚动条轨道的一部分（里面装有滚动条滑块 Thumb），
   使用 & 和 ::-webkit-scrollbar-track-piece 伪元素 */
&::-webkit-scrollbar-track-piece {
    // 将滚动条轨道部分的背景颜色设置为透明
    background-color: transparent;
}
/* 选择器用于匹配滚动条里面的小方块（滚动条滑块 Thumb），它能上下或左右移动，
   使用 & 和 ::-webkit-scrollbar-thumb 伪元素 */
&::-webkit-scrollbar-thumb {
    // 将滚动条滑块的背景颜色设置为 #007cd6
    background: #007cd6;
    // 设置鼠标指针悬停在滚动条滑块上时的样式为指针，即显示为手型
    cursor: pointer;
    // 将滚动条滑块的边角设置为圆角，半径为 4px
    border-radius: 4px;
}
/* 选择器用于匹配两个滚动条的交汇处（边角），
   使用 & 和 ::-webkit-scrollbar-corner 伪元素 */
&::-webkit-scrollbar-corner {
    // 将两个滚动条交汇处的边角设置为不显示
    display: none;
}
/* 选择器用于匹配两个滚动条交汇处上用于通过拖动调整元素大小的小控件，
   使用 & 和 ::-webkit-resizer 伪元素 */
&::-webkit-resizer {
    // 将用于调整元素大小的小控件设置为不显示
    display: none;
}
```

## 二、涉及的属性和方法说明
### 1. 滚动条相关伪元素及属性
| 伪元素名称 | 所属选择器类型 | 描述 | 涉及属性 |
| ---- | ---- | ---- | ---- |
| ::-webkit-scrollbar | 滚动条整体伪元素 | 匹配 Webkit 内核浏览器的滚动条整体部分，用于设置滚动条的基本尺寸等样式 | width（设置垂直滚动条宽度）、height（设置水平滚动条高度） |
| ::-webkit-scrollbar-button | 滚动条轨道按钮伪元素 | 匹配滚动条轨道两端的按钮，可设置其显示或隐藏等样式 | display（设置元素是否显示，这里设置为 none 即不显示） |
| ::-webkit-scrollbar-track | 滚动条轨道伪元素 | 匹配滚动条的轨道部分，用于设置轨道的背景等样式 | background（设置轨道背景颜色，这里设置为 transparent 即透明） |
| ::-webkit-scrollbar-track-piece | 滚动条轨道部分伪元素 | 匹配滚动条轨道的一部分，可进一步设置轨道的样式 | background-color（设置轨道部分背景颜色，这里设置为 transparent 即透明） |
| ::-webkit-scrollbar-thumb | 滚动条滑块伪元素 | 匹配滚动条里面的滑块，用于设置滑块的外观、交互等样式 | background（设置滑块背景颜色，这里为 #007cd6）、cursor（设置鼠标指针样式，这里为 pointer 即手型）、border-radius（设置滑块边角圆角半径，这里为 4px） |
| ::-webkit-scrollbar-corner | 滚动条交汇处伪元素 | 匹配两个滚动条的交汇处（边角），可设置其显示或隐藏等样式 | display（设置元素是否显示，这里设置为 none 即不显示） |
| ::-webkit-resizer | 调整大小控件伪元素 | 匹配两个滚动条交汇处上用于拖动调整元素大小的小控件，可设置其显示或隐藏等样式 | display（设置元素是否显示，这里设置为 none 即不显示） |

以上是关于滚动条样式自定义的详细说明，通过这些伪元素和属性的设置，可以对 Webkit 内核浏览器的滚动条样式进行个性化定制。 