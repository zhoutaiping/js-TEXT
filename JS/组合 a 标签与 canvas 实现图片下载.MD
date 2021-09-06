# 组合 a 标签与 canvas 实现图片资源的安全下载

普通用户下载图片时只需一个「右键另存为」操作即可完成，但当我们做在线编辑器、整个 UI 都被自定义实现时，如何解决不同域问题并实现页面中图片资源的安全下载呢？
本文就解决该问题过程中所涉及的正则表达式、Web API 和 canvas 操作进行记录。

- 利用 a 标签下载任意资源
- 解析 DOM 获取图片链接
- 分情况处理图片链接
- 工具函数中的正则表达式完善
- canvas 绘制图片资源并转 Data URLs 返回
- 实际使用与总结

## 利用 a 标签下载任意资源

最简单的办法，当然是利用 <a> 标签。根据 MDN 描述，<a> 标签有一个属性叫 download，此属性指示浏览器下载 URL 而不是导航到它，因此将提示用户将其保存为本地文件。

如果我们再给该属性赋值，那么此值将在下载保存过程中作为预填充的文件名。

所以我们可以将需要资源链接附在一个带 download 属性的 <a> 标签上，以此实现下载的功能，

例如：

```javascript
<a 
    href="http://hijiangtao.github.io/README.md"
    download="default"
>
    下载 README
</a>

```

但需要注意的是，此属性仅适用于同源 URL，如果我们给 <a> 标签塞入一个跨域图片，那么在 chrome 中点击的效果将会是在一个页面中打开并展示这张图片，而没有下载行为。所以，面对跨域图片资源时，我们该怎么办呢？

我们都知道 <img> 加载图片资源时是不受跨域限制的，而 canvas 画布可以绘制任意图片资源，并将自身转换为 Data URLs。是的，按照这个思路，我们来一步步来解决问题。


##  解析 DOM 获取图片链接

首先从 DOM 中找到 <img> 标签并提取图片资源链接，如果你可以通过选择器直接取到 <img> 对象，那么直接取 src 属性便可，

例如：

```javascript
const {src} = document.getElementById("hijiangtao");

```

如果你拿到的是一串 HTML 字符串，那么你将会用到如下一条正则表达式，用于匹配 <img> 标签并提取其中 src 内容：

```javascript

// @Input - rawHTML
const re = /<img\s.*?src=(?:'|")([^'">]+)(?:'|")/gi;
const matchArray = re.exec(rawHTML);
const src = matchArray && matchArray[1]) || '';

```

## 分情况处理图片链接

拿到 src 即图片链接后我们来分情况讨论下，处理逻辑应该分这几步 （本文中 Data URLs 特指 base64 形式图片 URL，以下不再额外说明）：

1. 同域图片或者 Data URLs 图片直接返回
2. 跨域图片转 Data URLs 返回

故我们的代码应该长成这样，考虑到 img 标签完成资源下载时需要回调，我们用一个 Promise 将函数结果包住：


```javascript
/**
 * 获取可安全下载的图片地址
 * @param src
 */
export const getDownloadSafeImgSrc = (src: string): Promise<string> => {
    return new Promise(resolve => {
        // 0. 无效 src 直接返回
        if (!src) {
            resolve(src);
        }

        // 1. 同域或 base64 形式 src 直接返回
        if (isValidDataUrl(src) || isSameOrigin(src)) {
            resolve(src);
        }

        // 2. 跨域图片转 base64 返回
        getImgToBase64(src, resolve);
    });
};

```


## 工具函数中的正则表达式完善


上例中我们新增了很多处理函数，在这里我们把他们一一实现，首先来看看判断图片是否为 base64 格式的函数实现。

base64 格式是 Data URLs 的一种。Data URLs，即前缀为 data: 协议的URL，其允许内容创建者向文档中嵌入小文件。

它由四个部分组成：前缀 data:、指示数据类型的MIME类型、如果非文本则为可选的base64标记、数据本身：

```javascript
data:[<mediatype>][;base64],<data>

```


其中标记部分可选，前缀和数据必选，MIME 我们后文再继续介绍。那么，知道了 Data URLs 的组成，我们便可以把判断 URL 是否为有效 Data URLs 的正则匹配方法写成这样：

```javascript
/**
 * 判断给定 URL 是否为 Data URLs
 * @param s
 */
export const isValidDataUrl = (s: string): boolean => {
    const rg = /^\s*data:([a-z]+\/[a-z0-9-+.]+(;[a-z-]+=[a-z0-9-]+)?)?(;base64)?,([a-z0-9!$&',()*+;=\-._~:@\/?%\s]*?)\s*$/i;
    return rg.test(s);
};

```


