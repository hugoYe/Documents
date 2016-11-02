目录
================
+ [1、背景介绍](#1)

+ [2、搭建RN开发环境](#2)
  * [2.1 Mac环境](#2.1)
  * [2.2 Windows环境](#2.2)
  
+ [附录、相关学习资源](#666)




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
