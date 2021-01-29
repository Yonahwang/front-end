<!--
 * @Description: 
 * @Version: 2.0
 * @Autor: fengjiao
 * @Date: 2020-06-28 17:49:38
 * @LastEditors: fengjiao
 * @LastEditTime: 2020-06-28 18:20:19
--> 

# 基于Vue来解析页面在浏览器中呈现的过程中所涉及到的知识点


此篇文章是基于Vue来分析一下从编辑好代码->打包->将dist文件夹放置到服务器上->访问该网址->页面呈现。此过程所涉及到的知识点，本篇很少涉及到具体的代码，主要是理论上的知识，如有错误，烦请各位在评论区指出。


## 概念
- Vue-cli(快速建立开发环境的脚手架)
- webpack(模块打包工具)
-  Vue-loader(将单文件组件.vue解析成JS代码形式)
- 单页面应用与多页面应用(SPA与MPA)
- 浏览器进程(包括JS脚本执行、HTML/CSS的渲染)
- JS、CSS的加载与阻塞
- 路由(封装好的Vue-Router)
- 动态加载JS脚本
1. 在我们初学Vue的时候，多数人是在html文件中创建 <script></script> 标签进行代码的编写，所有我们所需要的组件都在一个或多个scrpit标签下进行注册。但是在真实项目中，我们会编写成千上万行代码，如果都放在一个文件下，那样会导致可读性十分地差。所以Vue官方推出了Vue-cli(cli = Command-Line Interface = 命令行界面)， 通过在命令行窗口中输入某条命令，即可创建一个真实项目中所需要的元素(包括目录、主文件入口、根组件、开发/生产环境的默认配置…)，这样我们只需要在对应目录编写代码，通过import/export引入对应模块，修改配置，再输入自带的命令，就可以生成一个页面，并且代码可读性十分强。
   
2. 其实在Vue-cli中已经集成了webpack，我们可以通过配置webpack.xxx.conf.js文件，按照你的需求进行打包。而为什么我们需要webpack这个打包工具呢？是因为在项目中， 我们需要有模块化的思想，将不同功能的模块分到不同的文件中，将一些公共资源如公共样式，背景图片等都提取出来。 关于webpack的原理、loader、plugin等在此处先不做解析(因为还不会…)。
   
3. .vue 单文件组件中包含 
   ```html
   <template></template>
   <script></script>
   <style></style>
   ```
    而浏览器是无法解析 .vue 文件的，所以我们需要一种工具将其转换为浏览器可以解析的JS代码，这个就是Vue-loader的作用，具体的原理仍然不做解析(还没有读懂源码…)
4. 单页面应用和多页面应用的区别:
    > 单页面是指只有一个主页面的应用，一开始只需加载一次js、css等相关资源。所有的内容都包含在主页面，对每一个功能模块组件化。单页应用跳转，就是切换相关组件，仅刷新局部资源。
多页面是指有多个独立的页面的应用，每个页面必须重复加载js,、css等相关资源。多页应用跳转，需要整页资源刷新。

5. 浏览器进程： **浏览器是多进程的、浏览器渲染进程是多线程的**
- 那么浏览器包括哪几个进程呢？
   - Browser进程：浏览器的主进程，只有一个。
      1. 负责浏览器界面的显示，与用户交互。比如前进、后退等
      2. 负责各个页面的管理，创建、销毁其余进程
      3. 网络资源的下载与管理
      4. 将渲染进程内部的渲染树绘制到用户界面上
    - 第三方插件进程：每种类型的插件对应一个进程，当使用该插件时才创建
    - GPU进程：进行3D CSS的渲染等
    - 浏览器渲染进程(浏览器内核、Render进程，内部是多线程的)：默认每个Tab页面都有一个，互不影响。功能：页面渲染、脚本执行、事件处理
  

- 那么浏览器渲染进程包括哪几个线程的呢？

    1. GUI渲染线程：
        - 负责渲染浏览器界面， 解析HTML、CSS，构建DOM树和Render树，进行布局与绘制
        - 当界面需要 重绘时或者由于某种操作引发回流 时，该线程就会执行
        - GUI渲染线程与JS引擎线程是互斥的 (十分重要)，当JS引擎执行时GUI渲染线程会被挂起，GUI更新会被保存在一个队列中等到JS引擎空闲时被立刻执行
    2. JS引擎线程
       - 也称为JS内核，负责 处理JavaScript脚本程序 。(例如我们熟悉的V8引擎)
       - JS引擎 一直等待着任务队列中任务的到来，然后加以处理 ，一个Tab页（Renderer进程）中无论什么时候都只有一个JS线程在运行JS程序
       - GUI渲染线程与JS引擎线程是互斥的，所以如果JS执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞。
    3. 事件触发线程
        - 归属于浏览器而不是JS引擎，用来控制事件循环 (事件队列)
        - ****当JS引擎执行代码块如setTimeout时(也可以是来自浏览器内核的其他线程，如DOM事件、Ajax异步请求等)，会将对应任务添加到事件触发线程中
        - 当对应的事件符合触发条件时会被触发， 该线程会把事件添加到待处理队列的队尾，等待JS引擎的处理
        - 由于JS的单线程关系，所以这些待处理队列中的事件都得排队等待JS引擎处理（当JS引擎空闲时才会去执行）
    4. 定时触发器线程
        - setInterval与setTimeout所在线程
        - 浏览器定时计数器并不是由JavaScript引擎计数的，（因为JavaScript引擎是单线程的，如果处于阻塞线程状态就会影响计时的准确） 因此通过单独线程来计时并触发定时（计时完毕后，添加到事件队列中，等待JS引擎空闲后执行）
        - W3C在HTML标准中规定，规定要求setTimeout中低于4ms的时间间隔算为4ms。
    5. 异步http请求线程
        - 在XMLHttpRequest连接后通过浏览器新开一个线程发送请求
        - 检测到状态变更时，如果设置有回调函数，异步线程就产生状态变更事件， 将这个回调再放入事件队列中进行注册，再由JavaScript引擎执行。
    6. JS、CSS的加载与阻塞：其实这一部分就是我们上面所说的GUI渲染线程与JS引擎线程的互斥。由于JavaScript是可操纵DOM的，如果在修改这些元素属性同时渲染界面(即JS线程与GUI线程同时运行)，那么渲染线程前后获得的元素就可能不一致了。接下来简单说一下浏览器渲染流程：
        - 解析HTML建立DOM树
        - 解析CSS与DOM树结合形成Render树
        - 布局Render树(Layout/Reflow)，负责各元素尺寸、位置的计算
        - 绘制Render树(Paint)，绘制页面像素信息
        - 浏览器将各层的信息发送给GPU，GPU会将各层合成(composite)，显示在屏幕上