这里我们再来说说 MIME，这个在我们完善 canvas 转 Data URLs 方法时会用上。MIME，全称 Multipurpose Internet Mail Extensions，我们通常说的 MIME 类型也称为媒体类型，它是一种用来表示文档、文件或字节流的性质和格式的标准。

对于图片资源来说，Web 页面中广泛支持的 MIME 类型包含以下几种：

| MIME | 类型 | 图片类型 |
| :-----| ----: | :----: |
| image/gif | GIF 图片 | (无损耗压缩方面被PNG所替代) |
| image/jpeg | JPEG |  |
| image/png | PNG |  |
| image/svg+xml | SVG图片 | (矢量图) |


如果不考虑 webp 以及 icon 等格式，我们想要从一个资源 URL 中提取出 MIME 格式便可以这样做：
```javascript
/**
 * 根据资源链接地址获取 MIME 类型
 * 默认返回 'image/png'
 * @param src
 */
export const getImgMIMEType = (src: string): string => {
    const PNG_MIME = 'image/png';

    // 找到文件后缀
    let type = src.replace(/.+\./, '').toLowerCase();

    // 处理特殊各种对应 MIME 关系
    type = type.replace(/jpg/i, 'jpeg').replace(/svg/i, 'svg+xml');

    if (!type) {
        return PNG_MIME;
    } else {
        const matchedFix = type.match(/png|jpeg|bmp|gif|svg\+xml/);
        return matchedFix ? `image/${matchedFix[0]}` : PNG_MIME;
    }
};

```

## canvas 绘制图片资源并转 Data URLs 返回

关于 canvas 这里不再多说，我们首先创建一个 canvas 画布，当开始下载图片时，我们使用 Image() 构造器创建新的 HTMLImageElement 对象，将图片的 crossOrigin 属性设置为”匿名”（即，允许对未经过验证的图像进行跨域下载）。
之后添加图片的 load 事件的监听来判断图片数据是否已接收，再决定是否调用画布 2D 上下文的 ctx.drawImage() API 来绘制图片，然后调用 canvas.toDataURL() 将资源转换成 Data URLs 并返回。

其中，之前提到的 MIME，我们将其作为参数传入 canvas.toDataURL()，默认入参为 'image/png'。最后，将图片的 src属性设置为图片的URL以触发图片下载。

``` javascript 
function getImgToBase64(url: string, callback: Function, mime?: string) {
    // canvas 画布
    const canvas: HTMLCanvasElement = document.createElement('CANVAS'),
    ctx = canvas.getContext('2d'),
    img = new Image();
    img.crossOrigin = 'Anonymous';
    img.onload = function() {
        canvas.height = img.height;
        canvas.width = img.width;

        // 绘制
        ctx!.drawImage(img, 0, 0);

        // 生成 Data URLs
        const dataURL = canvas.toDataURL(mime || 'image/png');
        callback.call(this, dataURL);
        canvas = null;
    };
    
    if (/http[s]{0,1}/.test(url)) {
        // 解决跨域问题
        img.src = url + '?random=' + Date.now();
    } else {
        img.src = url;
    }
}
```

最后，我们再将之前的 getImgToBase64(src, resolve) 调用改成 getImgToBase64(src, resolve, getImgMIMEType(src))，这个模块便大功告成。



## 实际使用与总结

``` javascript 

<a 
    *ngIf="downloadImageUrl" 
    href="" 
    download="image" 
    class="context-menu-link"
>
    保存图片至本地
</a>


而对于 TypeScript 脚本，除了引入 getDownloadSafeImgSrc 实现外，我们需要在某一个流更新所通知到的方法中增加如下引用：

import { getDownloadSafeImgSrc } from './utils.ts';
// ...

// 某一个流更新所通知到的方法
function updateDownloadImgState(editors: any[]) {
    // 假设 editors 里面存有各类选中的 DOM HTML
    const rawHTML = editors.getSelectionInnerHTML(); 
    const re = /<img\s.*?src=(?:'|")([^'">]+)(?:'|")/gi;
    const matchArray = re.exec(rawHTML);
    this.downloadImageUrl = await getDownloadSafeImgSrc((matchArray && matchArray[1]) || '');
}

```

此，不论图片资源是否跨域，我们都可以利用 <a> + canvas 的方式将其安全地下载下来，并保留图片的原始格式