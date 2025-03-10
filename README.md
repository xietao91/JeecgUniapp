# 项目介绍

JeecgUniapp 是`JeecgBoot低代码平台`的配套`APP移动框架`，项目采用 Uniapp、Vue3.0、Vite、 Wot-design-uni、TypeScript 等最新技术栈，包括二次封装组件、路由拦截、请求拦截等功能。实现了与 `JeecgBoot` 完美对接:
目前已经实现登录、用户信息、通讯录、公告、移动首页、九宫格、聊天、Online表单、仪表盘等功能，提供了丰富的组件!

当前最新版本： 3.0.0（发布日期：2025-03-10）


[![AUR](https://img.shields.io/badge/license-Apache%20License%202.0-blue.svg)](https://github.com/zhangdaiscott/jeecg-boot/blob/master/LICENSE)
[![](https://img.shields.io/badge/Author-北京国炬软件-orange.svg)](http://www.jeecg.com)
[![](https://img.shields.io/badge/version-3.0.0-brightgreen.svg)](https://github.com/zhangdaiscott/jeecg-boot)
[![GitHub stars](https://img.shields.io/github/stars/zhangdaiscott/jeecg-boot.svg?style=social&label=Stars)](https://github.com/zhangdaiscott/jeecg-boot)
[![GitHub forks](https://img.shields.io/github/forks/zhangdaiscott/jeecg-boot.svg?style=social&label=Fork)](https://github.com/zhangdaiscott/jeecg-boot)



### 新版特点
- 一份代码多终端适配，小程序、H5、安卓、ios、鸿蒙Next。
- 学习成本低、组件丰富、兼容性好、支持iframe嵌入。
- 新版APP具备低代码能力，包括表单设计、仪表盘设计等。
- 新版最大亮点是架构升级到 Vue3，适配鸿蒙 Next。
- 支持使用 VSCode 和 IntelliJ IDEA 开发，不再必须 HBuilderX，这显著提升了开发体验和效率。
- 支持低代码能力，例如Online表单可以在APP端展示和进行数据的添加与修改，仪表盘和大屏也支持移动端展示。

### 前端技术栈

- 前端 IDE 建议：Vscode、HBuilderX、Intellij IDEA
- 最新技术栈：Uniapp + Vue3.0 + Vite + TypeScript + Wot-design-uni + pinia + unocss
- 依赖管理：node、pnpm

### 环境要求

- 本地环境安装 node（18+）、pnpm （7.3+）



### 后台源码

- https://github.com/jeecgboot/jeecg-boot

### 技术交流

- 产品官网： [http://jeecg.com/appIndex](http://jeecg.com/appIndex)
- 开发文档： [https://help.jeecg.com/uniapp3](https://help.jeecg.com/uniapp3)
- 官方支持： 遇到BUG可以在github上 [发Issue](https://github.com/jeecgboot/jeecg-uniapp/issues/new)
- QQ交流群： 716488839

### 快速启动

#### 配置接口地址

> 配置文件：`env/.env.development`

请把 http://localhost:8080/jeecg-boot 替换成自己地址，其他不用改。

```javascript
//  变量必须以 VITE_ 为前缀才能暴露给外部读取
NODE_ENV = 'development';
//  是否去除console 和 debugger
VITE_DELETE_CONSOLE = false;
//  是否开启sourcemap
VITE_SHOW_SOURCEMAP = true;
// 后台接口全路径地址(必填)
VITE_SERVER_BASEURL = 'http://localhost:8080/jeecg-boot';
```

#### 启动项目

执行命令安装依赖
```
 pnpm i
```
运行启动命令，运行 `H5`

```
 pnpm run dev
```


### 新旧版本对比

| 特性                 | 旧版           | 新版                                             |
| -------------------- | -------------- |------------------------------------------------|
| **技术栈**           | Vue2，技术陈旧 | Vue3，现代化开发体验                                   |
| **UI 框架**          | 未集成         | 集成`wot-design-uni`                             |
| **编辑器**           | 仅限 HbuilderX | 支持 VSCode、Intellij IDEA 等主流编辑器                              |
| **鸿蒙**             | 不支持         | 支持鸿蒙系统                                         |
| **构建工具**         | Webpack        | Vite，构建更快                                      |
| **Unocss 原子化**    | 不支持         | 支持 Unocss 原子化                                  |
| **TypeScript 支持**  | 不支持         | 支持 TS，提供类型提示                                   |
| **基础组件封装**     | 较少           | 封装丰富组件（用户、部门、分类字典树、自定义树、popup、popupDict、导航组件等） |
| **代码片段快捷创建** | 无             | 支持 v3 快速创建页面片段                                 |

### 入门必备

本项目需要一定前端基础知识，请确保掌握 Vue 的基础知识，以便能处理一些常见的问题。 建议在开发前先学一下以下内容，提前了解和学习这些知识，会对项目理解非常有帮助:

- [Vue3 文档](https://cn.vuejs.org)
- [Uniapp](https://uniapp.dcloud.net.cn/)
- [Wot-design-uni](https://wot-design-uni.cn)
- [Unibest 文档](https://www.unibest.tech)
- [TypeScript](https://www.typescriptlang.org)
- [Es6](https://es6.ruanyifeng.com/)
- [Vitejs](https://vitejs.dev)
- [Pinia(vuex 替代方案)](https://pinia.esm.dev/introduction.html)
- [Vue-RFCS](https://github.com/vuejs/rfcs)
- [UnoCSS](https://unocss.dev)





### 效果预览

- 基础功能

![](https://oscimg.oschina.net/oscnet/up-9fb74025440e6066651599d78b4bc78f2cd.png)

![](https://oscimg.oschina.net/oscnet/up-7605e213638a559bba64279b6db93af3ed0.png)

![](https://oscimg.oschina.net/oscnet/up-43ddd52486509ab06a920c3f99f42b8b432.png)

![](https://oscimg.oschina.net/oscnet/up-02d83a8fe3fab4c0153862a9084f8a94cbb.png)

![](https://oscimg.oschina.net/oscnet/up-937a63d5e13869c40e6f1437452171d8235.png)

- 聊天功能

![](https://oscimg.oschina.net/oscnet/up-fbdae8c064e4a756412dec8cb2848c70823.png)

![](https://oscimg.oschina.net/oscnet/up-a49b94df0a04139b296b415c6fa3abc5ae3.png)

![](https://oscimg.oschina.net/oscnet/up-918a5c81f0f709712311f1437f4f9838ef2.png)

![](https://oscimg.oschina.net/oscnet/up-748e1ccab26f9d9773e160b10f9b649b13e.png)

![](https://oscimg.oschina.net/oscnet/up-37ef1c19ffb592d8577c29dbad9815879cc.png)

- Online表单

![](https://oscimg.oschina.net/oscnet/up-51e504d5ffe2ba7f54bf490f233a1f7b5a1.png)

![](https://oscimg.oschina.net/oscnet/up-a58d90af419635ba27084c4f57b217b0d83.png)

![](https://oscimg.oschina.net/oscnet/up-db01f71412a232d4b387fa09e28d23c3b8a.png)

- 仪表盘

![](https://oscimg.oschina.net/oscnet/up-fa3ffd746f132d816470fd50599a803f43c.png)

![](https://oscimg.oschina.net/oscnet/up-cf3721dff7c440f6752b45a5cdae5b3ae69.png)

- 动画展示

![](https://oscimg.oschina.net/oscnet/up-49e27699eb278c7c6b6748bfeaeb6c13b72.gif)




### 功能模块
```
├─框架实现
│  ├─APP开发框架搭建
│  ├─登录对接
│  ├─TOKEN接口机制
│  ├─热更新\覆盖更新
├─基础功能
│  ├─菜单栏目
│  ├─登录页面
│  ├─移动首页
│  ├─个人信息设置
├─消息中心
│  ├─通讯录
│  ├─系统公告
│  ├─消息推送
│  ├─在线聊天
├─低代码功能
│  ├─Online表单（列表+表单渲染）
│  ├─仪表盘（移动展示）
├─示例代码
│  ├─调用摄像头扫码(扫码)
│  ├─获取地理位置(定位)
├─新增组件
│  ├─页面滚动
│  ├─日历
│  ├─时间选择
│  ├─下拉选择
│  ├─图片上传
├─。。。
```
