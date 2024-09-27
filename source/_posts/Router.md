---
title: 页面跳转传参
date: 2024-09-27 10:57:01
tags:
---
在鸿蒙（HarmonyOS）系统中，页面跳转传参是一个常见的需求，它允许开发者在不同页面之间传递数据。以下是在鸿蒙系统中实现页面跳转传参的基本步骤和示例代码：

### 一、页面跳转

页面跳转通常通过路由（Router）模块来实现。在鸿蒙系统中，你可以使用`@ohos.router`模块来管理页面间的跳转。
在下面图中选择的位置要加入跳转页面的地址
![](https://foruda.gitee.com/images/1726803113937088458/d24134f9_14874917.png "屏幕截图")

#### 1. 引入路由模块

在需要进行页面跳转的页面文件中，首先需要引入路由模块：

```javascript
import router from '@ohos.router';
```

#### 2. 编写跳转逻辑


RouterParam是类
![输入图片说明](https://foruda.gitee.com/images/1726803262853265584/5d6de37a_14874917.png "屏幕截图")
里面代码如下：

```
export class RouterParam {
  src: string = '';

  constructor(src: string) {
    this.src = src;
  }
}
```
在需要触发页面跳转的组件或方法中，编写跳转逻辑。例如，在点击事件处理函数中，使用`router.pushUrl`方法实现页面跳转，并传递参数：

```
  router.pushUrl({
            url: 'pages/informationDetailsPage',
            params: new RouterParam(this.enablementItem.id)
          })
```

- url: 'pages/informationDetailsPage'：这指定了要导航到的页面的路径。在这个例子中，它指向了 'pages/informationDetailsPage'，这很可能是应用中的一个页面组件的路径。
- params: new RouterParam(this.enablementItem.id)：这指定了要传递给目标页面的参数。这里，它创建了一个 RouterParam 的新实例，并将 this.enablementItem.id 作为参数传递给该实例的构造函数。RouterParam 可能是一个自定义的类，用于封装和传递路由参数。
- 这里的`url`是目标页面的路径，`params`是一个对象，包含了需要传递的参数。


### 二、接收参数

在目标页面中，需要接收从上一个页面传递过来的参数。

#### 1. 引入路由模块

同样，在目标页面的文件中，也需要引入路由模块：

```javascript
import router from '@ohos.router';
```

#### 2. 接收参数


```
@State params: RouterParam = router.getParams() as RouterParam;
  @State message: string = this.params.src;
build() {
    Column() {
      Text('信息详情页')
        .fontSize(CommonConstants.DEFAULT_FONT_SIZE)
        .fontColor(Color.Black)
        .margin(CommonConstants.DEFAULT_MARGIN)
        .fontWeight(FontWeight.Bold)
      Text(this.message)
        .fontSize(CommonConstants.DEFAULT_FONT_SIZE)
        .fontColor(Color.Black)
        .margin(CommonConstants.DEFAULT_MARGIN)
        .fontWeight(FontWeight.Bold)
      Button("返回列表")
        .fontSize(20)
        .width('120vp')
        .height('40vp')
        .backgroundColor(Color.Orange)
        .onClick( () => {
          router.back()
        } )
    }
    .width(CommonConstants.FULL_WIDTH)
  }
```

- @State params: RouterParam = router.getParams() as RouterParam;: 定义了一个名为params的状态变量，其类型为RouterParam。通过调用router.getParams()获取当前路由的参数，并将其强制类型转换为RouterParam。这个状态变量用于存储路由传递的参数。
- @State message: string = this.params.src;: 定义了一个名为message的状态变量，类型为string。它初始化为params对象的src属性的值。这意味着message将显示从路由参数中获取的某个特定信息。


### 三、注意事项

1. **参数类型**：在传递和接收参数时，需要注意参数的类型。如果参数是复杂类型（如对象、数组等），可能需要额外的序列化/反序列化操作。
2. **页面路径**：在`router.pushUrl`方法中指定的`url`需要确保是正确的页面路径。如果路径错误，将无法跳转到目标页面。
3. **生命周期函数**：在鸿蒙系统中，页面组件有多个生命周期函数，如`onCreate`、`onPageShow`、`aboutToAppear`等。根据实际需求，在合适的生命周期函数中接收参数。
4. **错误处理**：在实际开发中，应该添加适当的错误处理逻辑，以处理可能出现的异常情况，如参数未传递、页面跳转失败等。

通过以上步骤，你可以在鸿蒙系统中实现页面跳转传参的功能。


### 登录页面完善
### 新建如下图组件
![输入图片说明](https://foruda.gitee.com/images/1726818949964165645/1a811959_14874917.png "屏幕截图")

- 假设的框架/库特点
- 链式调用：这个框架/库允许你通过链式调用方法来设置组件的属性。每个方法返回组件实例本身，从而允许你继续调用其他方法。
- 
- 组件构建：使用Column（可能是一个布局容器）来包裹子组件，如TextInput和Line。
- 
- 属性设置：每个组件都提供了多个方法来设置其样式和属性，如maxLength、type、placeholderColor等。
- 
- 样式单位：使用非标准的样式单位，如'48vp'和'18vp'，这可能代表某种形式的视口（Viewport）单位，用于确保UI在不同屏幕尺寸上的一致性。

### 编写如下代码

```
@Component
export struct UsernamePasswordForm {
  build() {
    Column() {
      TextInput({placeholder: '账号'})
        .maxLength(20)
        .type(InputType.USER_NAME)
        .placeholderColor('#99182431')
        .height('48vp')
        .fontSize('18vp')
        .backgroundColor(Color.White)
        .width('100%')
        .padding({left: 0})
        .margin({top: '12vp'})
      Line()
        .width('100%')
        .height('1vp')
        .backgroundColor('#0D182431')
      TextInput({placeholder: '密码'})
        .maxLength(20)
        .type(InputType.Password)
        .placeholderColor('#99182431')
        .height('48vp')
        .fontSize('18vp')
        .backgroundColor(Color.White)
        .width('100%')
        .padding({left: 0})
        .margin({top: '12vp'})
    }
    .backgroundColor(Color.White)
    .borderRadius('24vp')
    .padding({
      top:'4vp',
      left:'12vp',
      bottom:'4vp',
      right:'12vp'
    })
  }
}
```
 **代码解释如下：** 

```
build() {
    // 创建一个Column容器，并设置其内部的子组件
    Column() {
        // 第一个TextInput组件，用于输入账号
        TextInput({placeholder: '账号'})
            .maxLength(20) // 设置最大长度
            .type(InputType.USER_NAME) // 设置输入类型
            // ... 其他样式和属性设置

        // 创建一个Line组件，可能用作分隔线
        Line()
            .width('100%') // 宽度占满容器
            .height('1vp') // 高度为1个视口单位
            // ... 其他样式设置

        // 第二个TextInput组件，用于输入密码
        TextInput({placeholder: '密码'})
            .maxLength(20) // 设置最大长度
            .type(InputType.Password) // 设置密码输入类型
            // ... 其他样式和属性设置
    }
    // 设置Column容器的样式
    .backgroundColor(Color.White) // 背景色为白色
    .borderRadius('24vp') // 边框圆角
    // ... 其他容器样式设置
}
```

### 在页面中编写如下代码：

```
 build() {
    Column() {
      Image($r('app.media.ic_logo'))
        .width('78vp')
        .height('78vp')
        .margin({
          top:'100vp',
          bottom:'8vp'
        })
      Text($r('app.string.hello_message'))
        .fontSize('24vp')
        .fontColor('#182431')
        .fontWeight(FontWeight.Medium)
      Text($r('app.string.class_message'))
        .fontSize('16vp')
        .fontColor('#99182431')
        .margin({
          top: '8vp',
          bottom:'28vp'
        })
      UsernamePasswordForm()
      Row() {
        Text('短信验证码登录')
          .fontColor('#0A59F7')
          .fontSize('16vp')
          .fontWeight(FontWeight.Medium)
        Text('忘记密码')
          .fontColor('#0A59F7')
          .fontSize('16vp')
          .fontWeight(FontWeight.Medium)
      }
      .width('100%')
      .padding({
        top: '12vp',
        left: '24vp',
        right: '12vp'
      })
      .justifyContent(FlexAlign.SpaceBetween)
      Button("登录", {type: ButtonType.Capsule})
        .fontSize('16vp')
        .width('100%')
        .height('40vp')
        .fontWeight(FontWeight.Medium)
        .backgroundColor(Color.Orange)
        .margin({
          top: '87vp',
          bottom: '12vp'
        })
        .onClick( () => {
          this.message = "已经点击登录";
          router.pushUrl({
            url: 'pages/SecondPage',
            params: new RouterParam('账户信息')
          })
        } )
      Text('注册账号')
        .fontSize('16vp')
        .fontColor('#0A59F7')
        .fontWeight(FontWeight.Medium)
    }
    .width(CommonConstants.FULL_WIDTH)
    .height(CommonConstants.FULL_HEIGHT)
    .backgroundColor('#F1F3F5')
    .padding({
      left:'12vp',
      right:'12vp',
      bottom:'24vp'
    })
  }

```
