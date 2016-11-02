目录
================
+ [1、背景介绍](#1)

+ [2、搭建RN开发环境](#2)
  * [2.1 Mac环境](#2.1)
  * [2.2 Windows环境](#2.2)
  
+ [附录、相关学习资源](#666)
  * [教程](#666.1)
  * [开源APP](#666.2)
  * [图书](#666.3)
  * [优秀开源组件（库）](#666.4)
  * [工具](#666.5)
  * [资源网站及社区](#666.6)
  * [业内讨论与对比](#666.7)




<h1 id="1"></h1>
1、背景介绍
-------------
[React Native](https://facebook.github.io/react-native/)(下文简称'RN')是facebook开源的前端框架，一套代码(80%以上复用代码)能运行于双平台上，真正拥有了 Hybrid 框架的所有优势，且拥有更接近于native的显示效果。

RN 的目标不是 “Write once, run anywhere"，因为不同平台的差异是客观存在的，设计风格也各有不同。它要做的，是 “Learn once, write anywhere”。React 里的 view 不仅可以是 DOM，也可以是 iOS 控件或者 Android 控件，不论是什么平台，都能 “Build In React”。

目前RN在[Github](https://github.com/facebook/react-native)上有近4w的star数目，国外使用这项技术的包括Facebook公司本身的Facebook和Facebook Groups，国内使用这项技术的APP包括京东商场APP，腾讯的手机QQ、 QQ空间、 QQ音乐， 阿里的天猫，阿里在此基础上开源了自己的[weex](https://github.com/alibaba/weex) 。

<h1 id="2"></h1>
2、搭建RN开发环境
-----------------
<h2 id="2.1"></h2>
### 2.1 Mac环境
**Homebrew**
Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件。
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

**Node**
React Native需要NodeJS 4.0或更高版本。使用Homebrew来安装[Node.js](https://nodejs.org/en/).
```
brew install node
```

安装完node后建议设置npm镜像以加速后面的过程（或使用科学上网工具）。
```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```

**RN的命令行工具**
React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。
```
npm install -g react-native-cli
```

如果你看到EACCES: permission denied这样的权限报错，那么请参照上文的homebrew译注，修复/usr/local目录的所有权：
```
sudo chown -R `whoami` /usr/local
```

**Flow**
[Flow](https://www.flowtype.org/)是一个静态的JS类型检查工具。你在很多示例中看到的奇奇怪怪的冒号问号，以及方法参数中像类型一样的写法，都是属于这个flow工具的语法。这一语法并不属于ES标准，只是Facebook自家的代码规范。所以新手可以直接跳过（即不需要安装这一工具，也不建议去费力学习flow相关语法）。
```
brew install flow
```

**Python**
注意目前不支持Python 3或更高版本。

**Nuclide**

[Nuclide](https://nuclide.io/)是由Facebook提供的基于atom的集成开发环境，可用于编写、[运行](https://nuclide.io/docs/platforms/react-native/#running-applications)和 [调试](https://nuclide.io/docs/platforms/react-native/#debugging)React Native应用。[Nuclide的入门文档。](https://nuclide.io/docs/quick-start/getting-started/)

**Watchman**
[Watchman](https://facebook.github.io/watchman/docs/install.html)是由Facebook提供的监视文件系统变更的工具。安装此工具可以提高开发时的性能（packager可以快速捕捉文件的变化从而实现实时刷新）。
```
brew install watchman
```

<h2 id="2.2"></h2>
### 2.2 Windows环境
通用软件类似Mac系统下。


<h1 id="666"></h1>
附录、相关学习资源
--------------------
<h2 id="666.1"></h2>
### 教程
#### React Native
[构建Facebook F8 2016 App / React Native 开发指南](https://f8-app.liaohuqiu.net/)       
[React Native入门指南](https://github.com/vczero/react-native-lesson)      
[30天学习React Native教程](https://github.com/fangwei716/30-days-of-react-native)     
[React Native 官方api文档](http://facebook.github.io/react-native/docs/getting-started.html)    
[React Native中文文档(极客学院)](http://wiki.jikexueyuan.com/project/react-native/)     
[React Native中文文档(react native中文网，人工翻译，官网完全同步)](http://reactnative.cn/docs/0.36/getting-started.html)    

#### React.js
[react.js中文文档](http://reactjs.cn/)    
[react.js入门教程(gitbook)](https://hulufei.gitbooks.io/react-tutorial/content/introduction.html)    
[react.js快速入门教程 - 阮一峰](http://www.ruanyifeng.com/blog/2015/03/react.html)   

#### ECMAScript
[深入浅出ES6（一）：ES6是什么](http://www.infoq.com/cn/articles/es6-in-depth-an-introduction)    
[深入浅出ES6（二）：迭代器和for-of循环](http://www.infoq.com/cn/articles/es6-in-depth-iterators-and-the-for-of-loop)  
[深入浅出ES6（三）：生成器 Generators](http://www.infoq.com/cn/articles/es6-in-depth-generators)   
[深入浅出ES6（四）：模板字符串](http://www.infoq.com/cn/articles/es6-in-depth-template-string)  
[深入浅出ES6（五）：不定参数和默认参数](http://www.infoq.com/cn/articles/es6-in-depth-rest-parameters-and-defaults)   
[ECMAScript 6入门 - 阮一峰](http://es6.ruanyifeng.com/)   

#### 系列教程
[深入浅出React（一）：React的设计哲学 - 简单之美](http://www.infoq.com/cn/articles/react-art-of-simplity)   
[深入浅出React（二）：React开发神器Webpack](http://www.infoq.com/cn/articles/react-and-webpack)    
[深入浅出React（三）：理解JSX和组件](http://www.infoq.com/cn/articles/react-jsx-and-component)   
[深入浅出React（四）：虚拟DOM Diff算法解析](http://www.infoq.com/cn/articles/react-dom-diff)    
[深入浅出React（五）：使用Flux搭建React应用程序架构](http://www.infoq.com/cn/articles/react-flux)   
[React Native探索（一）：背景、规划和风险](http://www.infoq.com/cn/articles/react-native-overview)   
[React Native探索（二）：布局篇](http://www.infoq.com/cn/articles/react-native-layout)  
[React Native探索（三）：与 react-web 的融合](http://www.infoq.com/cn/articles/react-native-web)  

<h2 id="666.2"></h2>
### 开源APP
[官方演示App](https://github.com/facebook/react-native/tree/master/Examples)  
[Facebook F8 App](https://github.com/fbsamples/f8app)  
[奇舞周刊 iOS 版（上架应用）](https://github.com/fakefish/Weekly75)   
[HackerNews-React Native](https://github.com/iSimar/HackerNews-React-Native)  
[知乎专栏app](https://github.com/LeezQ/react-native-zhihu-app)  
[开源中国的Git@OSC客户端](http://git.oschina.net/rplees/react-native-gitosc)  
[数独 - 重拾纯粹数独的乐趣](https://github.com/nihgwu/react-native-sudoku)   
[React Native-gitfeed - GitHub客户端](https://github.com/xiekw2010/react-native-gitfeed)  


<h2 id="666.3"></h2>
### 图书
《React Native入门与实战》
《React Native开发指南》
《React Native跨平台移动应用开发》✨推荐✨
《React Native：用JavaScript开发移动应用》

<h2 id="666.4"></h2>
### 优秀开源组件（库）
[React Native组件库（比较全的组件库）](https://js.coach/)   
[最佳轮播类组件](https://github.com/leecade/react-native-swiper)   
[下拉刷新组件](https://github.com/jsdf/react-native-refreshable-listview)    
[模态框](https://github.com/brentvatne/react-native-modal)   
[HTML显示组件](https://github.com/jsdf/react-native-htmlview)  
[Material React Native (MRN) - Material Design组件库](https://github.com/react-native-community/React-Native-Elements) 
[React Native-Elements - React Native样式组件库](https://github.com/react-native-community/React-Native-Elements)   
[Shoutem UI - React Native样式组件库](https://github.com/shoutem/ui) 
[React Native组件库（又是一个组件库）](https://react.parts/native) 

<h2 id="666.5"></h2>
### 工具
[React Native-snippets(代码提示)](https://github.com/Shrugs/react-native-snippets)    
[React Native-babel(使用ES6+)](https://github.com/roman01la/react-native-babel)  
[sqlite for React Native](https://github.com/almost/react-native-sqlite)   
[Pepperoni - React Native项目初始化套件](https://github.com/futurice/pepperoni-app-kit)  
[ignite - React Native CLI项目生成器](https://github.com/infinitered/ignite)   
[Deco IDE - React Native IDE](https://www.decosoftware.com/)  

<h2 id="666.6"></h2>
### 资源网站及社区
[React Native官网](http://facebook.github.io/react-native/)  
[React-China社区](http://react-china.org/)   
[React Native中文社区](http://bbs.reactnative.cn/)   
[Use React Native 资讯站(使用技巧及新闻)](http://www.reactnative.com/) 
[稀土掘金](http://gold.xitu.io/tag/React%20Native)  

<h2 id="666.7"></h2>
### 业内讨论与对比
[跨平台开发时代的 (再次) 到来？（ Xamarin，NativeScript 和 React Native 对比）](https://onevcat.com/2015/03/cross-platform/)    
[谈谈 React Native - 唐巧](http://blog.devtang.com/2015/02/01/talk-about-react-native/)  
[如何评价React-Native?](https://www.zhihu.com/question/27852694/answer/43990708)    
[React Native概述：背景、规划和风险](http://div.io/topic/938)   
[Native与Web的融合 - Qcon中React-Native演讲](http://www.infoq.com/cn/presentations/the-fusion-of-native-and-web)  
[Weex & ReactNative & JSPatch大对比](http://awhisper.github.io/2016/07/22/Weex-ReactNative-JSPatch/)  
[Weex&ReactNative对比](https://zhuanlan.zhihu.com/p/21677103)   
[React Native一周年回顾--Martin Konicek](http://www.jianshu.com/p/8714243d07a5)   
