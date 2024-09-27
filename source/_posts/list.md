---
title: list组件使用
date: 2024-09-27 10:52:57
tags:
---
### List组件的使用
## 简介
在我们常用的手机应用中，经常会见到一些列表和网格布局方式，如设置页面、通讯录、商品列表等。下图中两个页面中“首页”页面中包含两个网格布局，“商城”页面中包含一个列表布局。

- List是很常用的滚动类容器组件，一般和子组件ListItem一起使用，List列表中的每一个列表项对应一个ListItem组件。
## 1.使用ForEach渲染列表
  **列表往往由多个列表项组成，所以我们需要在List组件中使用多个ListItem组件来构建列表，这就会导致代码的冗余。使用循环渲染（ForEach）遍历数组的方式构建列表，可以减少重复代码，示例代码如下：** 

```
 @Entry
@Component struct ListDemo {
  private arr: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
  build() {
    Column() {
      List({ space: 10 }) {
        ForEach(this.arr, (item: number) => {
          ListItem() {
            Text(`${item}`)
              .width('100%')
              .height(100)
              .fontSize(20)
              .fontColor(Color.White)
              .textAlign(TextAlign.Center)
              .borderRadius(10)
              .backgroundColor(0x007DFF)
          }
        }, (item: number) => JSON.stringify(item))
      }
    }
    .padding(12)
    .height('100%')
    .backgroundColor(0xF1F3F5)
  }
}
```
效果图如下：
![输入图片说明](https://foruda.gitee.com/images/1726219701005598286/6f07cdb8_14874917.png "屏幕截图")

## 2.ListItem
 **说明** 

- 该组件从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
- 该组件的父组件只能是List或者ListItemGroup。
- 当ListItem配合LazyForEach使用时，ListItem子组件在ListItem创建时创建。配合if/else、ForEach使用时，或父组件为List/ListItemGroup时，ListItem子组件在ListItem布局时创建
。
 **滚动条组件ScrollBar，用于配合可滚动组件使用，如List、Grid、Scroll。** 




 ### 编写首页子组件用到的ArkUI组件

 ## 自定义组件-构建单个item视图
我们在编写界面的时候，可以将常用的、重复的ArkUI组件，组合成为一个新的组件，方便重复调用，例如，消息列表。
![消息列表](https://foruda.gitee.com/images/1726220056386632235/ea57b956_484447.png "屏幕截图")

其中单个item视图：
![item视图分析](https://foruda.gitee.com/images/1726220120823018102/e5d6c625_484447.png "1718887180121.png")

这里我们将Row、Column、Text、Image组件组合成为一个新的模块，案例代码如下：
 
```
Row(){
      Column(){
        Text(this.tutorialItem.title)
          .height(19)
          .width('100%')
          .fontSize(14)
          .textAlign(TextAlign.Start)
          .textOverflow({overflow: TextOverflow.Ellipsis})
          .maxLines(1)
          .fontWeight(400)
          .margin({top: 4})
        Text(this.tutorialItem.brief)
          .height(32)
          .width('100%')
          .fontSize(12)
          .textAlign(TextAlign.Start)
          .textOverflow({overflow: TextOverflow.Ellipsis})
          .maxLines(2)
          .fontWeight(400)
          .margin({top: 4})
      }
      .height('100%')
      .layoutWeight(1)
      .alignItems(HorizontalAlign.Start)
      .margin({right: 12})

      Image(this.tutorialItem.imageSrc)
        .height('100%')
        .width(108)
        .borderRadius(16)
        .objectFit(ImageFit.Cover)
    }
    .width('100%')
    .height(88)
    .borderRadius(16)
    .backgroundColor(Color.White)
    .padding(12)
    .alignItems(VerticalAlign.Top)
```
## 父组件中数据导入到子组件中
#### 使用`@Prop`装饰器：父子单向同步
@Prop装饰的变量可以和父组件建立单向的同步关系。@Prop装饰的变量是可变的，但是变化不会同步回其父组件。

> @Prop装饰的变量和父组件建立单向的同步关系：
> - @Prop变量允许在本地修改，但修改后的变化不会同步回父组件。
> - 当数据源更改时，@Prop装饰的变量都会更新，并且会覆盖本地所有更改。因此，数值的同步是父组件到子组件（所属组件)，子组件数值的变化不会同步到父组件。



### Scroll使用

## 1 Scroll
 **可滚动的容器组件，当子组件的布局尺寸超过父组件的尺寸时，内容可以滚动。** 
 **说明** 

- 该组件从API version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
- 该组件嵌套List子组件滚动时，若List不设置宽高，则默认全部加载，在对性能有要求的场景下建议指定List的宽高。
- 该组件滚动的前提是主轴方向大小小于内容大小。
- Scroll组件通用属性clip的默认值为true。
