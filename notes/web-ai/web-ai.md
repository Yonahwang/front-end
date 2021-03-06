# 从零开始，打造AI落地场景
## 1. 了解AI技术，构建应用场景
- 根据实际情况了解目前拥有的AI技术，构想可应用的场景来最优化的展示给用户。 
- 目前我们实验室相对成熟的AI技术有以下9个方面。
  - [x] 智能语音翻译机
  - [x] 智能人脸检测
  - [x] 智能人脸分析
  - [x] 智能物体检测
  - [x] 自然场景识别
  - [x] 肺炎辅助检测
  - [x] 智能语音识别
  - [x] 智能语音合成
  - [x] 智能文本翻译
  
## 2. 根据应用场景，规划落地方案
### 2.1 使用工具sketch
- 为什么使用Sketch ?
   > Sketch就是一款UI设计的工具，它把设计和代码优雅地结合起来。 

   > 我们在Sketch中画一个按钮，实际上是已经把按钮的css样式给做出来了：width、height、border-radius、border、background、font等等。通过拷贝可以获取对应的css样式代码，技术有时候可以直接用，设计与技术间的鸿沟浅了很多。

   > 还有一些Sketch插件，比如蓝湖，可以一键导出html标注，每个元素的css样式都可以一目了然，也是基于Sketch对UI界面的代码化。
   
   > 其中Sketch最为重要的功能就是symbol。一套界面有几十个按钮，如果修改，在PS里面非常麻烦，但是在Sketch中却轻而易举。实际上symbol可以粗略理解为一种css样式，牵一发而动全身，只要使用这个symbol的组件，更改了一个symbol后，其他都会更改，这就是前端中的css作用。

- [Sketch 中文网](http://www.sketchcn.com/)
- Sketch 下载地址[点击这里]()
- 由设计稿一键智能生成前端代码[imgcook](https://imgcook.taobao.org/)
    > imgcook 专注以 Sketch、PSD、静态图片等形式的视觉稿作为输入，通过智能化技术一键生成可维护的前端代码，包含视图代码、数据字段绑定、组件代码、部分业务逻辑代码等。目前此产品是阿里巴巴前端委员会智能化小组的服务化的内外落地产品。
   > - 100% 还原：对设计师而言，减少视觉走查成本。
   > - 100% 兼容：对测试而言，减少 UI 适配成本。
   > - 极速上线：最重要的是对前端而言，可自动生成视图代码、数据字段绑定、组件代码和部分业务逻辑代码；以天猫双十一会场为例，目前生成代码的可用率为 79.43%（imgcook 生成被保留上线后代码 / 总模块总线上代码）。
  

### 2.2 微信小程序展示
- 为什么要选择小小程序？ 
   > 移动化，轻量级，方便公司员工的对外宣传，更直观的告诉大家我们有的技术，速度快，免下载。
快速知道我们的AI能力，并解决实际问题

- [小程序开发指南](https://developers.weixin.qq.com/miniprogram/dev/framework/)

### 2.3 项目进度与代码管理
- Github 小程序git
  > 代码管理是为开发者提供的一项代码管理服务，方便微信开发者进行代码推送、拉取、版本管理和多人协作。
- 通过浏览器微信扫码登录：https://git.weixin.qq.com
   > Note下面可以清楚的看到小程序修改的分支，每一个分支中，对应同学修改的代码。在不同分支中进行修改代码互不影响，可以在修改完成后合并分支，这样大大优化了各个同学的开发效率与沟通合作，使产品更新迭代的速度加快。
  
   ![ninitool](./minitool.png)


## 3. 设计与实施
- 以下为几个使用sketch 设计实施的例子， 大多数的模块由图片组合和文字组成，经过拼接与设计，最后出成品图给到前端，进行功能的实现。
  
- 小程序开发主页

   ![小程序开发主页](./sketch1.png)
- 智能审核
  
   ![智能审核](./sketch2.png)

## 4. 测试与上线
- 产品组只成立一个月，在一周内做了两次大版本更新，，四次迭代，只有一个前端，不是因为我们有多聪明，而是我们新的方法理论，小幅迭代。
不是像大公司一样，做一个任务要排个队（急，紧急，十分紧急，十万火急）
- 实际开发的过程
   ![实际开发的过程](./miniapp1.png)

这个是目前已经发布的稳定版本的二维码，扫码体验。小程序架构理念（又轻又重）我们知道传统软件开发的模式，但是不适合现在的敏捷开发，不拘于于形势，直接反馈，PM也具有代码写作的能力。我们一周内出来了V2.0 版本。敏捷重要是小幅迭代。我们弱文档。

   ![小程序二维码](./miniapp.png)



## 5. 版本更新与迭代
- 新功能的添加
  > “云润AI体验中心” 小程序处于研发状态的还有 智能QA问答系统，人工智能量体裁衣，中山大膀胱癌预测，智能OA系统，智能考勤等。
- 前端框架的更新
  - Vue
    > 荐使用的是Vue 的框架，是一套构建用户界面的渐进式框架。
    - 为什么是Vue ?
    > 轻量级框架、简单易学、双向数据绑定、组件化、视图、数据和结构的分离、虚拟DOM、运行速度快。引入Vue 框架的主要优势是组件话，就是说把我们经常使用的一些功能模块可以使用新定义一些组件，这样就实现了代码复用，与代码简化。
  - uni-app
    > 如果要一套代码多端运行呢，还是需要使用uni-app 框架。是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、H5、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉）等多个平台。(基于通用的前端技术栈，采用vue语法+微信小程序api，无额外学习成本。)
    - [uni-app 官网](https://uniapp.dcloud.io/component/README)




～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～

# 课外补充
## 前端学习网站推荐

> 前端学习网站推荐   
      1. 极客标签：     http://www.gbtags.com/   
      2. 码农周刊：     http://weekly.manong.io/issues/   
      3. 前端周刊：     http://www.feweekly.com/issues   
      4. 慕课网：       http://www.imooc.com/   
      5. div.io：		 http://div.io   
      6. Hacker News： https://news.ycombinator.com/news   
      7. InfoQ：       http://www.infoq.com/   
      8. w3cplus：     http://www.w3cplus.com/   
      9. Stack Overflow： http://stackoverflow.com/   
      10. w3school：    http://www.w3school.com.cn/   
      11. mozilla：     https://developer.mozilla.org/zh-CN/docs/Web/JavaScript


## 文档推荐
- [jQuery 基本原理](https://docs.huihoo.com/jquery/jquery-fundamentals/zh-cn/index.html)

- [JavaScript 秘密花园](http://bonsaiden.github.io/JavaScript-Garden/zh/)

- [CSS参考手册](http://css.doyoe.com/)

- [JavaScript 标准参考教程](http://javascript.ruanyifeng.com/)

- [ECMAScript 6入门](http://es6.ruanyifeng.com/)