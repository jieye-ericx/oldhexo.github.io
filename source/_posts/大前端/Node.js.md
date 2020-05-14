---
toc: true
title: Node.js
categories: 
 - 笔记
tags:
 - 大前端
 - node
---
记录node学习
<!-- more -->
# process

**[文档](http://nodejs.cn/api/process.html)**

`process` 对象是一个全局变量，它提供有关当前 Node.js 进程的信息并对其进行控制。 作为一个全局变量，它始终可供 Node.js 应用程序使用，无需使用 `require()`。 它也可以使用 `require()` 显式地访问：

```js
const process = require('process');
```

## process.env

`process.env` 属性返回包含用户环境的对象。

此对象的示例如下所示：

```js
{
  TERM: 'xterm-256color',
  SHELL: '/usr/local/bin/bash',
  USER: 'maciej',
  PATH: '~/.bin/:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin',
  PWD: '/Users/maciej',
  EDITOR: 'vim',
  SHLVL: '1',
  HOME: '/Users/maciej',
  LOGNAME: 'maciej',
  _: '/usr/local/bin/node'
}
```

可以修改此对象，但这些修改不会反映到 Node.js 进程之外，或者（除非明确请求）反映到其他 [`Worker`](http://nodejs.cn/s/S15DqM) 线程。 换句话说，以下示例不起作用：

```console
$ node -e 'process.env.foo = "bar"' && echo $foo
```

以下示例则起作用：

```js
process.env.foo = 'bar';
console.log(process.env.foo);
```

在 `process.env` 上分配属性将**隐式地将值转换为字符串**。 不推荐使用此行为。 当值不是字符串、数字或布尔值时，Node.js 的未来版本可能会抛出错误。

```js
process.env.test = null;
console.log(process.env.test);
// => 'null'
process.env.test = undefined;
console.log(process.env.test);
// => 'undefined'
```

使用 `delete` 可以从 `process.env` 中删除属性。

```js
process.env.TEST = 1;
delete process.env.TEST;
console.log(process.env.TEST);
// => undefined
```

在 Windows 操作系统上，环境变量不区分大小写。

```js
process.env.TEST = 1;
console.log(process.env.test);
// => 1
```

除非在创建 [`Worker`](http://nodejs.cn/s/S15DqM) 实例时明确指定，否则每个 [`Worker`](http://nodejs.cn/s/S15DqM) 线程都有自己的 `process.env` 副本，基于其父线程的 `process.env`，或者指定为 [`Worker`](http://nodejs.cn/s/S15DqM) 构造函数的 `env` 选项的任何内容。 对于 `process.env` 的更改将在 [`Worker`](http://nodejs.cn/s/S15DqM) 线程中不可见，并且只有主线程可以进行对操作系统或本机加载项可见的更改。