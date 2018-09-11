### 过程描述

前天交给我一个开发任务：一个页面，一天时间，我信誓旦旦说可以做完。
昨天上午来的有些晚，来了就开始开发。

这个页面的图片是这样的：

![](https://upload-images.jianshu.io/upload_images/5077517-b362f04710d4faf1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`布局`是这样；`显示逻辑`是刚进来的时候有一个浇水动画，同时选中树的状态的环会有一个动画效果；`交互逻辑`只有点击分享按钮可以分享到家长端app一个h5页面；`业务逻辑`是右侧和左侧的文案根据奖章数量变化。

理清了思路（其实理清思路的过程还挺长，需求文档写的需求点重复而且需求点之间没有结构化和顺序可言），开始动手。

设计稿是这样:

![](https://upload-images.jianshu.io/upload_images/5077517-b974b40ab6f4903e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我是第一次接触到这样的设计稿，一个静态的图片，标记好的尺寸，配套的图片资源，缺了什么找ui要。其实这样很容易有一些没有标出的东西需要自己去量，下载ps的过程挺漫长，最终也没装好，放弃了。

之前页面组件存在的问题是没有做任何组件划分，看组件代码只能看到一坨html，并不能一眼看清结构，比如这样：

![](https://upload-images.jianshu.io/upload_images/5077517-d1b31d7d8ac5403e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这种代码看着特别的累，想理清那一块代码是界面上的哪一块要花很多的时间。所以，我先划分了组件，是这样划分的。

![](https://upload-images.jianshu.io/upload_images/5077517-ec68dcdec82c61c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

文件有这些：

![](https://upload-images.jianshu.io/upload_images/5077517-2163e6bf07753753.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分享按钮在components这个目录下，因为它被多个页面用到了。

首先是左边的ResultTreeInfo组件，他的特点是 树的图片，树的状态的图片以及下面的文案，都是根据树的状态来联动变化的。所以我是这么封装。

![](https://upload-images.jianshu.io/upload_images/5077517-1cc2194120997568.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/5077517-654cf0420cf9848e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

treeInfo封装treeImg、 statusImgs、 texts这三种资源的所有情况，每一个都是数组存放。然后模板里循环渲染所有的资源。并且根据treeStatus和msgIndex显示对应的资源，treeStatus是树的状态，这个需要请求后端接口，根据奖章数、冰冻次数等来确定（还没写完）。msgIndex设计为computed是因为他是根据一些其他状态的数据来联动修改（从服务端接口获取的数据）。isShowTreeImg和isShowTreeMsg是选中对应资源的逻辑。

右边的ResultList组件，是做布局以及提供ResultListItem渲染所需要的参数。

![](https://upload-images.jianshu.io/upload_images/5077517-43101f0ea10812a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/5077517-26a8c9daa33d0a1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

渲染的treeInfo、medalInfo、iceInfo放在state里，因为是相同的结构，所以提供了resultList这个computed属性来简化渲染逻辑。

然后是ResultListItem组件，组件的封装和类或者其他东西的封装一样，都是分离变与不变，不变的部分封装到内部，变的部分通过props、events、slot等暴露出去。

![](https://upload-images.jianshu.io/upload_images/5077517-6df567bae7ba9cb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里不变的是布局，变的是icon、iconText、numText以及numText里面用到的数字，所以我暴露了这四个参数。

![](https://upload-images.jianshu.io/upload_images/5077517-04931c86c45006ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中icon提供了枚举值，文本提供了占位符：

![](https://upload-images.jianshu.io/upload_images/5077517-ece6152c104eb8dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体的渲染的时候，用的是根据参数处理之后的数据：


![](https://upload-images.jianshu.io/upload_images/5077517-87b56b3112fd7f3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/5077517-d2641075a3a301a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

numInfo设计成这样是因为，文本中数字的位置不确定，所以通过占位符 + 替换的数字 的方式来做的。

### 现状

昨天做了一天，没做完，没做完的地方如下：

1，组件：分享组件的封装。
2.  显示逻辑：进入页面的浇水动画效果
3. 交互逻辑：分享组件的分享功能
4. 业务逻辑： ResultTreeInfo的treeStatus和msgIndex的计算，ResultList的  treeInfo、medalInfo、iceInfo的计算。这些都是根据奖章数、冰冻次数等来计算的。
5. 课程结束时候的菜单栏和左侧快捷入口的路由替换和悬浮效果。

### 总结与反思

估了一天时间，结果没做完，差的还不少，对自己的很是不满意。分析了一下原因，至少这有几点吧。

- 看需求文档，开始没有理清思路，花了挺长时间
- 之前做react开发基于antd组件库，更多的是调整各种参数，js写得多，布局写的少，生疏了，所以布局写的挺费劲。
- 过程中的ps下载花了一些时间
- 调试的有些费劲，所以我把store和router挂到了window下

![](https://upload-images.jianshu.io/upload_images/5077517-0bae3a24889b750d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 因为没有理清需求，和对自己的过于自信，导致了估时间果断，2天也许更适合现在的自己。熟悉之后会快一些。

### 优化和建议

- 流程中没有 tech design（技术设计）的阶段，很多东西都是开发时候现想，这样挺容易出问题的。我们之前公司是有一个技术设计阶段，写技术设计的文档，里面带了估时，这样别人review方案可以进行改进，同时估时也更接近真实。

- 没有一些通用的组件的封装，比如Icon、ShareBtn这种，重复了很多次的，每次都要重新开发。

- 需求文档的需求点如果能按照顺序来会更好一些。

- 我自己要加强css的训练，以及tech design这个阶段的重视吧。效率还是不够高。

### 思考所得

- 页面组件 就是`布局 + 组件`， 布局的部分交给父组件， 组件负责具体每一块的布局，然后组件内部也是一样的 `布局 + 组件`，直到 `布局 + 元素`，这是递归的终止，这是开发一颗组件树的过程。布局部分如果用在了多个地方，应该单独抽取一个布局组件。

-  什么时候要封装组件，什么时候不封装组件呢。首先一个组件的模板或jsx应该是结构清晰的，这是基础，然后某一部分用到的次数如果大于1，就有封装的必要。比如布局，如果只用到了一次，那么就写在组件里，如果用到了多次，那么就应该单独抽取出来做一个布局组件。从一到多，就应该封装，就像类是对象的集合一样，组件类也是具体的页面区块的集合，一到多的变化就是写死到封装的变化。而列表组件天生就带了多的属性，所以listItem几乎是必封装的组件，不过还要根据listItem的复杂度来决定。 一和多是一个有趣的关系。



























