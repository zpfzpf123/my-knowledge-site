---
title: "Vue插件vue-contextmenujs使用指南"
date: 2025-08-13
tags:
  - vue2
---

# vue-contextmenujs 插件使用说明文档

## 一、安装
通过 npm 安装 vue-contextmenujs 插件，命令如下：
```
npm install vue-contextmenujs
```

## 二、使用
在 Vue 项目中引入并使用该插件，代码如下：
```javascript
// 导入 vue-contextmenujs 插件中的 Contextmenu 对象
import Contextmenu from "vue-contextmenujs"
// 使用 Vue.use() 方法将 Contextmenu 插件注册为全局插件
Vue.use(Contextmenu);
```

## 三、代码实现
### 1. 模板（template）部分
```html
<template>
  <!-- 定义一个 div 元素，设置宽度为 100vw（视口宽度），高度为 100vh（视口高度） -->
  <!-- @contextmenu.prevent 绑定了名为 onContextmenu 的方法，并阻止默认的右键菜单行为 -->
  <div style="width:100vw;height:100vh" @contextmenu.prevent="onContextmenu"></div>
</template>
```
### 2. 脚本（script）部分
```javascript
<script>
// 导入 Vue 构造函数
import Vue from 'vue'
// 导入 vue-contextmenujs 插件中的 Contextmenu 对象
import Contextmenu from "vue-contextmenujs"
// 使用 Vue.use() 方法将 Contextmenu 插件注册为全局插件
Vue.use(Contextmenu);

export default {
    // 定义组件的方法
    methods: {
        // onContextmenu 方法，在右键点击时触发
        onContextmenu(event) {
            // 使用 this.$contextmenu 方法来显示右键菜单，并传入菜单配置项
            this.$contextmenu({
                // items 数组，包含右键菜单的各个选项
                items: [
                    {
                        // 选项的文本标签为 "返回(B)"
                        label: "返回(B)",
                        // 选项的点击回调函数，点击时设置 this.message 并打印日志
                        onClick: () => {
                            this.message = "返回(B)";
                            console.log("返回(B)");
                        }
                    },
                    // 选项的文本标签为 "前进(F)"，设置为禁用状态
                    { label: "前进(F)", disabled: true },
                    // 选项的文本标签为 "重新加载(R)"，设置有分割线，图标为 "el-icon-refresh"
                    { label: "重新加载(R)", divided: true, icon: "el-icon-refresh" },
                    // 选项的文本标签为 "另存为(A)..."
                    { label: "另存为(A)..." },
                    // 选项的文本标签为 "打印(P)..."，图标为 "el-icon-printer"
                    { label: "打印(P)...", icon: "el-icon-printer" },
                    // 选项的文本标签为 "投射(C)..."，设置有分割线
                    { label: "投射(C)...", divided: true },
                    {
                        // 选项的文本标签为 "使用网页翻译(T)"，设置有分割线，最小宽度为 0，且有子菜单
                        label: "使用网页翻译(T)",
                        divided: true,
                        minWidth: 0,
                        children: [{ label: "翻译成简体中文" }, { label: "翻译成繁体中文" }]
                    },
                    {
                        // 选项的文本标签为 "截取网页(R)"，最小宽度为 0，且有子菜单
                        label: "截取网页(R)",
                        minWidth: 0,
                        children: [
                            {
                                // 子选项的文本标签为 "截取可视化区域"，点击时设置 this.message 并打印日志
                                label: "截取可视化区域",
                                onClick: () => {
                                    this.message = "截取可视化区域";
                                    console.log("截取可视化区域");
                                }
                            },
                            // 子选项的文本标签为 "截取全屏"
                            { label: "截取全屏" }
                        ]
                    },
                    // 选项的文本标签为 "查看网页源代码(V)"，图标为 "el-icon-view"
                    { label: "查看网页源代码(V)", icon: "el-icon-view" },
                    // 选项的文本标签为 "检查(N)"
                    { label: "检查(N)" }
                ],
                // 传入鼠标事件信息，用于确定菜单的显示位置
                event, 
                // 自定义菜单的 class 名称为 "custom-class"
                customClass: "custom-class", 
                // 菜单样式的 z-index 值为 3
                zIndex: 3, 
                // 主菜单的最小宽度为 230
                minWidth: 230 
            });
            // 返回 false 阻止默认行为，确保右键菜单正常显示
            return false;
        }
    }
};
</script>
```

## 四、自定义样式
打开控制台，查看元素即可查看到菜单的各个 class 名称。最外层的 class 为上面的 `customClass` 属性设置的值，可根据需求自行调整样式。示例样式如下：
```css
<style>
// 当鼠标悬停在可用的菜单项上或菜单项展开时，设置背景颜色为 #ffecf2，文字颜色为 #ff4050
.custom-class .menu_item__available:hover,
.custom-class .menu_item_expand {
    background: #ffecf2 !important;
    color: #ff4050 !important;
}
</style>
```

## 五、总结
以上是 vue-contextmenujs 插件的基本使用方法，该插件比自行封装节省时间。注意菜单会在点击左键或者滚轮滚动时自动销毁，同时也可调用 `this.$contextmenu.destroy()` 在其他场景自行销毁 。以下是插件的参数配置：

### 1. MenuOptions 菜单属性
| 属性名 | 类型 | 描述 |
| ---- | ---- | ---- |
| items | Array | 包含右键菜单各个选项的数组，每个选项是一个对象，包含 `label`、`onClick` 等属性 |
| event | Object | 鼠标事件信息对象，用于确定菜单的显示位置 |
| customClass | String | 自定义菜单的 class 名称 |
| zIndex | Number | 菜单样式的 z-index 值，用于设置菜单的层级 |
| minWidth | Number | 主菜单的最小宽度 |

### 2. MenuItemOptions 选项属性
| 属性名 | 类型 | 描述 |
| ---- | ---- | ---- |
| label | String | 选项的文本标签 |
| onClick | Function | 选项的点击回调函数，点击时执行 |
| disabled | Boolean | 选项是否禁用，`true` 表示禁用 |
| divided | Boolean | 选项是否显示分割线，`true` 表示显示 |
| icon | String | 选项的图标名称，如 "el-icon-refresh" 等 |
| minWidth | Number | 选项的最小宽度 |
| children | Array | 选项的子菜单数组，每个子菜单也是一个包含 `label` 等属性的对象 |

希望通过以上文档，您能更好地理解和使用 vue-contextmenujs 插件。 