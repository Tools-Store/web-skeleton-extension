web骨架屏
===

通过chrome扩展程序，向页面注入代码，解析dom，替换样式，生成骨架屏，最后导出对应的html文件

参考
* [一种自动化生成骨架屏的方案](https://github.com/Jocs/jocs.github.io/issues/22)

## Todo
* [ ] 移动端屏幕适配
* [ ] 浏览器默认字体样式、行高等处理

## 开发环境

将扩展程序根目录指向src目录，指向`npm run dev`，修改后`content.js`需要手动reload插件一下

## 生成思路
将页面划分成不同的部件，通过自定义属性`skeleton-type`设置该部件需要展示的状态，最后从根节点遍历，依次为各个部件添加骨架屏的样式，最后导出带样式的骨架屏html文件

整个工具依赖`skeleton-type`类型，控制该渲染类型的手段有
* 开发时通过源码直接写在页面结构上
* 打开Chrome开发者工具，通过Console或者Elements面板直接修改

**文字**

把文字占据的空间看做上行高、内容、下行高，多行文字就可以实现斑马状条纹形式的骨架屏结构

```css
.line {
    background-image: linear-gradient(red 25%,blue 25%, blue 75%, red 75%);
    /*background-image: linear-gradient(red 25%,blue 0, blue 75%, red 0); // 与上面等价*/
}
```

**图片**

使用1像素的base64灰色图片，替换页面结构原本图片的src属性,图片原本的宽高照旧

```html
<img src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" alt="">
```

**布局块**

页面上较大的布局块使用灰色背景的块占据

**边框**

替换对应边框的颜色
