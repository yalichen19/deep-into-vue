# 深入理解Vue
源码学习包括但不限于：

- 整体文件架构

- 设计模式

- 奇技淫巧


整体架构学习和大知识点会在文档列出，细节的学习会通过在源码中添加注释完成

# 起步
从 package.json 文件入手，查看打包指令

`"dev": "rollup -w -c scripts/config.js --environment TARGET:web-full-dev",`

查看scripts/config.js文件中的web-full-dev配置，找到入口文件，位于src/platforms/web/entry-runtime-with-compiler.js

`entry: resolve('web/entry-runtime-with-compiler.js')`

继续追寻，查找Vue对象引用源头
``` 
import Vue from './runtime/index'     // entry-runtime-with-compiler.js
import Vue from 'core/index'          // runtime/index.js
import Vue from './instance/index'    // core/index.js
``` 
最终找到Vue构造函数
``` 
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' &&
    !(this instanceof Vue)
  ) {
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  this._init(options)
}

initMixin(Vue)          // 原型注入_init方法
stateMixin(Vue)         // 原型注入$data、$props 对象
eventsMixin(Vue)        // 原型注入$on、$once、$off、$emit方法
lifecycleMixin(Vue)     // 原型注入_update、$forceUpdate、$destroy方法
renderMixin(Vue)        // 原型注入$nextTick、_render方法
``` 
可以看出，构造函数中没有什么关键代码，之后几个方法的调用真正给Vue添加了关键属性和方法

接下来就是对每个方法单独分析

# 其他相关知识点
task 和 microtask 讲解文章 [Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/). 
