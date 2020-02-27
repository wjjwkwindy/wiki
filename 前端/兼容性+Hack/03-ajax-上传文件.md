# IE9+ 以上的文件 ajax 上传方式
参考：https://harttle.land/2016/07/04/jquery-file-upload.html#

## 现代浏览器
> 这里的文件上传方式依赖于FormData对象， 这是XMLHttpRequest Level 2接口， 需要 IE 10+, Firefox 4.0+, Chrome 7+, Safari 5+, Opera 12+

1.html

```html
<input type="file" id="avatar" name="avatar" multiple />
```

2.获取文件

```javascript
var $input = $('#avatar');
// 相当于： $input[0].files,
//         $input.get(0).files
var files = $input.prop('files');

// 也可以使用原生 js
var files = document.querySelector('#avatar').files;
```

上传文件比较特殊，其内容是二进制数据，而 HTTP 提供的是基于文本的通信协议。 这时需要采用 multipart/form-data 编码的 HTTP 表单。

```javascript
var formData = new FormData();
formData.append('avatar', files[0]);

$.ajax({
  type: 'POST',
  url: '/api/upload',
  data: formData,
  cache: false,
  processData: false,
  contentType: false
});
```

3.ajax 参数说明  

- `cache`  
cache设为false可以禁止浏览器对该URL（以及对应的HTTP方法）的缓存。 jQuery通过为URL添加一个冗余参数来实现。  
该方法只对GET和HEAD起作用，然而IE8会缓存之前的GET结果来响应POST请求。 这里设置cache: false是为了兼容IE8。

- `contentType`  
jQuery中content-type默认值为application/x-www-form-urlencoded， 因此传给data参数的对象会默认被转换为query string（见HTTP 表单编码 enctype）。  
我们不需要jQuery做这个转换，否则会破坏掉multipart/form-data的编码格式。 因此设置contentType: false来禁止jQuery的转换操作。

- `processData`  
jQuery会将data对象转换为字符串来发送HTTP请求，默认情况下会用 application/x-www-form-urlencoded编码来进行转换。 我们设置contentType: false后该转换会失败，因此设置processData: false来禁止该转换过程。  
**我们给的data就是已经用FormData编码好的数据，不需要jQuery进行字符串转换。**

## IE9

> 低版本浏览器只能使用直接提交文件表单的形式，但提交大文件表单页面会长时间不响应，提交过后好像还会自动刷新页面，如果希望在低版本浏览器中解决该问题，就只能使用其它方式，比如很多支持多文件上传的文件进度的 flash 插件或者是 jquery 插件。

1.使用 jquery 插件