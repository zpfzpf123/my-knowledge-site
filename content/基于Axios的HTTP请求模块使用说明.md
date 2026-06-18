---
title: "基于Axios的HTTP请求模块使用说明"
date: 2025-08-13
tags:
  - vue2
---

# 基于 axios 的 HTTP 请求模块使用说明文档

## 一、模块代码及注释
```javascript
// 引入 axios 库，用于发送 HTTP 请求
import axios from 'axios';
// 引入 element-ui 中的 Message 组件，用于显示提示信息
import {Message} from 'element-ui';

// 定义统一的请求路径前缀，这里设置为空字符串，实际使用中可根据项目情况修改
let baseApi = "/";

// 设置 axios 的默认超时时间为 20000 毫秒，即 20 秒
axios.defaults.timeout = 20000;

// 请求拦截器，在请求发送之前调用
// config 是请求的配置对象，可对其进行修改后返回
axios.interceptors.request.use(config => {
    return config;
}, err => {
    // 如果请求出现错误（如超时等），使用 Message 组件显示错误信息 "请求超时"
    Message.error('请求超时');
    // 返回一个被拒绝的 Promise，将错误传递下去
    return Promise.reject(err);
});

// 响应拦截器，在接收到响应之后调用
// response 是服务器返回的响应对象，这里直接返回响应的数据部分 response.data
axios.interceptors.response.use(response => {
    return response.data;
}, (err) => {
    // 如果响应状态码不为 200，即出现错误
    // 使用 Message 组件显示错误信息，将错误对象转换为字符串显示
    Message.error(err.toString());
    // 返回一个被拒绝的 Promise，将错误传递下去
    return Promise.reject(err);
});

// 封装的 GET 请求方法
// url 是请求的路径，params 是请求的参数
export const getRequest = (url, params) => {
    return axios({
        method: 'get',
        // 拼接完整的请求 URL，由 baseApi 和传入的 url 组成
        url: `${baseApi}${url}`,
        params: params,
    });
};

// 封装的 POST 请求方法
// url 是请求的路径，params 是请求的参数
export const postRequest = (url, params) => {
    return axios({
        method: 'post',
        // 拼接完整的请求 URL，由 baseApi 和传入的 url 组成
        url: `${baseApi}${url}`,
        // POST 请求的数据放在 data 中
        data: params,
    });
};

// 封装的 PUT 请求方法
// url 是请求的路径，params 是请求的参数
export const putRequest = (url, params) => {
    return axios({
        method: 'put',
        // 拼接完整的请求 URL，由 baseApi 和传入的 url 组成
        url: `${baseApi}${url}`,
        // PUT 请求的数据放在 data 中
        data: params,
    });
};
```

## 二、配置说明
| 配置项 | 类型 | 默认值 | 描述 |
| ---- | ---- | ---- | ---- |
| baseApi | String | "/" | 统一的请求路径前缀，用于拼接完整的请求 URL |
| axios.defaults.timeout | Number | 20000 | 请求的超时时间，单位为毫秒，超过该时间请求将被视为失败 |

## 三、拦截器说明
| 拦截器名称 | 作用时机 | 功能描述 |
| ---- | ---- | ---- |
| 请求拦截器 | 请求发送之前 | 对请求配置进行处理，若请求出现错误（如超时），显示错误消息 "请求超时" 并返回被拒绝的 Promise |
| 响应拦截器 | 响应返回之前 | 对响应进行处理，若响应状态码不为 200，显示错误消息并返回被拒绝的 Promise，否则返回响应的数据部分 |

## 四、方法说明
| 方法名 | 参数 | 返回值 | 描述 |
| ---- | ---- | ---- | ---- |
| getRequest | url（String）：请求的 URL <br> params（Object）：请求的参数 | Promise | 发送 GET 请求，返回一个 Promise，成功时 resolve 响应数据，失败时 reject 错误信息 |
| postRequest | url（String）：请求的 URL <br> params（Object）：请求的参数 | Promise | 发送 POST 请求，返回一个 Promise，成功时 resolve 响应数据，失败时 reject 错误信息 |
| putRequest | url（String）：请求的 URL <br> params（Object）：请求的参数 | Promise | 发送 PUT 请求，返回一个 Promise，成功时 resolve 响应数据，失败时 reject 错误信息 |

## 五、使用示例
### 1. 二次封装示例（note.js）
```javascript
// 从封装的 axios 请求模块中引入相关方法
import { getRequest, postRequest, delRequest, putRequest } from '@/request/axios'

// 导出一个对象，包含对具体接口的封装方法
export default {
    // 获取笔记的方法，调用 getRequest 发送 GET 请求
    getNote: (params) => {
        return getRequest('note', params)
    },
    // 新增笔记的方法，调用 postRequest 发送 POST 请求
    postNote: (params) => {
        return postRequest('note', params)
    },
    // 删除笔记的方法，调用 delRequest 发送 DELETE 请求（原文档中 delRequest 未定义，这里假设已有定义）
    delNote: (params) => {
        return delRequest(`note/${params}`)
    },
    // 更新笔记的方法，调用 putRequest 发送 PUT 请求
    putNote: (url1, params) => {
        return putRequest(`note`, url1, params)
    }
}
```
### 2. 接口使用示例
```javascript
// 引入二次封装的 Note 对象
import Note from "@/api/note";

// 使用 getNote 方法发送请求
Note.getNote()
   .then((list) => {
        // 请求成功时，打印响应数据
        console.log(list);
    })
   .catch((_) => {
        // 请求失败时，可进行错误处理，这里为空
    });
```

通过以上文档，您可以清晰地了解该基于 axios 的 HTTP 请求模块的配置、拦截器、方法及使用方式，以便在项目中正确使用。 