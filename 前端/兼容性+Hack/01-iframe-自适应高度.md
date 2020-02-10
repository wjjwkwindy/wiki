# iframe 自适应高度

## 1.父页面和 iframe 同源

## 2.父页面和 iframe 不同源

> [iframe-resizer 开源代码](https://github.com/davidjbradshaw/iframe-resizer)  
> 注：v3 才支持 IE8

使用 `iframeResizer.min.js` 和 `iframeResizer.contentWindow.min.js` 来处理。
`iframeResizer.min.js` 放在父页面中，`iframeResizer.contentWindow.min.js` 放在 iframe 中。

### 父页面

>需要注意的是width必须是百分比，scrolling设置为no（为了兼容性）

```html
  <body>
    <iframe
      id="formB"
      frameborder="0"
      scrolling="no"
      src="http://example.com/example/index.php"
      width="100%"
      height="100%"
    ></iframe>

    <script src="iframeResizer.min.js"></script>

    <script>
      $('#formB').iFrameResize([{ log: true }]);
    </script>
  </body>
```

### iframe

```html
    <body>

        其它内容

        <script src="iframeResizer.contentWindow.min.js"></script>
    </body>
```
