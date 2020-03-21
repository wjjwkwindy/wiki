# Gulp.js 使用

## 安装 gulp.js

```bash
npm install gulp-cli -g
cd my-project
npm install gulp -D
gulp --version
```

## 创建任务(task)

> 每个 gulp 任务（task）都是一个**异步的** JavaScript 函数，此函数是一个可以接收 callback 作为参数的函数，或者是一个返回 `stream`、`promise`、`event emitter`、`child process` 或 `observable` 类型值的函数。

### 导出任务

任务（tasks）可以是 **public（公开）** 或 **private（私有）** 类型的。

- **公开任务（Public tasks）** 从 gulpfile 中被导出（export），可以通过 `gulp` 命令直接调用。
- **私有任务（Private tasks）** 被设计为在内部使用，通常作为 `series()` 或 `parallel()` 组合的组成部分。

一个私有（private）类型的任务（task）在外观和行为上和其他任务（task）是一样的，但是不能够被用户直接调用。如需将一个任务（task）注册为公开（public）类型的，只需从 gulpfile 中导出（export）即可。

```javascript
const { series } = require('gulp');

// `clean` 函数并未被导出（export），因此被认为是私有任务（private task）。
// 它仍然可以被用在 `series()` 组合中。
function clean(cb) {
  // body omitted
  cb();
}

// `build` 函数被导出（export）了，因此它是一个公开任务（public task），并且可以被 `gulp` 命令直接调用。
// 它也仍然可以被用在 `series()` 组合中。
function build(cb) {
  // body omitted
  cb();
}

exports.build = build;
exports.default = series(clean, build);
```

### 组合任务

Gulp 提供了两个强大的组合方法： `series()` 和 `parallel()`，允许将多个独立的任务组合为一个更大的操作。这两个方法都可以接受任意数目的任务（task）函数或已经组合的操作。`series()` 和 `parallel()` 可以互相嵌套至任意深度。

如果需要让任务（task）按顺序执行，请使用 `series()` 方法。

```javascript
const { series } = require('gulp');

function transpile(cb) {
  // body omitted
  cb();
}

function bundle(cb) {
  // body omitted
  cb();
}

exports.build = series(transpile, bundle);
```

对于希望以最大并发来运行的任务（tasks），可以使用 `parallel()` 方法将它们组合起来。

```javascript
const { parallel } = require('gulp');

function javascript(cb) {
  // body omitted
  cb();
}

function css(cb) {
  // body omitted
  cb();
}

exports.build = parallel(javascript, css);
```

当 `series()` 或 `parallel()` 被调用时，任务（tasks）被立即组合在一起。这就允许在组合中进行改变，而不需要在单个任务（task）中进行条件判断。

```javascript
const { series } = require('gulp');

function minify(cb) {
  // body omitted
  cb();
}


function transpile(cb) {
  // body omitted
  cb();
}

function livereload(cb) {
  // body omitted
  cb();
}

if (process.env.NODE_ENV === 'production') {
  exports.build = series(transpile, minify);
} else {
  exports.build = series(transpile, livereload);
}
```

## 异步执行

> Node 库以多种方式处理异步功能。最常见的模式是**[error-first callbacks](https://nodejs.org/api/errors.html#errors_error_first_callbacks)**，但是你还可能会遇到 [streams](https://nodejs.org/api/stream.html#stream_stream)、[promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)、[event emitters](https://nodejs.org/api/events.html#events_events)、[child processes](https://nodejs.org/api/child_process.html#child_process_child_process), 或 [observables](https://github.com/tc39/proposal-observable/blob/master/README.md)。gulp 任务（task）规范化了所有这些类型的异步功能。

### 任务（task）完成通知

当从任务（task）中返回 stream、promise、event emitter、child process 或 observable 时，成功或错误值将通知 gulp 是否继续执行或结束。如果任务（task）出错，gulp 将立即结束执行并显示该错误。

当使用 `series()` 组合多个任务（task）时，任何一个任务（task）的错误将导致整个任务组合结束，并且不会进一步执行其他任务。当使用 `parallel()` 组合多个任务（task）时，一个任务的错误将结束整个任务组合的结束，但是其他并行的任务（task）可能会执行完，也可能没有执行完。

#### 返回 stream

```javascript
const { src, dest } = require('gulp');

function streamTask() {
  return src('*.js')
    .pipe(dest('output'));
}

exports.default = streamTask;
```

#### 返回 promise

```javascript
function promiseTask() {
  return Promise.resolve('the value is ignored');
}

exports.default = promiseTask;
```

#### 返回 event emitter

```javascript
const { EventEmitter } = require('events');

function eventEmitterTask() {
  const emitter = new EventEmitter();
  // Emit has to happen async otherwise gulp isn't listening yet
  setTimeout(() => emitter.emit('finish'), 250);
  return emitter;
}

exports.default = eventEmitterTask;
```

#### 返回 child process

```javascript
const { exec } = require('child_process');

function childProcessTask() {
  return exec('date');
}

exports.default = childProcessTask;
```

#### 返回 observable

```javascript
const { Observable } = require('rxjs');

function observableTask() {
  return Observable.of(1, 2, 3);
}

exports.default = observableTask;
```

#### 使用 callback

如果任务（task）不返回任何内容，**则必须使用 callback 来指示任务已完成**。在如下示例中，callback 将作为唯一一个名为 `cb()` 的参数传递给你的任务（task）

```javascript
function callbackTask(cb) {
  // `cb()` should be called by some async work
  cb();
}

exports.default = callbackTask;
```

如需通过 callback 把任务（task）中的错误告知 gulp，请将 `Error` 作为 callback 的唯一参数。

```javascript
function callbackError(cb) {
  // `cb()` should be called by some async work
  cb(new Error('kaboom'));
}

exports.default = callbackError;
```

然而，你通常会将此 callback 函数传递给另一个 API ，而不是自己调用它。

```javascript
const fs = require('fs');

function passingCallback(cb) {
  fs.access('gulpfile.js', cb);
}

exports.default = passingCallback;
```

### gulp 不再支持同步任务（Synchronous tasks）

gulp **不再支持同步任务（Synchronous tasks）**了。因为同步任务常常会导致难以调试的细微错误，例如忘记从任务（task）中返回 stream。

当你看到 *"Did you forget to signal async completion?"* 警告时，说明你并未使用前面提到的返回方式。你需要使用 callback 或返回 stream、promise、event emitter、child process、observable 来解决此问题。

### 使用 async/await

如果不使用前面提供到几种方式，你还可以将任务（task）定义为一个 [`async` 函数](https://developers.google.com/web/fundamentals/primers/async-functions)，它将利用 promise 对你的任务（task）进行包装。这将允许你使用 `await` 处理 promise，并使用其他同步代码。

```javascript
const fs = require('fs');

async function asyncAwaitTask() {
  const { version } = fs.readFileSync('package.json');
  console.log(version);
  await Promise.resolve('some result');
}

exports.default = asyncAwaitTask;
```
