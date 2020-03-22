# IE9+ 以上的文件 ajax 上传方式

参考：https://harttle.land/2016/07/04/jquery-file-upload.html#

## 现代浏览器上传文件

> 这里的文件上传方式依赖于 FormData 对象， 这是 XMLHttpRequest Level 2 接口， 需要 IE 10+, Firefox 4.0+, Chrome 7+, Safari 5+, Opera 12+

1.html

```html
<input type="file" id="avatar" name="avatar" multiple />
```

2.获取文件

```javascript
var $input = $("#avatar");
// 相当于： $input[0].files,
//         $input.get(0).files
var files = $input.prop("files");

// 也可以使用原生 js
var files = document.querySelector("#avatar").files;
```

上传文件比较特殊，其内容是二进制数据，而 HTTP 提供的是基于文本的通信协议。 这时需要采用 multipart/form-data 编码的 HTTP 表单。

```javascript
var formData = new FormData();
formData.append("avatar", files[0]);

$.ajax({
  type: "POST",
  url: "/api/upload",
  data: formData,
  cache: false,
  processData: false,
  contentType: false
});
```

3.ajax 参数说明

- `cache`  
  cache 设为 false 可以禁止浏览器对该 URL（以及对应的 HTTP 方法）的缓存。 jQuery 通过为 URL 添加一个冗余参数来实现。  
  该方法只对 GET 和 HEAD 起作用，然而 IE8 会缓存之前的 GET 结果来响应 POST 请求。 这里设置 cache: false 是为了兼容 IE8。

- `contentType`  
  jQuery 中 content-type 默认值为 application/x-www-form-urlencoded， 因此传给 data 参数的对象会默认被转换为 query string（见 HTTP 表单编码 enctype）。  
  我们不需要 jQuery 做这个转换，否则会破坏掉 multipart/form-data 的编码格式。 因此设置 contentType: false 来禁止 jQuery 的转换操作。

- `processData`  
  jQuery 会将 data 对象转换为字符串来发送 HTTP 请求，默认情况下会用 application/x-www-form-urlencoded 编码来进行转换。 我们设置 contentType: false 后该转换会失败，因此设置 processData: false 来禁止该转换过程。  
  **我们给的 data 就是已经用 FormData 编码好的数据，不需要 jQuery 进行字符串转换。**

## IE9 上传文件

> 低版本浏览器只能使用直接提交文件表单的形式，但提交大文件表单页面会长时间不响应，提交过后好像还会自动刷新页面，如果希望在低版本浏览器中解决该问题，就只能使用其它方式，比如很多支持多文件上传的文件进度的 flash 插件或者是 jquery 插件。

1.使用 jquery 插件
> [ajaxupload.3.6.js](https://gist.github.com/wjjwkwindy/7a8e8db39914f7808384993f92032c8a)  
> ps:在网上找的 ajaxupload.js 报错，上面的文件是在 lqy 官网的复工页面找到。  
> ps2:IE9 以下的浏览器兼容性未知。

```javascript
$.ajaxFileUpload({
  url: "/api/upload",
  type: "post",
  secureuri: false,
  fileElementId: fileID,
  dataType: "json",
  success: function(data, status) {
    // 执行成功操作
  },
  error: function(data, status, e) {
    console.log("error");
  }
});
```
