gulp重新学习

gulp是项目目录下名为gulpfile.js的文件，在运行gulp命令时会被自动加载。在这个文件中，你经常会看到类似src()，dest()，series()或parallel()函数之类的gulp API。任何导出(export)的函数都将注册到gulp的任务(task)系统中 。

##### 创建任务（task）

每个 gulp 任务（task）都是一个异步的 JavaScript 函数，此函数是一个可以接收 callback 作为参数的函数，或者是一个返回stream，promise，event emitter，child process或observable类型值的函数。由于某些平台的限制而不支持异步任务，因此gulp还提供了一个漂亮替代品。



任务（tasks）可以是 **public（公开）** 或 **private（私有）** 类型的。

- 公开任务从gulpfile中被导出export，可以通过`gulp` 命令直接调用
- **私有任务（Private tasks）** 被设计为在内部使用，通常作为 `series()` 或 `parallel()` 组合的组成部分。

一个私有（private）类型的任务（task）在外观和行为上和其他任务（task）是一样的，但是不能够被用户直接调用。如需将一个任务（task）注册为公开（public）类型的，只需从 gulpfile 中导出（export）即可。

```js
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

![ALT TEXT MISSING](https://gulpjs.com/img/docs-gulp-tasks-command.png)

##### 组合任务

Gulp 提供了两个强大的组合方法： `series()` 和 `parallel()`，允许将多个独立的任务组合为一个更大的操作。这两个方法都可以接受任意数目的任务（task）函数或已经组合的操作。`series()` 和 `parallel()` 可以互相嵌套至任意深度。

> 如果需要让任务（task）按顺序执行，请使用 `series()` 方法。

```js
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

> 对于希望以最大并发来运行的任务（tasks），可以使用 `parallel()` 方法将它们组合起来。

```js
const { parallel } = require('gulp');

function javascript(cb) {
  // body omitted
  cb();
}

function css(cb) {
  // body omitted
  cb();
}

exports.filefe = parallel(javascript, css);
```

![图片](https://coding-net-production-pp-ci.codehub.cn/d40b5e3e-2073-4d9c-83fb-2f58e4fb66af.png)

当series()或parallel()被调用时，任务(tasks)被立即组合在一起。这就允许在组合中进行改变，而不需要在单个任务（task）中进行条件判断。

当使用series()组合多个任务（task）时，任何一个任务的错误将导致整个任务组合结束，并且不会进一步执行其他任务。当使用 `parallel()` 组合多个任务（task）时，一个任务的错误将结束整个任务组合的结束，但是其他并行的任务（task）可能会执行完，也可能没有执行完。

```js
//返回stream
const { src, dest } = require('gulp');

function streamTask() {
  return src('*.js')
    .pipe(dest('output'));
}

exports.default = streamTask;
```

```js
//返回promise
function promiseTask(){
  return Promise.resolve('this value is ignored')
}
```

```js
const { EventEmitter } = require('events');

function eventEmitterTask() {
  const emitter = new EventEmitter();
  // Emit has to happen async otherwise gulp isn't listening yet
  setTimeout(() => emitter.emit('finish'), 250);
  return emitter;
}

exports.default = eventEmitterTask;
//gulp default [11:31:49] Finished 'emitters' after 258 ms
```

##### 使用callback

如果任务（task）不返回任何内容，则必须使用 callback 来指示任务已完成。在如下示例中，callback 将作为唯一一个名为 `cb()` 的参数传递给你的任务（task）。

```js
function callbackTask(cb) {
  // `cb()` should be called by some async work
  cb();
}

exports.default = callbackTask;
```

如需通过 callback 把任务（task）中的错误告知 gulp，请将 `Error` 作为 callback 的唯一参数。

```js
function callbackError(cb) {
  // `cb()` should be called by some async work
  cb(new Error('kaboom'));
}

exports.default = callbackError;
```

gulp暴露了src()和dest()方法用于处理计算机上存放的文件.

`src()` 接受 [glob](https://www.gulpjs.com.cn/docs/getting-started/explaining-globs) 参数，并从文件系统中读取文件然后生成一个 [Node 流（stream）](https://nodejs.org/api/stream.html)。它将所有匹配的文件读取到内存中并通过流（stream）进行处理。

由 `src()` 产生的流（stream）应当从任务（task）中返回并发出异步完成的信号，就如 [创建任务（task）](https://www.gulpjs.com.cn/docs/getting-started/creating-tasks) 文档中所述。

```js
const { src, dest } = require('gulp');

exports.default = function() {
  return src('src/*.js')
    .pipe(dest('output/'));
}
```

流(stream)所提供的主要的API是.pipe()方法，用于连接转换流(transform streams)或可写流（Writable streams）。

```js
const {src,dest} = require('gulp')
const babel = require('gulp-babel')
exports.default = function() {
  return src('src/*.js')
    .pipe(babel())
    .pipe(dest('output/'));
}
```

dest()接受一个输出目录作为参数，并且它还会产生一个Node流（stream）,通常作为终止流terminator stream。当它接收到通过管道（pipeline）传输的文件时，它会将文件内容及文件属性写入到指定的目录中。

gulp 还提供了 `symlink()` 方法，其操作方式类似 `dest()`，但是创建的是链接而不是文件（ 详情请参阅 [`symlink()`](https://www.gulpjs.com.cn/docs/api/symlink) ）。

大多数情况下，利用 `.pipe()` 方法将插件放置在 `src()` 和 `dest()` 之间，并转换流（stream）中的文件。

`src()` 也可以放在管道（pipeline）的中间，以根据给定的 glob 向流（stream）中添加文件。新加入的文件只对后续的转换可用。如果 [glob 匹配的文件与之前的有重复](https://www.gulpjs.com.cn/docs/getting-started/explaining-globs#overlapping-globs)，仍然会再次添加文件。

这对于在添加普通的 JavaScript 文件之前先转换部分文件的场景很有用，添加新的文件后可以对所有文件统一进行压缩并混淆（uglifying）。













 