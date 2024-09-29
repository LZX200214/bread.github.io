---
title: 主界面导航栏跳转
date: 2024-09-27 10:53:20
tags:
---
### 创建主界面导航栏
 **在如下图选中的文件中编写如下代码：** 

![输入图片说明](https://foruda.gitee.com/images/1727600105568320109/45728243_14874917.png "屏幕截图")
```
 {
      "name": "tabContent_one",
      "value": "message"
    },
    {
      "name": "tabContent_two",
      "value": "favorite"
    },
    {
      "name": "tabContent_three",
      "value": "profile"
    }
```
 **在如下图选中的文件中编写如下代码： ** 
![输入图片说明](https://foruda.gitee.com/images/1727600220031929254/1d7fe6ab_14874917.png "屏幕截图")

```
 {
      "name": "tabContent_one",
      "value": "消息"
    },
    {
      "name": "tabContent_two",
      "value": "收藏"
    },
    {
      "name": "tabContent_three",
      "value": "我的"
    }
```


### 在如下图中的位置view中新建一个视图组件MessageListView
![](https://foruda.gitee.com/images/1727599373097610480/51b50f07_14874917.png "屏幕截图")
#### 编写的代码如下：

```
import { Banner } from '../component/Banner'
import { EnablementView } from './EnablementView'
import { TutorialView } from './TutorialView'

@Component
export struct MessageListView {
  build() {
    Column(){
      Text("第二页面")
        .fontSize(20)
        .fontColor(Color.Black)
        .fontWeight(FontWeight.Bold)
        .textAlign(TextAlign.Start)
        .padding({left:16})
        .lineHeight(33)

      Scroll(){
        Column(){
          Banner()
          EnablementView()
          TutorialView()
        }
      }
      .layoutWeight(1)
      .scrollBar(BarState.Off)
      .align(Alignment.TopStart)
    }
    .width('100%')
    .backgroundColor('#F1F3F5')
    .height('100%')
  }
}
```
 这段TypeScript代码定义了一个名为MessageListView的组件，它使用了某种类似于Flutter或类似框架的组件化构建方式，但具体的语法和API调用（如@Component装饰器、Column、Text、Scroll等）并不是标准TypeScript或React/Vue/Angular等常见前端框架的一部分。这很可能是某个特定框架（比如Baidu Comate，尽管其语法不完全符合已知的Baidu Comate文档）或者自定义的UI框架的语法。

 **下面是对这段代码的解释：** 
1. 这里是列表文本导入依赖：


- 从../component/Banner路径导入Banner组件。
- 从./EnablementView路径导入EnablementView组件。
- 从./TutorialView路径导入TutorialView组件。

2. 这里是列表文本定义MessageListView组件：


- 使用@Component装饰器标记MessageListView为一个组件。这个装饰器可能是框架提供的，用于声明一个类为可复用的UI组件。
- MessageListView组件通过build方法定义了其UI结构。

3. 这里是列表文本构建UI：


- 使用Column容器作为最外层容器，它可能是一个垂直布局的容器，用于包含其他子组件。
- 在Column内，首先放置了一个Text组件，显示文本“第二页面”，并设置了字体大小、颜色、粗细、对齐方式、内边距和行高等样式。
- 接着，定义了一个Scroll组件，它可能是一个滚动视图，用于在内容超出可视区域时提供滚动功能。
- 在Scroll内部，又定义了一个Column容器，用于垂直排列Banner、EnablementView和TutorialView三个组件。
- Scroll组件还设置了权重（layoutWeight(1)）、滚动条状态（scrollBar(BarState.Off)，可能是关闭滚动条）和对齐方式（align(Alignment.TopStart)，可能是顶部左对齐）。

4. 这里是列表文本设置外层Column的样式：

- 这里是列表文本外层的Column设置了宽度为100%（占满父容器宽度），背景颜色为#F1F3F5，高度为100%（占满父容器高度）。


### 下面这段TypeScript代码是为一个基于ArkUI或类似HarmonyOS UI框架的应用编写的，旨在创建一个带有标签页的MainPage。
 **在如下图中新建arKts页面MainPage** 
![输入图片说明](https://foruda.gitee.com/images/1727599234504315245/25a04d34_14874917.png "屏幕截图")
1. 这里是列表文本导入：从../view/MessageListView导入MessageListView组件。

2. 这里是列表文本MainPage组件：


- 使用@Entry和@Component装饰器标记为应用的入口页面和组件。
- 有一个响应式状态currentIndex用于跟踪当前选中的标签页索引。
- 有一个TabsController实例tabsController，但在这个代码片段中并没有看到TabsController类的定义，可能是在其他地方定义的。

3. 这里是列表文本tabBuilder方法：


- 使用@Builder装饰器（可能是自定义的或框架提供的），用于构建标签页的标题栏。
- 根据当前索引currentIndex和传入的索引index来切换选中的图片和文字颜色。
- 定义了点击事件来更新currentIndex并通知tabsController改变索引。

4. 这里是列表文本tabContentBuilder方法：


- 意图是构建标签页的内容，但实现上有误。它尝试在TabContent内部直接定义内容，但这种方式可能并不符合框架的要求。
- tabBar属性的使用也是不正确的，因为它通常用于Tabs组件，而不是TabContent组件。

5. 这里是列表文本build方法：


- 定义了MainPage的UI结构。
- 使用Tabs组件来创建标签页。
- 在Tabs内部，首先定义了一个默认的TabContent，其中包含了MessageListView，但紧接着尝试通过tabBuilder方法设置tabBar，这是不正确的，因为tabBar是Tabs的属性，不是TabContent的。
- 注释掉的代码展示了尝试为其他标签页定义内容的尝试，但同样存在tabBar使用错误的问题。
- this.tabContentBuilder的调用似乎是想为每个标签页定义内容，但这种方式在ArkUI或HarmonyOS的UI框架中通常不是直接支持的。标签页的内容应该通过Tabs组件的控制器和当前索引来动态渲染。

#### 代码如下

```
import { MessageListView } from '../view/MessageListView';

@Entry
@Component
struct MainPage {
  @State currentIndex: number = 0;
  private tabsController: TabsController = new TabsController();

  @Builder
  tabBuilder(title: Resource, index: number, selectedImg: Resource, normalImg: Resource) {
    Column(){
      Image(this.currentIndex == index? selectedImg : normalImg)
        .width('24vp')
        .height('24vp')
        .objectFit(ImageFit.Contain)
      Text(title)
        .margin({top: '4vp'})
        .fontSize('10fp')
        .fontColor(this.currentIndex == index? '#3388ff' : '#E6000000')
    }
    .justifyContent(FlexAlign.Center)
    .height('52vp')
    .width('100%')
    .onClick( ()=>{
      this.currentIndex = index;
      this.tabsController.changeIndex(this.currentIndex)
    } )
  }
  @Builder
  tabContentBuilder(text: Resource,index: number, selectedImg: Resource, normalImg: Resource){
    TabContent() {
      Row(){
        Text(text)
          .height(300)
          .fontSize('30fp')
      }
      .width('100%')
      .justifyContent(FlexAlign.Center)
    }
    .padding({left:'12vp', right:'12vp'})
    .backgroundColor(Color.White)
    .tabBar(this.tabBuilder(text, index , selectedImg,normalImg))
  }

  build() {
  Tabs({
    barPosition: BarPosition.End,
    controller: this.tabsController
  }) {
    TabContent() {
      MessageListView()
    }
    .backgroundColor(Color.White)
    .tabBar(this.tabBuilder($r('app.string.tabContent_one'), 0, $r('app.media.media_activeMessage'), $r('app.media.media_message')))


    // this.tabContentBuilder($r('app.string.tabContent_one'), 0, $r('app.media.media_activeMessage'), $r('app.media.media_message'))
   // TabContent(){
   //   Text('收藏')
   //     .fontSize(20)
   // }
   // .backgroundColor(Color.Black)
   // .tabBar(this.tabBuilder($r('app.string.tabContent_two'), 1, $r('app.media.media_activeMessage'), $r('app.media.media_message')))

    // TabContent(){
    //   // MessageListView()
    //   Text('我的')
    //     .fontSize(20)
    // }
    // .backgroundColor(Color.Blue)
    // .tabBar(this.tabBuilder($r('app.string.tabContent_three'), 2, $r('app.media.media_activeMessage'), $r('app.media.media_message')))

    this.tabContentBuilder($r('app.string.tabContent_two'), 1, $r('app.media.media_activeStar'), $r('app.media.media_star'))
    this.tabContentBuilder($r('app.string.tabContent_three'), 2, $r('app.media.media_activePeople'), $r('app.media.media_people'))
  }
  .width('100%')
  .backgroundColor('#F3F4F5')
  .barHeight('52vp')
  .barMode(BarMode.Fixed)
    .onAnimationStart((index: number, targetIndex:number)=>{
      this.currentIndex = targetIndex;
    })
  }
}
```
 **效果图如下所示：** 
![输入图片说明](https://foruda.gitee.com/images/1727599680832336615/832162e0_14874917.png "屏幕截图")
![输入图片说明](https://foruda.gitee.com/images/1727599689780468851/88fc671b_14874917.png "屏幕截图")
![输入图片说明](https://foruda.gitee.com/images/1727599701192321620/98f0869c_14874917.png "屏幕截图")