现在具体来说一下在代码中JS、CSS阻塞的应用：`
首先先来明确几个概念：

    - CSS加载不会阻塞DOM树解析
    - CSS加载会阻塞Render树的渲染
    - CSS会阻塞JS执行
    - JS会阻塞DOM树解析
    - DOMContentLoaded事件
1. 当初始的HTML文档被完全加载和解析完成之后，DOMContentLoaded事件被触发，而无需等待样式表、图像和子框架的完成加载。DOMContentLoaded事件必须等待其所属script之前的样式表加载解析完成才会触发
2. load事件 当onload事件触发时，页面上所有的DOM、样式表、脚本、图片都已经加载完成了
3. CSS是由单独的下载线程异步下载的
4. DOMContentLoaded事件只等待其所属的script之前的样式表
5. 当解析起遇到script标签时，文档会立即停止解析直到脚本执行完毕，如果脚本是外部的，那么解析过程会停止，直到从网络同步抓取资源完成后再继续。因为我们的脚本会操作DOM，所以在脚本跑完之前浏览器不知道脚本会把DOM改成什么样，所以就等脚本执行完再解析DOM
6. async模式下，JS不会阻塞浏览器做其他事情，它的加载是异步的，当它加载结束后，JS脚本会立即执行。
7. defer模式下，JS加载是异步的，执行是被推迟的。等整个文档解析完成、DOMContentLoaded事件即将被触发时，被标记了defer的JS文件才会开始依次执行
8. 从应用的角度来说，一般当我们的脚本与DOM元素和其它脚本之间的依赖关系不强时，我们会选用async；当脚本依赖于DOM元素和其它脚本的执行结果时，我们会选用defer。一句话，defer是”解析完DOM再执行”，async是”下载完就执行”。另外，如果有多个defer脚本，会按照它们在页面出现的顺序加载，而多个async脚本是不能保证加载顺序的。
9. preload属性与prefetch属性：
    - preload(预加载)： \<link> 元素的rel属性的属性值preload能够让你在你的HTML页面中 \<head> 元素内部书写一些声明式的资源获取请求， 可以指明那些资源是在页面加载完成后即刻需要的 。对于这种即刻需要的资源，你可能希望在页面加载的生命周期的早期阶段就开始获取，在浏览器的主渲染机制介入前就进行预加载。 这一机制使得资源可以更早的得到加载并可用，且更不易阻塞页面的初步渲染，进而提升性能。
    - 只是预加载，并不运行。
    - 用下面两种方法即可加载完运行
        预加载CSS
        ```html
        <link rel="preload" as="style" href="https://www.tuicool.com/articles/B7rAjun/async_style.css" onload="this.rel='stylesheet'">
        ```
        预加载js
        ```html
        <link rel="preload" as="script" href="https://www.tuicool.com/articles/B7rAjun/async_script.js" onload="var script = document.createElement('script');script.src = this.href; document.body.appendChild(script);">
        ```
    - prefetch(预获取)：为了提示浏览器，用户 未来的浏览有可能需要加载目标资源 ，所以浏览器有可能通过 事先获取和缓存对应资源，优化用户体验。
- 路由(Vue-router)：在我们的单页面应用中，切换页面是通过改变URL从而改变对应的组件来实现局部刷新，而这里的URL可以等同于路由。路由可以分为前端路由和后端路由，在这里说的是前端路由。前端路由的实现主要基于两种：
    1. hash
    2. history API

- 最后就是动态加载JS了，因为单页面应用在页面初始化时会将所有组件的内容都以一个JS文件的形式进行加载，当文件过大时，会造成加载时间过长，首页白屏，用户体验差，所以呢，我们采用路由懒加载的技术，将不同路由对应的组件拆分成不同的JS文件，当我们访问对应路由时才去访问对应的JS文件，而这里就设计到动态加载JS了，其实这里webpack内部有一个插件已经帮我们实现了，当访问某路由时，会解析JS文件并且将对应的组件挂载到容器下。
## 结论
最后，再整体地回顾一下流程。 当运行npm run build命令时，会将所编写好的文件进行打包生成dist文件夹，里面包含着index.html、static-css、js、img、fonts文件夹，index.html就是我们访问的主页面，在 \<head> 标签中通过 \<link> 引入css文件，在 \<body> 有一个 \<div id='app'> ，这个就是我们的主容器，而在下面有各种 \<script src='https://www.tuicool.com/articles/B7rAjun/xxx'> ，通过脚本的形式动态地将各组件/页面挂载到容器下，在切换路由的时候，动态引入 <script> 标签，再动态引入脚本动态挂载对应组件。