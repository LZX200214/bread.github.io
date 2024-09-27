---
title: UIAbility初始化以及Text控件内容修改
date: 2024-09-27 10:52:07
tags:
---
### 相关概念

- UIAbility：UIAbility组件是一种包含UI界面的应用组件，主要用于和用户交互。UIAbility组件是系统调度的基本单元，为应用提供绘制界面的窗口；一个UIAbility组件中可以通过多个页面来实现一个功能模块。每一个UIAbility组件实例，都对应于一个最近任务列表中的任务。
- 自定义组件的生命周期：自定义组件的生命周期回调函数用于通知用户该自定义组件的生命周期，这些回调函数是私有的，在运行时由开发框架在特定的时间进行调用，不能从应用程序中手动调用这些回调函数。
- 窗口开发指导：窗口模块用于在同一块物理屏幕上，提供多个应用界面显示、交互的机制。

### 创建UIAbility组件
![UIAbility组件](https://foruda.gitee.com/images/1725609260603116985/9d277d6d_14874917.png "屏幕截图")

### Text的使用

```
build() {
    Column() {
      Text($r('app.string.hello_message'))
        .fontSize(CommonConstants.DEFAULT_FONT_SIZE)
        .fontColor(this.textColor)
        .margin(CommonConstants.DEFAULT_MARGIN)
        .fontWeight(FontWeight.Bold)
      Text($r('app.string.class_message'))
        .fontSize(CommonConstants.DEFAULT_FONT_SIZE)
        .fontColor(this.textColor)
        .margin(CommonConstants.DEFAULT_MARGIN)
        .fontWeight(FontWeight.Bold)
    }
    .width(CommonConstants.FULL_WIDTH)
```
鸿蒙（HarmonyOS）应用开发中，ArkTS（Ark TypeScript）是一种基于TypeScript的声明式UI开发范式，它允许开发者以更简洁和直观的方式来构建跨设备的UI界面。在ArkTS中，文本显示主要通过`Text`组件来实现，该组件用于在界面上显示文本内容。

### 基本使用

在ArkTS中，你可以直接在页面的`.ets`文件中使用`Text`组件，并通过其属性来设置文本内容、样式等。以下是一个简单的示例，展示了如何在ArkTS中使用`Text`组件：

```typescript
// 在.ets文件中
@Entry
@Component
struct MyComponent {
    build() {
        // 使用Text组件显示文本
        Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
            Text('Hello, HarmonyOS!')
                .fontSize(24) // 设置字体大小
                .fontWeight(FontWeight.Bold) // 设置字体加粗
                .color(Color.Red) // 设置文本颜色
                .margin({ top: 10 }) // 设置外边距
        }
    }
}
```

### Text组件的属性

`Text`组件提供了丰富的属性来定制文本的外观，包括但不限于：

- `value`: 设置文本内容，类型为`string`。
- `fontSize`: 设置字体大小，类型为`number`或`string`（如`'24px'`）。
- `fontColor`: 设置字体颜色，类型为`Color`。
- `fontWeight`: 设置字体粗细，类型为`FontWeight`枚举值，如`FontWeight.Bold`。
- `textAlign`: 设置文本对齐方式，类型为`TextAlign`枚举值，如`TextAlign.Center`。
- `lineHeight`: 设置行高，类型为`number`或`string`。
- `maxLines`: 设置文本最大行数，类型为`number`。
- `overflow`: 设置文本溢出时的处理方式，类型为`TextOverflow`枚举值。

### 注意事项

- 在ArkTS中，样式设置通常是通过链式调用实现的，如上例所示。
- `Text`组件的属性可能随鸿蒙OS版本的更新而发生变化，请参考最新的官方文档。
- 除了`Text`组件外，鸿蒙还提供了`Span`组件，允许你在一个`Text`组件内部设置不同样式的文本片段。

通过上述介绍，你应该能够开始在鸿蒙应用中使用ArkTS语言来创建和定制`Text`组件了。记得查阅最新的鸿蒙开发文档，以获取更多高级功能和最佳实践。
### 文字的更改
1 resources -> base -> element -> string.json
![输入图片说明](https://foruda.gitee.com/images/1725610338138949087/95e74447_14874917.png "屏幕截图")
2 resources -> base -> en_US -> string.json
![输入图片说明](https://foruda.gitee.com/images/1725610353782396089/df28b924_14874917.png "屏幕截图")
3 resources -> base -> zh_CN -> string.json
![输入图片说明](https://foruda.gitee.com/images/1725610364046003751/c23e0bdf_14874917.png "屏幕截图")




### 用按钮实现两个页面的跳转

在鸿蒙（HarmonyOS）应用开发中，使用ArkTS（Ark TypeScript）语言可以方便地构建跨设备的用户界面。如果你想在两个页面之间通过Button控件的`onClick`事件进行跳转，你需要理解鸿蒙应用中的页面导航机制，通常是通过路由（Routing）来实现的。以下是一个基本的步骤说明，展示了如何设置Button的`onClick`事件，并在事件处理函数中实现页面跳转。

### 1. 准备页面和路由

首先，确保你的项目中至少有两个页面，我们假设这两个页面分别是`LifeCyclePage`和`SecondPage`。同时，你需要在你的应用中配置路由，使得系统能够知道如何从一个页面导航到另一个页面。在鸿蒙中，这通常是通过在`config.json`或相应的路由配置文件中定义路由来实现的。
在 resources -> profile -> main_pages.json 中编写如下链接
![输入图片说明](https://foruda.gitee.com/images/1725615197507431618/30c86858_14874917.png "屏幕截图")

### 2. 在`HomePage`中设置Button

在`HomePage`的ArkTS文件中，你需要添加一个`Button`控件，并为其设置`onClick`事件处理函数。

```typescript
`import {router} from '@kit.ArkUI'`
@Entry
@Component
struct LifeCyclePage{
  build() {
    Row() {
      Button('Go to Second Page')
        .onClick(() => {
// 在这里实现页面跳转
          router.pushUrl({
            url:'pages/SencondPage'
        })
    }
  }
}
```


### 3. 实现路由跳转

在鸿蒙开发中，页面跳转的具体实现依赖于你的项目结构和你使用的路由或状态管理库。如果你没有使用任何特定的库，你可能需要手动管理页面栈，或者依赖于鸿蒙提供的页面跳转API（如果有的话）。
### 4.在`SecondPage`中设置Button
 

```
import { router } from '@kit.ArkUI';

@Entry
@Component
struct SencondPage {
  @State message: string = "未点击登录";
  build() {
  Column(){
    Text("第二页面")
      .fontSize(20)
      .fontColor(Color.Black)
      .margin(30)
      .fontWeight('40vp')
    Button("退出")
      .fontSize(20)
      .width('120vp')
      .height('40vp')
      .backgroundColor(Color.Pink)
      .onClick( () => {
        this.message = '已经点击登录';
        router.back();
      } )
    Text(this.message)
      .fontSize(20)
      .fontColor(Color.Black)
      .margin(30)
      .fontWeight('40vp')
  }
 .width('100%')
  }
}

```
 ### router.back();可以返回跳转过来的页面
