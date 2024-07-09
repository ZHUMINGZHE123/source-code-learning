### 1、Node.JS

##### 1.1、node在执行js脚本时，V8和libuv如何为node提供支持

- 初始化环境：注册一系列的C++模块

- 创建libuv的消息循环：这个消息循环会伴随着整个应用的生命周期，直到运行线程退出。libuv内部持有一个对象，当用户的代码开始读取文件或发起网络请求时，Node.js就会给这个对象增加一个cb，libuv的消息循环会不断地遍历这个对象上的cb，在适当时机执行

- 创建V8运行环境：这是一个拥有自己栈的隔离环境。

- 绑定底层模块：Node会使V8引擎执行一个内置的js脚本（node_bootstrap.js），负责绑定Node初始化时注册的C++模块。

- 读取并执行js文件的内容

##### 1.2、初始化时注册的C++模块是如何绑定（被JS调用）
- Binding、LinkedBinding和Internal-Binding，这三种方式使用V8公开的C++ API将原生方法转换成可被JavaScript代码使用的方法

``` cpp
// C++里定义
Handle<FunctionTemplate> Test = FunctionTemplate::New(cb); // cb是一个C++的方法指针
global->Set(String::New("yourCppFunction"), Test);
```
```js
// js里使用
var test = new yourCppFunction();
```

- InternalBinding绑定内部私有的C++ Api

- LinkedBinding绑定开发者自己实现的C++ Api，Electron内部也大量使用了这种绑定形式


### 2、源码结构

- build：Electron构建脚本。

- buildflags：编译配置文件，用于构建Electron工程时裁剪掉不必要的模块，比如pdf_viewer、color_chooser、spellchecker、dark_mode_window等

- chromium_src：部分从chrome内核中复制来的代码。

- default_app：Electron提供的默认应用，node_modules依赖包的这个路径(node_modules\electron\dist\resources\default_app.asar)下的文件就是它编译的结果。默认情况下展示的界面也是这个default_app.asar

- docs：文档目录

- lib：ts源码，提供JS API，比如app、ipcMain、screen等等

- npm：npm相关配置

- patches：补丁文件，在编译Electron源码前，补丁工具会使用这些文件来
修改Node.js、Chromium以及V8等项目的代码，使这些项目能正常被Electron所集成

- script：此目录下存放编译所需的工具脚本，上面所述的执行补丁文件的脚本也被存放在此目录下。

- shell：此目录下存放了一系列的C++文件，这是Electron所有API的原生支持代码，子目录的结构也与lib目录一一对应。Electron的入口函数也存放在目录shell\app\electron_main.cc下，如果读者要分析Electron的源码，可以从这个文件开始

##### 补丁代示例（Electron的编译脚本通过这种方式修改依赖项目的源码）
![补丁代示例](01-1.png)