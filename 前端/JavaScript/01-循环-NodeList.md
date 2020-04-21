# 循环 NodeList

```javascript
var doms = documents.querySelectorAll('span');
```

## 1. for 循环

> 兼容性最好，支持所有浏览器

```javascript
for(var i = 0; i < doms.length; i++){
    var element = doms[i];
}
```

## 2. Array 的 forEach 函数

> IE 9 及以上浏览器  
> [Can I use](https://caniuse.com/#feat=mdn-javascript_builtins_array_foreach)

```javascript
Array.prototype.forEach.call(doms,function(element){
  // Do something with DOM
})
```

```javascript
[].foreach.call(doms,function(element){
  // Do something with DOM
})
```

## 3. 继承 Array 的 forEach 函数

> IE 9 及以上浏览器

```javascript
NodeList.prototype.forEach = Array.prototype.forEach;
doms.foreach(function (element){
  // Do something with DOM
})
```

## 4. 原生 Nodelist forEach 函数

> 2016 年以后版本的浏览器 [Can I use](https://caniuse.com/#feat=mdn-api_nodelist_foreach) Global -> 89.72%  
> [MDN - NodeList.prototype.forEach()](https://developer.mozilla.org/zh-CN/docs/Web/API/NodeList/forEach)  
> 语法：`NodeList.forEach(callback[, thisArg]);`

```javascript
doms.forEach(function(currentValue,currentIndex,listObj){
  // Do something with DOM
  // currentValue: 当前循环的元素
  // currentIndex：当前序号
  // listObj：应用 forEach 的 NodeList 元素
  // thisArg：执行回调时，使用 `this` 可以获取到的值
},
'myThisAry'
)
```

### Polyfill

```javascript
if (window.NodeList && !NodeList.prototype.forEach) {
    NodeList.prototype.forEach = function (callback, thisArg) {
        thisArg = thisArg || window;
        for (var i = 0; i < this.length; i++) {
            callback.call(thisArg, this[i], i, this);
        }
    };
}
```

*或者*

```javascript
if (window.NodeList && !NodeList.prototype.forEach) {
    NodeList.prototype.forEach = Array.prototype.forEach;
}
```
