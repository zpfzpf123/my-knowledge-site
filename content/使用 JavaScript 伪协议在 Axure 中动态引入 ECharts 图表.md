---
title: "使用 JavaScript 伪协议在 Axure 中动态引入 ECharts 图表"
date: 2025-08-13
tags:
  - Axure
  - ECharts
---

# 在 Axure 中通过 JavaScript 伪协议动态引入 ECharts 图表使用说明文档

## 简介
本教程旨在介绍如何在 Axure 中通过 JavaScript 伪协议直接执行 ECharts 代码，从而实现动态引入图表的功能。Axure 支持使用 JavaScript 伪协议执行 JavaScript 代码，借助此特性，我们可以引入 ECharts 的 JS 库，并在 Axure 中直接绘制图表。

## 核心功能
- **动态引入 ECharts 库**：通过 JavaScript 伪协议在 Axure 中动态加载 ECharts 的 JS 库文件。
- **绘制 ECharts 图表**：在 Axure 中执行 ECharts 的图表绘制代码，将图表展示在指定的容器中。

## 快速入门
### 3.1 代码名称及建议目录
- **代码名称**：无特定名称，主要是在 Axure 的事件中编写 JavaScript 伪协议代码。
- **建议目录**：将从 ECharts 官网下载的 `echarts.min.js` 文件放在 Axure 的安装目录下的 `D:\Program Files (x86)\Axure\Axure RP 9\DefaultSettings\Prototype_Files\resources\scripts` 文件夹中。

### 3.2 具体步骤及代码示例
#### 3.2.1 插入图表容器
在 Axure 中插入一个矩形框，作为 ECharts 图表绘制的容器，并命名为 `test`（名字可自定义）。为了对比，可先画一个矩形框作为背景。

![[axure1.png]]


#### 3.2.2 插入事件触发代码执行
插入一个载入时事件（若要单击时才出现图表，可插入单击事件），用来触发 JavaScript 伪协议的执行。选择“链接到 URL 或文件路径”，点击 `fx`，在弹出的窗口中输入 JavaScript 伪协议代码。

#### 3.2.3 编写 JavaScript 伪协议代码
代码基础框架如下：
```javascript
// 动态加载 ECharts 的 JS 库文件
$axure.utils.loadJS('resources/scripts/echarts.min.js');
// 设置一个定时器，确保 ECharts 库加载完成后再执行图表绘制代码
setTimeout(function(){
    // 获取前面插入的矩形框，作为图表绘制的容器
    var dom = $('[data-label=test]').get(0);
    // 初始化 ECharts 实例
    var Chart = echarts.init(dom);
    // 定义图表的配置项
    var option = {
        // 这里可以将 ECharts 官网示例代码的 option 部分内容拷贝过来
    };
    // 检查 option 是否存在且为对象类型，如果是则设置图表的配置项
    if (option && typeof option === "object"){
       Chart.setOption(option, true);    
    }
}, 1000);
```
将上述代码整体复制到 Axure 的编辑框中。注意这里名字保持一致。

![[axure2.png]]


## 方法和事件
### 4.1 JavaScript 代码中的方法
| 方法名 | 参数 | 返回值 | 描述 |
| ---- | ---- | ---- | ---- |
| `$axure.utils.loadJS` | `url`：要加载的 JS 文件的路径 | 无 | 动态加载指定路径的 JS 文件。 |
| `echarts.init` | `dom`：图表绘制的容器元素 | `ECharts 实例` | 初始化一个 ECharts 实例。 |
| `Chart.setOption` | `option`：图表的配置项对象<br>`notMerge`：是否不合并配置项，布尔值，默认为 `false` | 无 | 设置 ECharts 实例的配置项。 |

### 4.2 Axure 中的事件
| 事件名 | 触发条件 | 描述 |
| ---- | ---- | ---- |
| 载入时事件 | 原型页面加载完成 | 触发 JavaScript 伪协议代码的执行。 |
| 单击事件 | 用户单击指定元素 | 触发 JavaScript 伪协议代码的执行。 |

## 最佳实践以及使用示例
### 5.1 简单柱状图示例
假设 ECharts 官网有一个简单柱状图的示例，其 `option` 部分内容如下：
```javascript
var option = {
    xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
        type: 'value'
    },
    series: [{
        data: [120, 200, 150, 80, 70, 110, 130],
        type: 'bar'
    }]
};
```
将上述 `option` 内容拷贝到我们的 JavaScript 伪协议代码中的 `option` 位置，最终代码如下：
```javascript
$axure.utils.loadJS('resources/scripts/echarts.min.js');
setTimeout(function(){
    var dom = $('[data-label=test]').get(0);
    var Chart = echarts.init(dom);
    var option = {
        xAxis: {
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
            type: 'value'
        },
        series: [{
            data: [120, 200, 150, 80, 70, 110, 130],
            type: 'bar'
        }]
    };
    if (option && typeof option === "object"){
       Chart.setOption(option, true);    
    }
}, 1000);
```
将此代码复制到 Axure 的编辑框中，运行 Axure 原型，即可看到简单柱状图展示在指定的矩形框中。

## 常见问题以及解决办法
### 6.1 图表未显示
#### 问题原因
- ECharts 库文件未正确加载。
- 图表容器元素未正确获取。
- `option` 配置项存在错误。

#### 解决办法
- 检查 `echarts.min.js` 文件是否放在正确的目录下，并且在代码中引用的路径是否正确。
- 检查 Axure 中矩形框的命名是否与代码中 `$('[data-label=test]')` 里的名称一致。
- 检查 `option` 配置项是否符合 ECharts 的规范，可以在 ECharts 官网的示例中验证配置项的正确性。

### 6.2 图表显示异常
#### 问题原因
- 定时器时间设置过短，ECharts 库还未加载完成就开始执行图表绘制代码。
- 图表容器的尺寸不符合预期。

#### 解决办法
- 适当增加 `setTimeout` 的时间，确保 ECharts 库完全加载后再执行图表绘制代码。
- 检查 Axure 中矩形框的尺寸设置，确保其能够正常显示图表。