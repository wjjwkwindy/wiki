# Glob 详解

> glob 是由普通字符和/或通配字符组成的字符串，用于匹配文件路径。可以利用一个或多个 glob 在文件系统中定位文件。
>
> `src()` 方法接受一个 glob 字符串或由多个 glob 字符串组成的数组作为参数。

- 在 glob 中，分隔符永远是 `/` 字符 - 不区分操作系统。

- 避免使用 Node 的 `path` 类方法来创建 glob，例如 `path.join`。

  ```bash
  const invalidGlob = path.join(__dirname, 'src/*.js');
  ```

## 特殊字符： * (一个星号)

  下面这个 glob 能够匹配类似 `index.js` 的文件，但是不能匹配类似 `scripts/index.js` 或 `scripts/nested/index.js` 的文件。

  ```javascript
  '*.js'
  ```

### 特殊字符： ** (两个星号)

在多个字符串片段中匹配任意数量的字符，包括零个匹配。 对于匹配嵌套目录下的文件很有用。请确保适当地限制带有两个星号的 glob 的使用，以避免匹配大量不必要的目录。

下面这个 glob 被适当地限制在 `scripts/` 目录下。它将匹配类似 `scripts/index.js`、`scripts/nested/index.js` 和 `scripts/nested/twice/index.js` 的文件。

```javascript
'scripts/**/*.js'
```

*在上面的示例中，如果没有* `scripts/` *这个前缀做限制，*`node_modules` *目录下的所有目录或其他目录也都将被匹配。*

## 特殊字符： ! (取反)

由于 glob 匹配时是按照每个 glob 在数组中的位置依次进行匹配操作的，所以 glob 数组中的取反（negative）glob 必须跟在一个非取反（non-negative）的 glob **后面**。第一个 glob 匹配到一组匹配项，然后后面的取反 glob 删除这些匹配项中的一部分。如果取反 glob 只是由普通字符组成的字符串，则执行效率是最高的。

```javascript
['script/**/*.js', '!scripts/vendor/']
```

## 匹配重叠（Overlapping globs）

两个或多个 glob 故意或无意匹配了相同的文件就被认为是匹配重叠（overlapping）了。如果在同一个 `src()` 中使用了会产生匹配重叠的 glob，gulp 将尽力去除重叠部分，但是在多个 `src()` 调用时产生的匹配重叠是不会被去重的。
