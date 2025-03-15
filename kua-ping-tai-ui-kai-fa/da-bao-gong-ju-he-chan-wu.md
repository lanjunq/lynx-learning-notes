---
icon: file-zipper
---

# 打包工具和产物



{% embed url="https://lynxjs.org/zh/rspeedy/" %}
官网
{% endembed %}



{% embed url="https://www.npmjs.com/package/@lynx-js/rspeedy" %}
NPM 包
{% endembed %}

> The Rspack-based build tool for Lynx.



写好的代码需要进行打包，以准备在最终设备上运行。原生开发需要编译 — 将代码及其依赖按照一定规则编译成机器能执行的代码。类比到 Web，就是把代码及其依赖转化成运行时能执行的代码。打包的过程可以包括：包含依赖代码，将新的语法规则转换成老的，打包静态资源，比如 TS -> JS，优化 (tree shaking/code splitting)。典型的工具包括 Webpack, Rollup, Vite, esbuild.&#x20;



前端打包工具在整个开发流程中的位置

<img src="../.gitbook/assets/file.excalidraw (2).svg" alt="" class="gitbook-drawing">

细节: lynx.config.ts 里面包含什么选项?



从原生 app 的角度来说，最终产物包含什么？



#### 深入研究构建产物

深入研究：如果用浏览器访问这个 URL，会得到什么呢？

会下载一个类型不明的文件。应该是 Lynx 自定义的文件格式，应该可以被 Lynx SDK 解析。用 text editor 打开会看到，最开始是乱码，后面是类似 JavaScript 的语法。看起来是某些产物打包到一起。

如果分析前端构建产物，会看到：

```
$ cd dist && tree -a
.
├── .rspeedy
│   ├── main
│   │   ├── background.js
│   │   ├── background.js.map  # JavaScritp Source Map (用于 Debug)
│   │   ├── debug-info.json    # lepusNG_debug_info ❓
│   │   ├── main.css           # 
│   │   ├── main.css.hot-update.json  # HMR 相关
│   │   └── tasm.json
│   └── main__main-thread
│       ├── main-thread.js      # 
│       └── main-thread.js.map  # JavaScritp Source Map (用于 Debug)
├── main.lynx.bundle
└── static
    └── image
        ├── arrow.aee54ba7.png
        ├── lynx-logo.620eb8d1.png
        └── react-logo.75eb3837.png
```

{% code overflow="wrap" %}
```
# main-thread.js 
main-thread.js 的内容很多，是因为是打包的结果，包含
1. 文件开头部分实现了一个模块加载和管理系统，类似于 webpack 的运行时。
2. 文件包含了 Preact（React 的轻量级替代品）的核心实现：虚拟 DOM 操作函数, 组件生命周期管理, diff 算法实现, DOM 渲染逻辑
3. 实现了常用的 React Hooks：useState, useEffect, useContext, useRef, useMemo, useCallback, useReducer, useLayoutEffect, useId, 等
4. 包含了你的应用主要代码：App 组件实现, 主入口点 (index.tsx), 相关样式处理
5. 大量代码用于实现热更新功能：模块热替换逻辑, 状态保持机制, 更新检测和应用
```
{% endcode %}

```
# background.js
```

```
# debug-info.json
```

```
# main.css
```

```
# tasm.json
```



