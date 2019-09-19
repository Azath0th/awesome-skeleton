# awesome-skeleton

[![NPM version][npm-image]][npm-url]
[![Build status][travis-image]][travis-url]
[![Coverage status][codecov-image]][codecov-url]
[![Dependency status][daviddm-image]][daviddm-url]

> 骨架屏生成工具

## 效果

![百度骨架图效果](https://pic3.zhimg.com/80/v2-17c5c49ecff01fb6c77dcd4c7f101cf6_hd.jpg)

## 说明
* 骨架图生成组件，仅限node端使用。该组件提供骨架图生成和骨架图模板注入两个能力。
* 骨架图生成逻辑：通过传入页面地址，使用 Puppeteer 无头浏览器打开页面地址，对页面首屏图片和文本等节点进行灰色背景处理，然后对页面首屏进行截图，生成压缩后的 base64 png 图片。

## 安装方法

```bash
$ npm i awesome-skeleton -g
```

## 使用方法

### 添加配置文件

skeleton.config.json:

```json
{
  "pageName": "baidu",
  "pageUrl": "https://www.baidu.com",
  "openRepeatList": false,
  "device": "iPhone X",
  "minGrayBlockWidth": 80,
  "minGrayPseudoWidth": 10,
  "debug": true,
  "debugTime": 3000,
  "cookies": [
    {
      "domain": ".baidu.com",
      "expirationDate": 1568267131.555328,
      "hostOnly": false,
      "httpOnly": false,
      "name": "BDORZ",
      "path": "/",
      "sameSite": "unspecified",
      "secure": false,
      "session": false,
      "storeId": "0",
      "value": "yyyyyyyyy",
      "id": 2
    }
  ]
}
```

### 生成骨架屏
```bash
$ skeleton -c ./skeleton.config.json
```

页面 DomReady 之后，会在页面顶部出现红色按钮：开始生成骨架屏。

生成完成后，会在运行目录生成 skeleton-output 文件件，里面包括骨架屏 png 图片、base64 文本、html 文件。

其中 html 文件可以直接拿来用，复制下面位置：

```html
<html>
  <head>
    <!--- 骨架屏代码 -->
  </head>
</html>
```


### 登录态解决

如果页面需要登录，则需要下载 Chrome 插件 EditThisCookie，将 Cookie 复制到配置参数中。

## 参数

| 参数名称 | 必填 | 默认值 | 说明 |
| --- | --- | --- | --- |
| pageUrl | 是 | - | 页面地址（此地址必须可访问） |
| pageName | 否 | output | 页面名称（仅限英文） |
| cookies | 否 |  | 页面 Cookies，用来解决登录态问题 |
| outputPath | 否 | skeleton-output | 骨架图文件输出文件夹路径，默认到项目 skeleton-output 中 |
| openRepeatList | 否 | true | 默认会将每个列表的第一项进行复制 |
| device | 否 | 空为PC | 参考 puppeteer/DeviceDescriptors.js，可以设置为 'iPhone 6 Plus' |
| debug | 否 | false | 是否开启调试开关 |
| debugTime | 否 | 0 | 调试模式下，页面停留在骨架图的时间 |
| minGrayBlockWidth | 否 | 0 | 最小处理灰色块的宽度 |
| minGrayPseudoWidth | 否 | 0 | 最小处理伪类宽 |

## dom 节点属性

这是获取优质骨架图的要点，通过设置以下几个 dom 节点属性，在骨架图中对某些节点进行移除、忽略和指定背景色的操作，去除冗余节点的干扰，从而使得骨架图效果达到最佳。

| 参数名称 | 说明 |
| --- | --- |
| data-skeleton-remove | 指定进行移除的 dom 节点属性 |
| data-skeleton-bgcolor | 指定在某 dom 节点中添加的背景色 |
| data-skeleton-ignore | 指定忽略不进行任何处理的 dom 节点属性 |
| data-skeleton-empty | 将某dom的innerHTML置为空字符串 |

示例：

```html
<div data-skeleton-remove><span>abc</span></div>
<div data-skeleton-bgcolor="#EE00EE"><span>abc</span></div>
<div data-skeleton-ignore><span>abc</span></div>
<div data-skeleton-empty><span>abc</span></div>
```

## 项目开发指南

### 安装依赖

```bash
$ cd awesome-skeleton && npm i
```

### 运行项目

由于生成骨架图的代码是通过动态脚本插入的，所以需要通过 rollup 将 src/script 中的代码打包到 src/script/dist/index.js 中。首先启动 rollup 打包

```bash
$ npm run dev
```

修改 demo/index.js 中的配置，从而生成不同页面的骨架图：

```bash
$ cd demo
$ node index.js
```

生成的结果在 demo/output 中，包括
- base64-test.png # 骨架图图片
- base64-test.txt # 骨架图 Base64 编码
- base64-test.html # 最终生成 HTML

### 在项目中使用

将 demo/output/base64-test.html 的内容复制到目标页面模版的 <head> 中。

注意：
- 骨架图默认在 onload 事件 1s 后销毁。
- 手动销毁方式：

```js
window.SKELETON && SKELETON.destroy();
```

**当然，你也可以在项目中直接使用生成的 Base64 图片**

# 感谢

- [puppeteer](https://github.com/GoogleChrome/puppeteer)
- [page-skeleton-webpack-plugin](https://github.com/ElemeFE/page-skeleton-webpack-plugin)
- [joe-skeleton](https://github.com/korbinzhao/joe-skeleton)
- [自动化生成 H5 骨架页面](https://zhuanlan.zhihu.com/p/34702561)
- [一个前端非侵入式骨架屏自动生成方案](https://korbinzhao.github.io/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/%E9%AA%A8%E6%9E%B6%E5%B1%8F/2018/06/23/skeleton-auto-generator/)

[npm-image]: https://img.shields.io/npm/v/awesome-skeleton.svg?style=flat-square&logo=npm
[npm-url]: https://npmjs.org/package/awesome-skeleton
[travis-image]: https://img.shields.io/travis/kaola-fed/awesome-skeleton/master.svg?style=flat-square&logo=travis
[travis-url]: https://travis-ci.org/kaola-fed/awesome-skeleton
[codecov-image]: https://img.shields.io/codecov/c/github/kaola-fed/awesome-skeleton/master.svg?style=flat-square&logo=javascript
[codecov-url]: https://codecov.io/gh/kaola-fed/awesome-skeleton
[daviddm-image]: https://img.shields.io/david/kaola-fed/awesome-skeleton.svg?style=flat-square
[daviddm-url]: https://david-dm.org/kaola-fed/awesome-skeleton
